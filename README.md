# Layout-Aware DocQA

A NLP study evaluating whether document layout information improves question answering and evidence retrieval compared with text-only retrieval.

## Project Overview

Document Question Answering (DocQA) systems answer natural-language questions about document images such as forms, reports, invoices, and business records.

Traditional systems often extract text using OCR and then search the extracted text. However, plain text may lose important spatial relationships, such as:

- Which value belongs to which table column
- Which heading belongs to a paragraph
- Which text belongs to the left or right column
- Which words form a document section

This project investigates whether using layout information improves document question answering and evidence retrieval.

## Research Question

> Does incorporating document layout information improve question answering and evidence retrieval compared with text-only retrieval?

The project will first establish text-only baselines. A layout-aware retrieval method will be implemented and evaluated in the next project phase.

## Dataset

The primary dataset is the **DocVQA Single Page Document Visual Question Answering dataset**.

The dataset includes:

- Document-page images
- Natural-language questions
- Human-provided answers
- OCR text
- Word- and line-level coordinates
- Question types and document identifiers

The official dataset page is:

<https://www.docvqa.org/datasets/docvqa>

The dataset can be downloaded through the official RRC challenge portal under:

**Task 1 – Single Page Document Visual Question Answering**

The raw document images are provided for non-commercial research and educational use. The dataset terms must be reviewed before use. Raw images and restricted download URLs must not be committed to or redistributed through this repository.

## Task Definition

Each example contains a question about a document page.

### Input

At prediction time, the system receives:

- A document image
- A natural-language question
- OCR-extracted text
- OCR word or line coordinates

### Output

The system produces:

- A predicted answer or answer-supporting passage
- A ranked list of relevant document text regions
- Evidence information when reliable evidence regions can be identified

The correct answer is used only for training, validation, or evaluation. It is not provided to the system during prediction.

## Project Methods

### October Baselines

The first phase focuses on establishing reproducible baselines.

#### Baseline 1: BM25 Text Retrieval

BM25 ranks OCR text passages according to their lexical similarity to the question.

This provides a simple and interpretable text-only reference.

#### Baseline 2: Dense Text Retrieval

If feasible, a dense retrieval baseline will represent questions and OCR passages as vector embeddings and rank them according to semantic similarity.

This provides a stronger text-only comparison.

### Planned Proposed Method

The proposed method will incorporate document layout information, such as:

- Word coordinates
- Text-region positions
- Spatial grouping
- Line or region structure
- Relationships between nearby text elements

The layout-aware method will be compared with the text-only baselines using the same documents, questions, data splits, and evaluation protocol.

### Planned Ablation

The main ablation will remove or shuffle layout information while keeping the remaining retrieval process unchanged.

This will help determine whether improvements come specifically from spatial information.

## Evaluation

The primary metric will be selected after verifying whether the dataset supports reliable evidence evaluation.

Possible metrics include:

- Evidence Recall@K
- Answer Exact Match
- Answer F1
- Average Normalized Levenshtein Similarity
- Mean Reciprocal Rank

The main evaluation will use a document-level split. All questions from the same document must remain in the same partition to prevent data leakage.

### Planned Error Analysis

The team will inspect performance on:

- Tables
- Forms
- Multi-column pages
- Dense text pages
- OCR-error examples
- Repeated answers
- Complex spatial layouts

The team will analyze examples where:

- Text-only retrieval succeeds
- Layout-aware retrieval succeeds
- Both methods fail
- The answer is correct but the evidence region is incorrect

## Data Split and Information Boundaries

The team will use separate development, validation, and protected test partitions.

- **Development/training data:** Used to fit learned components when necessary.
- **Validation data:** Used for parameter selection and system decisions.
- **Protected test data:** Used only for the final fixed comparison.

Questions from the same document will not be divided across different partitions.

The team will record:

- Dataset version
- Download date
- Document identifiers
- Split identifiers
- Preprocessing rules
- Model configurations
- Random seeds
- Evaluation commands
- Predictions and metrics

## Installation

Clone the repository:

```bash
git clone https://github.com/YOUR-USERNAME/layout-aware-docqa.git
cd layout-aware-docqa
```

Create a virtual environment:

```bash
python -m venv .venv
```

Activate it on Windows:

```bash
.venv\Scripts\activate
```

Activate it on macOS or Linux:

```bash
source .venv/bin/activate
```

Install the dependencies:

```bash
pip install -r requirements.txt
```

## Planned Dependencies

The project may use the following Python packages:

```text
numpy
pandas
scikit-learn
rank-bm25
Pillow
tqdm
matplotlib
sentence-transformers
```

The final dependency list will be updated after the baseline implementation is finalized.

## Data Setup

1. Create a local data directory:

```text
data/
├── images/
├── ocr/
└── annotations/
```

2. Download the DocVQA Task 1 files from the official RRC portal.
3. Extract the image, OCR, and annotation files into the appropriate local folders.
4. Do not commit the raw dataset to GitHub.
5. Record the dataset version and download date in the evidence record.

Example local structure:

```text
data/
├── images/
│   └── documents/
├── ocr/
│   ├── train/
│   ├── validation/
│   └── test/
└── annotations/
    ├── train.json
    ├── validation.json
    └── test.json
```

The exact folder names may differ depending on the downloaded archive structure.

## Running the Baselines

The exact commands will be added after implementation.

Example planned commands:

```bash
python src/create_split.py --config configs/baseline_config.yaml
```

```bash
python src/bm25_baseline.py --config configs/baseline_config.yaml
```

```bash
python src/dense_retrieval_baseline.py --config configs/baseline_config.yaml
```

```bash
python src/evaluate.py --predictions results/baseline_predictions.json
```

## Current Project Status

### Completed

- Project direction selected
- DocVQA Task 1 selected as the primary dataset
- Research question defined
- Text-only and layout-aware comparison planned
- Document-level split requirement identified
- Dataset archives downloaded

### In Progress

- Dataset inspection
- OCR and annotation verification
- Document-level split creation
- BM25 baseline implementation
- Evaluation protocol finalization

### Planned

- Dense text-retrieval baseline
- Layout-aware retrieval method
- Controlled ablation
- Error analysis
- Reproducibility check by another teammate

## Limitations

The standard DocVQA dataset may not provide an explicit evidence bounding box for every answer. Therefore, evidence regions may need to be inferred by matching answer text to OCR tokens.

This may be unreliable when:

- The answer appears multiple times
- OCR contains errors
- The answer requires multiple regions
- The answer depends on a table relationship
- The evidence is visual rather than textual

The project will report this limitation clearly and will use answer-level evaluation or manually verified evidence subsets when necessary.

The results may not generalize to:

- All PDF documents
- Handwritten documents
- Languages other than English
- Private enterprise document collections
- Documents outside the DocVQA distribution

## License and Dataset Usage

The code in this repository is intended for academic and research use.

The DocVQA dataset is governed by the terms provided by the dataset organizers and the original document sources. Users must review and follow those terms. Raw document images and restricted download links should not be redistributed through this repository.

## Citation

If you use the DocVQA dataset, cite:

```bibtex
@InProceedings{mathew2021docvqa,
    author = {Mathew, Minesh and Karatzas, Dimosthenis and Jawahar, C. V.},
    title = {DocVQA: A Dataset for VQA on Document Images},
    booktitle = {Proceedings of the IEEE/CVF Winter Conference on Applications of Computer Vision},
    year = {2021}
}
```
