 (cd "$(git rev-parse --show-toplevel)" && git apply --3way <<'EOF' 
diff --git a/islamic_llm_pipeline.md b/islamic_llm_pipeline.md
new file mode 100644
index 0000000000000000000000000000000000000000..dbf8e5e4983b9178ebe385a6d6af2c0d82af3af4
--- /dev/null
+++ b/islamic_llm_pipeline.md
@@ -0,0 +1,160 @@
+# Domain-Specific LLM Pipeline for Islamic Texts
+
+This document outlines an end-to-end pipeline to build a domain-specific large language model (LLM) focused on Islamic texts (Qur’an, Hadith, Tafsir, and Fiqh) using PyTorch, Hugging Face, and distributed training.
+
+## 1) Data Curation and Governance
+
+### 1.1 Source Inventory
+- **Qur’an**: Uthmani script, multiple translations, word-by-word morphology, revelation order metadata.
+- **Hadith**: Canonical collections (e.g., Bukhari, Muslim, Abu Dawud, Tirmidhi, Nasa’i, Ibn Majah) with isnad and grading (sahih/hasan/da’if).
+- **Tafsir**: Classical and modern exegesis (e.g., Tabari, Ibn Kathir, Qurtubi, Jalalayn), with verse alignment.
+- **Fiqh**: Madhhab texts, usul al-fiqh references, fatwa archives with version history and jurisdiction metadata.
+
+### 1.2 Legal, Ethical, and Theological Compliance
+- Verify licensing, provenance, and usage permissions for each dataset.
+- Store source hashes, version tags, and citation metadata.
+- Separate **authoritative** vs **opinionated** material and label by genre.
+- Create a **scholar review board** checklist for sensitive content.
+
+### 1.3 Dataset Schema (Normalized)
+- `source_type`: quran | hadith | tafsir | fiqh
+- `source_name`: collection or author
+- `language`: ar | en | ur | fr | etc.
+- `text`: normalized text
+- `metadata`: JSON (surah/ayah, hadith grade, madhhab, topic tags, references)
+- `span_alignment`: verse/hadith alignment indices
+
+## 2) Preprocessing and Normalization
+
+### 2.1 Text Normalization
+- Arabic normalization: unify alef/ya/ta marbuta variants, remove diacritics (optional), normalize punctuation.
+- English translation normalization: lowercasing, Unicode cleanup, quote standardization.
+- Preserve **verse boundaries** and **isnad** blocks with special tokens.
+
+### 2.2 Segmenting and Alignment
+- Segment into **document blocks** (e.g., Qur’an ayah, hadith matn + isnad, tafsir paragraph, fiqh ruling).
+- Align tafsir and fiqh commentary to Qur’an verses when available.
+- Create **cross-reference links** for verse/hadith citations.
+
+### 2.3 Tokenization
+- Train a **multilingual SentencePiece tokenizer** with domain-specific vocabulary.
+- Include special tokens:
+  - `<AYAH>`, `<SURAH>`, `<HADITH>`, `<ISNAD>`, `<MATN>`, `<TAFSIR>`, `<FIQH>`, `<FATWA>`
+- Maintain two tokenizers if needed:
+  - **Arabic-heavy tokenizer** (Qur’an, classical Arabic)
+  - **Multilingual tokenizer** (translations, commentaries)
+
+### 2.4 Data Splits
+- Stratify by source type, era, madhhab, and language.
+- Keep **verse/hadith blocks intact** across splits.
+- Define **gold evaluation sets** with scholar annotations.
+
+## 3) Model Architecture (from Scratch)
+
+### 3.1 Base Model
+- **Decoder-only Transformer** (GPT-style) for generative tasks.
+- Hidden size and depth scaled by budget:
+  - **Small**: 350M–1B
+  - **Medium**: 1B–7B
+  - **Large**: 7B+
+
+### 3.2 Training Objectives
+- Autoregressive language modeling (causal LM).
+- Optional **span corruption** auxiliary objective for improved factual recall.
+
+### 3.3 Implementation Stack
+- **PyTorch** + **Hugging Face Transformers** + **Accelerate/DeepSpeed**.
+- Use **FSDP** or **ZeRO-3** for large models.
+- Mixed precision (bf16/float16), gradient checkpointing.
+
+## 4) Pretraining Pipeline
+
+### 4.1 Data Loader
+- Streaming dataset using `datasets` library.
+- Dynamic packing of sequences with length-balanced batches.
+- Curriculum strategy: start with Qur’an + tafsir, then add hadith and fiqh.
+
+### 4.2 Training Setup
+- Cosine LR schedule with warmup.
+- AdamW optimizer with weight decay.
+- Periodic evaluation on held-out verses and hadith.
+- Save checkpoints with **data versioning** tags.
+
+### 4.3 Distributed Training
+- Multi-node training with NCCL backend.
+- Use **Hugging Face Accelerate** config for cluster.
+- Shard datasets across nodes, deterministic shuffling.
+
+## 5) Task-Specific Fine-Tuning
+
+### 5.1 QA (Question Answering)
+- Build QA dataset from Qur’an/Hadith queries.
+- Fine-tune with **instruction-style prompts**.
+- Include answer citations: verse/hadith reference tags.
+
+### 5.2 Summarization
+- Summarize tafsir or fiqh rulings while preserving scholarly nuance.
+- Evaluate against gold summaries written by scholars.
+
+### 5.3 Legal Reasoning (Fiqh)
+- Train with multi-step reasoning format:
+  - Evidence → Principle → Ruling → Caveats
+- Include **madhhab-specific** conditioning tokens.
+
+## 6) Evaluation Framework
+
+### 6.1 Domain Benchmarks
+- Qur’an verse retrieval and thematic classification.
+- Hadith grading consistency (match known grades).
+- Fiqh ruling consistency with known decisions.
+
+### 6.2 Comparative Evaluation
+- Evaluate against general LLMs (e.g., Llama, GPT-style baselines).
+- Metrics: accuracy, BLEU/ROUGE for summaries, retrieval recall.
+- Use **human evaluation** for theological coherence.
+
+### 6.3 Safety and Hallucination Analysis
+- Create adversarial prompts to test fabricated citations.
+- Measure hallucination frequency and severity.
+- Implement **truthfulness filters**:
+  - Citation verification (match source index)
+  - Refusal policies for unverifiable claims
+
+## 7) Theological Consistency and Risk Analysis
+
+### 7.1 Consistency Checks
+- Cross-validate responses with known Qur’an/hadith databases.
+- Track **source alignment**: ensure outputs reference known verses/hadith.
+- Use rule-based filters for sensitive topics (aqidah, legal rulings).
+
+### 7.2 Scholarly Review Loop
+- Build a workflow for **scholar review** of model outputs.
+- Continuous feedback for fine-tuning and filtering.
+
+### 7.3 Risk Mitigation
+- Add **disclaimer templates** for speculative interpretations.
+- Use **confidence calibration** and uncertainty estimation.
+- Provide abstentions when no reliable sources found.
+
+## 8) Deployment Considerations
+
+- Host in a controlled environment with **audit logs**.
+- Provide transparent model cards and dataset documentation.
+- Implement **rate limits** and **misuse monitoring**.
+
+---
+
+## Suggested Hugging Face Configuration (Example)
+
+- **Model Config**: custom GPT-2 style config with domain tokenizer.
+- **Training**: `Trainer` or `accelerate` with `DeepSpeed`.
+- **Evaluation**: custom script combining retrieval + generation metrics.
+
+---
+
+## Deliverables
+- Cleaned and versioned datasets.
+- Tokenizer models.
+- Pretrained base LLM.
+- Fine-tuned task models (QA, summarization, fiqh reasoning).
+- Evaluation report and theological review summary.
 
EOF
)
