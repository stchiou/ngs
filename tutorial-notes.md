# NGS Tutorial Notes
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
   + `xargs` builds and execute command lines from standard input. In this case, it receives the result of the `cut` command from standard out, converts them into separated <b> arguments </b> for the `wget` command.

+ Instruction for downloading from SRA can be found here:
https://www.ncbi.nlm.nih.gov/sra/docs/sradownload/#ui-ncbiinpagenav-heading-7

### FASTQ Format
+ A paired-end sequencing run will always have <b>two</b> FASTQ files--one for the forward reads, one for the backward reads.
+ The first line of each FASTQ read snippet contains the read ID.
 + Format for Illumina  < version 1.8  
 `@<machine_id>:<lane>:<title>:<x_coord>:<y_coord>#<index>/<read_#>`
 + Format for Illumina <u>\></u> version 1.8  
 `@<machine_id>:<run number>:<flowcell ID>:<lane>:<tile>:<x-pos>:<y-pos><read>:<is filtered>:<control number>:<index sequence>`  
 For example:  
`@7<DBADDDBH?DHHI@DH>HHHEGHIIIGGIFFGIBFAAGAFHA'5?B@D
@ERR459145.2 DHKW5DQ1:219:D0PT7ACXX:2:1101:2652:2237/1
GCAGCATCGGCCTTTTGCTTCTCTTTGAAGGCAATGTCTTCAGGATCTAAG`  
`+`  
`@@;BDDEFGHHHHIIIGBHHEHCCHGCGIGGHIGHGIGIIGHIIAHIIIGI
@ERR459145.3 DHKW5DQ1:219:D0PT7ACXX:2:1101:3245:2163/1
TGCATCTGCATGATCTCAACCATGTCTAAATCCAAATTGTCAGCCTGCGCG`

## Read Alignment
## Read Quantification
## Normalizing and Transforming Read Counts
## Differential Gene Expression Analysis
