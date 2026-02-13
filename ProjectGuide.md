## **Step-by-Step Project overview**

1. **Defined the biological question**  
   - Compare host transcriptomic responses in COVID-19 vs RSV.  
   - Identify severity-associated molecular modules and hub genes.

2. **Collected data and metadata**  
   - Used GEO dataset `GSE152418`.  
   - Sample metadata prepared in `data/SraRunTable.csv` (Condition, Gender, Severity).

3. **RNA-seq upstream preprocessing (pipeline stage)**  
   - Downloaded raw reads (SRA toolkit).  
   - Ran quality control (FastQC).  
   - Trimmed adapters/low-quality bases (Trim Galore).  
   - Aligned reads to human reference genome hg38 (HISAT2).  
   - Converted/sorted/indexed alignments (SAMtools).  
   - Generated gene-level count matrix (featureCounts).

4. **Loaded counts + metadata in R (`scripts/WGCNA.R`)**  
   - Read `gene_counts.txt` and metadata table.  
   - Cleaned sample IDs and matched count columns to sample metadata.

5. **Preprocessed count matrix in R**  
   - Removed extra/non-expression columns.  
   - Converted values to numeric.  
   - Merged duplicate sample columns by summing counts.

6. **Normalized expression using DESeq2**  
   - Built `DESeqDataSetFromMatrix`.  
   - Filtered low-count genes (`rowSums > 10`).  
   - Estimated size factors (`estimateSizeFactors`).  
   - Exported normalized counts.

7. **Prepared matrix for WGCNA**  
   - Transposed expression matrix to samples x genes format.  
   - Calculated gene variance.  
   - Selected top variable genes (top 10,000).

8. **Selected WGCNA soft threshold**  
   - Tested candidate powers (1–50).  
   - Evaluated scale-free topology fit and mean connectivity.  
   - Chose **soft power = 14**.

9. **Constructed co-expression network**  
   - Ran `blockwiseModules` (signed network, min module size 30, merge cut height 0.25).  
   - Detected color-labeled gene modules.  
   - Saved module assignments.

10. **Calculated module eigengenes and trait associations**  
    - Computed module eigengenes (MEs).  
    - Correlated modules with traits:  
      - Condition (COVID-19 / Healthy / Convalescent)  
      - Severity (Moderate / Severe / ICU / Healthy)  
      - Gender (Male / Female)  
    - Computed p-values and generated heatmaps.

11. **Visualized key outputs**  
    - Soft-threshold plot (`Rplot02.pdf`)  
    - Dendrogram (`dendrogram.png`)  
    - Module-trait heatmaps (condition/gender/severity)  
    - Volcano plot (RSV DEG visualization)  
    - Venn diagram for overlap.

12. **Identified hub genes**  
    - Calculated module membership (MM).  
    - Applied hub threshold (MM > 0.90).  
    - Generated top hub gene outputs (`Top100_Overall_HubGenes.csv`, etc.).

13. **Compared COVID and RSV signatures**  
    - Used RSV top genes table (`top_100_genes_RSV.csv`).  
    - Intersected with COVID hub genes.  
    - Found shared genes: **OASL, TXN, RBCK1** (`overlap_genes.csv`).

14. **Interpreted biological significance**  
    - Severity-linked modules (notably MEyellow/MEgrey).  
    - Strong interferon/antiviral signatures among hubs.  
    - Shared core antiviral response + disease-specific programs.

