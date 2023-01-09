# PhyloGWAS
Use PhyloGWAS to identify genes that are associated with floral color in *Pedicularis*.

## Basic information
PhyloGWAS is used to identify genetic variation associate with trait evolution. More specifically, PhyloGWAS tries to find **nonsynonymous variants that are shared by species with same phenotypic trait but are *not* in a monophyletic group.**

PhyloGWAS is a variant of GWAS (Genome-wide associate studies). Instead of sequencing more than 100 individuals of two populations with distinct phenotypic trait, PhyloGWAS sequences multiple species with a distinct phenotypic trait and divide these species into two groups (like "populaitons" in GWAS). It believes that these species undergo a parallell evolution under similar environmental conditions. Therefore, compared to GWAS, PhyloGWAS:
- sequences multiple species rather than individuals
- requires species that have same phenotypic trait are not in a monophyletic group

## Procedures
### Preparation
- software: 
  - MVFtools
  - ms
- scripts: 
  - orf_aln_process.py
- input file: a bunch of single copy orthologue alignment files (.fa)

### Workflow
1. remove all gaps and missing data in alignment files using a python script(wum5/JaltPhylo\)
```{bash}
python orf_aln_process.py -i <inDIR> -o <outDIR> -s seqname -d 14
```
2. convert format of alignment files from fasta to mvf using MVFtools (peaselab/mvftools\)
```{bash}
python3 mvftools.py ConvertFasta2MVF --fasta DATA.fasta --mvf DATA.mvf
```
3. infer group specific allelles using MVFtools (ACColin/PhyloJaltoWorkflow\)
```{bash}
python3 mvftools/mvftools.py InferGroupSpecificAllele 
--mvf Pease_tomato_codon.mvf --out Pease_tomato_pH 
--allelegroups ACIDIC:LA0436_starmap5.Aligned.out.sorted.bam,LA0429_starmap5.Aligned.out.sorted.bam,LA2933_starmap5.Aligned.out.sorted.bam,LA1322_starmap5.Aligned.out.sorted.bam BASIC:LA1589_starmap5.Aligned.out.sorted.bam,LA2744_starmap5.Aligned.out.sorted.bam,LA2964_starmap5.Aligned.out.sorted.bam,LA1782_starmap5.Aligned.out.sorted.bam,LA4117_starmap5.Aligned.out.sorted.bam 
--speciesgroups
GAL:LA0436_starmap5.Aligned.out.sorted.bam CHE:LA0429_starmap5.Aligned.out.sorted.bam LYC:LA2933_starmap5.Aligned.out.sorted.bam NEO:LA1322_starmap5.Aligned.out.sorted.bam PIM:LA1589_starmap5.Aligned.out.sorted.bam PER:LA2744_starmap5.Aligned.out.sorted.bam PER:LA2964_starmap5.Aligned.out.sorted.bam CHI:LA1782_starmap5.Aligned.out.sorted.bam CHI:LA4117_starmap5.Aligned.out.sorted.bam
``` 
4. test significance of these group specific allelles using ms (wum5/JaltPhylo\)
```{bash}
qsub ms_sim.sh
```

## References
**papers**
1. Pease JB, Haak DC, Hahn MW, Moyle LC (2016) Phylogenomics Reveals Three Sources of Adaptive Variation during a Rapid Radiation. PLoS Biol 14(2): e1002379. doi:10.1371/journal. pbio.1002379
2. Wu et. al. (2018) Dissecting the basis of novel trait evolution in a radiation with widespread phylogenetic discordance. Molecular Ecology. 27 (16): doi:10.1111/mec.14780

**github repos**
1. wum5/JaltPhylo\
https://github.com/wum5/JaltPhylo#introgression-analysis
2. ACColin/PhyloJaltoWorkflow\
https://github.com/ACColin/PhyloJaltoWorkflow
3. peaselab/mvftools\
https://github.com/peaselab/mvftools


