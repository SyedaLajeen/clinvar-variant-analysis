# 🧬 Genomic Variant Pathogenicity Analysis — Assignment 2

Investigating the pathogenicity of 6 clinically significant genetic variants using ClinVar, OMIM, UCSC Genome Browser, and ACMG/AMP guidelines.

> All coordinates based on **GRCh38**.

---

## Selected Variants

### Common Genetic Diseases

| Disease | Gene | Variant (HGVS) | GRCh38 Position | ClinVar ID | Review Status |
|---|---|---|---|---|---|
| Hereditary Breast & Ovarian Cancer | BRCA2 | NM_000059.4:c.5946delT (p.Ser1982Argfs*22) | chr13:32,340,301 | 52289 | Pathogenic  |
| Cystic Fibrosis | CFTR | NM_000492.4:c.1521_1523delCTT (p.Phe508del) | chr7:117,559,591 | 7105 | Pathogenic |
| Phenylketonuria (PKU) | PAH | NM_000277.3:c.1222C>T (p.Arg408Trp) | chr12:102,838,900 | — | Pathogenic |

### Rare Genetic Diseases

| Disease | Gene | Variant (HGVS) | GRCh38 Position | ClinVar ID | Review Status |
|---|---|---|---|---|---|
| Huntington's Disease | HTT | NM_002111.8:c.53CAG[>36] | chr4:3,074,877 | — | Pathogenic  |
| Marfan Syndrome | FBN1 | NM_000138.5:c.5788G>A (p.Gly1930Ser) | chr15:~48,700,000 | — | Pathogenic  |
| Gaucher Disease (Type 1) | GBA1 | NM_000157.4:c.1226A>G (p.Asn409Ser) | chr1:155,234,452 | 4288 | Pathogenic  |

---

## Methodology

### 1. Variant Selection — ClinVar

