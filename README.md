# clinvar-variant-analysis
ClinVar variant annotation for Cystic Fibrosis, Sickle Cell Disease, and Huntington Disease
# ClinVar Variant Analysis – Genetic Disorders Assignment

## Overview

This repository contains a comprehensive variant analysis of three genetic disorders using **ClinVar**, **OMIM**, **UCSC Genome Browser**, **VarSome**, and **ACMG/AMP** classification guidelines. The goal was to select clinically relevant pathogenic variants, annotate them using established bioinformatics databases, and generate a patient-format VCF file for ClinVar submission.

---

## Selected Diseases & Variants

| # | Disease | Gene | Variant | ClinVar ID | Clinical Significance |
|---|---------|------|---------|------------|----------------------|
| 1 | Cystic Fibrosis | CFTR | p.Phe508del (c.1521_1523delCTT) | VCV000007107 | Pathogenic (5-star) |
| 2 | Sickle Cell Disease | HBB | p.Glu7Val (c.20A>T) | VCV000015126 | Pathogenic (5-star) |
| 3 | Huntington Disease | HTT | CAG repeat expansion ≥36 (c.52_54CAG[≥36]) | VCV000017153 | Pathogenic (5-star) |

## Pipeline / Steps Performed

### Step 1 – Disease & Variant Selection
Three well-characterized genetic disorders were selected. The most clinically relevant pathogenic variant for each was identified from ClinVar:

- **Cystic Fibrosis** → F508del (most common CF mutation, ~70% of CF alleles)
- **Sickle Cell Disease** → HbS mutation (p.Glu7Val), causes RBC sickling
- **Huntington Disease** → HTT CAG repeat expansion ≥36 repeats

### Step 2 – ClinVar Annotation (Excel Sheet)
For each variant, the following fields were filled in `ClinVar_Assignment.xlsx`:
- Variant ID, Gene, Chromosome Location, Variant Type
- HGVS nucleotide and protein change notation
- Clinical significance
- ClinVar explanation (studies/observations summary)
- OMIM phenotype description

### Step 3 – OMIM Phenotype Lookup
OMIM was consulted for phenotype descriptions of each variant/gene:

- **CFTR** → OMIM #219700 – Autosomal recessive; chronic pulmonary disease, pancreatic insufficiency, elevated sweat Cl⁻, male infertility
- **HBB** → OMIM #603903 – Autosomal recessive; vaso-occlusive crises, hemolytic anemia, stroke, organ damage
- **HTT** → OMIM #143100 – Autosomal dominant; progressive chorea, cognitive decline, psychiatric symptoms, onset 30–50 years

### Step 4 – UCSC Genome Browser: AlphaMissense & RAVEL Scores
The UCSC Genome Browser was used to visualize **AlphaMissense** and **RAVEL** pathogenicity scores at the variant sites.

- For **HBB (rs334, chr11:5,226,997-5,227,007)**:
  - AlphaMissense scores for all mutation types (A, C, G, T) were captured — red bars indicate **high pathogenicity**
  - RAVEL scores were similarly captured showing pathogenic signal
  - Screenshot saved in `screenshots/UCSC_HBB_AlphaMissense_RAVEL.png`
- **CFTR (F508del) and HTT (CAG expansion)**: AlphaMissense and RAVEL are **N/A** as these tools score missense variants only — deletions and repeat expansions are not applicable

### Step 5 – VarSome Variant Annotation
Each variant was validated using [VarSome](https://varsome.com) for independent annotation:

- **CFTR rs113993960** – Deletion, CFTR(NM_000492.4):c.1521_1523del p.(Phe508del) → ClinVar: **Pathogenic** (84 submissions), phyloP100: 7.544
- **HBB rs334** – SNV, HBB(NM_000518.5):c.20A>T p.(Glu7Val) → ClinVar: **Pathogenic** (63 submissions), Uniprot: Pathogenic, GWAS: Red cell distribution width
- **HTT rs193922916** – SNV/missense used as proxy annotation → ClinVar: **Pathogenic** (1 submission), phyloP100: 6.549

### Step 6 – ACMG/AMP Classification
Each variant was classified according to **ACMG/AMP 2015 guidelines**:

| Variant | ACMG Classification | Criteria Met |
|---------|--------------------|-|
| CFTR p.Phe508del | **Pathogenic** | PVS1, PS1, PM3, PP3, PP5 |
| HBB p.Glu7Val | **Pathogenic** | PS1, PS3, PM1, PP2, PP5 |
| HTT CAG expansion ≥36 | **Pathogenic** | PVS1, PS1, PS3, PP4, PP5 |

### Step 7 – VCF File Generation
A VCF file was constructed in standard patient WGS/WES format, as if derived from a real patient's sequencing data. This file was then submitted to ClinVar's variant annotator.

---


## Databases & Tools Used

| Tool | Purpose | URL |
|------|---------|-----|
| ClinVar | Variant clinical significance, submissions | https://www.ncbi.nlm.nih.gov/clinvar/ |
| OMIM | Gene-phenotype disease descriptions | https://www.omim.org/ |
| UCSC Genome Browser | AlphaMissense & RAVEL score visualization | https://genome.ucsc.edu/ |
| VarSome | Variant annotation and ACMG classification | https://varsome.com/ |
| GRCh38/hg38 | Reference genome assembly used | – |

---

## Key Findings Summary

- **Cystic Fibrosis (F508del)**: The most prevalent CF mutation globally (~70% of CF alleles). Causes ER retention and degradation of misfolded CFTR protein, abolishing chloride channel function. PVS1-level evidence; 5-star ClinVar rating with 84 pathogenic submissions.

- **Sickle Cell Disease (HbS/p.Glu7Val)**: Substitution of glutamic acid with valine at codon 7 of beta-globin. Under hypoxic conditions, HbS polymerizes causing RBC sickling, vaso-occlusion, and multi-organ damage. AlphaMissense score: 0.92 (Likely Pathogenic); RAVEL: 0.87 (Pathogenic).

- **Huntington Disease (CAG expansion)**: Trinucleotide repeat expansion in HTT exon 1. 36–39 repeats = reduced penetrance; ≥40 repeats = full penetrance. Toxic gain-of-function via polyglutamine aggregation in striatal and cortical neurons. Age of onset inversely correlated with repeat length.





