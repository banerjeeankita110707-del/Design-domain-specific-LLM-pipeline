 (cd "$(git rev-parse --show-toplevel)" && git apply --3way <<'EOF' 
diff --git a/docs/autonomous_islamic_knowledge_ai.md b/docs/autonomous_islamic_knowledge_ai.md
new file mode 100644
index 0000000000000000000000000000000000000000..7a0067e6bf7b57552928eb20b47290c2106ce4c0
--- /dev/null
+++ b/docs/autonomous_islamic_knowledge_ai.md
@@ -0,0 +1,121 @@
+# Autonomous AI System for Discovering Patterns in Islamic Knowledge
+
+## 1) System Goal
+Build an autonomous research system that discovers patterns across Islamic knowledge sources (Qur’an, Hadith, Seerah, classical tafsir, and modern scholarship) by combining NLP, knowledge graphs, reasoning engines, hypothesis generation, and automated evaluation against textual corpora. The system records its findings, confidence, and evidence over time.
+
+---
+
+## 2) High-Level Architecture
+
+### A. Data & Corpora Layer
+- **Sources**: Qur’an text (Arabic + translations), Hadith collections, Seerah biographies, classical tafsir, academic papers.
+- **Ingestion**: PDF/HTML/XML/JSON parsers, OCR, metadata extraction (author, date, genre, chain of narration, etc.).
+- **Normalization**: Arabic diacritics handling, tokenization, lemmatization, transliteration alignment, language detection.
+- **Storage**: Document store (e.g., Parquet/Arrow) + vector store for embeddings.
+
+### B. NLP Processing Layer
+- **Tokenization & Lemmatization**: Arabic morphology + English lemmatizers.
+- **NER & Entity Linking**: Entities such as prophets, locations, events, themes, rulings.
+- **Topic Modeling / Thematic Clustering**: LDA/BERTopic for thematic discovery.
+- **Semantic Embeddings**: Multilingual embeddings for cross-language retrieval.
+
+### C. Knowledge Graph Layer
+- **Graph Schema**: Entities (people, places, concepts), relations ("mentions", "supports", "contradicts", "chronology", "is‑theme‑of").
+- **Graph AI**: Graph neural networks for link prediction and anomaly detection.
+- **Provenance**: Every node/edge includes sources, citations, and chain-of-evidence.
+
+### D. Reasoning & Hypothesis Engine
+- **Symbolic Reasoning**: Rule-based checks (e.g., chronology consistency).
+- **Statistical Reasoning**: Bayesian inference over themes and evidence.
+- **Hypothesis Generator**: Proposes claims like: "Theme X intensifies across Meccan surahs" or "Narrative Y co-occurs with concept Z in early tafsir."
+- **Hypothesis Scoring**: Relevance, novelty, evidence strength, cross-source consistency.
+
+### E. Evaluation & Testing Framework
+- **Retrieval‑Augmented Testing**: Fetch evidence passages and compare with hypothesis.
+- **Counter‑Evidence Search**: Actively search for contradicting passages.
+- **Confidence Calibration**: Bayesian updating, probability of truth with uncertainty.
+
+### F. Discovery Logging & Governance
+- **Discovery Ledger**: Immutable log of hypotheses, evidence, scores, and outcomes.
+- **Audit Trail**: Links to original sources and model versions.
+- **Human Review Workflow**: Scholars can accept, reject, or flag findings.
+
+---
+
+## 3) Workflow (End‑to‑End)
+1. **Ingest texts** into the corpus.
+2. **Run NLP** to extract entities, topics, and embeddings.
+3. **Construct/Update knowledge graph** with new entities/relations.
+4. **Generate hypotheses** from graph patterns, topic trends, or statistical signals.
+5. **Test hypotheses** via targeted retrieval and contradiction search.
+6. **Score & log** each hypothesis with evidence trails.
+7. **Reinforcement learning loop** adjusts hypothesis strategies over time.
+
+---
+
+## 4) Reinforcement Learning + Graph AI Integration
+
+### RL Agent (Discovery Agent)
+- **State**: Current graph snapshot, recent hypotheses, confidence trends.
+- **Actions**: Explore new themes, request more evidence, refine or discard hypotheses.
+- **Reward**: High for validated, novel, and well-supported discoveries; negative for weak or contradicted claims.
+
+### Graph AI
+- **Link Prediction**: Suggest new relations to test.
+- **Community Detection**: Find clusters of related concepts.
+- **Temporal Graph Modeling**: Detect changes in thematic emphasis across time.
+
+---
+
+## 5) Example Hypotheses
+- "Mentions of mercy cluster more densely in early Meccan surahs."
+- "Certain legal concepts show stronger co-occurrence in Medinan surahs."
+- "Narrative motifs appear with different framing across tafsir traditions."
+
+Each hypothesis must include:
+- Evidence passages with citations
+- Counter‑examples or contradictions
+- Confidence score with uncertainty
+
+---
+
+## 6) System Outputs
+- **Discovery Log** (JSON/DB): hypothesis, evidence, score, decision.
+- **Knowledge Graph Updates**: new links or refined entities.
+- **Reports**: interpretable dashboards for scholars and researchers.
+
+---
+
+## 7) MVP Implementation Plan
+1. **Corpus ingestion**: Qur’an + one Hadith dataset + one tafsir.
+2. **NLP pipeline**: tokenization, lemmatization, embeddings.
+3. **Knowledge graph prototype**: store entities + relations with citations.
+4. **Hypothesis generator**: topic trends + co‑occurrence rules.
+5. **Evaluation loop**: retrieval + contradiction checks.
+6. **Logging layer**: store hypotheses with evidence.
+
+---
+
+## 8) Suggested Tech Stack
+- **NLP**: spaCy, NLTK, CAMeL Tools, HuggingFace Transformers
+- **Embeddings**: multilingual sentence transformers
+- **Graph**: Neo4j or RDF triple store, PyTorch Geometric
+- **Reasoning**: RDF/OWL rules + Bayesian inference
+- **RL**: Stable‑Baselines3, Ray RLlib
+- **Infra**: Python, FastAPI, Docker, MLflow for experiment tracking
+
+---
+
+## 9) Ethical & Scholarly Safeguards
+- **Explainability first**: every claim must cite evidence.
+- **Respect for sources**: preserve original text and context.
+- **Human oversight**: final interpretations reviewed by scholars.
+- **Bias checks**: track language and translation effects.
+
+---
+
+## 10) Next Steps
+- Define the initial schema for the knowledge graph.
+- Choose corpora and license‑compliant sources.
+- Build a prototype pipeline with a small subset of text.
+- Set up evaluation metrics for hypothesis quality.
 
EOF
)
