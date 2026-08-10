# 🤖 Evaluating LLM Performance in Question-Answering

### Automatic NLP Metrics + Appen Human Evaluation for LLM-Generated QA

This project develops an end-to-end evaluation framework for measuring the **quality, consistency, and reliability of Large Language Model (LLM)-generated question-answering**.

The framework combines **LLM-based question generation, document and summary-based answer generation, automatic NLP evaluation metrics, Appen-based human evaluation, and correlation analysis** to investigate how well automated metrics reflect human judgments of answer quality.

The experiments use **CNN and PubMed datasets** and leverage **Meta Llama 3.1 8B Instruct** for generative question-answering workflows.

---

## 🎯 Project Objective

Traditional question-answering systems often rely on extractive methods, where an answer is selected directly from the source text.

LLMs introduce a more challenging evaluation problem because their answers are often **abstractive**. Two answers may communicate the same meaning while using very different wording, making simple lexical comparison insufficient.

This project investigates:

- LLM-based question generation
- Document-based question answering
- Summary-based question answering
- Automatic evaluation of generated answers
- Semantic similarity between generated answers
- Human evaluation using **Appen**
- Agreement between automatic metrics and human judgments
- Reliability of different QA evaluation approaches

The primary objective is to determine whether commonly used automatic evaluation metrics provide scores that are consistent with **human assessments of LLM-generated answers**.

---

## 📚 Datasets

The implementation uses two document collections:

### 📰 CNN

CNN documents provide general-domain news content for evaluating LLM-generated questions and answers on relatively concise, real-world text.

### 🧬 PubMed

PubMed documents provide longer and more technical scientific and biomedical content, allowing the framework to evaluate LLM performance on domain-specific material.

The initial datasets contain approximately **1,000 documents from each source**.

---

## 🧹 Data Preprocessing

Before LLM inference, the datasets are cleaned and prepared for downstream QA generation.

The preprocessing workflow includes:

- Missing-value analysis
- Duplicate detection
- Removal of invalid documents
- Text normalization
- Lowercasing
- Special-character removal
- Whitespace normalization
- Document and reference-summary cleaning
- Document-length analysis
- Summary-length analysis

Cleaned versions of the CNN and PubMed datasets are then used for question and answer generation.

---

## ✨ Key Highlights

- 🤖 Implemented LLM-based question generation using **Meta Llama 3.1 8B Instruct**
- 📝 Generated **15 diverse questions per document**
- 📄 Generated answers using both **full-document context and summary context**
- 📊 Implemented multiple lexical and semantic QA evaluation metrics
- 🧠 Used **BERTScore** for contextual semantic similarity
- 👥 Incorporated **Appen-based human evaluation**
- 🔍 Evaluated question validity and answer quality through human judgments
- 📈 Performed **Pearson correlation analysis** between human scores and automatic metrics
- 📰 Evaluated the framework across both general-domain CNN and biomedical PubMed content

---

## 🧠 Evaluation Architecture

```text
                       CNN / PubMed
                            │
                            ▼
                    Data Preprocessing
                            │
                            ▼
                    Cleaned Documents
                            │
                            ▼
                  Llama 3.1 8B Instruct
                            │
                            ▼
                    Question Generation
                            │
                            ▼
                15 Questions per Document
                            │
                 ┌──────────┴──────────┐
                 │                     │
                 ▼                     ▼
          Original Document          Summary
               Context               Context
                 │                     │
                 ▼                     ▼
              Adoc                  Asum
        Document-Based Answer   Summary-Based Answer
                 │                     │
                 └──────────┬──────────┘
                            ▼
                     Answer Comparison
                            │
               ┌────────────┴────────────┐
               │                         │
               ▼                         ▼
       Automatic Evaluation       Human Evaluation
               │                     via Appen
               │                         │
       Exact Match                    Clarity
       Partial Match              Supportiveness
       BLEU                         Accuracy
       ROUGE                       Consistency
       BERTScore
               │                         │
               └────────────┬────────────┘
                            ▼
                  Pearson Correlation
                         Analysis
                            │
                            ▼
                Metric Reliability Analysis
```

---

# 🔬 Methodology

## 1. Dataset Preparation

CNN and PubMed documents are loaded and inspected for missing values, duplicates, and structural inconsistencies.

