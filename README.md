# Quality Control and Trimming

==:warning: **ATENTION**==

==we highly recomend the usage of conda environments with a **python version=3.6** to avoid installation problems or incompatibilities between the programs==

Reads obtained after sequencing need to be processed before using them. 
This is mainly due to the fact that not all the nucleotides have the same quality. To see the quality of our reads first we need to check them
using **FastQC**

**_FASTQC_**
> Is a program with a usable GUI that allows us to see the quality through out all the length of the reads . However during this tutorial we we will only use the
> Command line, as this is the standardized environment in Bioinformatics, or at least the most used.

To run ```FastQC```
```Bash
$ fastqc <R1.fastq.gz> <R2.fastq.gz>
```
For each file (R1 & R2), FASTQC will produce an **html** and a **.zip** archive containing all the plots.

Once we have already revised the quality of our reads we need to get rid of the contaminant substrings in the reads. 

_**SCYTHE**_
>**Scythe** uses a Naive Bayesian aproach to, considereing the quality information of the reads, pick out 3'-end adapters.

Before using **Scythe** first we need to have a file with our adapters to be able to trim them, then we execute **Scythe** on both read files

To run ```Scythe```
```Bash
scythe -a <adapters.fasta> -o <exit_R1_file_name.fastq> <R1_file_.fastq.gz>
scythe -a <adapters.fasta> -o <exit_R2_file_name.fastq> <R2_file_.fastq.gz>
```

But Scythe is not able to trimm those regions only to target them, therefore we need another program called **Sickle**

_**SICKLE**_

>Sickle is a tool that uses sliding windows along with quality and length thresholds to determine when quality is sufficiently low to trim the 3'end of reads
> and also determines when the quality is sufficiently high enough to tri the 5'-end of reads. This program will also discard reads based upon a length threshold

Mainly due to modern sequencing technologies, reads tend to have a deteriorating quality towards the 3'-end and some towars de 5'-end as wll.
We use **Sickle** for eliminating those regions and avoid the negative impact that those would have on assembles and mapping

To run ```Sickle```
```Bash
Sickle pe -f <exit_R1_file_name.fastq> -r <exit_R2_file_name.fastq> \ 
-t sanger -o <trimmed_R1_file_name.fastq> -p <trimmed_R2_file_name.fastq>\
-s /dev/null -q 25
```

Nevertheless, The usage of 3 different programs to execute this Quality Control, can be optimized by the usage of only one tool called **FastP**

_**FASTP**_
>FastP is an all-in-one tool designed to provide fast processing for FastQ files.

To run ```FastP```
```Bash
fastp -i <in_R1.fastq.gz_> -I <in_R2.fastq.gz_> -o <out_R1.fastq.gz_> -O <out_R2.fastq.gz_>
```

by default an HTML file is generated ```fastp.html```but you can change that name by specifying ```-h <desired name>``` at the end of the command, same for the JSON file
but with the ```-j <desired name>``` command