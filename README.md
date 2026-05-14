# 🎯 Resume Analyzer

An **AI-powered resume matcher** that uses semantic embeddings to score how well your resume fits a job description — with detailed skill gap analysis, keyword density, experience matching, and formatting feedback.

---

## ✨ Features

- 📄 **PDF resume parsing** via PyMuPDF
- 🧠 **Semantic matching** using `all-mpnet-base-v2` (80–85% accuracy)
- 🎯 **Skill gap analysis** — matched, missing, and partial skills
- 🔍 **Keyword density scoring** from job description
- 📅 **Experience year extraction** and requirement comparison
- 💼 **Job title similarity** detection
- 🧾 **Formatting quality** checker
- 📊 **Weighted overall score** with interactive gauges

---

## 🚀 Deploy on Hugging Face Spaces (Recommended)

### Step 1 — Create a new Space

1. Go to [huggingface.co/spaces](https://huggingface.co/spaces)
2. Click **"Create new Space"**
3. Fill in:
   - **Space name:** `resume-analyzer-pro`
   - **License:** MIT
   - **SDK:** `Streamlit`
   - **Visibility:** Public or Private
4. Click **"Create Space"**

### Step 2 — Upload your files

In your new Space, go to the **"Files"** tab → **"Add file"** and upload:

```
app.py
requirements.txt
```

> ⚠️ Do **not** upload `Dockerfile` or `docker-compose.yml` — Hugging Face manages the container for you when using the Streamlit SDK.

### Step 3 — Wait for build

Hugging Face will automatically install dependencies and launch the app.  
**First build takes ~5–8 minutes** (downloads the `all-mpnet-base-v2` model, ~420MB).

Once status shows **"Running"**, your app is live at:
```
https://huggingface.co/spaces/<your-username>/resume-analyzer-pro
```

### Step 4 — (Optional) Upgrade hardware

Default **CPU Basic (Free)** works fine. For faster inference:
- Go to **Settings → Space hardware**
- Upgrade to **CPU Upgrade** or **T4 GPU**

---

## 🐳 Run Locally with Docker

```bash
# Clone the repo
git clone https://github.com/<your-username>/Resume.git
cd Resume

# Build and start
docker compose up --build

# Open in browser
http://localhost:8501
```

> First run downloads the model (~420MB). Subsequent starts use the cached volume and are instant.

---

## 💻 Run Locally without Docker

```bash
# 1. Create a virtual environment
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate

# 2. Install dependencies
pip install -r requirements.txt

# 3. Launch
streamlit run app.py
```

---

## 🧠 How Scoring Works

| Component | Weight | Method |
|-----------|--------|--------|
| Semantic content match | 35% | Cosine similarity of resume vs job description embeddings |
| Skill match | 25% | Batch embedding similarity per skill vs threshold |
| Keyword density | 15% | Exact keyword presence from job description |
| Experience years | 10% | Regex extraction + requirement comparison |
| Job title match | 10% | Title embedding vs resume similarity |
| Formatting quality | 5% | Bullet count, line length, caps usage |

---

## 📁 Project Structure

```
Resume/
├── app.py               # Main Streamlit application
├── requirements.txt     # Python dependencies
├── Dockerfile           # Container definition (local / Docker deploy)
├── docker-compose.yml   # Local orchestration with resource limits
└── README.md
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| [Streamlit](https://streamlit.io) | Web UI framework |
| [sentence-transformers](https://www.sbert.net) | Semantic embeddings (`all-mpnet-base-v2`) |
| [PyMuPDF](https://pymupdf.readthedocs.io) | PDF text extraction |
| [Plotly](https://plotly.com) | Interactive gauge charts |

---

## 📌 Usage

1. Enter your skills in the **sidebar** (comma-separated, e.g. `Python, Docker, AWS`)
2. Upload your **resume as a PDF**
3. Paste the **job description**
4. Review your match scores and recommendations

---

## 🐛 Bug Fixes (v2)

| Bug | Fix |
|-----|-----|
| Skill embeddings re-encoded per skill in a loop | Batch encode all skills in one call — ~20x faster |
| `extract_job_title` returned a list instead of a string → runtime crash | Refactored to always return a plain `str` |
| `fuzzywuzzy` / `python-Levenshtein` in requirements but unused | Removed — slimmer install |
| `language_tool_python` + Java in Dockerfile but not used in code | Removed — saves ~400MB image size |
| `mem_limit` / `cpu_count` deprecated in Compose v3 | Moved to `deploy.resources` |

---

## 📄 License

MIT License — free to use, modify, and deploy.
