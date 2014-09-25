Minerva-Mount-Sinai
===================

As a new user of Mount Sinai's Minerva high performance computying (HPC) system (https://hpc.mssm.edu/), it is taking me some time to learn basic (and not so basic) commands of its LSF (Load Sharing Facility) queueing system to schedule jobs. The aim of this README file is that such learning can be re-used by other users, although many know-how items described here may be extensively documented somewhere else...


## fastqc

As many other toosl, fastq is a module in Minerva and using it requires loading it:

> module load fastqc/0.10.1

This has to be done in every Minerva session so you better add the command above to your scripts!

In general, modules in Minerva can be searched with:

> module avail


## trimmomatic

As of Sep 25th 2014 the trimmomatic tool (http://www.usadellab.org/cms/?page=trimmomatic) is not installed in Minerva (re-check it with 'module avail'). It can be used though just loading java and calling the executable. This is an example of an script using trimmomatic:

Absolute path to the trimmomatic executable:

> trimmomatic=/hpc/users/quilej01/Software/Trimmomatic-0.32/trimmomatic-0.32.jar

Right now in Minerva this is the latest version of java available, which is used to call trimmomatic

> module load java/1.7.0_60 

To call trimmomatic:

> java -Xmx5g -jar $trimmomatic SE -phred33 <input_file> <output_file> ILLUMINACLIP:<fasta_with_overrepresented_seqs>:1:30:15 MINLEN:36

Additional comments:
* It is good to require 5 Gb (-Xmx5g) or trimmomatic may fail to open
* For these specific trimmomatic options, and using two input files of 50 Mb (small) and 9 Gb (rather big) for testing purposes, I noticed that:
  * Maximum memory (important when requesting memory resources in Minerva) does not increases with the size of input file
  * Requesting ~5 Gb of computing memory should be OK for most input FASTQ files
  * The computing time relative to the size of the input file seems to be constant
  * Requesting ~5 hours of walltime should be OK for most input FASTQ files (the job for the 9 Gb file completed in ~4 hours)
