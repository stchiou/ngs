# NGS Tutorial Notes
## Introduction
+ The Lander-Waterman equation is used to described the relationship between coverage, reads, and the genome size. The formula is:

```tex
\frac{readlength*number of reads}{haploid genome length}
```

## Read Data
### Download Sequences
+ The sequence files can be downloaded from public sequence  such as the Sequence Read Archive (SRA) of the National Center for Biotechnology Information (NCBI), the European Nucleotide Archive (ENA) of the European Bioinformatics Institute (EBI), and the DNA Databank of Japan (DDBJ).
+ Use wget to download individual sequence files as:  
```
$ wget <link copied from the website>
```
+ To download multiple sequences in the same project in ENA, the following procedure can be used:
  1.  Download the summary of the sample information in text file format. This text file contains the record each sample in the study including the links to download them in the 11th column of the file.
  ```
  $ `wget -0 samples_at_ENA.txt "<LINK>"`
  ```
  2.  Use the cut command to extract the 11th column and pipes it to the wget command to download all of the sequences.
  ```
  $ cut -f11 samples_at_ENA.txt | xargs wget
  ```
   + `cut` removes sections from each line of files the `-f` flag indicates the fields that being selected.
   + The `|` is called pipe. It passes the output of a previous command on STDIN (the standard input stream) to the input of the next one, or to the shell.
   + `xargs` builds and execute command lines from standard input. In this case, it receives the result of the `cut` command from standard out, converts them into separated **arguments** for the `wget` command.

+ Instruction for downloading from SRA can be found here:
https://www.ncbi.nlm.nih.gov/sra/docs/sradownload/#ui-ncbiinpagenav-heading-7

### FASTQ Format
+ A paired-end sequencing run will always have **two** FASTQ files--one for the forward reads, one for the backward reads.

#### Sequence Identifier
+ The first line of each FASTQ read snippet contains the read ID.
 + Format for Illumina  < version 1.8  
 `@<machine_id>:<lane>:<title>:<x_coord>:<y_coord>#<index>/<read_#>`
 + Format for Illumina <u>\></u> version 1.8  
 `@<machine_id>:<run number>:<flowcell ID>:<lane>:<tile>:<x-pos>:<y-pos><read>:<is filtered>:<control number>:<index sequence>`  
 For example:  
`@7<DBADDDBH?DHHI@DH>HHHEGHIIIGGIFFGIBFAAGAFHA'5?B@D`
`@ERR459145.2 DHKW5DQ1:219:D0PT7ACXX:2:1101:2652:2237/1`  
`GCAGCATCGGCCTTTTGCTTCTCTTTGAAGGCAATGTCTTCAGGATCTAAG`  
`+`  
`@@;BDDEFGHHHHIIIGBHHEHCCHGCGIGGHIGHGIGIIGHIIAHIIIGI`  
`@ERR459145.3 DHKW5DQ1:219:D0PT7ACXX:2:1101:3245:2163/1`  
`TGCATCTGCATGATCTCAACCATGTCTAAATCCAAATTGTCAGCCTGCGCG`

#### Base Call Quality Scores
+ The deduction of nucleotide sequences from the images acquired during sequencing is referred to as **base calling**.
+ FASTQ files store the DNA sequence of each read with a position-specific quality score that represents the error probability.
+ The **Phred score, Q**, is proportional to the probability _p_ that a base call is incorrect.
**Q=-10*_log_<sub>10</sub>(_p_)**
+ Phred scores are typically represented as ASCII characters.
+ In the Illumina platform, the ASCII offset for version < 1.8 is 64, for 1.8+ is 33 and can be converted through the `sed` command as following:
```
$ sed -e '4~4y/@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_`abcdefghi/!"#$%&'()*+,-.\/0123456789:; <=>?@ABCDEFGHIJ/' originalFile.fastq
```
 + `sed` is a stream editor for filterning and transforming text.
 + The code above executes (**-e** flag) the transliteration (**y** command) on every fourth line (**4~4**) of the file originalFile.fastq. to change any of the characters in the following list
```
!"#$%&'()*+,-.\/0123456789:; <=>?@ABCDEFGHIJ
```
to its corresponding in the following list
```
@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_`abcdefghi
```
### Quality control

#### Differential Gene Expression Analysis Workflow

|Step|Input Files |Process |Required Tools |Output|Quality Control|
|----|------|--------|---------|------|---------|
|1   |.tif  |Base calling & demultiplexing |Bustard/RTA/OLB, CASAVA|Raw reads|FASTQC |
|2   |.fastq|Mapping |STAR|Aligned reads|RSeQc|
|3   |.sam/.bam|Counting|featureCounts|Read count table|Descriptive plots
|4   |.txt|Normalizing|DESeq2, edgeR|Normalized read count table|Descriptive plots|
|5   |.Robj| DE test & multiple testing correction|DESeq2, edgeR, limma|List of fold changes & statistical values|Descriptive plots|
|6   |.Robj, .txt|Filtering|customized scripts|downsteam analyses on DE genes|n/a|

+ A poor sequence run will be most likely caused by
  + PCR duplicates
  + adapter contamination
  + rRNA and tRNA reads
  + unmappable reads

##### FASTQC

+ FASTQC is a java app that can be downloaded from http://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.5.zip
+ Once downloaded, installing FASTQC is simply to unzip the archive and change mode of the `fasqc` wrapper so that the file inside the folder becomes executable.
+ The following command is used to run FASTQC:
```
$ fastqc ERR458493.fastq.gz --extract
```
+ The results of FASTQC are stored in a folder that contains the sample name.
```
[chious@ehshpclp131/jobs rna-seq-lab]$ ls ERR458493_fastqc -l
total 672
-rw-r--r-- 1 chious others 138484 Jul 13 16:48 fastqc_data.txt
-rw-r--r-- 1 chious others   9716 Jul 13 16:48 fastqc.fo
-rw-r--r-- 1 chious others 369158 Jul 13 16:48 fastqc_report.html
drwxr-xr-x 2 chious others  32768 Jul 13 16:48 Icons
drwxr-xr-x 2 chious others  32768 Jul 13 16:48 Images
-rw-r--r-- 1 chious others    566 Jul 13 16:48 summary.txt
```
+ The `summary.txt`

## Read Alignment
## Read Quantification
## Normalizing and Transforming Read Counts
## Differential Gene Expression Analysis
