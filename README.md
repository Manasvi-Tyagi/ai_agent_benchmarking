# 🤖 AI Agents Inference Benchmarking System

A benchmarking framework designed to evaluate the inference performance, reasoning quality, and classification accuracy of LLM-powered AI agents on structured tabular datasets.

The project leverages **LLaMA 3.1-8B-Instant** through the Groq API and utilizes **LangGraph** for agent orchestration to benchmark AI-driven classification tasks on the Date Fruit Dataset.

---

## 🚀 Features

### 🧠 LLM-Powered Classification

* Uses **LLaMA 3.1-8B-Instant** for feature interpretation and classification
* Automated reasoning over structured agricultural data
* Natural language explanations for model predictions

### 🔄 Agent Workflow Orchestration

* Built using **LangGraph**
* Modular agent pipeline for preprocessing, analysis, and classification
* Extensible architecture for benchmarking multiple AI agents

### 📊 Performance Benchmarking

* Inference latency measurement
* Accuracy evaluation against ground truth labels
* Response consistency analysis
* End-to-end benchmark reporting

### 📈 Visualization & Analytics

* Benchmark performance dashboards
* Latency distribution analysis
* Classification performance charts
* Dataset feature exploration and reporting

---

## 🏗️ System Architecture

```text
Date Fruit Dataset
        │
        ▼
 Data Preprocessing
        │
        ▼
 Feature Engineering
        │
        ▼
 LangGraph Agent Pipeline
        │
        ▼
 LLaMA 3.1-8B-Instant
        │
        ▼
 Prediction & Reasoning
        │
        ▼
 Benchmark Evaluation
        │
        ▼
 Reports & Visualizations
```

---

## 🛠️ Tech Stack

### AI & Agent Frameworks

* LangGraph
* Groq API
* LLaMA 3.1-8B-Instant

### Machine Learning & Evaluation

* Scikit-Learn
* NumPy

### Data Processing

* Pandas

### Visualization

* Matplotlib
* Seaborn

### Utilities

* Python
* JSON
* dotenv
* Regular Expressions

---

## 📂 Dataset

The system evaluates agents using the **Date Fruit Dataset**, containing numerical and morphological characteristics of different date fruit varieties.

### Sample Features

* Area
* Perimeter
* Major Axis Length
* Minor Axis Length
* Convex Area
* Solidity
* Eccentricity
* Texture Features
* Color Features

### Target Classes

* BERHI
* DEGLET
* DOKOL
* ROTANA
* SAFAWI
* SOGAY
* And other date fruit varieties

---

## 📊 Benchmark Metrics

The framework evaluates agent performance across multiple dimensions:

| Metric                   | Description                     |
| ------------------------ | ------------------------------- |
| Inference Latency        | Response time per prediction    |
| Classification Accuracy  | Prediction correctness          |
| Consistency Score        | Stability across runs           |
| Feature Analysis Quality | Quality of generated reasoning  |
| Throughput               | Samples processed per unit time |

---

## 🎯 Key Components

### DateFruitAgent

An intelligent agent responsible for:

* Feature preprocessing
* Data interpretation
* Fruit classification
* Reasoning generation
* Benchmark result logging

### Benchmark Engine

* Execution time tracking
* Accuracy measurement
* Agent performance comparison
* Automated report generation

### Reporting Module

* JSON benchmark exports
* Visualization generation
* Performance summaries
* Statistical analysis

---

## 📈 Results

### Performance Highlights

* Average inference latency: **2–5 seconds**
* Automated classification using LLM reasoning
* Benchmark tracking across multiple samples
* Detailed performance analytics and visual reporting

---

## 📁 Generated Outputs

```text
reports/
├── benchmark/
│   └── benchmark_results.json
├── charts/
│   ├── latency_analysis.png
│   ├── accuracy_report.png
│   └── benchmark_summary.png
└── analysis_report.json
```

---

## ▶️ Running the Project

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Configure Environment Variables

```bash
GROQ_API_KEY=your_api_key
```

### Launch Notebook

```bash
jupyter notebook
```

Run the benchmarking workflow to generate evaluation reports and visualizations.

---

## 🎯 Key Highlights

* LLM-powered classification benchmark system
* Agent orchestration using LangGraph
* Automated performance evaluation pipeline
* Latency and accuracy benchmarking
* Explainable AI reasoning generation
* Structured reporting and visualization framework

---

## Future Improvements

* Multi-agent benchmarking support
* Comparison across multiple LLMs
* RAG-enhanced classification workflows
* Distributed benchmark execution
* Real-time evaluation dashboards

---

## 📄 License

MIT License
