#AI Agents Inference Benchmarking - Complete Project Guide

## 📋 Project Overview

This is a comprehensive AI agent benchmarking project that evaluates different AI agent frameworks (LangGraph, CrewAI) for various data analysis and classification tasks. The project focuses on comparing performance metrics like latency, accuracy, and API usage across different frameworks using Large Language Models (LLMs) via Groq API.

## 🎯 Project Purpose

The project aims to:
- **Benchmark AI agent frameworks** (LangGraph vs CrewAI) for data analysis tasks
- **Evaluate LLM performance** using Groq API (LLaMA models)
- **Compare inference speed and accuracy** across different frameworks
- **Provide reproducible benchmarking** with standardized metrics collection

## 📁 Project Structure

```
OmdenaKnowledge_AIAgentsInferenceBenchmarking/
├── README.md                          # Main project documentation
├── requirements.txt                   # Python dependencies
├── venv/                              # Virtual environment (Python 3.9)
│
├── Data Analysis with SQL Queries/    # SQL Query Benchmarking Module
│   ├── config.yaml                    # Configuration file
│   ├── data/
│   │   ├── amazon_cleaned.csv         # Dataset
│   │   ├── amazon_cleaned.db          # SQLite database
│   │   ├── questions.py               # Benchmark questions
│   │   └── expected_results.csv       # Expected answers
│   ├── utils/
│   │   ├── helper_functions.py       # Utility functions
│   │   ├── logging_config.py          # Logging setup
│   │   └── metrics_collector.py       # Metrics collection
│   ├── logs/                          # Log files
│   ├── results/                       # Benchmark results (CSV, JSON, PNG)
│   ├── Data Analysis with SQL Queries, CrewAI.py
│   ├── Data Analysis with SQL Queries, LangGraph.py
│   └── README.md
│
├── data/                              # Main datasets
│   └── Date_Fruit_Datasets.xlsx      # Date fruit classification dataset
│
├── reports/                           # Generated reports and visualizations
│   ├── benchmark/                     # Benchmark JSON files
│   ├── charts/                       # Performance charts (PNG)
│   └── date_fruit_analysis_report_*.md
│
└── Notebooks/                         # Jupyter notebooks
    ├── AI_agent_Data_analysis.ipynb
    ├── AI_agent_Data_analysis_with_benchmark.ipynb
    ├── classification_benchmark.ipynb
    └── CrewAI_data_analyst_Agent.ipynb
```

## 🔧 Components Breakdown

### 1. **Data Analysis with SQL Queries** Module

**Purpose**: Benchmarks CrewAI vs LangGraph for SQL-based data analysis tasks

**Key Features**:
- Connects to SQLite database (Amazon product data)
- Uses three tools:
  - `list_tables_tool`: Lists available database tables
  - `get_schema_tool`: Retrieves table schemas
  - `execute_sql_tool`: Executes SQL queries
- Evaluates accuracy by comparing agent answers to expected numerical results
- Collects metrics: API calls, latency, retries, correctness percentage

**Files**:
- `Data Analysis with SQL Queries, CrewAI.py` - CrewAI implementation
- `Data Analysis with SQL Queries, LangGraph.py` - LangGraph implementation
- `config.yaml` - Configuration (LLM models, iterations, paths)

**Dataset**: `amazon_cleaned.db` (SQLite database with Amazon product data)

### 2. **Date Fruit Classification** Module

**Purpose**: Classifies date fruits using AI agents with feature analysis

**Key Features**:
- Uses Date Fruit Dataset with features like:
  - Area, Perimeter, Major/Minor Axis
  - Eccentricity, Solidity, Convex Area
  - Texture and color features
- Classification labels: BERHI, DEGLET, DOKOL, etc.
- Feature preprocessing and scaling
- Performance benchmarking with latency tracking

**Notebooks**:
- `AI_agent_Data_analysis.ipynb` - Main analysis notebook
- `AI_agent_Data_analysis_with_benchmark.ipynb` - With benchmarking
- `classification_benchmark.ipynb` - Classification benchmarking

**Dataset**: `Date_Fruit_Datasets.xlsx`

### 3. **Disaster Tweet Detection** Module

**Purpose**: Benchmarks AI agents for disaster tweet classification

**Key Features**:
- Hybrid approach: ML models + LLMs
- Evaluates inference speed and accuracy
- Uses Twitter disaster tweet dataset from Omdena

**Location**: `ai-disaster-tweets-detection-agent/`

**Dataset**: `tweet_data_clean.csv` (from Omdena datasets)

## 🚀 How to Start the Project

### Step 1: Environment Setup

1. **Navigate to project directory**:
```bash
cd /Users/garvittyagi/ai_benchmarking/OmdenaKnowledge_AIAgentsInferenceBenchmarking
```

2. **Activate virtual environment**:
```bash
source venv/bin/activate
```

3. **Verify Python version** (should be 3.9):
```bash
python --version
```

4. **Install dependencies** (if not already installed):
```bash
pip install -r requirements.txt
```

### Step 2: API Keys Configuration

