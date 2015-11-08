# fusionfusion 

## Introduction

fusionfusion is a software for detecting gene fusion 
using the putative chimeric transcript generated by several well-known transcriptome alignment tools (STAR, MapSplice2 and TopHat2).
Many of those predicted chimeric transcripts are "false positives".
However, by performing effective filtering, sensitive and accurate gene fusion detection is possible.
After the alignment steps, the software can generate final gene fusion candidates
and integrating our software into the pipeline will come very easily to you!

## Dependency

### Python
Python (>= 2.7), `pysam (>= 0.8.1)` packages

### Software
blat

## Install

```
git clone https://github.com/Genomon-Project/fusionfusion.git
cd fusionfusion 
python setup.py build
python setup.py install
```
For the last command, you may need to add --user if you are using a shared computing cluster.
```
python setup.py install --user
```

## Preparation

First, you need to perform transcriptome sequencing alignemnt by STAR, MapSplice2, TopHat2. 

For STAR, our software uses the chimeric sam file
```
{output_prefix}.Chimeric.out.sam
```

For MapSplice2, our software uses the read alignment file
```
alignments.sam (bam)
```
You do not need to care about the sorting status.

For TopHat2, our software uses the read alignment file
```
accepted_hits.bam
```

Then, gene and exon annotation bed file should be prepared.
The easiest way is
```
cd resource
bash prepGeneInfo.sh
```

Then, a configuration files used by the ConfigPareser package (https://docs.python.org/2/library/configparser.html) 
should be prepared.
```
param.cfg
```
See an example file for description of each parameters.
The parameters you typically want to modify is *reference_genome*, *blat_path*, *annotation_dir*.
For other parameters, the default setting will work fine for 100bp-length paired read data.

## Commands

```
fusionfusion --star star.Chimeric.out.sam --ms2 ms2.bam --th2 th2.bam --out output_dir --param param.cfg
```
At least one of --star, --ms2, --th2 arguments should be specified.
The other arguments are mandatory.

## Results

For the result generated by single tool (star.fusion.result.txt, ms2.fusion.result.txt and th2.fusion.result.txt):

1. chromosome for the 1st breakpoint
1. coordinate for the 1st breakpoint
1. direction of the 1st breakpoint
1. chromosome for the 2nd breakpoint
1. coordinate for the 2nd breakpoint
1. direction of the 2nd breakpoint
1. inserted nucleotides within the breakpoints
1. #read_pairs supporting the fusion
1. gene overlapping the 1st breakpoint
1. exon-intron junction overlapping the 1st breakpoint
1. gene overlapping the 2nd breakpoint
1. exon-intron junction overlapping the 2nd breakpoint
1. contig match score for the 1st breakpoint
1. contig size for the 1st breakpoint
1. contig match score for the 2nd breakpoint
1. conting size for the 2nd breakpoint

For the merged result (fusionfusion.result.txt):

1. chromosome for the 1st breakpoint
1. coordinate for the 1st breakpoint
1. direction of the 1st breakpoint
1. chromosome for the 2nd breakpoint
1. coordinate for the 2nd breakpoint
1. direction of the 2nd breakpoint 
1. inserted nucleotides within the breakpoints 
1. gene overlapping the 1st breakpoint
1. exon-intron junction overlapping the 1st breakpoint
1. gene overlapping the 2nd breakpoint
1. exon-intron junction overlapping the 2nd breakpoint
1. #read_pairs supporting the variant (by MapSplice2 if --ms2 is specified)
1. #read_pairs supporting the variant (by STAR if --star is specified)
1. #read_pairs supporting the variant (by TopHat2 if --th2 is specified)