Each disease was searched in [ClinVar](https://www.ncbi.nlm.nih.gov/clinvar/) and filtered for **Pathogenic / Likely Pathogenic** variants with ≥2-star review status and multiple independent submitters. For each variant, the gene name, HGVS nomenclature, GRCh38 coordinates, ClinVar ID, variant type, and associated conditions were extracted. Supporting functional studies and clinical observations were reviewed and summarized in the Excel sheet.

### 2. Phenotype & Inheritance — OMIM

Each gene was looked up in [OMIM](https://www.omim.org/) to document the disease phenotype, major clinical features, and mode of inheritance.

| Gene | OMIM # | Inheritance | Key Features |
|---|---|---|---|
| BRCA2 | #612555 | Autosomal Dominant | ~70% lifetime breast cancer risk, ~20% ovarian cancer risk; also pancreatic and male breast cancer |
| CFTR | #219700 | Autosomal Recessive | Obstructive lung disease, pancreatic insufficiency, elevated sweat chloride, male infertility |
| PAH | #261600 | Autosomal Recessive | Intellectual disability (if untreated), seizures, hypopigmentation, mousy odor |
| HTT | #143100 | Autosomal Dominant | Progressive chorea, dementia, psychiatric disturbance; fatal 15–20 years post-onset |
| FBN1 | #154700 | Autosomal Dominant | Aortic root dilation/dissection, ectopia lentis, tall stature, arachnodactyly |
| GBA1 | #230800 | Autosomal Recessive | Hepatosplenomegaly, pancytopenia, bone marrow infiltration, no CNS involvement (Type 1) |

### 3. Computational Pathogenicity Scores — UCSC Genome Browser

Each variant was visualized in the [UCSC Genome Browser](https://genome.ucsc.edu/) (hg38) with two tracks enabled:

- **AlphaMissense** — AI-based missense pathogenicity classifier (score >0.564 = likely pathogenic)
- **REVEL** — Ensemble score integrating 13 in silico tools (score >0.5 = likely pathogenic)

The classification label and numerical score were recorded for each missense SNV. Screenshots were taken at each locus and saved in the `/Screenshots` directory. 

### 4. ACMG/AMP Variant Classification

Each variant was evaluated using the [ACMG/AMP 2015 guidelines](https://www.acmg.net/) across evidence criteria spanning four strength levels — Very Strong (PVS1), Strong (PS1–PS4), Moderate (PM1–PM5), and Supporting (PP1–PP5). Evidence types considered included functional studies, population frequency (gnomAD), computational predictions, co-segregation, and clinical observations. A final classification was assigned per variant with full justification recorded in the Excel file.

### 5. VCF File Construction & ClinVar Annotation

A multi-variant VCF (v4.2) was manually built using GRCh38 coordinates for all 6 variants. INFO fields included `GENE`, `PROT`, `CONSEQUENCE`, `CLNSIG`, `CLNDN`, `CLNREVSTAT`, `ALPHAMISSENSE`, `REVEL`, ACMG criteria, and `TRANSCRIPT`. The file was then compressed, sorted, indexed, and annotated against the official NCBI ClinVar GRCh38 VCF using `vcfanno` in WSL (Ubuntu).

---

## Results Summary

| Disease | Gene | Variant | AlphaMissense | REVEL | ACMG Criteria | Classification |
|---|---|---|---|---|---|---|
| Hereditary Breast & Ovarian Cancer | BRCA2 | p.Ser1982Argfs*22 | N/A‡ | N/A‡ | PVS1, PS4, PM2, PP5 | **Pathogenic** |
| Cystic Fibrosis | CFTR | p.Phe508del | N/A* | N/A* | PS3, PS4, PM3_very_strong, PM4, PP3 | **Pathogenic** |
| Phenylketonuria (PKU) | PAH | p.Arg408Trp | 0.9612 | 0.931 | PS1, PS3, PM1, PM2, PP3, PP5 | **Pathogenic** |
| Huntington's Disease | HTT | CAG >36 repeats | N/A† | N/A† | PVS1-equivalent, PS4, PS3, PM2 | **Pathogenic** |
| Marfan Syndrome | FBN1 | p.Gly1930Ser | 0.9203 | 0.887 | PM1, PM2, PM5, PP2, PP3, PP5 | **Pathogenic** |
| Gaucher Disease (Type 1) | GBA1 | p.Asn409Ser | 0.8934 | 0.821 | PS1, PS3, PM3, PP3, PP5 | **Pathogenic** |


---

## Steps to Reproduce

> All commands run in **WSL (Ubuntu)**.

```bash
# Install tools
sudo apt update && sudo apt install bcftools tabix wget -y

# Install vcfanno
wget https://github.com/brentp/vcfanno/releases/download/v0.3.5/vcfanno_linux64
chmod +x vcfanno_linux64 && sudo mv vcfanno_linux64 /usr/local/bin/vcfanno

# Download ClinVar GRCh38 reference
wget https://ftp.ncbi.nlm.nih.gov/pub/clinvar/vcf_GRCh38/clinvar.vcf.gz
wget https://ftp.ncbi.nlm.nih.gov/pub/clinvar/vcf_GRCh38/clinvar.vcf.gz.tbi

# Compress and index patient VCF
# (tabix used as workaround for contig header parsing issue during sort)
bgzip patient_variants.vcf
tabix -p vcf patient_variants.vcf.gz

# Sort and index
bcftools sort patient_variants.vcf.gz -Oz -o patient_variants_sorted.vcf.gz
bcftools index -t patient_variants_sorted.vcf.gz

# Annotate with ClinVar
vcfanno clinvar_config.toml patient_variants_sorted.vcf.gz > patient_variants_annotated.vcf

# Export summary as TSV
bcftools query \
  -f '%CHROM\t%POS\t%REF\t%ALT\t%INFO/GENE\t%INFO/CLNSIG\t%INFO/CLNDN\t%INFO/CLNREVSTAT\n' \
  patient_variants_annotated.vcf -o annotated_summary.tsv
```

The `clinvar_config.toml` extracts the following ClinVar INFO fields: `CLNSIG`, `CLNDN`, `CLNREVSTAT`, `CLNDISDB`, `CLNHGVS`.

---

*Submitted by: Maheen Ali, Syeda Lajeen, Hafsa Asghar*
