
# DADA2 tutorial

## Overview
In this tutorial we will analyse 16S rRNA sequencing data using the R package DADA2 in Galaxy, following the same steps as the [DAD2 tutorial](https://benjjneb.github.io/dada2/tutorial.html). We will use MiSeq Standard Operating Procedure (SOP) developed for the Mothur software package.

In this tutorial we will cover:
 - [Importing the data into Galaxy](#importing-the-data-into-galaxy)  
 - [Inspect read quality profiles](#)
 - [Unzip collection](#unzip-collection)
 - [Filter and trim](#filter-and-trim)
 - [Learn error rates](#learn-error-rates)
 - [Sample inference](#sample-inference)
 - [Merge paired reads](#merge-paired-reads)
 - [Construct sequence table](#construct-sequence-table)

## Importing the data into Galaxy

### 1. Make sure you have an empty analysis history and give it a name.
Click the + icon at the top of the history panel.
Select 'Unamed hisotry' and give it a name e.g. DAD2 tutorial


### 2. Import sample data and reference data
Import the sample FASTQ files and reference data to your history from Zenodo 
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

Click on the tick icon at top of your history.
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

Select 'dada2: plotQualityProfile' from the tool panel and set the following paramters:
* 'Processing mode': batch
* 'Paired reads': paired - in a data set pair
* 'Paired short read data': SOP data (or whatever name you gave the collection)

Once complete, there will be two collections of plots in your galaxy history. One for the forward reads and another for the reverse. We can view the quality profiles using the 'eye' icon. Below are quality profiles for the forward and reverse reads of sample F3D0. 

In gray-scale is a heat map of the frequency of each quality score at each base position. The mean quality score at each position is shown by the green line, and the quartiles of the quality score distribution by the orange lines. The red line shows the scaled proportion of reads that extend to at least that position (this is more useful for other sequencing technologies, as Illumina reads are typically all the same length, hence the flat red line). 

![quality profile forward](/images/quality_profile_forward.png)

The forward reads are good quality. We generally advise trimming the last few nucleotides to avoid less well-controlled errors that can arise there. These quality profiles do not suggest that any additional trimming is needed. 

![quality profile reverse](/images/quality_profile_reverse.png)

The reverse reads are of significantly worse quality, especially at the end, which is common in Illumina sequencing. This isn’t too worrisome, as DADA2 incorporates quality information into its error model which makes the algorithm robust to lower quality sequence, but trimming as the average qualities crash will improve the algorithm’s sensitivity to rare sequence variants. 

Based on these profiles, we will truncate the forward reads at position 240 and the reverse reads at position 160.

## Unzip collection

For the remainder of the DADA2 pipeline we will need to work with forward and reverse reads separately. Galaxy has a tool called 'Unzip Collection' which will separate forward and reverse reads. 

Select 'Unzip Collection' from the tool panel and set the following paramters:
* 'Input Paired Dataset': SOP data (or whatever name you gave the collection)

This will create two collections in your Galaxy history, one for forward reads and another for reverse reads. 


## Filter and trim 

We will use the tool 'dada2: filterAndTrim' to clean the data. 

Select 'dada2: filterAndTrim' from the tool panel and set the following paramters:
* 'Paired reads': paired - in two separate datasets
* 'Forward read data': select the dataset collection tab and then the forward collection from the drop down list 
* 'Reverse read data': select the dataset collection tab and then the reverse collection from the drop down list
* Select 'Trimming paramters' to see more options
  * 'Truncate read length': 240
* Select 'Filtering paramters' to see more options
  * 'Remove reads by number expected errors': 2
* Separate filters and trimmers for reverse reads: yes
* Select 'Trimming paramters' to see more options
  * 'Truncate read length': 160
* Select 'Filtering paramters' to see more options
  * 'Remove reads by number expected errors': 2

This will add three collections to the history for forward reads, reverse reads and statistics.

## Learn error rates

The DADA2 algorithm makes use of a parametric error model and every amplicon dataset has a different set of error rates. The learnErrors method learns this error model from the data, by alternating estimation of the error rates and inference of sample composition until they converge on a jointly consistent solution. As in many machine-learning problems, the algorithm must begin with an initial guess, for which the maximum possible error rates in this data are used (the error rates if only the most abundant sequence is correct and all the rest are errors).

We will use the tool 'dada2: learnErrors' to generate error models for the forward and reverse reads.

Select 'dada2: learnErrors' from the tool panel and set the following paramters:
* 'Short read data': select the dataset collection tab and then the filterAndTrim collection for forward reads from the drop down list 

This will add two datasets to you Galaxy history:
* "dada2: learnErrors on data x and data y and others: error rates plot" 
* "dada2: learnErrors on data x and data y, and others" 

However, it is not clear that this step was performed on the forward samples. So we need to rename the results in our history to:
* "dada2: learnErrors forward: error rates plot"
* "dada2: learnErrors forward"

To change the the name of a result in your history: 
* Select the 'pencil' icon
* Edit the name 
* Save changes

Below the plot produced for the forward reads 

![learn errors forward](/images/learn_errors_forward.png)

The error rates for each possible transition (A→C, A→G, …) are shown. Points are the observed error rates for each consensus quality score. The black line shows the estimated error rates after convergence of the machine-learning algorithm. The red line shows the error rates expected under the nominal definition of the Q-score. Here the estimated error rates (black line) are a good fit to the observed rates (points), and the error rates drop with increased quality as expected. Everything looks reasonable and we proceed with confidence.

Repeats the learnError step for the reverse reads as above. 

## Sample inference

We are now ready to apply the core sample inference algorithm (dada) to the filtered and trimmed sequence data.

Select 'dada2: dada' from the tool panel and set the following paramters:
* 'Process samples in batches': yes
* 'Reads': select the dataset collection tab and then the filterAndTrim collection for forward reads from the drop down list 
* 'Error rates': learn error output for the forward reads

This will add a single list to you Galaxy history:
* "dada2: dada on data x and data y and others" 

As above, we need to rename the the output so it is clear the results are for the forward reads. Rename the dada result in our history to:
* "dada2: dada forward" 

To change the name of a list:
* Select the list name
* click the result name to rename 
* hit enter

Repeat the dada step for the reverse reads as above.

## Merge paired reads

We now merge the forward and reverse reads together to obtain the full denoised sequences. Merging is performed by aligning the denoised forward reads with the reverse-complement of the corresponding denoised reverse reads, and then constructing the merged “contig” sequences. By default, merged sequences are only output if the forward and reverse reads overlap by at least 12 bases, and are identical to each other in the overlap region (but these conditions can be changed via function arguments).

Select 'dada2: mergePairs' from the tool panel and set the following paramters:
* 'Dada results for forward reads': select the dataset collection tab and then the dada forward collection from the drop down list
* 'Forward reads': select the dataset collection tab and then the filterAndTrim collection for forward reads from the drop down list 
* 'Dada results for reverse reads': select the dataset collection tab and then the dada reverse collection from the drop down list
* 'Reverse reads': select the dataset collection tab and then the filterAndTrim collection for reverse reads from the drop down list 

## Construct sequence table

We can now construct an amplicon sequence variant table (ASV) table, a higher-resolution version of the OTU table produced by traditional methods.

Select 'dada2: makeSequenceTable' from the tool panel and set the following paramters:
* 'samples': select the dataset collection tab and then the mergePairs output collection from the drop down list

## Remove chimeras

The core dada method corrects substitution and indel errors, but chimeras remain. Fortunately, the accuracy of sequence variants after denoising makes identifying chimeric ASVs simpler than when dealing with fuzzy OTUs. Chimeric sequences are identified if they can be exactly reconstructed by combining a left-segment and a right-segment from two more abundant “parent” sequences.

Select 'dada2: removeBimeraDenovo' from the tool panel and set the following paramters:
* 'sequence table': select makeSequenceTable output

## Track reads through the pipeline

As a final check of our progress, we’ll look at the number of reads that made it through each step in the pipeline:

Select 'dada2: sequence counts' from the tool panel and set the following paramters:
* 'Datasets(s)': select the dataset collection tab and then the filterAndTrim statistics output
* 'name': filterAndTrim
* select 'inset data sets' 
* 'Datasets(s)': select the dataset collection tab and then the dada forward output
* 'name': denoisedF
* select 'inset data sets'
* 'Datasets(s)': select the dataset collection tab and then the dada reverse output
* 'name': denoisedR
* select 'inset data sets'
* 'Datasets(s)': select the dataset collection tab and then the mergePairs output
* 'name': merged
* select 'inset data sets'
* 'Datasets(s)': select removeBimeraDenovo output
* 'name': nonchim


## Assign taxonomy

It is common at this point, especially in 16S/18S/ITS amplicon sequencing, to assign taxonomy to the sequence variants. The DADA2 package provides a native implementation of the naive Bayesian classifier method for this purpose. The assignTaxonomy function takes as input a set of sequences to be classified and a training set of reference sequences with known taxonomy, and outputs taxonomic assignments with at least minBoot bootstrap confidence.

Download the silva_nr_v132_train_set.fa.gz file

Open the Galaxy Upload Manager ('Upload data' botton on the top of the tool panel)
Select 'Paste/Fetch Data'
Paste the URL https://zenodo.org/record/4587955/files/silva_nr99_v138.1_train_set.fa.gz into the text field 
Press Start
Close the window

Select 'dada2: assignTaxonomy and addSpecies' from the tool panel and set the following paramters:
* 'sequences to be assigned': removeBimeraDenovo output
* 'Select a reference dataset your history or use a built-in?': Use reference data from history
 * 'Reference dataset': silva_nr99_v138.1_train_set.fa.gz

We know have the two main outputs from the DADA2 pipeline. 
removeBimeraDenovo output - a table with ASV counts (rows) across samples (columns)
assignTaxonomy and add species output - a table with ASV taxonomy at different taxonomic ranks



## Remove the mock community

One of the samples included in this sample dataset was a “mock community”. For simplicity we will remove this sample. 

Select 'Advanced cut' from the tool panel and set the following paramters:
* 'File to cut': removeBimeraDenovo output
* 'Operation': Discard
* 'List of fields': column 21

Rename the advanced cut output to something more meaningful e.g 'removeBimeraDenovo no mock'


## Hand-over to phyloseq

The phyloseq R package is a powerful framework for further analysis of microbiome data. We now demonstrate how to straightforwardly import the tables produced by the DADA2 pipeline into phyloseq. We’ll also add the small amount of metadata we have – the samples are named by the gender (G), mouse subject number (X) and the day post-weaning (Y) it was sampled (eg. GXDY).



