# 🔬 AI Research System — Multi-Agent

> An autonomous, multi-agent AI pipeline that transforms any research topic into a full, structured research report — powered by **Groq**, **LangGraph**, **LangChain**, and **Tavily**.

---

## ✨ What It Does

Type a topic, click a button — get back a polished research report complete with an introduction, key findings, conclusion, cited sources, and an AI critic's score and feedback. No searching, reading, or writing required.

---

## 🏗️ Architecture

The system runs a **4-step agentic pipeline**, where each step is handled by a dedicated agent or chain:

```
User Input (Topic)
       │
       ▼
┌─────────────────┐
│  Search Agent   │  ← LangGraph ReAct agent + Tavily Search
│  (Step 1)       │    Finds top 5 relevant web results
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Reader Agent   │  ← LangGraph ReAct agent + BeautifulSoup / Tavily Extract
│  (Step 2)       │    Picks the best URL and scrapes full page content
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Writer Chain   │  ← LangChain prompt chain + Groq LLM
│  (Step 3)       │    Writes a structured report from gathered research
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Critic Chain   │  ← LangChain prompt chain + Groq LLM
│  (Step 4)       │    Reviews, scores, and gives feedback on the report
└─────────────────┘
         │
         ▼
  📄 Final Report + Score
```

### Step-by-Step Breakdown

| Step | Component | Role |
|------|-----------|------|
| 1 | **Search Agent** | Queries Tavily Search API for the top 5 results (title, URL, snippet) |
| 2 | **Reader Agent** | Picks the most useful URL; scrapes with `requests + BeautifulSoup`, falls back to Tavily Extract if blocked |
| 3 | **Writer Chain** | Combines all gathered research and generates a structured report via Groq LLM |
| 4 | **Critic Chain** | Independently reviews the report and returns a score, strengths, improvements, and a one-line verdict |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Language** | Python 3.9+ |
| **UI** | Streamlit |
| **LLM** | Groq — `llama-3.1-8b-instant` |
| **Agent Framework** | LangGraph (`create_react_agent`) |
| **LLM Chains** | LangChain (`ChatPromptTemplate` + `StrOutputParser`) |
| **Web Search** | Tavily Python SDK |
| **Web Scraping** | `requests` + `BeautifulSoup4` + Tavily Extract (fallback) |
| **Environment** | `python-dotenv` |

---

## 📁 Project Structure

```
Multi-agent-research-system/
│
├── app.py              # Streamlit UI + pipeline orchestration
├── agents.py           # Search Agent, Reader Agent, Writer & Critic chains
├── tools.py            # web_search and scrape_url tool definitions
├── pipeline.py         # Standalone CLI pipeline runner
├── requirements.txt    # All Python dependencies
└── .env                # API keys (never commit this!)
```

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Aryan1patel/AI-Research-System---Multi-Agent.git
cd AI-Research-System---Multi-Agent
```

### 2. Create and activate a virtual environment

```bash
python3 -m venv .venv
source .venv/bin/activate        # macOS / Linux
.venv\Scripts\activate           # Windows
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Set up API keys

Create a `.env` file in the project root:

```env
GROQ_API_KEY=gsk_...
TAVILY_API_KEY=tvly-...
```

> Get your keys at → [Groq Console](https://console.groq.com) · [Tavily](https://app.tavily.com)

### 5. Run the app

```bash
streamlit run app.py
```

Then open **http://localhost:8501** in your browser.

---

## 🖥️ CLI Usage (No UI)

You can also run the pipeline directly from the terminal:

```bash
python pipeline.py
```

You'll be prompted to enter a topic, and the full pipeline will run in your terminal with step-by-step output.

---

## 📄 Sample Output

```
Score: 8/10

Strengths:
- Well-structured with clear sections
- Cites multiple reliable sources
- Key findings are specific and evidence-backed

Areas to Improve:
- Could explore more recent (2025) studies
- Conclusion could be more actionable

One line verdict:
A thorough and professional report that covers the topic with depth and clarity.
```

---

## ⚙️ How the Scraper Handles Bot Protection

The `scrape_url` tool uses a two-layer strategy:

1. **Primary** — Direct HTTP request with realistic browser headers (handles most sites)
2. **Fallback** — Tavily Extract API (handles Cloudflare-protected and JS-rendered sites)

---

## 📦 Key Dependencies

```
langchain >= 0.2.0
langchain-groq >= 0.1.0
langgraph >= 0.1.0
streamlit >= 1.35.0
tavily-python >= 0.3.0
beautifulsoup4 >= 4.12.0
python-dotenv >= 1.0.0
```

---

## 🤝 Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss what you'd like to change.

---

## 📜 License

This project is open source and available under the [MIT License](LICENSE).
