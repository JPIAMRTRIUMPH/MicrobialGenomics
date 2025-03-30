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
In the previous exercises we detected a resistance gene. If you want to investigate if the genetic region is similar, suggesting a common source or clonal spread, the region can be investigated using a tool called https://github.com/gamcil/clinker . Clinker is available via commandline but also via the https://cagecat.bioinformatics.nl/ webserver. We will discuss both options and perform one of the options below. We have also included a piece of shell code to extract and annotate the regions, however you can also cut and paste the sequence from the contigs using a text editor. For this course, we provide an example of several regions with a resistance gene, however the code can be used to generate your own at a later timepoint.

## Webbased use of Clinker

Download the zip file with extracted regions here: 
Unzip the extracted regions file to a folder

Visit the website https://cagecat.bioinformatics.nl/ and press start at the "cblaster" button

Fill in the details. Use NR for all proteins as annotation source, Refseq for only complete reference genomes, Swissprot for the manuallly curated Swissprot database. NR will be slow and might have false annotations. Refseq is faster. With Swissprot the annotations will be very precise but not alle proteins are in Swissprot. Press Query file and selected the unzipped .fasta files in the folder that contains the genetic regions you are interested in. Each region should be one file.

RUn Clinker. Press start.

## Manual commandline use of Clinker

This manual Clinker exercise should only be done *after* you have completed the annotation exercise from day 3 and if you feel compfortable using the commandline. It requires a good understanding of the input files of Clinker and how to get annotations. The commandline extraction procedure is complex and possibly it is better to do this by hand.

### Extracting regions
~~~
mkdir ~/regions/
cd ~/assembly/

for sample in barcode02 barcode03; do
  grep "CTX-M-15" "$sample/amrfinderplus.txt" | cut -f 5,6,7 | while read contig start stop; do
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

    # Define extraction range
    seq_length=${#seq}
    extract_start=$((start - 5000))
    extract_stop=$((stop + 5000))

    # Ensure the boundaries are valid
    if (( extract_start < 1 )); then extract_start=1; fi
    if (( extract_stop > seq_length )); then extract_stop=$seq_length; fi

    # Extract the subsequence
    extracted_seq=$(echo "$seq" | cut -c "$extract_start"-"$extract_stop")

    # Write to output file
    echo -e ">$contig:$extract_start-$extract_stop\n$extracted_seq" > ~/regions/"$sample".fasta
  done
done

~~~
{: .bash}


### Annotating extracted regions

~~~
$ cd ~/regions
$ for region in barcode02 barcode02 ; do
  prokka --outdir ~/regions/"$region" --prefix $sample ~/regions/"$region".fasta --usegenus -genus Escherichia --cpus 1 --rawproduct --locustag $region
done

~~~
{: .bash}

### Running Clinker in the commandline
~~~
$ cd ~/regions/
$ clinker barcode*/*.gbk -p regions.html
$ 

~~~
{: .bash}

Download the .svg and html files you have just created and open it into a webbrowser. 


> ## Challenge: Is the resistance gene location conserved?
>
> Compare the regions, are the genes the same on all regions? What differences can you see?. 
> 
> 
> > ## Solution
> >
> > Discuss with the class. 
> > {: .output}
> {: .solution}
{: .challenge}


{% include links.md %}