You need API keys for:
- **Groq API** (for LLaMA models)
- **LangSmith** (optional, for tracing)
- **OpenAI API** (if using OpenAI models)

**Option A: Environment Variables**
Create a `.env` file in the project root:
```env
GROQ_API_KEY=your_groq_api_key_here
OPENAI_API_KEY=your_openai_api_key_here
LANGCHAIN_API_KEY=your_langsmith_key_here
LANGCHAIN_TRACING_V2=true
LANGCHAIN_PROJECT=LiveLangraph
```

**Option B: Direct in Code**
The notebooks have API keys hardcoded (not recommended for production):
```python
groq_api_key = "your_key_here"
langsmith = "your_key_here"
```

### Step 3: Running Different Modules

#### A. **SQL Query Analysis Benchmark**

1. **Navigate to module directory**:
```bash
cd "Data Analysis with SQL Queries"
```

2. **Configure settings** in `config.yaml`:
   - Set LLM models (Groq models)
   - Set number of iterations
   - Configure logging paths

3. **Run CrewAI benchmark**:
```bash
python "Data Analysis with SQL Queries, CrewAI.py"
```

4. **Run LangGraph benchmark**:
```bash
python "Data Analysis with SQL Queries, LangGraph.py"
```

5. **View results**:
   - CSV files in `results/` directory
   - Metrics JSON files with timestamps
   - Visualization PNG files
   - Logs in `logs/amazon_query.log`

#### B. **Date Fruit Classification** (Jupyter Notebooks)

1. **Start Jupyter**:
```bash
jupyter notebook
# or
jupyter lab
```

2. **Open notebook**:
   - `AI_agent_Data_analysis.ipynb` - Main analysis
   - `AI_agent_Data_analysis_with_benchmark.ipynb` - With benchmarking
   - `classification_benchmark.ipynb` - Classification only

3. **Select correct kernel**:
   - In VS Code/Cursor: Click kernel selector → Choose `venv/bin/python`
   - Or select "Python (venv)" kernel

4. **Run cells sequentially**:
   - Install packages if needed
   - Set API keys
   - Load and analyze data
   - Run classification and benchmarking


### Results Location

- **SQL Query Analysis**: `Data Analysis with SQL Queries/results/`
  - CSV files: `results_iter{N}_CrewAI.csv`, `results_iter{N}_LangGraph.csv`
  - Metrics JSON: `metrics_{timestamp}_iter{N}.json`
  - Charts: `metrics_{timestamp}_iter{N}.png`

- **Date Fruit Analysis**: `reports/`
  - Benchmark JSON: `reports/benchmark/benchmark_*.json`
  - Charts: `reports/charts/*.png`
  - Reports: `reports/date_fruit_analysis_report_*.md`

## 🔑 Key Configuration Files

### `config.yaml` (SQL Query Module)
```yaml
llm:
  crewai_agent_model: "groq/llama-3.3-70b-versatile"
  langgraph_agent_model: "llama-3.3-70b-versatile"
  temperature: 0.1
  max_tokens: 1000

benchmarks:
  iterations: 3
  total_questions: 5

logging:
  file: "logs/amazon_query.log"
  level: "INFO"
```

### Models Used
- **LLaMA 3.1-8B-instant** (Date Fruit Classification)
- **LLaMA 3.3-70B-versatile** (SQL Query Analysis)
- **LLaMA 3.2-90B-vision-preview** (Rating model)

## 🛠️ Troubleshooting

### Common Issues

1. **ModuleNotFoundError for pandas**:
   - Ensure you've activated the venv
   - Select correct kernel in Jupyter
   - Install packages: `pip install -r requirements.txt`

2. **API Key Errors**:
   - Check `.env` file exists
   - Verify API keys are set correctly
   - Check environment variables: `echo $GROQ_API_KEY`

3. **Database Connection Issues**:
   - Verify `amazon_cleaned.db` exists in `Data Analysis with SQL Queries/data/`
   - Check SQLite installation: `sqlite3 --version`

4. **Kernel Selection Issues**:
   - In VS Code/Cursor: Use kernel selector in top-right
   - Select: `venv/bin/python` or "Python (venv)"
   - Restart kernel after switching

## 📚 Dependencies

Key packages (from `requirements.txt`):
- `langchain`, `langchain-groq`, `langchain-community`
- `langgraph`, `langgraph-checkpoint`
- `crewai` (for CrewAI framework)
- `pandas`, `numpy`, `matplotlib`, `seaborn`
- `scikit-learn`, `scipy`
- `jupyter`, `ipykernel`, `ipython`
- `sqlalchemy` (for database connections)
- `python-dotenv` (for environment variables)

## 🎯 Next Steps

1. **Run benchmarks** for each module
2. **Compare results** across frameworks
3. **Analyze performance metrics** in generated reports
4. **Customize configurations** for your use case
5. **Extend with new datasets** or tasks

## 📝 Notes

- The project uses **Python 3.9** in the virtual environment
- Some packages may require Python 3.10+ (versions have been adjusted)
- Benchmarks run multiple iterations for statistical reliability
- All results are timestamped for tracking
- Logs are saved for debugging and analysis

---

**Happy Benchmarking! 🚀**

