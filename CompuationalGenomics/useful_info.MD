https://www.nist.gov/programs-projects/genome-bottle


1000 Genomes Project:
https://www.internationalgenome.org/data


https://github.com/genome-in-a-bottle/genome-stratifications/releases


SNP performance of NanoCaller and other variant callers in difficult-to-map regions on Nanopore reads of the Ashkenazim trio
BED file sources of different difficult-to-map regions with respect to GRCh38 reference genome:
1.	All difficult regions.
Source: ftp://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/genome-stratifications/v2.0/GRCh38/union/GRCh38_alldifficultregions.bed.gz
2.	Low mappability regions in Table S8.
Source: ftp://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/genome-stratifications/v2.0/GRCh38/mappability/GRCh38_lowmappabilityall.bed.gz
3.	Segmental duplications in Table S9.
Source: ftp://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/genome-stratifications/v2.0/GRCh38/SegmentalDuplications/GRCh38_segdups.bed.gz
4.	Tandem and homopolymer repeats (perfect homopolymers longer than 6bp and imperfect homopolymers longer than 10bp) in Table S10.
Source: ftp://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/genome-stratifications/v2.0/GRCh38/LowComplexity/GRCh38_AllTandemRepeatsandHomopolymers_slop5.bed.gz
5.	Perfect homopolymers longer than 6bp and imperfect homopolymers longer than 10bp.
Source: https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/genome-stratifications/v2.0/GRCh38/LowComplexity/GRCh38_AllHomopolymers_gt6bp_imperfectgt10bp_slop5.bed.gz 
6.	Perfect homopolymers of lengths 4-6bp.
Source: https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/genome-stratifications/v2.0/GRCh38/LowComplexity/GRCh38_SimpleRepeat_homopolymer_4to6_slop5.bed.gz 
 
7.	Table S11.
Source: ftp://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/genome-stratifications/v2.0/GRCh38/OtherDifficult/GRCh38_MHC.bed.gz

The details of these files are provided here: https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/release/genome-stratifications/v2.0/GRCh38/LowComplexity/v2.0-GRCh38-LowComplexity-README.txt

Various difficult-to-map regions
The following difficult-to-map regions are used for performance evaluation and comparison: 1) “all difficult-to-map” regions, 2) low mappability regions, 3) segmental duplications, 4) tandem and homopolymer repeats, and 5) MHC. For each of these categories, we get the evaluation regions by intersecting the corresponding BED file with high-confidence intervals for each genome using BEDtools.
Easy genomic regions
Easy genomic regions are also used during performance comparison and are defined as the complement of “all difficult-to-map” regions (GRCh38_alldifficultregions.bed). Easy genomic regions are obtained by removing “all difficult-to-map” regions from each genome’s high-confidence intervals.

Definition of homopolymer and non-homopolymer regions for indel performance evaluation
GIAB genome stratification BED is used to obtain homopolymer regions and non-homopolymer regions for evaluating indel performance. Homopolymer regions are obtained by combining GRCh38_AllHomopolymers_gt6bp_imperfectgt10bp_slop5.bed and GRCh38_SimpleRepeat_homopolymer_4to6_slop5.bed BED files. These first BED file contains intervals with perfect homopolymer regions longer than 6bp and imperfect homopolymers longer than 10bp (defined by GIAB as a ‘single base was repeated >10bp except for a 1bp interruption by a different base’, e.g. AAAATAAAAAA). The second BED file contains all intervals of homopolymer regions of lengths between 4 to 6bp. It is important to note that GRCh38_AllTandemRepeatsandHomopolymers_slop5.bed contains homopolymer regions from the first BED file only, and does not contain homopolymers of lengths 4-6bp from the second BED file.


