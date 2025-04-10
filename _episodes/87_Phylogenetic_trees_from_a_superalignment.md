---
start: false
title: "Phylogenetic trees from the core genome and visualization"
teaching: 10
exercises: 90
questions:
- "Is there a specific clone associated with resistance"
objectives:
- "How to determine a build a tree from a set of core genes"
- "Compare the tree with the gene absence presence tree"
- "Colour the tree with annotation data"
keypoints:
- "A tree can be generated from a combined set of genes for better resolution. More genes = more resolution"
---

## Building a super alignment tree

A phylogenetic tree from a single gene may be less informative because it does not contain enough information. One solution is to build a tree from a set of concatenated core genes. This is done by Roary using the "-e -n" option, where it will use all core genes determined automatically. This takes a lot of time and may not be feasible when you are following this course instructor-led.  

If you have made the core gene alignment using the -e -n option in Roary (see previous exercise), then we can build the super alignment tree from all core genes. This may take some time.

You can choose two methods:

Quick method. Less precise. This extracts only the SNPs and uses FastTree in the fastest setting
~~~
$ cd ~/orthology_en
$ snp-sites core_gene_alignment.aln > snpsites.core_gene_alignment.aln
$ FastTree -nt -fastest snpsites.core_gene_alignment.aln > snpsites.core_gene_alignment.tree
~~~
{: .bash} 

OR

More precise method. Can be very slow. This uses the complete core gene superalignment
~~~
$ cd ~/orthology_en
$ FastTree -nt core_gene_alignment.aln > core_gene_alignment.tree
~~~
{: .bash} 

Inspect the superalignment. How many bases are in the alignment?. Also download the tree and view it using Figtree which can be downloaded here: [https://github.com/rambaut/figtree/releases](https://github.com/rambaut/figtree/releases) . An alternative is [iTOL](https://itol.embl.de/upload.cgi) . Does it look comparable to the gene presence absence tree?  Look at the reference isolate. FastTree is a tool for a quick first tree. Better tools would be RaxML (https://cme.h-its.org/exelixis/web/software/raxml/) or IQ-TREE (http://www.iqtree.org/) however these take quite some time to run.

## Visualizing phenotypes

It is possible to color the labels of the trees using the "Import Annotations" option in Figtree or the "Datasets" option in [https://itol.embl.de/](https://itol.embl.de) (use the paste function in the raw data table). Download the file with annotations here: [annotations.txt](../files/annotations.txt) and import this file. Click on the triangle next to tip labels and select "Colour by". Select "Resistance" in the dropdown box. The isolates in red are the susceptible isolates. 

Is there a specific clone associated with resistance? We have seen it already in the gene presence absence tree. Inspect both the gene presence absence tree and the tree from the superalignment of all core genes. 