Text normalization is applied to both source documents and reference summaries to create standardized inputs for the language model.

Basic exploratory analysis is also performed to understand differences in document and summary lengths across the two datasets.

---

## 2. LLM-Based Question Generation

The project uses:

**Meta Llama 3.1 8B Instruct**

through the Hugging Face Transformers ecosystem.

A structured prompt instructs the model to generate **15 unique and diverse questions** from each document.

The generated questions cover multiple reasoning styles, including:

- Who
- What
- When
- Why
- How
- Yes/No
- Multiple-choice
- Open-ended analytical questions

Documents are processed in batches to support larger-scale generation.

---

## 3. Document-Based and Summary-Based Answer Generation

For each generated question, the LLM produces two answers using different contexts.

### Adoc — Document-Based Answer

The model receives the **original source document** as context.

```text
Question + Original Document
            ↓
           LLM
            ↓
          Adoc
```

### Asum — Summary-Based Answer

The same question is answered using only the **reference summary**.

```text
Question + Document Summary
            ↓
           LLM
            ↓
          Asum
```

Comparing `Adoc` and `Asum` makes it possible to investigate how much answer quality and consistency are preserved when the model receives condensed information rather than the complete source document.

---

# 📊 Automatic Evaluation Framework

Multiple automatic evaluation metrics are implemented to compare document-based and summary-based answers.

## Exact Match

Measures whether two generated answers match exactly after normalization.

Exact Match provides a strict lexical comparison but may underestimate quality when semantically equivalent answers use different wording.

---

## Partial Match / F1

Token overlap is used to calculate precision, recall, and F1 between answers.

This provides a more flexible lexical comparison than Exact Match.

---

## BLEU

**BLEU (Bilingual Evaluation Understudy)** measures n-gram overlap between generated answers.

It helps quantify lexical similarity while allowing some variation in wording.

---

## ROUGE

The framework evaluates:

- **ROUGE-1**
- **ROUGE-2**
- **ROUGE-L**

ROUGE measures overlap between answer sequences and captures unigram, bigram, and longest-common-subsequence similarity.

---

## BERTScore

BERTScore provides a semantic evaluation of generated answers using contextual transformer representations.

The project calculates:

- BERT Precision
- BERT Recall
- BERT F1

Unlike purely lexical metrics, BERTScore can recognize semantically similar answers even when they use different vocabulary.

---

# 👥 Appen Human Evaluation

A major component of this project is **human evaluation through Appen**.

Automatic NLP metrics cannot always determine whether an LLM-generated answer is genuinely clear, factually supported, or meaningful.

Appen-based human evaluation is therefore incorporated as an external assessment of question and answer quality.

---

## Human Evaluation Design

Human evaluators assess both the **validity of generated questions** and the **quality of generated answers**.

The evaluation considers:

### ❓ Question Validity

Evaluators determine whether a generated question is meaningful and appropriate given the source material.

Only validated questions are considered for downstream answer-quality analysis.

### 💬 Clarity

Evaluates whether the generated answer is understandable and clearly expressed.

### 📚 Supportiveness

Evaluates whether the information contained in the generated answer can be supported by the corresponding:

- Original document, or
- Document summary

### 🎯 Accuracy

Evaluates whether the information in the generated answer is correct according to the provided source context.

### 🔄 Consistency

Evaluates whether answers generated from the original document and the corresponding summary convey consistent information.

---

# 🔗 Automatic Metrics vs. Human Evaluation

The central research question is not simply:

> **Which automatic metric produces the highest score?**

Instead, the project asks:

> **Which automatic evaluation metrics best align with human judgments of LLM answer quality?**

Human-evaluation results are combined with automatic metric scores to create an integrated evaluation dataset.

---

## 📈 Pearson Correlation Analysis

A **Pearson correlation matrix** is calculated across human-evaluation scores and automatic metrics.

The analysis compares human judgments with metrics such as:

```text
Human Evaluation
        │
        ├──────── Exact Match
        ├──────── Partial F1
        ├──────── BLEU
        ├──────── ROUGE-1
        ├──────── ROUGE-2
        ├──────── ROUGE-L
        ├──────── BERT Precision
        ├──────── BERT Recall
        └──────── BERT F1
```

This analysis helps investigate whether high automatic evaluation scores correspond to high human assessments of answer quality.

