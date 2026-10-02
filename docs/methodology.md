
# Research Methodology

## Overview

ScriptoNet follows an interdisciplinary methodology combining Digital Humanities, Literary Studies, Natural Language Processing, Machine Learning, and Explainable AI.

The objective is to develop an AI-assisted framework that can identify and map representations of therapeutic processes in literary fiction.

The AI system is designed as a supporting tool for literary analysis, not as a replacement for human interpretation.


# Research Workflow

The research follows five major stages:

Literary Corpus Preparation

↓

Text Processing and Passage Creation

↓

Human Annotation

↓

AI Model Development

↓

Evaluation and Interpretation


---

# Stage 1: Literary Corpus Preparation

The first stage involves collecting literary texts suitable for computational analysis.

Initial experiments will use publicly available literary works.

The corpus will contain:

- Author information
- Work title
- Text content
- Chapter structure
- Metadata


The first experimental corpus will focus on literary fiction containing themes related to:

- trauma
- memory
- emotional conflict
- identity
- healing
- transformation


---

# Stage 2: Text Processing

Literary works are converted into machine-readable format.

Processing steps include:

1. Text cleaning

Removal of:

- unnecessary formatting
- metadata
- unwanted symbols


2. Passage segmentation

Large novels are divided into smaller meaningful passages.

Example:

Novel

↓

Chapter

↓

Paragraph

↓

Passage


Each passage becomes an individual research unit.


---

# Stage 3: Human Annotation

Artificial Intelligence requires examples created through expert interpretation.

Human annotators identify:

## Therapeutic Representation

Does the passage represent a therapeutic or healing-related process?

Labels:

- Yes
- No


## Therapeutic Mechanism

Possible categories:

1. Scriptotherapy

Writing, confession, or documentation represented as emotional processing.


2. Poetry-mediated Processing

Poetry represented as a method of emotional expression or reflection.


3. Music-mediated Processing

Music represented as a pathway for emotional access or memory.


4. Narrative Reconstruction

A character changes their understanding of past experiences or identity.


5. Emotional Catharsis

Expression or release of suppressed emotions.


## Intensity

The strength of the represented therapeutic process can be rated on a scale.

Example:

1 = weak representation

5 = strong representation


---

# Stage 4: Artificial Intelligence Development

After annotation, machine learning models are trained using labelled literary passages.


## Baseline Models

Initial experiments will use traditional machine learning methods:

- TF-IDF
- Logistic Regression
- Support Vector Machines
- Random Forest


Purpose:

Create a performance baseline.


## Advanced NLP Models

Later experiments will explore transformer-based language models:

- BERT
- RoBERTa


These models can capture contextual meaning beyond individual keywords.


---

# Stage 5: Explainable AI

Because literary interpretation requires transparency, explainability is an important component.

The research will investigate:

- Which words influenced predictions?
- Which phrases contributed to classification?
- Do model explanations align with literary interpretation?


Possible methods:

- SHAP
- Integrated Gradients


The goal is to make AI predictions understandable to humanities researchers.


---

# Evaluation

The system will be evaluated using:

## Classification Performance

Metrics:

- Accuracy
- Precision
- Recall
- F1-score


## Human Agreement

Compare AI predictions with expert annotations.


## Interpretation Analysis

Study whether AI-identified patterns correspond with meaningful literary interpretations.


---

# Expected Contribution

The research aims to create:

1. An AI-assisted framework for analysing therapeutic representations in literature.

2. A structured dataset of annotated literary passages.

3. A bridge between computational methods and humanities interpretation.

4. A reproducible methodology for Digital Humanities research.
