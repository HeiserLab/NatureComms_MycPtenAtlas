Readme for Doha, Wang et all manuscript submitted to Nature Communications (NCOMMS-22-49559-T)

Included are six .rmd scripts used for analyzing the single cell RNA-seq data used in this manuscript.
Scripts are designed to run sequentially (s1->s2->...s6), and will store their output in the 'analysis_files' directory.

System requirements
>64GB system memory

All code was originally processed using:
	Software:
		Windows 10 Enterprise (21H2)
		Rstudio (2022.07.2, Build 576)
		R (4.2.2)
	System hardware:
		Ryzen 2990WX
		128GB System memory
	
Included code files:
	s0_package_installation.rmd
	s1_vehicle_stroma_preprocessing.rmd
	s2_qc_integration.rmd
	s3_cluster_optimization
	s4_celltype_definition.rmd
	s5_human_tnbc_integration_rliger.rmd
	s6_manuscript_figures.rmd

Other files:
	readme.txt : readme file
	library_metadata : Metadata file used to associate treatment / hashtag / phenotype data with sequencing results
	s{1:6}*.html : .html output of associated code file when knitted.


Code descriptions:
s0: Installation for all required analysis packages.
s1: Load UMI count matrices from 10X cellranger output, use Soupx to remove contaminating transcripts, perform hashtag demultiplexing, identify doublets with DoubletFinder and combine all libraries to a single .rds file.
s2: Combine libraries into a single Seurat object, normalize/dimensionality reduction / cluster without integration. Compute QC metrics, and assign cell cycle. Filter to high quality singlets, and then integrate with rLiger and harmony.
s3: Clustering resolution sweep (Leiden algorithm) and optimize resolution to minimize RMSE and maximize approximate silhouette width. Compute DEGs across optimized clusters and assign celltype lineage based on canonical markers. Subset proliferative population and assign lineage.
s4: Compute DEGs across clusters within lineage, and perform gene enrichment analysis. Assign cluster label.
s5: Train a classifier on celltype identified in Wu et al (Nat.Genet. 2021) and apply to MycPten;fl data. Integrate Wu et al human data set with MycPten;fl data using iNMF (Rliger).
s6: Visualize results for manuscript main and supplemental figures.

Code runtime estimate for standard desktop:
	s0: ~10 minutes
	s1: ~3 hours
	s2: ~12 hours
	s3: ~3 hours
	s4: ~1 hour
	s5: ~4 hours
	s6: ~10 minutes


Required R packages (version used, source)
	Matrix (1.5-1, CRAN)
	tidyverse (1.3.2, CRAN)
	Seurat (4.3.0, CRAN)
	ggalluvial (0.12.3, CRAN)
	harmony (0.1.1, CRAN)
	SoupX (1.6.2, CRAN)
	cluster (2.1.4, CRAN)
	clusterProfiler (4.4.4, Bioconductor)
	org.Mm.eg.db (3.16, Bioconductor)
	bluster (1.6.0, Bioconductor)
	enrichplot (1.16.2, Bioconductor)
	rliger (1.0.0, github: 'welch-lab/liger')
	DoubletFinder (2.0.3, github: 'chris-mcginnis-ucsf/DoubletFinder')
	SeuratWrappers (0.3.0, github: 'satijalab/seurat-wrappers')
	nichenetr (1.1.0, github: 'saeyslab/nichenetr')
	scPred (1.9.2, github: 'immunogenomics/scPred')

Demo data:
	s1_seurat_list_subset.rds (A subset of 50 cells per library, produced by s1. ~30MB uncompressed)