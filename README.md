# target_enrichment
Tools for empirical design of probes for hybridization-based target enrichment from 3' single-cell RNA-seq

This repository contains scripts and notebooks used in "Scalable single-cell CRISPR screens by direct guide RNA capture and targeted library enrichment", Replogle JM et al., Nature Biotechnology 2020. At present, these are simply presented as used in the manuscript, with notebooks serving as examples. This target enrichment strategy was designed by Tom Norman and Joseph Replogle.

I use two notebooks to choose targeted regions: 
 
(1) process_bams.ipynb

The purpose of this notebook is to process a bam file (10x scRNA-seq in your cell type of interest) to transcript-resolved mappings. 
We use the library plastid (https://plastid.readthedocs.io/en/latest/)

(2) pick_targets.ipynb

(a) For each target gene, we determine all transcript isoforms that account for >80% of RNA-seq reads in your cell type of interest (K562 RNA-seq dataset: https://www.encodeproject.org/files/ENCFF717EVE/). 

(b) For each of these transcripts, we then perform a peak finding procedure to find the region to target with probes. 
We align all reads from a cell-type matched 3’ scRNA-seq dataset and smooth them using a median filter. 
We then find the region that contains >80% of reads, pad the selected region with 25 bp on the 5' end and 200 bp on the 3' end, and extract the targeted sequence. 
For transcripts where the resulting sequence is >2 kb (e.g. if there is an extraneous peak of density early in a transcript), then we threshold the sequence on the 5’ end to make a 2 kb peak. 
For transcripts with insufficient sequencing coverage for empirical design, we simply target 300 bp starting at the annotated 3' end of the transcript. 

(c) Next, for each gene, we compare the regions chosen across different transcripts. 
If one region is a strict subset another, then we eliminate the smaller region.

(d) For these target sequences, design and synthesize 120 bp biotinylated probes using your favorite oligo supplier.
