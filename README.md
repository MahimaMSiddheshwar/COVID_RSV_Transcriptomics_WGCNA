## **1. Project Title**  
**Comparative Transcriptomic Profiling of COVID-19 and RSV: Insights from RNA-Seq and Weighted Gene Co-expression Network Analysis**

---

## **2. Project Description**  
This project compares the transcriptomic profiles of COVID-19 and Respiratory Syncytial Virus (RSV) using **RNA-Seq** data. Key steps include **data preprocessing**, **alignment**, **gene quantification**, and **differential expression analysis (DEA)**, followed by **Weighted Gene Co-expression Network Analysis (WGCNA)** to identify hub genes and modules associated with disease states and clinical traits.

### **Key Findings**  
- Identified **disease-specific modules** linked to COVID-19 severity and immune response.  
- Shared hub genes (**OASL**, **TXN**, **RBCK1**) between COVID-19 and RSV.  
- Best-performing clustering pipeline selected based on **silhouette scores** and **stability metrics**.  

---

## **3. Dataset Details**  
- **GEO Accession**: [GSE152418](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE152418)  
- **Samples**:  
   - **32 COVID-19**  
   - **34 Healthy**  
   - **2 Convalescent**  
- **Data Type**: RNA-Seq SRR files (68 samples).  

---

## **4. Project Structure**

```
COVID_RSV_Transcriptomics_WGCNA/
├── data/                          # Input data files
│   ├── SraRunTable.csv           # Sample metadata (REQUIRED)
│   └── gene_counts.txt           # From featureCounts (REQUIRED)
│
├── scripts/                       # Analysis scripts
│   └── WGCNA.R                   # Main R analysis script
│
├── results/                       # Generated output files
│   ├── merged_gene_counts.csv
│   ├── normalized_counts.csv
│   ├── Module_Assignments.csv
│   ├── Top20_Overall_HubGenes.csv
│   ├── Top10_HubGenes_Yellow.csv
│   ├── Top10_HubGenes_Red.csv
│   ├── Top100_Overall_HubGenes.csv
│   ├── top_100_genes_RSV.csv
│   └── overlap_genes.csv
│
├── figures/                       # Generated figures
│   ├── dendrogram.png
│   ├── dendrogram_with_module_colors_2.pdf
│   ├── heatmap_plot.png
│   ├── heatmap_plot.pdf
│   ├── heatmap_conditions_separate.png
│   ├── heatmap_conditions_separate.pdf
│   ├── heatmap_severity_separate.png
│   ├── heatmap_severity_separate.pdf
│   ├── heatmap_gender_separate.png
│   ├── heatmap_gender_separate.pdf
│   ├── heatmap_new_severity.png
│   ├── heatmap_new_gender.png
│   ├── heatmap_condition_new.png
│   ├── Venndiagram_new.png
│   ├── Rplot02.pdf
│   ├── module_membership_distribution.png
│   └── Final Report.pdf
│
└── README.md                      # This file
```

---

## **5. Required Files**  

### **Input Files** (Place in `data/` folder):  
   - `data/SraRunTable.csv`: Metadata for samples.  
   - `data/gene_counts.txt`: Gene count matrix from featureCounts.  

### **Generated Outputs** (Saved in `results/` and `figures/`):
   - **Results**: CSV files with gene counts, normalized counts, module assignments, hub genes
   - **Figures**: Heatmaps, dendrograms, soft-threshold plots

---

## **6. Tools and Libraries**

### **Software/Modules**  
| Tool                | Purpose                                  |  
|---------------------|------------------------------------------|  
| **sra-toolkit**     | Download and convert SRR files to FASTQ. |  
| **FastQC**          | Quality control for raw/trimmed reads.   |  
| **Trim Galore**     | Trimming low-quality reads.              |  
| **HISAT2**          | Read alignment to the reference genome.  |  
| **SAMtools**        | Processing, sorting, and indexing BAM files. |  
| **featureCounts**   | Gene expression quantification.          |  

### **R Libraries**  
- **DESeq2**: Differential expression analysis.  
- **WGCNA**: Co-expression network analysis.  
- **pheatmap**: Visualization of heatmaps.  
- **ggplot2**: Data plotting.  
- **gridExtra**: Multi-panel plots.  

---

## **7. Workflow**

### **Preprocessing Pipeline** (Shell scripts - not included)
1. Download SRR files with SRA Toolkit
2. Convert SRA to FASTQ with fasterq-dump
3. Quality control with FastQC
4. Trim reads with Trim Galore (length 30, quality 20)
5. Align to hg38 with HISAT2
6. Quantify genes with featureCounts

### **WGCNA Analysis** (R script)
```bash
cd scripts
Rscript WGCNA.R
```

**Or in RStudio:**
```r
setwd("C:/Users/mmsid/Downloads/COVID_RSV_Transcriptomics_WGCNA-main")
source("scripts/WGCNA.R")
```

---

## **8. WGCNA Parameters**

- **Soft-threshold power**: 14 (selected for scale-free topology)
- **Network type**: Signed
- **Min module size**: 30
- **Merge cut height**: 0.25
- **Hub gene threshold**: Module Membership > 0.90
- **Variable genes**: Top 10,000 by variance

---

## **9. Outputs**

### **Key Results**  
1. **Hub Genes**:  
   - `results/Top20_Overall_HubGenes.csv`
   - `results/Top10_HubGenes_Yellow.csv`
   - `results/Top10_HubGenes_Red.csv`

2. **Module Assignments**:
   - `results/Module_Assignments.csv`

3. **Shared Hub Genes**:
   - `results/overlap_genes.csv`

### **Visual Outputs**  
- **Heatmaps**: Module-trait correlations (Condition, Gender, Severity)
- **Dendrogram**: Gene clustering with module colors
- **Soft-threshold Plots**: Scale-free topology analysis

---

## **10. Results Summary**  
- **MEyellow** and **MEgrey** modules are strongly associated with COVID-19 severity.  
- Shared hub genes (**OASL**, **TXN**, and **RBCK1**) indicate conserved antiviral and immune response pathways between COVID-19 and RSV.  
- Limited overlap of hub genes highlights unique molecular pathways for each disease.

---

## **11. Dependencies**

### **Install R packages:**
```r
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install(c("DESeq2", "WGCNA"))
install.packages(c("pheatmap", "ggplot2", "gridExtra"))
```

---

## **12. Notes**

- **Working Directory**: The R script assumes you're in the project root folder
- **Memory**: WGCNA requires ~32GB RAM for 10,000 genes
- **Runtime**: Full WGCNA analysis takes ~30-60 minutes
- **Storage**: Results and figures are ~50MB total

---

## **13. Citation**

**Dataset**: [GSE152418](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE152418)

**Methods**:  
- HISAT2: Kim et al. (2015) Nature Methods  
- featureCounts: Liao et al. (2014) Bioinformatics  
- DESeq2: Love et al. (2014) Genome Biology  
- WGCNA: Langfelder & Horvath (2008) BMC Bioinformatics

---

## **14. Acknowledgments**

**Team Members:**  
Asra Tasneem Shaik, Muni Manasa Vema, Mahima Mahabaleshwar Siddheshwar.  

**Course:** Genomic Data Analytics and Precision Medicine (INFO B636).  

---
