
# DADA2 tutorial

## Overview
In this tutorial we will analyse 16S rRNA sequencing data using the R package DADA2 in Galaxy, following the same steps as the [DAD2 tutorial](https://benjjneb.github.io/dada2/tutorial.html). We will use MiSeq Standard Operating Procedure (SOP) developed for the Mothur software package.

## Importing the data into Galaxy

### 1. Make sure you have an empty analysis history and give it a name.
Click the + icon at the top of the history panel.
If the + icon is missing:
1. Click on the gear icon on the top of the history panel
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
There are a lot of files in the history now but Galaxy can organise our files into collections to make it more managable. Since we have paired-end data, each sample consists of two separate fastq files, one containing the forward reads, and one containing the reverse reads. We can recognise the pairing from the file names, which will differ only by _R1 or _R2 in the filename. We can tell Galaxy about this paired naming convention, so that our tools will know which files belong together. We do this by building a List of Dataset Pairs.

Click on the tick icon param-check at top of your history.
Select all the FASTQ files (40 in total). Tip: Type 'fastq' in the search bar at the top of your history to filter only the FASTQ files; you can now use the 'All' button at the top instead of having to individually select all 40 input files.
Click on 'For all selected..'
Select 'Build List of Dataset Pairs' from the dropdown menu
In the next dialog window you can create the list of pairs. By default Galaxy will look for pairs of files that differ only by a _1 and _2 part in their names. In our case however, these should be _R1 and _R2.
Change these values accordingly
Change _1 to _R1 in the text field on the top left
Change _2 to _R2 om the text field on the top right
You should now see a list of pairs suggested by Galaxy:

![Collection_1](/images/collection_1.png)

Click on auto-pair to create the suggested pairs.

![Collection_2](/images/collection_2.png)

Name the pairs
The middle segment is the name for each pair.
These names will be used as sample names in the downstream analysis, so always make sure they are informative!
Check that the pairs are named F3D0-F3D9, F3D141-F3D150 and Mock.
If needed, the names can be edited by clicking on them
Name your collection at the bottom right of the screen, for e.g. 'SOP data'
Click the 'Create List' button. A new dataset collection item will now appear in your history

## Inspect read quality profiles

Now we have the data in galaxy, we can visulaise the quality profiles of the forward reads and reverse reads. 

The 'dada2: plotQualityProfile' tool will allow us to do this.  

Select 'dada2: plotQualityProfile' from the tool panel and set the follwing paramters
'Processing mode': batch
'Paired reads': paired - in a data set pair
'Paired short read data': SOP data (or whatever name you gave the collection)

Once complete, we can view the quality profiles using the 'eye' icon. 



In gray-scale is a heat map of the frequency of each quality score at each base position. The mean quality score at each position is shown by the green line, and the quartiles of the quality score distribution by the orange lines. The red line shows the scaled proportion of reads that extend to at least that position (this is more useful for other sequencing technologies, as Illumina reads are typically all the same length, hence the flat red line). Below are quality profiles for sample F3D0 for the forward and reverse reads. 

![quality profile forward](/images/quality_profile_forward.png)

The forward reads are good quality. We generally advise trimming the last few nucleotides to avoid less well-controlled errors that can arise there. These quality profiles do not suggest that any additional trimming is needed. We will truncate the forward reads at position 240 (trimming the last 10 nucleotides).

![quality profile reverse](/images/quality_profile_reverse.png)
