# Epigenetic Cell Deconvolution to Study Prostate Cancer Across Age Groups
Disha Hegde, Sayan Bakshi, Jeffrey R. Hage, Brock C. Christensen

iSURF NH-INBRE 2025, University of New Hampshire, Dartmouth Cancer Center, Geisel School of Medicine at Dartmouth

## Background
### Prostate cancer
  * Prostate cancer is the most common type of malignant cancer diagnosed worldwide and displays significant heterogeneity in its onset, progression, and prognosis.
  * Age is a major risk factor, with most cases occurring in older men. Clinical outcomes are consistently associated with being worse when the patient is older at diagnosis[^1].
  * The primary tumor originates in the prostate gland, a small organ located below the bladder that contributes to seminal fluid production. In the early stages, prostate cancer is often asymptomatic.
  * As the disease progresses, symptoms may include difficulty urinating, blood in urine or semen, pelvic discomfort, and bone pain in cases of metastasis[^2].
  * The tumor microenvironment (TME) refers to the complex milieu surrounding tumor cells, including immune cells, stromal cells (such as fibroblasts), blood vessels, extracellular matrix, and signaling molecules. The TME plays a crucial role in cancer initiation, progression, immune evasion, and response to treatment[^3]. 

### Cell Deconvolution
  * Cell deconvolution is a computational technique used to estimate the relative abundance of different cell types within a bulk tissue sample, using data from DNA methylation microarray sequencing[^4].
  * In cancer research, cell deconvolution enables us to infer the cellular composition of tumors and their microenvironments without needing costly and technically challenging single-cell sequencing data.
  * This information is vital for identifying TME-related mechanisms, understanding patient heterogeneity, and guiding personalized therapeutic strategies. Understanding how the TME varies across age groups is essential for improving diagnosis, treatment, and risk assessment.

**This project integrates bioinformatics with cancer epigenetics to address a pressing question in oncology: how does aging affect the tumor microenvironment? By leveraging existing methylation data, the project is well-suited to a short research timeline. The results could inform age-specific treatment strategies and enhance our understanding of prostate cancer heterogeneity.**

## Experimental Design 
**Aim:** To study age-related differences across TME of prostate cancer across distinct age groups using DNA methylation-based cell deconvolution. 

### Data
  * Platform: Illumina EPIC Methylation Array
  * Data Source: Gene Expression Omnibus (GEO)
  * GSE Accession: [GSE183015](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE183015)
  * Samples Included:
    - Normal prostate tissue
    - Tumor prostate tissue
    - Buffy coat (blood samples)

 ### Data Acquisition
  * This project was conducted using R programming language and [`RStudio`](https://rstudio-education.github.io/hopr/starting.html).
  * Downloaded raw DNA methylation data (IDAT files) and covariate data (sample ID, age, tissue type, etc) using [`GEOquery`](https://bioconductor.org/packages/release/bioc/html/GEOquery.html).

### Preprocessing 
#### 1. Quality Control (QC) 
 * Threshold: Removed samples with >10% of probes failing detection p-value
 * Excluded low-quality samples
 * Used [`minfi`](https://bioconductor.org/packages/3.20/bioc/vignettes/minfi/inst/doc/minfi.html) R package

#### 2. Normalization
 * Applied Functional Normalization 

#### 3. Probe Filtering
 * Removed probes with detection p-value > 0.01
 * Filtered out:
   - CpG probes on the Y chromosome
   - CpG probes on the X chromosome
   - SNP-affected/Cross-hybridizing probes

#### 4. Beta Value Extraction
 * Extracted normalized β-values (range: 0 to 1)

#### 5. Visualization
 * Cleaned data was visualized to check for mislabeled/contaminated samples using PCA, heatmaps, and SNP correlation plots

### Cell Deconvolution
#### 1. Beta value separation
 * Beta values were subset based on tissue type (Buffy coat, normal prostate tissue, and tumor prostate tissue)

#### 2. Deconvolution
 * [`HiTIMED`](https://github.com/SalasLab/HiTIMED) was used on the normal and tumor prostate tissue beta values to separate cell type proportions[^5]
 * Used layer 4 deconvolution as per recommended by the program (Started with the highest level, 6, and lowered as recommended)

#### 3. Visualization
 * Converted `HiTIMED` results to long format
 * Created box plots, jitter plots, linear models, and stacked bar plots comparing cell type proportions and age (as both a continuous variable and age tertiles)

## Acknowledgments

This project was conducted during my NH-INBRE iSURF internship under Dr. Brock Christensen at Dartmouth Cancer Center.  

Thank you to Dr. Brock Christensen and members of the Christensen Lab and Salas Lab at Dartmouth Cancer Center for their mentorship and guidance. I am also grateful to Erin Plummer, Jennifer Therriault, and Dr. Patricia A. Pioli guidance during the program.

FUNDING: Research supported by New Hampshire- INBRE through an Institutional Development Award (IDeA), P20GM103506, from the National Institute of General Medical Sciences of the NIH.

Data was accessed via GEO and analyzed using open-source tools in R.


### References
[^1]: Zhou, C. D., Pettersson, A., Plym, A., Svitlana Tyekucheva, Penney, K. L., Sesso, H. D., Kantoff, P. W., Mucci, L. A., & Stopsack, K. H. (2022). Differences in Prostate Cancer Transcriptomes by Age at Diagnosis: Are Primary Tumors from Older Men Inherently Different? Cancer Prevention Research, 15(12), 815–825. https://doi.org/10.1158/1940-6207.capr-22-0212

[^2]: Leslie, S. W., Soon-Sutton, T. L., Sajjad, H., & Siref, L. E. (2023, November 13). Prostate Cancer. National Library of Medicine; StatPearls Publishing. https://www.ncbi.nlm.nih.gov/books/NBK470550/

[^3]: Anderson, N. M., & Simon, M. C. (2020). The Tumor Microenvironment. Current Biology, 30(16), R921–R925. https://doi.org/10.1016/j.cub.2020.06.081

[^4]: Houseman, E. A., Accomando, W. P., Koestler, D. C., Christensen, B. C., Marsit, C. J., Nelson, H. H., Wiencke, J. K., & Kelsey, K. T. (2012). DNA methylation arrays as surrogate measures of cell mixture distribution. BMC Bioinformatics, 13(1). https://doi.org/10.1186/1471-2105-13-86

[^5]: Zhang, Z., Wiencke, J. K., Kelsey, K. T., Koestler, D. C., Christensen, B. C., & Salas, L. A. (2022). HiTIMED: hierarchical tumor immune microenvironment epigenetic deconvolution for accurate cell type resolution in the tumor microenvironment using tumor-type-specific DNA methylation data. Journal of Translational Medicine, 20(1). https://doi.org/10.1186/s12967-022-03736-6