A stronger relationship suggests that an automatic metric may serve as a more useful proxy for human evaluation in document-based QA tasks.

---

# 🛠️ Technology Stack

### Programming & Data Processing

`Python` · `Pandas` · `NumPy`

### Large Language Models

`Meta Llama 3.1 8B Instruct` · `Hugging Face Transformers`

### Deep Learning

`PyTorch` · `CUDA`

### NLP Evaluation

`NLTK` · `BLEU` · `ROUGE` · `BERTScore` · `Exact Match` · `Partial F1`

### Human Evaluation

`Appen`

### Analysis & Visualization

`Matplotlib` · `Seaborn` · `Pearson Correlation`

### Development

`Google Colab` · `Jupyter Notebook` · `Git` · `GitHub`

---

# 🔄 End-to-End Workflow

```text
CNN + PubMed
      │
      ▼
Data Cleaning & Preprocessing
      │
      ▼
LLM Question Generation
      │
      ▼
15 Questions / Document
      │
      ├──────────────────────┐
      ▼                      ▼
Original Document          Summary
      │                      │
      ▼                      ▼
    Adoc                   Asum
      │                      │
      └──────────┬───────────┘
                 ▼
         Automatic Evaluation
                 │
      ┌──────────┼───────────┐
      ▼          ▼           ▼
   Lexical    Semantic     Other
   Metrics      Metrics    Metrics
      │          │           │
      └──────────┴───────────┘
                 │
                 ▼
           Appen Human
            Evaluation
                 │
                 ▼
         Human Evaluation
              Scores
                 │
                 ▼
       Pearson Correlation
             Analysis
                 │
                 ▼
       Evaluation of Metric
            Reliability
```

---

# 📁 Repository Structure

```text
LLM-Question-Answering-Evaluation/
│
├── README.md
├── LLM_Question_Answering_Evaluation.ipynb
└── .gitignore
```

The notebook contains the end-to-end implementation covering:

- CNN and PubMed preprocessing
- Exploratory text analysis
- Llama-based question generation
- Prompt construction
- Batch inference
- Document-based answer generation
- Summary-based answer generation
- Exact Match evaluation
- Partial F1 evaluation
- BLEU evaluation
- ROUGE evaluation
- BERTScore evaluation
- Integration of human-evaluation results
- Pearson correlation analysis
- Visualization of evaluation relationships

---

# 💡 Why This Project Matters

LLM evaluation is fundamentally different from traditional NLP evaluation.

Consider two answers:

```text
Answer A:
The treatment reduced mortality among high-risk patients.

Answer B:
High-risk patients experienced a lower mortality rate after receiving the treatment.
```

These answers communicate similar information but have substantial lexical differences.

A strict Exact Match score could classify them as completely different, while semantic metrics or human evaluators may recognize their equivalence.

This project therefore examines evaluation from **three complementary perspectives**:

### 1. Lexical Evaluation
Exact Match, Partial Match, BLEU, and ROUGE

### 2. Semantic Evaluation
BERTScore

### 3. Human Evaluation
Appen-based assessment of validity, clarity, supportiveness, accuracy, and consistency

Correlation analysis connects these approaches to investigate which automated metrics most closely reflect human judgment.

---

# ⚠️ Limitations

- LLM outputs depend on model configuration, prompting strategy, and sampling parameters.
- Lexical metrics may penalize semantically equivalent answers that use different wording.
- Semantic metrics may capture similarity without guaranteeing factual correctness.
- Human evaluation can contain subjective variation between evaluators.
- Results from CNN and PubMed may not generalize to every domain or QA dataset.
- Correlation between a metric and human evaluation does not necessarily establish that the metric fully captures answer quality.

---

# 🔬 Research Context

This project explores a broader challenge in modern Generative AI:

> **How should we reliably evaluate open-ended LLM outputs when multiple answers can be correct?**

Rather than relying on a single metric, the project combines **lexical similarity, semantic similarity, and human judgment** to provide a more comprehensive framework for evaluating generated question-answering.

---

# 👩‍💻 Author

**Swathi Nallabilli**  
M.S. Data Science — University of North Texas

**Areas of Interest:**  
Generative AI · Large Language Models · NLP · LLM Evaluation · Machine Learning · RAG · Human-in-the-Loop AI

---
