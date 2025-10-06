---
title: "Introduction"
exercises: 80
teaching: 40
questions:
- "How to speak the languange of the commandline"
- "Where does the dataset come from?"
- "How to login"
- "Where are the files located"
objectives:
- "Understand the data"
- "Choose login details"
- "Familiarize yourself with the environment"
keypoints:
- "Sequencing *E.coli*  isolates to determine assocations of bacterial genes with antibiotic resistance"
---

## Introduction

In case you come from a computational background and need an introduction to the why and how of sequencing for molecular epidemiology of pathogens, please follow this presentation: [Link](https://jpiamrtriumph.github.io/MicrobialGenomics/files/An%20introduction%20to%20using%20sequence%20data%20for%20the%20epidemiology%20of%20pathogens.pptx)

We will be making use of the command line interface on the [Jupyterhub site](https://klif.uu.nl:8080/). 

### How to login

The server we will be using has host address [Jupyterhub site](https://klif.uu.nl:8080/). Please login using your webbrowser. The username and password have been given in the group chat. Please take a look at the  the [Google Sheets table](https://docs.google.com/spreadsheets/d/1ImRY5QPblAv_LZrwCkHGQOKdyYQ4XHkvtYl9k8UNoKI/edit?usp=sharing) and write your name in the appropriate field to find out which two samples are assigned to you. To acccess the terminal, click on "New", top left and open "Linux Terminal". Bookmark it and give it an appropriate name so you can find it again later. 

### Learning how the speak the language of the Linux commandline. 

We will make use of a lecture and a set of exercises originally developed for the Fleming Fund / JPIAMR COINCIDE course by Rahadian Pratama, Soe Yu Naing and Aldert Zomer. After this basic Linux command line course which we will do together, we will continue on with the rest of the course which can be done at your own pace. The lecture and exercises are available below but will also be presented on screen.  

The lecture can be found here: [Link](https://jpiamrtriumph.github.io/MicrobialGenomics/files/Intro%20Linux%20Commandline%20and%20Nanopore%20Microbial%20Genomics%20Course.pptx)
Most software is in the "genomics" conda environment. Activate that when starting the course with "conda activate genomics". 

### Dataset

The ESBL resistant dataset we will be using comes from this paper: [Within-farm dynamics of ESBL-producing Escherichia coli in dairy cattle: Resistance profiles and molecular characterization by long-read whole-genome sequencing](https://pmc.ncbi.nlm.nih.gov/articles/PMC9366117/) and the non-ESBL resistant dataset comes from our own lab. This second set non-resistant set is only needed for the pangenome and GWAS studies and will be provided on day 4. The ESBL resistance E. coli read files have been downloaded from [ENA](https://www.ebi.ac.uk/ena/browser/view/PRJNA833969).

### Where are the files located

In your home folder (~/), you may find different files. It is your own responsibility to take care of your files. We will create the folders you will be using and download the read files that are part of this study. As assembling of all the genomes in this study would be too time consuming, we will assembling only two genomes per person. We will combine the outputs of each person later on for the genome comparisons.
  
### Nanopore read files

For the TRIuMPH course, we have downloaded the files for you and placed them in your folder. We will be making use of the folder called "reads".  

~~~
$ cd ~/reads
$ ls
~~~
You can see your readfiles in this folder. 

### How to get the nanopore read files if you use your own data. Not for this course. 

If you take this course on your own, on your own server, you have to make an appropriate folder for your read files and get them from the minknow run folder.

~~~
$ cd ~
$ mkdir reads
$ ls
~~~

Only for your own data and your own server: You will see you have created the folder reads. Next we need to get the appropriate files from the server. Go to the folder containing the run of minknow using the terminal. You will need one folder for each sample. In the example I have picked the top two.

The MinKNOW software of Nanopore often generates several "barcode" folders of the samples you sequenced. Each barcode corresponds to one sample. In the folder you will find several files, each has several thousand nanopore reads. We need to combine these reads into a single file so that we can process these further. The command we will be using is [zcat](https://manpages.debian.org/testing/zutils/zcat.1.en.html). This commands combines unzipping a file with displaying it. We will redirect (>) the output into a new file which we can use.

~~~
$ cd _location_of_run_folder_minknow_ #replace with your own run
$ cd fastq_pass
$ ls
$ zcat barcode02/*.fastq.gz > ~/reads/barcode02.fastq
$ zcat barcode03/*.fastq.gz > ~/reads/barcode03.fastq
$ cd ~/reads/
$ ls
~~~

Some sequencing methods have one folder or one file (Nanopore) and others have two files (Illumina). Why is that? Please continue on with the next part of the course which is a lecture on sequence quality of read files. 
