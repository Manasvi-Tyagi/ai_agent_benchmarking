## Project Guide: AI Agent Benchmarking (Prototype)

This guide focuses on the two implemented prototype features:

- Data Analysis with SQL Queries (CrewAI and LangGraph agents)
- Date Fruit Classification (notebook-based prototype)

It explains prerequisites, setup, how to run both features, outputs, and common issues.

---

## Architecture and Components

### A) Data Analysis with SQL Queries

- Core modules
  - PROJECT_GUIDE.md: `Data Analysis with SQL Queries/config.yaml`
  - Data: `Data Analysis with SQL Queries/data/amazon_cleaned.db`
  - Workload:
    - Questions: `Data Analysis with SQL Queries/data/questions.py`
    - Expected answers: `Data Analysis with SQL Queries/data/expected_results.csv`
  - Utilities: `Data Analysis with SQL Queries/utils/`
    - `helper_functions.py`: LLM bootstrap via `GROQ_API_KEY`, numeric extraction, CSV save, answer comparison
    - `metrics_collector.py`: iteration metrics (latency, API calls, avg API latency) + plots
    - `logging_config.py`: logger wiring (console + file)
  - Agents:
    - CrewAI runner: `Data Analysis with SQL Queries, CrewAI.py`
    - LangGraph runner: `Data Analysis with SQL Queries, LangGraph.py`

- Tools (shared idea across runners)
  - `ListSQLDatabaseTool`: list available tables
  - `InfoSQLDatabaseTool`: fetch table schema/sample rows
  - `execute_sql_tool`: executes SQL via `db.run_no_throw(query)` and returns results or an error prompt

- End-to-end flow (per iteration)
  1. Load config and LLM (Groq) using `GROQ_API_KEY`
  2. For each question in `data/questions.py`:
     - Agent calls tools sequentially: list tables → get schema → generate SQL → execute SQL
     - Extract a single numeric value from the final natural-language response
     - Append `(question, numeric_answer)` to results list
     - Track per-call latency and total iteration metrics
  3. Save iteration CSV (results) and update metrics
  4. After iterations complete:
     - Write `metrics_*.json` and `metrics_*.png` (plots)
     - Compare generated answers vs `expected_results.csv` → store match % in metrics JSON

- CrewAI specifics
  - Defines a single `Agent` and `Task`, orchestrated by a `Crew` with the Groq LLM as manager
  - Sequentially processes each question, writing iteration results and metrics

- LangGraph specifics
  - Builds a `StateGraph` with `assistant` and `tools` nodes, using `tools_condition`
  - Streams events per question, then extracts the final assistant message content for scoring
  - Includes a fallback handler to surface tool errors back to the agent

### B) Date Fruit Classification (Notebook Prototype)

- Core assets
  - Notebooks: `AI_agent_Data_analysis.ipynb`, `AI_agent_Data_analysis_with_benchmark.ipynb`, `classification_benchmark.ipynb`
  - Dataset: `data/Date_Fruit_Datasets.xlsx`
  - Reports/Charts: `reports/benchmark/*`, `reports/charts/*`, `reports/date_fruit_analysis_report_*.md`

- Typical workflow (per notebook)
  1. Load dataset and perform basic EDA (shape, distributions, correlations)
  2. Feature handling (scaling/selection as defined in notebook cells)
  3. LLM/agent-assisted analysis steps (reasoning/explanations)
  4. Classification run(s) and evaluation
  5. Benchmark logging and plots (latency/throughput/class distribution)
  6. Write summary charts and Markdown report under `reports/`

- Outputs
  - JSON benchmark snapshots, PNG charts, and Markdown narrative reports
  - Artifacts are timestamped to avoid overlap between runs

## 1) Prerequisites

- Python 3.11 recommended
- macOS or Linux shell
- A Groq API key
- (Optional) OpenAI API key if you adapt parts that use it

Make sure you have a clean virtual environment and do not hardcode secrets in source files.

---

## 2) Setup

From the project root (`/Users/garvittyagi/ai_agent_benchmarking 2 copy`):

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r "/Users/garvittyagi/ai_agent_benchmarking 2 copy/requirements.txt"

# Required
export GROQ_API_KEY="your_groq_api_key"

