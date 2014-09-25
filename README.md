Minerva-Mount-Sinai
===================

As a new user of Mount Sinai's Minerva high performance computying (HPC) system (https://hpc.mssm.edu/), it is taking me some time to learn basic (and not so basic) commands of its LSF (Load Sharing Facility) queueing system to schedule jobs. The aim of this README file is that such learning can be re-used by other users, although many know-how items described here may be extensively documented somewhere else...


## fastqc

As many other toosl, fastq is a module in Minerva and using it requires loading it:

  > module load fastqc/0.10.1

This has to be done in every Minerva session so you better add the command above to your scripts!

In general, modules in Minerva can be searched with:

  > module avail
