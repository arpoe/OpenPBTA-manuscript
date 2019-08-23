---
author-meta:
- John Doe
- Jane Roe
date-meta: '2019-08-23'
keywords:
- pediatric cancer
- brain tumor
- tumor atlas
lang: en-US
title: An Open Pediatric Brain Tumor Atlas
...






<small><em>
This manuscript
([permalink](https://AlexsLemonade.github.io/OpenPBTA-manuscript/v/d1adc40f314fa076cabd37773cdfc0e0fb7da7a4/))
was automatically generated
from [AlexsLemonade/OpenPBTA-manuscript@d1adc40](https://github.com/AlexsLemonade/OpenPBTA-manuscript/tree/d1adc40f314fa076cabd37773cdfc0e0fb7da7a4)
on August 23, 2019.
</em></small>

## Authors



+ **John Doe**<br>
    ![ORCID icon](images/orcid.svg){.inline_icon}
    [XXXX-XXXX-XXXX-XXXX](https://orcid.org/XXXX-XXXX-XXXX-XXXX)
    · ![GitHub icon](images/github.svg){.inline_icon}
    [johndoe](https://github.com/johndoe)
    · ![Twitter icon](images/twitter.svg){.inline_icon}
    [johndoe](https://twitter.com/johndoe)<br>
  <small>
     Department of Something, University of Whatever
     · Funded by Grant XXXXXXXX
  </small>

+ **Jane Roe**<br>
    ![ORCID icon](images/orcid.svg){.inline_icon}
    [XXXX-XXXX-XXXX-XXXX](https://orcid.org/XXXX-XXXX-XXXX-XXXX)
    · ![GitHub icon](images/github.svg){.inline_icon}
    [janeroe](https://github.com/janeroe)<br>
  <small>
     Department of Something, University of Whatever; Department of Whatever, University of Something
  </small>



## Abstract {.page_break_before}




## Introduction

Introduction will go here.


## Materials and Methods

### Biospecimen collection

The Pediatric Brain Tumor Atlas specimens are comprised of samples from Children's Brain Tumor Tissue Consortium (CBTTC) and the Pediatric Pacific Neuro-oncology Consortium (PNOC). 
Blood and tumor biospecimens from patients enrolled within the CBTTC were sent to the Children's Hospital of Philadelphia for RNA and/or DNA extraction.
PNOC collected blood and tumor biospecimens from newly-diagnosed DIPG patients as part of the clinical trial [PNOC003/NCT02274987](https://clinicaltrials.gov/ct2/show/NCT02274987) [@BLNV1DbL]. 

#### Nucleic acids extraction and library preparation

##### PNOC samples
The Translational Genomic Research Institute (TGEN; Phoenix, AZ) performed DNA and RNA extractions on tumor biopsies using a DNA/RNA AllPrep Kit (Qiagen, #80204). 
All RNA used for library prep had a minimum RIN of 7 but no QC thresholds were implemented for the DNA.
For library preparation, 500ng of nucleic acids were used as input for RNA-Seq and WXS.
The RNA prep was performed using the TruSeq RNA Sample Prep Kit (Illumina, #FC-122-1001) and the exome prep was performed using KAPA Library Preparation Kit (Kapa Biosystems, #KK8201) using Agilent's SureSelect Human All Exon V5 backbone with custom probes. 
These probes include CGH probes that target 44,000 evenly spaced genomic loci to assess copy number changes, as well as probes that tile across tumor suppressor genes and genes involved in common cancer translocations.

##### CBTTC samples

#### Data generation

NantHealth Sequencing Center (Culver City, CA) performed whole genome sequencing (WGS) on all paired tumor (~60X) and constitutive (~30X) DNA samples.
WGS libraries were 2x150 bp and sequenced on an Illumina X/400.
NantHealth Sequencing Center performed ribosomal-depleted whole transcriptome stranded RNA-Seq to an average depth of 100M reads for CBTTC tumor samples.
The Translational Genomic Research Institute (TGEN; Phoenix, AZ) performed paired tumor (~200X) and constitutive whole exome sequencing (WXS) and poly-A selected RNA-Seq (~200M reads) for  PNOC tumor samples. 
PNOC WXS and RNA-Seq libraries 2x100 bp and sequenced on an Illumina HiSeq 2500.

### DNA WGS Alignment

We used BWA-MEM [@gmH6YDca] v0.7.17 for alignment of paired-end DNA-seq reads.
The alignment reference that we used was Homo Sapiens Human Genome (hg) version 38, patch release 12, fasta file obtained from UCSC [@1AWRgvxVu].
Alignments were futher processed using following the Broad Institute's Best Practices [@EHMzG34o=11165] for processing BAMs in preparation for variant discovery.
Duplicates were marked using Samblaster[@IzUbOv92] v0.1.24, BAMs merged and sorted using Sambamba [@PcJwlZj3] v0.6.3.
Lastly, resultant BAMs were processing using Broad's Genome Analysis Tool Kit (GATK) [@RsmkaFIb] v4.0.3.0, BaseRecalibrator submodule.

### Germ Line Single Nucleotide Variant Calling

### Somatic Single Nucleotide Variant Calling
#### SNV and INDEL calling

We used Strelka2 [@REfkDUtE] v2.9.3 and Mutect2 from GATK v4.1.1.0.
Strelka2 was run using default parameters on human genome reference hg38, canonical chromosomes only (chr1-22, X,Y,M), as recommended by the author.
Mutect2 was run following Broad best practices outlined from their Workflow Description Language (WDL) [@E5aTvXmQ].  

#### VCF annotation and MAF creation

We filtered outputs from both callers on the "PASS" filter, and annotated using The ENSEMBL Variant Effect Predictor [@p1f5DxRQ], reference release 93, and created MAFs using MSKCC's vcf2maf [@11Fn5jqUI] v1.6.16.

### Somatic Copy Number Variant Calling

We used Control-FREEC [@ZQ0L3o1q; @1F3i4BvCt] v8.7 and CNVkit [@UTxRcYIQ] v0.9.3 for copy number variant calls. 
CNVkit was run using default parameters on human genome reference hg38 and using the batch command for tumor-normal pairs rather than a panel of normals.

### Somatic Structural Variant Calling

We used Manta SV [@kTn1PIj5] v1.4.0 for structural variant (SV) calls.
Manta SV calling was also limited to regions used in Strelka2.
We also ran LUMPY SV [@2dosQoAM] v0.2.13 in express mode using default parameters. 
The hg38 reference used was also limited to canonical chromosome regions.

### Gene Expression Abundance Estimation
We used STAR [@tTu8Ds9Z] v2.6.1d to align paired-end RNA-seq reads.
This output was used for all subsequent RNA analysis. The reference we used was that of ENSEMBL's GENCODE 27 [@9oRUnb2k], "Comprehensive gene annotation."
We used RSEM [@Dh2n1EV3] v1.3.1 for transcript- and gene-level quantification.
We also added a second method of quantification using kallisto [@12KZMHMQl] v0.43.1.
This method differs in that it uses pseudoaligments using fastq reads directly to the aforementioned GENCODE 27 reference.

### RNA Fusion Calling and Prioritization
#### Gene fusion detection

We set up [Arriba v1.1.0](https://github.com/suhrig/arriba/) and STAR-Fusion 1.5.0 [@rqYlIiAi] fusion detection tools using CWL on CAVATICA.
For both these tools we used aligned BAM and chimeric SAM files from STAR as inputs and GRCh38_gencode_v27 GTF for gene annotation.
We ran STAR-Fusion with default parameters and annotated all fusion calls with GRCh38_v27_CTAT_lib_Feb092018.plug-n-play.tar.gz provided in the STAR-fusion release. 
For Arriba, we used a blacklist file (blacklist_hg38_GRCh38_2018-11-04.tsv.gz) from the Arriba release tarballs to remove recurrent fusion artifacts and transcripts present in healthy tissue.
We also provided Arriba with strandedness information or set it to auto-detection for polyA samples.

#### Fusion prioritization

We built a [fusion prioritization pipeline](https://github.com/d3b-center/fusion_filtering_pipeline) to filter and annotate fusions.
We considered all inframe and frameshift fusion calls with 1 or more junction reads and fused genes expressed with TPM greater than one to be true calls.
If a fusion call had large number of spanning fragment reads compared to junction reads (spanning fragment minus junction read greater than ten) or if either 5\` or 3\` genes fused to more than five different genes we removed these calls as a potential false positive.
We also removed fusions if the 5\` or 3\` ends were the same gene, and these were tagged as non-canonical splicing or duplication.
We used a list of curated fusion calls for each histology to capture each occurrence of the fusion as a putative driver fusion.
We prioritized a union of fusion calls as true calls if the fused genes were detected by both callers, the same fusion was recurrent in histology (>2 samples) or the fusion was specific to the broad histology. 
We annotated putative driver fusions and prioritized fusions lists with kinases, oncogenic, tumor suppressor, transcription factor, fused genes and known TCGA fusions from curated [datasheets](https://github.com/d3b-center/fusion_filtering_pipeline/tree/master/references).
We also added chimerDB [@FLBuvqQe] annotations to both driver and prioritized fusion list.

### Clinical Data Harmonization


## Results

Results section stub.


## Conclusions

Stub in conclusions section   


## References {.page_break_before}

<!-- Explicitly insert bibliography here -->
<div id="refs"></div>