# NGS Tutorial
## Read Data
### FASTQ Format
+ A paired-end sequencing run will always have <b>two</b> FASTQ files--one for the forward reads, one for the backward reads.
+ The first line of each FASTQ read snippet contains the read ID.
 + Format for Illumina  < version 1.8  
 `@<machine_id>:<lane>:<title>:<x_coord>:<y_coord>#<index>/<read_#>`
 + Format for Illumina <u>\></u> version 1.8  
 `@<machine_id>:<run number>:<flowcell ID>:<lane>:<tile>:<x-pos>:<y-pos><read>:<is filtered>:<control number>:<index sequence>`

## Read Alignment
## Read Quantification
## Normalizing and Transforming Read Counts
## Differential Gene Expression Analysis
