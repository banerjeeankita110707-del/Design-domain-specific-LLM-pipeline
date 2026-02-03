 (cd "$(git rev-parse --show-toplevel)" && git apply --3way <<'EOF' 
diff --git a/README.md b/README.md
index 34c8cee05024a7b4248c50e91baefd4d9e13b00d..f17ac371ae2feeb5c60f0fea26ec50fe6001dddd 100644
--- a/README.md
+++ b/README.md
@@ -36,25 +36,30 @@ To build **responsible, impactful technology** that serves the Ummah and the wid
 - **Cloud**: GitHub Pages / Vercel
 
 ---
 
 ## 🚀 Roadmap
 - [ ] Curate datasets and sources (open, ethical, consent‑based)
 - [ ] Prototype a simple dashboard
 - [ ] Build a first media demo page
 - [ ] Write a research/blog post on Islamic ethics in AI
 
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
+
+---
+
+## 🧩 Autonomous Islamic Knowledge Discovery
+- See the system design: [Autonomous AI System for Discovering Patterns in Islamic Knowledge](docs/autonomous_islamic_knowledge_ai.md)
 
EOF
)
