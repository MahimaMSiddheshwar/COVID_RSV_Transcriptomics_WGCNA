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
│   └── SraRunTable.csv           # Sample metadata with 68 samples
│
├── scripts/                       # Analysis scripts
│   └── WGCNA.R                   # Main R analysis script (fixed bugs)
│
├── results/                       # Generated output files
│   ├── Top100_Overall_HubGenes.csv      # Top 100 hub genes from WGCNA
│   ├── top_100_genes_RSV.csv           # Top 100 differentially expressed genes (RSV analysis)
│   └── overlap_genes.csv               # 3 shared hub genes: OASL, TXN, RBCK1
│
├── figures/                       # Generated figures and visualizations
│   ├── Final Report.pdf                # Complete project report
│   ├── Rplot02.pdf                     # Soft-threshold power selection plots
│   ├── Venndiagram_new.png             # Venn diagram of overlapping genes
│   ├── dendrogram.png                  # Gene clustering dendrogram with module colors
│   ├── heatmap_condition_new.png       # Module-trait correlation (Condition)
│   ├── heatmap_new_gender.png          # Module-trait correlation (Gender)
│   └── heatmap_new_severity.png        # Module-trait correlation (Severity)
│
└── README.md                      # This file
```

---

## **5. Results Files Description**

### **results/Top100_Overall_HubGenes.csv**
Contains the top 100 hub genes identified from WGCNA analysis with highest module membership scores. These genes are highly connected within their modules and represent key regulatory genes.

**Key hub genes include:**
- IFI44L, XAF1, IFIT3 (Interferon response genes)
- IRF7, OASL (Antiviral response)
- TXN, RBCK1 (Shared with RSV)

### **results/top_100_genes_RSV.csv**
Contains differentially expressed genes from RSV analysis with statistical metrics:
- **Columns**: Probe ID, adj.P.Val, P.Value, t-statistic, B-statistic, logFC, Gene ID, Gene symbol, Gene title
- **Key genes**: IFI27, IGF2BP3, TPST2, HP, SLA2, CD1C, XK, GYPE, GYPB

### **results/overlap_genes.csv**
**3 shared hub genes** between COVID-19 and RSV:
1. **OASL** - 2'-5'-oligoadenylate synthetase-like (antiviral defense)
2. **TXN** - Thioredoxin (oxidative stress response)
3. **RBCK1** - RanBP-type and C3HC4-type zinc finger containing 1 (immune regulation)

---

## **6. Figures Description**

### **figures/Final Report.pdf**
Complete project documentation with methodology, results, and conclusions.

### **figures/Rplot02.pdf**
Soft-threshold power selection plots showing:
- Scale-free topology model fit (R²) vs power
- Mean connectivity vs power
- **Selected power: β = 14**

### **figures/Venndiagram_new.png**
Venn diagram showing overlap between COVID-19 and RSV hub genes, highlighting 3 shared genes.

### **figures/dendrogram.png**
Hierarchical clustering dendrogram of genes colored by module assignment. Shows 10+ distinct co-expression modules.

### **figures/heatmap_condition_new.png**
Module-trait correlation heatmap for **Condition** (Convalescent vs COVID-19 vs Healthy):
- Red = positive correlation
- Blue = negative correlation
- Shows which modules are activated/suppressed in each condition

### **figures/heatmap_new_gender.png**
Module-trait correlation heatmap for **Gender** (Female vs Male):
- Identifies sex-specific gene expression patterns

### **figures/heatmap_new_severity.png**
Module-trait correlation heatmap for **Severity** (Convalescent, Moderate, Severe, ICU, Healthy):
- **MEyellow** and **MEgrey** modules strongly associated with severity
- Key finding for disease progression biomarkers

---

## **7. Tools and Libraries**

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

## **8. Workflow**

### **Preprocessing Pipeline**
1. Download SRR files with SRA Toolkit
2. Convert SRA to FASTQ with fasterq-dump
3. Quality control with FastQC
4. Trim reads with Trim Galore (length 30, quality 20)
5. Align to hg38 with HISAT2
6. Quantify genes with featureCounts

### **WGCNA Analysis**
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

## **9. WGCNA Parameters**

- **Soft-threshold power**: 14 (selected for scale-free topology R² > 0.85)
- **Network type**: Signed
- **Min module size**: 30
- **Merge cut height**: 0.25
- **Hub gene threshold**: Module Membership > 0.90
- **Variable genes**: Top 10,000 by variance
- **Modules identified**: 10+ (yellow, grey, brown, tan, greenyellow, lightcyan, blue, purple, etc.)

---

## **10. Key Results Summary**

### **Module-Trait Associations**
- **MEyellow & MEgrey**: Strongly associated with COVID-19 severity
- Condition-specific modules distinguish disease states
- Gender-specific patterns identified

### **Hub Genes**
- **100 hub genes** with highest module membership (>0.90)
- **3 shared genes** (OASL, TXN, RBCK1) indicate conserved antiviral pathways
- Interferon response genes dominate (IFI44L, IFIT3, IFIT1, RSAD2)

### **Biological Interpretation**
- Shared hub genes suggest common immune mechanisms between COVID-19 and RSV
- Unique hub genes indicate disease-specific pathways
- Potential drug targets and diagnostic biomarkers identified

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
- **Input Required**: Place `gene_counts.txt` in `data/` folder before running
- **Bugs Fixed**: Corrected undefined variable error and removed duplicate code lines

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
