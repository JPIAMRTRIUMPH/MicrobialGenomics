--- 
title: "Visualizing genomic regions using Clinker"
start: false
teaching: 10
exercises: 50
questions:
- "How does the genetic region of your resistance gene look like"
objectives:
- "Extracting and annotating a genetic region"
- "Visualizing gene differences and similarities"
keypoints:
- "Resistance genes can be found on the chromosome and on plasmids"
- "Visualizing the region can help understand the history of the genetic locus containing the resistance gene"
---


## Introduction
In the previous exercises we detected a resistance gene. If you want to investigate if the genetic region is similar, suggesting a common source or clonal spread, the region can be investigated using a tool called [Clinker](https://github.com/gamcil/clinker) . Clinker is available via commandline but also via the [https://cagecat.bioinformatics.nl/](https://cagecat.bioinformatics.nl/) webserver. We will discuss both options and perform one of the options below. We have also included a piece of shell code to extract and annotate the regions, however you can also cut and paste the sequence from the contigs using a text editor. For this course, we provide an example of several regions with a resistance gene, however the code can be used to generate your own at a later timepoint.

## Webbased use of Clinker

Download the zip file with extracted regions here: [regions.zip](https://jpiamrtriumph.github.io/MicrobialGenomics/files/regions.zip)
Unzip the extracted regions file to a folder called "regions"

Visit the website [https://cagecat.bioinformatics.nl/](https://cagecat.bioinformatics.nl/) and press the "Start" button at the Clinker section. 
Click on Genome Files and the navigate to the unzipped "regions" folder. Select all gbk files by pressing Shift and selecting all files. Press Open and then press the Submit button. It takes about a minute to run the analysis. You can now adjust the output to suit your needs. Move the fragments around (click on the name and move up or down) or flip them (double click on the sequence). Use the "Scale factor", "Show gene labels" and "Label type" options to improve the visualization. 

## Manual commandline use of Clinker

This manual Clinker exercise should only be done *after* you have completed the annotation exercise from day 3 and if you feel compfortable using the commandline. This is not an easy command. It requires a good understanding of the input files of Clinker and how to get annotations. The commandline extraction procedure is complex and possibly it is better to do this by hand.

### Extracting regions

This exercise can be done on all the genomes from the study. If you want to run this, please perform the following commands. Please just copy and paste the code into a text document, adjust the settings if necessary, and then copy and paste into your shell. 

~~~
#first we get all assembled genomes with all AMRFinderPlus outputs in them, same as you generated before, but now for all genomes
cp -r /mnt/netappits/users/courses2/shared/triumph/assembly_all ~/assembly_all

# We make the output folder
mkdir ~/regions/

# We run the script that finds CTX-M-14 from all the AMRFinderPlus outputs from all assembled genomes. You can change CTX-M-14 to another gene of interest (e.g. CTX-M-15). The gene itself, 5 kb left of the gene and 5 kb right of the gene are extracted and placed into a file in the folder ~/regions/

cd ~/assembly_all/
for sample in barcode01 barcode02 barcode03 barcode04 barcode05 barcode06 barcode07 barcode08 barcode09 barcode10 barcode11 barcode12 barcode13 barcode14 barcode15 barcode16 barcode17 barcode18 barcode19 barcode20 barcode21 barcode22 barcode23 barcode24; do
  grep "CTX-M-14" "$sample/amrfinderplus.txt" | cut -f 2,3,4 | while read contig start stop; do
    # Extract the sequence of the specified contig
    seq=$(awk -v contig="$contig" '
      BEGIN { found=0 }
      /^>/ {
        if(found) exit;
        found=($0 ~ ">"contig);
        next;
      }
      found { printf "%s", $0 }
    ' "$sample/assembly.fasta")

    # Define extraction range. In the example 5000 basepairs, but you can adjust this
    seq_length=${#seq}
    extract_start=$((start - 5000))
    extract_stop=$((stop + 5000))

    # Ensure the boundaries are valid
    if (( extract_start < 1 )); then extract_start=1; fi
    if (( extract_stop > seq_length )); then extract_stop=$seq_length; fi

    # Extract the subsequence
    extracted_seq=$(echo "$seq" | cut -c "$extract_start"-"$extract_stop")

    # Write to output file
    echo -e ">"$sample"\n$extracted_seq" > ~/regions/"$sample".fasta
  done
done

~~~
{: .bash}


### Annotating extracted regions

The above command was run on all the assembled genomes from this study. CTX-M-14 is found in barcode01 barcode06 barcode14 barcode15 barcode16 barcode17 barcode18 barcode19 barcode20 barcode24 according to the AMRFinderPlus analysis. 
We can annotate these using the following commands so that we can compare the genes in the regions using Clinker

~~~
$ cd ~/regions
$ for region in barcode01 barcode06 barcode14 barcode15 barcode16 barcode17 barcode18 barcode19 barcode20 barcode24; do
  prokka --outdir ~/regions/"$region" --prefix "$region" ~/regions/"$region".fasta --usegenus -genus Escherichia --cpus 1 --rawproduct --locustag "$region"
done

~~~
{: .bash}

### Running Clinker in the commandline

Finally, we can run Clinker on the annotated regions. Clinker needs to use gbk (Genbank) files

~~~
$ cd ~/regions/
$ clinker barcode*/*.gbk -p regions.html
$ 

~~~
{: .bash}

Download the html file regions.html you have just created and open it into a webbrowser. 



> ## Challenge: Is the resistance gene location conserved?
>
> Compare the regions, are the genes the same on all regions? What differences can you see?. 
> 
> 
> > ## Solution
> > The output can be made to look like this after moving the fragments around (click on the name and move up or down) and flipping them (double click on the sequence)
> > 
![image](https://github.com/user-attachments/assets/a8f48a9f-1eeb-4afa-aa2a-824e87c98dee)
> >
> > Discuss with the class.
> > The report.html can also be downloaded here: [regions.html](https://jpiamrtriumph.github.io/MicrobialGenomics/files/regions.html) : use right mouse button, click save target as.
> > 
> > {: .output}
> {: .solution}
{: .challenge}


{% include links.md %}
