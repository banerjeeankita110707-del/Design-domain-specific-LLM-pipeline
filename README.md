 (cd "$(git rev-parse --show-toplevel)" && git apply --3way <<'EOF' 
diff --git a/README.md b/README.md
index 34c8cee05024a7b4248c50e91baefd4d9e13b00d..b8df727e4d9bea17d66be9855e4eb3ad8760b5d9 100644
--- a/README.md
+++ b/README.md
@@ -4,57 +4,62 @@
 An inspiring, faith‑centered portfolio repository that showcases how Islamic values can guide ethical data science, media, and technology. This space highlights research ideas, project plans, and community‑benefit applications—bridging tradition with innovation.
 
 ---
 
 ## ✨ Vision
 To build **responsible, impactful technology** that serves the Ummah and the wider world—by combining Islamic ethics, data science, and media innovation.
 
 ## 📌 Project Pillars
 ### 1) Islamic Data Science
 - **Ethical AI & Analytics**: fairness, transparency, and accountability.
 - **Community Data Projects**: mosques, nonprofits, education, and social welfare.
 - **Research Topics**: bias mitigation, privacy, and explainable AI.
 
 ### 2) Islamic Media & Technology
 - **Digital Storytelling**: rich, authentic Islamic narratives.
 - **Interactive Learning**: apps for Qur’an, Hadith, Seerah, and Arabic.
 - **Creative Tech**: VR/AR for historical journeys and immersive learning.
 
 ---
 
 ## 🧠 Key Ideas (Starter Concepts)
 - **Islamic Knowledge Graph**: connect Qur’an, Hadith, and history.
 - **Zakat Impact Dashboard**: visualize outcomes of charitable projects.
 - **Ethical Content Recommender**: media suggestions aligned with Islamic values.
 - **Multilingual Media Archive**: preserve and explore Islamic heritage.
+- **Domain-Specific LLM Pipeline**: end-to-end plan for Qur’an/Hadith/Tafsir/Fiqh modeling.
 
 ---
 
 ## 🛠️ Tech Stack (Planned)
 - **Data**: Python, Pandas, NumPy, scikit‑learn
 - **Apps**: React / Next.js, Node.js
 - **Visualization**: Plotly, D3.js
 - **Cloud**: GitHub Pages / Vercel
 
 ---
 
 ## 🚀 Roadmap
 - [ ] Curate datasets and sources (open, ethical, consent‑based)
 - [ ] Prototype a simple dashboard
 - [ ] Build a first media demo page
 - [ ] Write a research/blog post on Islamic ethics in AI
+- [ ] Draft an LLM pipeline document for Islamic text modeling
+
+## 📚 Documents
+- [Domain-Specific LLM Pipeline for Islamic Texts](islamic_llm_pipeline.md)
 
 ---
 
 ## 🤝 Contributing
 Ideas, feedback, or collaboration? Open an issue or reach out. Let’s build technology that **benefits people and honors values**.
 
 ---
 
 ## 📜 License
 This project is open for learning and collaboration. License to be added soon.
 
 ---
 
 ## 🌙 Inspiration
 > “Allah loves that when anyone of you does a job, he should perfect it.”  
 > — Prophet Muhammad ﷺ (Reported by al‑Bayhaqi)
 
EOF
)
