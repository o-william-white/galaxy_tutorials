
# DADA2 tutorial

## Overview
In this tutorial we will analyse 16S rRNA sequencing data using the R package DADA2 in Galaxy, following the same steps as the [DAD2 tutorial](https://benjjneb.github.io/dada2/tutorial.html). We will use MiSeq Standard Operating Procedure (SOP) developed for the Mothur software package.

## Importing the data into Galaxy

### 1. Make sure you have an empty analysis history and give it a name.
Click the + icon at the top of the history panel.
If the + icon is missing:
1. Click on the gear icon (History options) on the top of the history panel
2. Select the option 'Create New' from the menu

### 2. Import sample data and referece data
Import the sample FASTQ files and reference data to your history from Zenodo using the URLs listed in the box below. 
Copy the URLs below
Open the Galaxy Upload Manager ('Upload data' bottun on the top of the tool panel)
Select 'Paste/Fetch Data'
Paste the URLs into the text field
Press Start
Close the window

```
https://zenodo.org/record/800651/files/F3D0_R1.fastq
https://zenodo.org/record/800651/files/F3D0_R2.fastq
https://zenodo.org/record/800651/files/F3D141_R1.fastq
https://zenodo.org/record/800651/files/F3D141_R2.fastq
https://zenodo.org/record/800651/files/F3D142_R1.fastq
https://zenodo.org/record/800651/files/F3D142_R2.fastq
https://zenodo.org/record/800651/files/F3D143_R1.fastq
https://zenodo.org/record/800651/files/F3D143_R2.fastq
https://zenodo.org/record/800651/files/F3D144_R1.fastq
https://zenodo.org/record/800651/files/F3D144_R2.fastq
https://zenodo.org/record/800651/files/F3D145_R1.fastq
https://zenodo.org/record/800651/files/F3D145_R2.fastq
https://zenodo.org/record/800651/files/F3D146_R1.fastq
https://zenodo.org/record/800651/files/F3D146_R2.fastq
https://zenodo.org/record/800651/files/F3D147_R1.fastq
https://zenodo.org/record/800651/files/F3D147_R2.fastq
https://zenodo.org/record/800651/files/F3D148_R1.fastq
https://zenodo.org/record/800651/files/F3D148_R2.fastq
https://zenodo.org/record/800651/files/F3D149_R1.fastq
https://zenodo.org/record/800651/files/F3D149_R2.fastq
https://zenodo.org/record/800651/files/F3D150_R1.fastq
https://zenodo.org/record/800651/files/F3D150_R2.fastq
https://zenodo.org/record/800651/files/F3D1_R1.fastq
https://zenodo.org/record/800651/files/F3D1_R2.fastq
https://zenodo.org/record/800651/files/F3D2_R1.fastq
https://zenodo.org/record/800651/files/F3D2_R2.fastq
https://zenodo.org/record/800651/files/F3D3_R1.fastq
https://zenodo.org/record/800651/files/F3D3_R2.fastq
https://zenodo.org/record/800651/files/F3D5_R1.fastq
https://zenodo.org/record/800651/files/F3D5_R2.fastq
https://zenodo.org/record/800651/files/F3D6_R1.fastq
https://zenodo.org/record/800651/files/F3D6_R2.fastq
https://zenodo.org/record/800651/files/F3D7_R1.fastq
https://zenodo.org/record/800651/files/F3D7_R2.fastq
https://zenodo.org/record/800651/files/F3D8_R1.fastq
https://zenodo.org/record/800651/files/F3D8_R2.fastq
https://zenodo.org/record/800651/files/F3D9_R1.fastq
https://zenodo.org/record/800651/files/F3D9_R2.fastq
https://zenodo.org/record/800651/files/Mock_R1.fastq
https://zenodo.org/record/800651/files/Mock_R2.fastq
https://zenodo.org/record/800651/files/HMP_MOCK.v35.fasta
https://zenodo.org/record/800651/files/silva.v4.fasta
https://zenodo.org/record/800651/files/trainset9_032012.pds.fasta
https://zenodo.org/record/800651/files/trainset9_032012.pds.tax
https://zenodo.org/record/800651/files/mouse.dpw.metadata
```
### 3. Organise data into paired collection
There are a lot of files in the history now but Galaxy can organise our files into collections to make it more managable. Since we have paired-end data, each sample consists of two separate fastq files, one containing the forward reads, and one containing the reverse reads. We can recognize the pairing from the file names, which will differ only by _R1 or _R2 in the filename. We can tell Galaxy about this paired naming convention, so that our tools will know which files belong together. We do this by building a List of Dataset Pairs.






