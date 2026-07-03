# 🤖 CrewAI Market Research & Investment Analyst

An autonomous AI-powered market research system built with **CrewAI** that takes any product idea and generates a full **professional investment memo** — including market sizing, competitive analysis, customer insights, product strategy, and financial projections — in minutes.

> Built by **Inam Ur Rehman** | AI Engineer

---

## 🚀 What It Does

You give it a product idea. It spins up **5 specialized AI agents** that work sequentially — like a real research team — and outputs a professional investment memo saved as `outputs/report.md`.

**Example input:**
```
An AI powered tool that summarizes YouTube videos and automatically posts 
the summary on LinkedIn, Instagram, Facebook, X (Twitter), and WhatsApp
```

**Example output:** A 600+ word investment memo with TAM/SAM/SOM, top competitors, customer personas, MVP roadmap, 3-year revenue projections, and a Go/No-Go recommendation.

---

## 🧠 How It Works — The Agent Pipeline

5 agents run **sequentially**, each passing their output as context to the next:

```
Product Idea
     │
     ▼
┌─────────────────────────────┐
│  1. Market Research         │  → TAM, SAM, SOM, trends, regulations
│     Specialist              │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│  2. Competitive Intelligence│  → Top competitors, weaknesses, market gaps
│     Analyst                 │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│  3. Customer Insights       │  → Customer segments, pain points, pricing
│     Researcher              │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│  4. Product Strategy        │  → MVP features, tech stack, 12-month roadmap
│     Advisor                 │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│  5. Business Analyst        │  → Full investment memo (Go/No-Go)
└──────────────┬──────────────┘
               │
               ▼
        outputs/report.md
```

---

## 🛠️ Tech Stack

| Component | Technology |
|-----------|-----------|
| Agent Framework | [CrewAI](https://github.com/crewAIInc/crewAI) |
| LLM | Groq — `llama-3.3-70b-versatile` |
| Web Search | DuckDuckGo (free, no API key needed) |
| Web Scraping | CrewAI `ScrapeWebsiteTool` |
| Web UI | Streamlit |
| Package Manager | `uv` (via `pyproject.toml`) |

---

## 📁 Project Structure

```
crewai-market-research-analyst/
│
├── src/
│   └── market_research_crew/
│       ├── config/
│       │   ├── agents.yaml       # Agent roles, goals, backstories
│       │   └── tasks.yaml        # Task descriptions & expected outputs
│       │
│       ├── crew.py               # Core: agents, tasks, tools, crew assembly
│       ├── main.py               # CLI entry point
│       └── custom_tool.py        # Template for adding custom tools
│
├── app.py                        # Streamlit web UI
├── outputs/
│   └── report.md                 # Generated investment memo (auto-created)
├── pyproject.toml                # Dependencies & project config
├── AGENTS.md                     # AI coding assistant guidance
└── README.md
```

---

## ⚙️ Setup & Installation

### Prerequisites
- Python `>=3.10, <3.13`
- [uv](https://docs.astral.sh/uv/) package manager
- A free [Groq API key](https://console.groq.com/)

### 1. Clone the repo

```bash
git clone https://github.com/yourusername/crewai-market-research-analyst.git
cd crewai-market-research-analyst
```

### 2. Install dependencies

```bash
uv sync
```

### 3. Set up environment variables

Create a `.env` file in the root directory:

```env
GROQ_API_KEY=your_groq_api_key_here
```

> 💡 Get your free Groq API key at [console.groq.com](https://console.groq.com/)

---

## ▶️ Running the Project

### Option A — Command Line

```bash
uv run market_research_crew
```

Or edit `main.py` to change the product idea, then:

```bash
python src/market_research_crew/main.py
```

### Option B — Streamlit Web UI

```bash
uv run streamlit run app.py
```

Then open `http://localhost:8501` in your browser, type your product idea, and click **Generate Report**.

---

## 📊 Sample Output Structure

The generated `outputs/report.md` contains:

```
# Executive Summary
# Market Opportunity          ← TAM / SAM / SOM with data
# Competitive Landscape       ← Top 5 competitors, strengths & weaknesses
# Customer Insights           ← 3 personas, pain points, willingness to pay
# Product Strategy & Roadmap  ← MVP features, tech stack, 3-phase roadmap
# Financial Projections       ← Year 1-3 revenue, pricing tiers
# Key Risks & Mitigation
# Final Go/No-Go Recommendation
```

---

## 🔧 Configuration

### Changing the LLM

In `crew.py`, update the `llm` object:

```python
llm = LLM(
    model="groq/llama-3.3-70b-versatile",  # Change model here
    api_key=os.getenv("GROQ_API_KEY"),
    temperature=0.1,   # Lower = more consistent
    max_tokens=1500
)
```

Supported Groq models: `llama-3.3-70b-versatile`, `llama3-70b-8192`, `llama-3.1-8b-instant`

### Adding a Custom Tool

Use `custom_tool.py` as a template:

```python
class MyCustomTool(BaseTool):
    name: str = "My Tool Name"
    description: str = "What this tool does — agents read this"

    def _run(self, argument: str) -> str:
        # Your logic here
        return result
```

Then add it to any agent in `crew.py`:
```python
tools=[search_tool, my_custom_tool]
```

---

## ⚠️ Known Limitations

- **Runtime:** 5–10 minutes per report (sequential agent pipeline + LLM calls)
- **Groq rate limits:** `max_rpm` is set conservatively to avoid 429 errors
- **Hallucination risk:** Business Analyst agent is intentionally given no tools — it synthesizes only from previous agents' verified research
- **DuckDuckGo search:** Occasionally returns no results for very niche queries; agents will retry automatically

---

## 📄 License

MIT License — feel free to use, modify, and distribute.

---

## 🙏 Acknowledgements

- [CrewAI](https://github.com/crewAIInc/crewAI) — the multi-agent orchestration framework
- [Groq](https://groq.com/) — blazing fast LLM inference
- [DuckDuckGo Search](https://github.com/deedy5/duckduckgo_search) — free web search