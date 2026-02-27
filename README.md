# 🧬 Genomic Variant Pathogenicity Analysis — Assignment 2

Investigating the pathogenicity of 6 clinically significant genetic variants using ClinVar, OMIM, UCSC Genome Browser, and ACMG/AMP guidelines.

> All coordinates based on **GRCh38**.

---

## Selected Variants

### Common Genetic Diseases

| Disease | Gene | Variant (HGVS) | GRCh38 Position | ClinVar ID | Review Status |
|---|---|---|---|---|---|
| Sickle Cell Anemia | HBB | NM_000518.5:c.20A>T (p.Glu7Val) | chr11:5,227,002 | 15333 | Pathogenic ⭐⭐⭐⭐⭐ |
| Cystic Fibrosis | CFTR | NM_000492.4:c.1521_1523delCTT (p.Phe508del) | chr7:117,559,591 | 7105 | Pathogenic ⭐⭐⭐⭐⭐ |
| Phenylketonuria | PAH | NM_000277.3:c.1222C>T (p.Arg408Trp) | chr12:102,838,900 | — | Pathogenic ⭐⭐⭐ |

### Rare Genetic Diseases

| Disease | Gene | Variant (HGVS) | GRCh38 Position | ClinVar ID | Review Status |
|---|---|---|---|---|---|
| Huntington's Disease | HTT | NM_002111.8:c.53CAG[>36] | chr4:3,074,877 | — | Pathogenic ⭐⭐⭐⭐⭐ |
| Marfan Syndrome | FBN1 | NM_000138.5:c.5788G>A (p.Gly1930Ser) | chr15:~48,700,000 | — | Pathogenic ⭐⭐⭐ |
| Gaucher Disease (Type 1) | GBA1 | NM_000157.4:c.1226A>G (p.Asn409Ser) | chr1:155,234,452 | 4288 | Pathogenic ⭐⭐⭐⭐ |

## Methodology

| Step | Tool | What Was Done |
|---|---|---|
| 1. Variant Selection | ClinVar | Searched each disease, filtered Pathogenic variants, extracted HGVS, coordinates, and clinical evidence |
| 2. Phenotype & Inheritance | OMIM | Reviewed gene entries for disease features and mode of inheritance |
| 3. Pathogenicity Scores | UCSC Genome Browser (hg38) | Enabled AlphaMissense and REVEL tracks; recorded scores and took screenshots |
| 4. ACMG Classification | ACMG/AMP 2015 Guidelines | Applied criteria (PVS1, PS1, PS3, PS4, PM1–PM5, PP2–PP5); assigned final classification |
| 5. VCF Construction | Manual + WSL | Built VCF v4.2 with all 6 variants; compressed, sorted, indexed, and annotated with ClinVar |

---

## Results Summary

| Disease | Gene | Variant | AlphaMissense | REVEL | ACMG Criteria | Classification |
|---|---|---|---|---|---|---|
| Sickle Cell Anemia | HBB | p.Glu7Val | 0.9741 | 0.952 | PS3, PS4, PM3, PM5, PP1_strong | **Pathogenic** |
| Cystic Fibrosis | CFTR | p.Phe508del | N/A* | N/A* | PS3, PS4, PM3, PM4, PP3 | **Pathogenic** |
| Phenylketonuria | PAH | p.Arg408Trp | 0.9612 | 0.931 | PS1, PS3, PM1, PM2, PP3, PP5 | **Pathogenic** |
| Huntington's Disease | HTT | CAG >36 repeats | N/A† | N/A† | PVS1-equiv., PS3, PS4, PM2 | **Pathogenic** |
| Marfan Syndrome | FBN1 | p.Gly1930Ser | 0.9203 | 0.887 | PM1, PM2, PM5, PP2, PP3, PP5 | **Pathogenic** |
| Gaucher Disease Type 1 | GBA1 | p.Asn409Ser | 0.8934 | 0.821 | PS1, PS3, PM3, PP3, PP5 | **Pathogenic** |

*Not applicable for in-frame deletions — CADD score >30 used as supplementary evidence.  
†Not applicable for trinucleotide repeat expansions — pathogenicity is repeat-length dependent.

---

## Steps to Reproduce

> Run in **WSL (Ubuntu)**.

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

# Export summary
bcftools query \
  -f '%CHROM\t%POS\t%REF\t%ALT\t%INFO/GENE\t%INFO/CLNSIG\t%INFO/CLNDN\t%INFO/CLNREVSTAT\n' \
  patient_variants_annotated.vcf -o annotated_summary.tsv
```

The `clinvar_config.toml` file extracts these ClinVar INFO fields: `CLNSIG`, `CLNDN`, `CLNREVSTAT`, `CLNDISDB`, `CLNHGVS`.

---

Submitted by:
Syeda Lajeen, Maheen Ali and Hafsa Asghar.
