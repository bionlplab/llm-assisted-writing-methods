# LLM assisted writing deserves empirical evaluation

Xuan Zhong Feng<sup>1</sup>, Yi Lin<sup>1</sup>, Yiye Zhang<sup>1</sup>, Chunhua Weng<sup>2</sup>, Yifan Peng<sup>1,*</sup>

<sup>1</sup> Department of Population Health Sciences, Weill Cornell Medicine, New York, NY, USA<br>
<sup>2</sup> Department of Biomedical Informatics, Columbia University, New York, NY, USA<br>
<sup>*</sup> Corresponding author: yip4002@med.cornell.edu

## Read the methods

This document preserves all information from the manuscript Methods section. Its wording and numerical values are unchanged; headings, lists, and citations are adapted for GitHub. Read the [PDF](methods.pdf) or the [original Methods TeX](methods-body.tex). Supplementary Prompts 1–6 and Supplementary Tables S1–S3 refer to the accompanying [Supplementary Information](supplementary-information.pdf).

- [Methods in Markdown](METHODS.md): complete methods, with navigable headings, equations, country lists, and references.
- [Methods PDF](methods.pdf): typeset reading copy.
- [Supplementary Information](supplementary-information.pdf): the existing supplementary tables and prompts cited by the methods.
- [Methods source](methods-body.tex), [standalone TeX wrapper](methods.tex), and [bibliography](references.bib): editable source and references.

The Methods section was relocated from the revised manuscript to keep the manuscript in Comment format. The complete source text is preserved verbatim in `methods-body.tex`; the Markdown changes presentation only. The supplementary PDF is an unchanged copy of the accompanying manuscript supplement. This repository contains documentation, not analysis code, trained models, or underlying datasets.

## Reviewer navigation

- Classifier training, calibration, and cohort assignment: [Identification of LLM-assisted writing](METHODS.md#identification-of-llm-assisted-writing).
- Semantic embeddings and topic mapping: [Feature extraction](METHODS.md#feature-extraction).
- Reference-list and citation measures: [Outcome measures](METHODS.md#outcome-measures).
- Journal access classification and sensitivity analyses: [Journal access model](METHODS.md#journal-access-model).
- Statistical comparisons and multiplicity adjustment: [Statistical analysis](METHODS.md#statistical-analysis).

## Build the PDF

With a TeX distribution providing the packages in `methods.tex` and the `naturemag` bibliography style:

```sh
pdflatex methods.tex
bibtex methods
pdflatex methods.tex
pdflatex methods.tex
```

Documentation snapshot: 2 November 2026.
