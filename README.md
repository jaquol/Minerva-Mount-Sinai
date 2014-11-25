Minerva-Mount-Sinai
===================

As a new user of Mount Sinai's Minerva high performance computying (HPC) system (https://hpc.mssm.edu/), it is taking me some time to learn basic (and not so basic) commands of its LSF (Load Sharing Facility) queueing system to schedule jobs. The aim of this README file is that such learning can be re-used by other users, although many know-how items described here may be extensively documented somewhere else...

<br>


## STAR aligner

In the mapping process, STAR loads into memory the genome to be used as reference, which will have been prepared in advance. This step demands quite a lot of memory ~30 GB and thus STAR includes the --genomeLoad parameter (which takes several options), which (in principle) allows that the genome is loaded only for one of the multiple files to be mapped. 

In Minerva, however, I recently experienced some memory-related issues when running multiple jobs using STAR in parallel. To note, my approach was submitting one job for each FASTQ file to be mapped with the chosen STAR parameters.

In first place, while the memory is loaded into the shared memory of a given node, it may happen that jobs are assigned to a different node and thus are not able to *see* the loaded genome --although this will not prevent the job from running, it is not optimal because another copy of the genome needs to be loaded.

Another more problematic issue is that unterminated jobs may leave their copies of loaded genomes in the shared memory. If we exceed the amount of shared memory we have as Minerva users, jobs will start failing due to lack of shared memory (it can be picked up from the log file).

The best solution I found is using including the parameter --genomeLoad NoSharedMemory to make sure each jobs creates its own copy of the genome. This may not be optimal in terms of memory but, considering the vast memory memory resources available in Minerva and the high STAR mapping speed, it is a very practical solution...

If you read the STAR manual you will see that NoSharedMemory is the default option of the --genomeLoad parameter. Well, we sort found that this is not how STAR behaves, at least in the STAR version installed in Minerva, so better include --genomeLoad NoSharedMemory in your scripts! 

PS: and once you get too many shared memory segments, it is useful just to contact hpchelp@mssm.edu so that they can clear the nodes for you...

<br>


## HTSeq

If you can't find the HTSeq (http://www-huber.embl.de/users/anders/HTSeq/doc/overview.html) module in Minerva using:

> module avail

Try:

> module load python py_packages

<br>

## RNASeQC

Did you load the rnaseqc module with:

>	module load rnaseqc

but do not know how to call the actual executable java file? Try:

> ls -l $RNASEQC_JAR

<br>


## fastqc

As many other tool, fastq is a module in Minerva and using it requires loading it:

> module load fastqc/0.10.1

This has to be done in every Minerva session so you better add the command above to your scripts!

In general, modules in Minerva can be searched with:

> module avail

###### Performance

| Size input FASTQ file  | 50 MB  | 9000 MB  |
|---|--:|--:|
| CPU time (s) | 12  | 1770  |
| Speed (MB/s) | 4  | 5  |
| Max. memory (MB) | 168  | 287  |
| Memory unit (MB size / MB computing) | 0.29  | 31.36  |  

<br>


## trimmomatic

As of Sep 25th 2014 the trimmomatic tool (http://www.usadellab.org/cms/?page=trimmomatic) is not installed in Minerva (re-check it with 'module avail'). It can be used though just loading java and calling the executable. This is an example of an script using trimmomatic:

Absolute path to the trimmomatic executable:

> trimmomatic=/hpc/users/quilej01/Software/Trimmomatic-0.32/trimmomatic-0.32.jar

Right now in Minerva this is the latest version of java available, which is used to call trimmomatic

> module load java/1.7.0_60 

To call trimmomatic:

> java -Xmx5g -jar $trimmomatic SE -phred33 <input_file> <output_file> ILLUMINACLIP:<fasta_with_overrepresented_seqs>:1:30:15 MINLEN:36

It is good to require 5 Gb (-Xmx5g) or trimmomatic may fail to open

###### Performance

| Size input FASTQ file  | 50 MB  | 9000 MB  |
|---|--:|--:|
| CPU time (s) | 75  | 13696  |
| Speed (MB/s) | 0.67  | 0.66  |
| Max. memory (MB) | 1772  | 3103  |
| Memory unit (MB computing / MB size) | 35  | 0.4  |

Therefore, for these specific trimmomatic options:
  * Maximum memory (important when requesting memory resources in Minerva) does not increases with the size of input file
  * Requesting ~5 Gb of computing memory should be OK for most input FASTQ files
  * The computing time relative to the size of the input file seems to be constant
  * Requesting ~5 hours of walltime should be OK for most input FASTQ files (the job for the 9 Gb file completed in ~4 hours)
 

