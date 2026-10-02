
# Dataset Documentation

## Overview

ScriptoNet requires a structured literary dataset for training and evaluating AI models.

The dataset consists of literary passages that are analysed for representations of therapeutic processes.

The purpose of the dataset is computational literary analysis, not clinical assessment.

---

# Data Structure

Each dataset entry represents one literary passage.

Example structure:

| Field | Description |
|---|---|
| passage_id | Unique identifier |
| author | Author name |
| work | Literary work title |
| chapter | Chapter or section information |
| text | Literary passage |
| therapeutic_label | Presence or absence of therapeutic representation |
| mechanism | Type of therapeutic process |
| intensity | Strength of representation |

---

# Therapeutic Categories

The dataset uses the following categories:

## Scriptotherapy

Writing, confession, letters, or documentation represented as emotional processing.

## Poetry-mediated Processing

Poetry used for reflection or emotional expression.

## Music-mediated Processing

Music represented as emotional access or memory.

## Narrative Reconstruction

A changed understanding of past experiences or identity.

## Emotional Catharsis

Expression or release of suppressed emotions.

---

# Data Sources

Initial experiments will use publicly available literary works.

Copyright-protected texts will not be uploaded to this repository.

Only:

- metadata
- processing scripts
- annotation guidelines
- derived research outputs

will be shared.

---

# Future Dataset Development

Future versions may include:

- multiple authors
- different literary traditions
- expert annotations
- expanded therapeutic categories

The goal is to create a reproducible Digital Humanities research workflow.
