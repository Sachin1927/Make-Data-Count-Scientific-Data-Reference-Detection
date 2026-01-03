# Make-Data-Count-Scientific-Data-Reference-Detection

## 📌 Project Overview
This project was developed for the **Make Data Count (MDC)** competition to address the issue of undervaluing scientific data. The goal is to build a pipeline that parses scientific literature (PDFs and XMLs) to identify citations of research data and classify them into two categories:
* **Primary:** Data generated as part of the study (e.g., *"we collected..."*).
* **Secondary:** Data reused from existing sources (e.g., *"dataset from previous study..."*).

This solution implements a high-performance **Heuristic & Regular Expression Pipeline** utilizing **Polars** for efficient data manipulation.

## ⚙️ Technical Approach

### 1. Data Ingestion & Parsing
The pipeline handles heterogeneous file formats from the scientific corpus:
* **PDF Parsing:** Utilized `pymupdf` to extract raw text from PDF documents.
* **XML Parsing:** Implemented `lxml` to parse specific XML schemas (`JATS`, `TEI`, `Wiley`, `BioC`) and extract body text while ignoring metadata.

### 2. Candidate Extraction (Regex)
Developed a robust Regular Expression (Regex) engine to identify dataset identifiers across various standards, including:
* **Accession Numbers:** (e.g., `GSE`, `PRJNA`, `SRP`, `CHEMBL`).
* **DOIs:** Identification of `10.xxxx` patterns and specific repositories (Dryad, Zenodo, Figshare).
* **Bioinformatics IDs:** Patterns for PDB, UniProt, and GenBank identifiers.

### 3. Classification Logic
Instead of a "black box" model, this pipeline uses a transparent context-window analysis to classify citations:
* **Context Extraction:** Extracts a window of text surrounding the matched ID.
* **Heuristic Classification:** Applies keyword proximity matching using optimized regex patterns:
    * *Primary Indicators:* "we used", "collected", "generated", "submitted".
    * *Secondary Indicators:* "previous study", "obtained from", "reference to".

### 4. Performance Optimization
* **Polars Integration:** Replaced standard Pandas workflows with **Polars** for faster string manipulation and efficient memory usage during the extraction phase.
* **False Positive Filtering:** Implemented validation logic to remove stub DOIs, self-citations, and malformed brackets/parentheses.

## 📊 Results
The pipeline achieved competitive performance on the Kaggle Public Leaderboard:
* **Public F1 Score:** 0.5788
* **Validation F1 Score:** 0.5110 (Training Split)

## 🛠️ Installation & Usage

### Prerequisites
* Python 3.10+
* Polars
* PyMuPDF
* lxml

### Setup
```bash
# Install dependencies
pip install polars pymupdf lxml tqdm matplotlib
