# eQTL-Detect: Nextflow based pipeline for eQTL detection 
eQTL-Detect is a Nextflow based bioinformatics workflow to detect the cis, trans and splicing eQTLs (expresssion quantitative trait loci) by perfoming associations with genotype and expression data.\
This repository provides the [Nextflow](https://www.nextflow.io/) scripts and demo data to test the eQTL analyses and run with large datasets. It was developed as a sub-workflow design (in five independent nextflow scripts and ordered numerically from 00 to 04) in order make the workflow distributable across different project partners and also as a standalsone script, to run the complete analysis with single command on HPC machines. It was primarily developed to detect eQTLs in cattle (_Bos taurus_), but it can be adopted for any other species by providing the reference genome assembly and transcriptome annotation gtf files of the species of interest. 


## Software required
- [Nextflow](https://www.nextflow.io/) version => 21.04 
- [Docker](https://www.docker.com/) version >=  20.10.8 installed on their machines to run these scripts.

  
## Commands to run scripts:

The analysis can run with a single script or using modular scripts based on user preferences.

- For single script analysis, the user should use the following command and should provide the read type and read strandedness for the RNAseq data:

  - Parameters 
    - readtype: --pairedEnd_reads, --singleEnd_reads, 
    - Strandedness: --firstStranded, --secondStranded and --unStranded

   **Script main:** _nextflow run [main.nf] (https://github.com/BovReg/BovReg_eQTL/blob/main/main.nf)
-params-file [main.json] (https://github.com/BovReg/BovReg_eQTL/blob/main/main.json) --pairedEnd_reads --firstStranded_

- If the user has aligned bam files, the alignment step can be skipped using the following command
  - Parameters 
     - Strandedness: --firstStranded, --secondStranded and --unStranded

  **Script main with bam files as input:** _nextflow run [main.nf] (https://github.com/BovReg/BovReg_eQTL/blob/main/main.nf)
-params-file [main.json] (https://github.com/BovReg/BovReg_eQTL/blob/main/main.json) --bamFiles_input --firstStranded_

- This script can also run by provinding the expression count matrices using the following command
   - Parameters 
     - --countMatrices_input_

  **Script main with count matrices as input:** _nextflow run [main.nf] (https://github.com/BovReg/BovReg_eQTL/blob/main/main.nf)
-params-file [main.json] (https://github.com/BovReg/BovReg_eQTL/blob/main/main.json) --countMatrices_input_


- For modular analysis users can opt for the following scripts.

  **Script 00:**  Indexing the reference genome.\
  _nextflow run [00_eQTLDetect.nf](https://github.com/BovReg/BovReg_eQTL/blob/main/00_eQTLDetect.nf)_  

  **Script 01:**  Alignment and quantification of expression data.\
  _nextflow run [01_eQTLDetect.nf](https://github.com/BovReg/BovReg_eQTL/blob/main/01_eQTLDetect.nf) -params-file [01_eQTLDetect.json](https://github.com/BovReg/BovReg_eQTL/blob/main/01_eQTLDetect.json)_  


  **Script 02:**  Extracting the genotype data from samples having corresponding RNAseq samples.\
   _nextflow run [02_eQTLDetect.nf](https://github.com/BovReg/BovReg_eQTL/blob/main/02_eQTLDetect.nf)_  

  **Script 03:**  Merging the read and transcript counts generated from stringtie and cluster introns found in junction files estimate covriates for splicing sites based on PCs.\
   _nextflow run [03_eQTLDetect.nf](https://github.com/BovReg/BovReg_eQTL/blob/main/03_eQTLDetect.nf) -params-file [03_eQTLDetect.json](https://github.com/BovReg/BovReg_eQTL/blob/main/03_eQTLDetect.json)_  

  **Script 04:**  Performing cis and trans QTL mapping using the RNA seq and corresponding genotype data.\
  _nextflow run [04_eQTLDetect.nf](https://github.com/BovReg/BovReg_eQTL/blob/main/04_eQTLDetect.nf) -params-file [04_eQTLDetect.json](https://github.com/BovReg/BovReg_eQTL/blob/main/04_eQTLDetect.json)_  


## Demo data
- The genotype demodata is available in the folder [Demo_genotype_BovReg](https://github.com/BovReg/BovReg_eQTL/tree/main/Demo_genotype_BovReg). 

- The Phenotype data can be provided in the following formats 1. Raw data (RNAseq expression data in fastq format), 2. Aligned reads (RNAseq expression data in bam format)  and 3. Expression counts across samples (expression count matrices in text file).\
These files can be downloaded from research data open repository 
 [Zenodo](https://zenodo.org/record/7949616) (Fastq),
 [Zenodo](https://zenodo.org/records/7950181) (expression counts and aligned bam files).

- The genotype-phenotype corresponding samples information can be found in the text file [RNA_WGS_CorresID_BovReg.txt](https://github.com/BovReg/BovReg_eQTL/blob/main/RNA_WGS_CorresID_BovReg.txt)

- The reference genome and annotation file for the demo analysis can be downloaded here [reference genome: fasta format ](https://ftp.ensembl.org/pub/release-109/fasta/bos_taurus/dna/Bos_taurus.ARS-UCD1.2.dna.toplevel.fa.gz) and [reference annotation: gtf format](https://ftp.ensembl.org/pub/release-109/gtf/bos_taurus/Bos_taurus.ARS-UCD1.2.109.gtf.gz).