# Optional (only if you enable OpenAI paths yourself)
export OPENAI_API_KEY="your_openai_api_key"
```

Security note: The agent scripts currently include hardcoded API keys. Before running, remove or comment those lines and rely on your environment variables. In particular:

- `Data Analysis with SQL Queries/Data Analysis with SQL Queries, LangGraph.py` (near line 39)
- `Data Analysis with SQL Queries/Data Analysis with SQL Queries, CrewAI.py` (near lines 35–38)

---

## 3) Data Analysis with SQL Queries (Agent Benchmark)

### 3.1 Overview

Agents answer simple analytical questions against a local SQLite database by:

- Discovering tables and schema
- Generating SQL
- Executing queries against `amazon_cleaned.db`
- Returning a single numerical answer

Two frameworks are provided and benchmarked:

- CrewAI: `Data Analysis with SQL Queries/Data Analysis with SQL Queries, CrewAI.py`
- LangGraph: `Data Analysis with SQL Queries/Data Analysis with SQL Queries, LangGraph.py`

Shared resources:

- Config: `Data Analysis with SQL Queries/config.yaml`
- DB: `Data Analysis with SQL Queries/data/amazon_cleaned.db`
- Questions: `Data Analysis with SQL Queries/data/questions.py`
- Expected answers: `Data Analysis with SQL Queries/data/expected_results.csv`
- Utils: `Data Analysis with SQL Queries/utils/`

### 3.2 Configuration

Edit `Data Analysis with SQL Queries/config.yaml`:

- `model.name`: Groq model (e.g., `llama-3.3-70b-versatile`)
- `benchmarks.iterations`: total full runs (default 3)
- `logging.file`: log path (default `logs/amazon_query.log`)

### 3.3 Run (LangGraph)

```bash
cd "/Users/garvittyagi/ai_agent_benchmarking 2 copy/Data Analysis with SQL Queries"
python "Data Analysis with SQL Queries, LangGraph.py"
```

### 3.4 Run (CrewAI)

```bash
cd "/Users/garvittyagi/ai_agent_benchmarking 2 copy/Data Analysis with SQL Queries"
python "Data Analysis with SQL Queries, CrewAI.py"
```

### 3.5 Outputs

Generated under `Data Analysis with SQL Queries/results/`:

- `results_iter{N}_{Framework}.csv`: per-iteration answers
- `metrics_YYYYMMDD_HHMMSS_iter{N}.json`: all metrics (iteration time, API calls, average API latency, match %)
- `metrics_YYYYMMDD_HHMMSS_iter{N}.png`: plots summarizing metrics

Logs:

- `Data Analysis with SQL Queries/logs/amazon_query.log`

### 3.6 Troubleshooting

- Missing `GROQ_API_KEY`: export the variable in the same shell as the run
- Rate limits: lower `benchmarks.iterations` or increase backoffs in `config.yaml`
- Result length mismatch: ensure `expected_results.csv` rows match questions in `data/questions.py`
- File paths: run the scripts from the `Data Analysis with SQL Queries` directory

### 3.7 Component Map (at a glance)

- Agent shell: CrewAI or LangGraph
- LLM: Groq via `ChatGroq` (configured in `config.yaml`)
- Database: SQLite (`amazon_cleaned.db`)
- Tools: list tables, get schema, execute SQL
- Metrics: `MetricsCollector` with JSON + PNG plots
- Validation: compare vs `expected_results.csv`

---

## 4) Date Fruit Classification (Notebook Prototype)

### 4.1 Overview

The notebook-based prototype benchmarks a Groq LLM workflow for analyzing and classifying date fruit samples, generating metrics and visualizations.

Relevant assets:

- Notebooks in project root, e.g.:
  - `AI_agent_Data_analysis.ipynb`
  - `AI_agent_Data_analysis_with_benchmark.ipynb`
  - `classification_benchmark.ipynb`
- Dataset: `data/Date_Fruit_Datasets.xlsx`
- Generated assets: `reports/` (charts and Markdown reports)

### 4.2 How to Run (Jupyter)

```bash
cd "/Users/garvittyagi/ai_agent_benchmarking 2 copy"
source .venv/bin/activate  # if not already activated
python -m ipykernel install --user --name ai-agent-bench --display-name "AI Agent Bench"
jupyter notebook  # or: jupyter lab
```

In Jupyter, select the "AI Agent Bench" kernel, open one of the notebooks listed above, set `GROQ_API_KEY` in the environment (or via a cell), then execute all cells. Adjust any notebook-specific parameters as needed.

### 4.3 Outputs

Generated reports and charts:

- `reports/benchmark/benchmark_*.json`: benchmark summaries
- `reports/charts/*performance_benchmark*.png`: performance charts
- `reports/charts/*classification_distribution*.png`: distribution charts
- `reports/date_fruit_analysis_report_*.md`: narrative analysis reports

---

## 5) Quickstart

```bash
# 1) Setup
cd "/Users/garvittyagi/ai_agent_benchmarking 2 copy"
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
export GROQ_API_KEY="your_groq_api_key"

# 2) Run SQL (LangGraph)
cd "Data Analysis with SQL Queries"
python "Data Analysis with SQL Queries, LangGraph.py"

# 3) Run SQL (CrewAI)
python "Data Analysis with SQL Queries, CrewAI.py"

# 4) Run Date Fruit Classification (Notebook)
cd ..
jupyter notebook
# Open a notebook (e.g., AI_agent_Data_analysis_with_benchmark.ipynb) and run all cells.
```

---

## 6) Notes & Best Practices

- Do not hardcode API keys. Use environment variables or a private `.env` loaded via `python-dotenv`.
- Keep `benchmarks.iterations` low for smoke tests and increase when collecting final metrics.
- Modify questions in `Data Analysis with SQL Queries/data/questions.py` and ensure `expected_results.csv` stays aligned.
- Outputs are timestamped; to start fresh, you may clear `Data Analysis with SQL Queries/results/` and `Data Analysis with SQL Queries/logs/`.


