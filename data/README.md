# Data preprocessing
- use a set of 27 primates from [UCSC](http://hgdownload.soe.ucsc.edu/goldenPath/hg38/multiz30way/maf/), extracting alignment with 1 Mb to filter out species with Jukes-Cantor model in IQ-Tree.  
```
/mnt/d/iqtree/bin/iqtree2.exe -s $seqfile -te $treefile  --scf 1000 -blfix --prefix scf_21
/mnt/d/iqtree/bin/iqtree2.exe -s $seqfile -te $treefile --scfl 1000 -m "JC" -blfix --prefix scfl_21
```
- 21 speceis remain, including hg38, panTro5, panPan2, gorGor5, nomLeu3, rheMac8, macFas5, macNem1, cerAty1, chlSab2, manLeu1, nasLar1, colAng1, rhiRox1, rhiBie1, calJac3, cebCap1, tarSyr2, micMur3, proCoq1, otoGar3. 
- each interal branch has sCF score above 60.
<br>![sCF after filtering](../src/scf_21.png)

- Get alignment for analysis
1. Find the coordinates of all tRNAs in [mt](https://www.ncbi.nlm.nih.gov/gene) and [nuclear genome](http://genome.ucsc.edu/cgi-bin/hgTables) for hg38. There are 22 tRNAs from mt and 411 tRNAs from autosomes.
2. Find the coordiantes of all coding genes in mt and stratified sample 500 genes from autosome in hg38. We have 13 coding gene for mt and [ ] for nuclear genome
3. extract alignment blocks based on the coordianates above, concatenate into four alignments: mt-trna, mt-coding, nuclear-trn, nuclear-coding

Next, use the alignment to infer branch length of a [fixed tree](https://bds.mpi-cbg.de/hillerlab/120MammalAlignment/Human120way/).

<!--- 
`alignment.phylip` includes 119 spececies and 1 million columns for filtering.  
`tree_119mammal.nwk` is the phylogeny given in the [paper](https://academic.oup.com/gigascience/article/9/1/giz159/5695847#191021819) that published the dataset [dataset](https://bds.mpi-cbg.de/hillerlab/120MammalAlignment/Human120way/)  
-->


<!--- 
To analysis the data, [IQTREE-2](http://www.iqtree.org/doc/) is used  
- Find the best substituion model  
`~/bin/iqtree2 -s $seqfile -te $treefile -m MF -mset GTR -mfreq F -mrate E,I,G,I+G -nt AUTO --prefix model_selection`  
- Estimate parameters with model selected from previous step  
`~/bin/iqtree2 -s $seqfile -te $treefile -m GTR+F+I+G4 -nt AUTO --prefix parameter_estimation`  
- Calculate the SCF score as criterion for filtering  
`~/bin/iqtree2 -s $seqfile -te $treefile --scfl 1000 -blfix -m "GTR{1.0217,3.18485,0.684926,1.13687,3.35696}+F{0.300271,0.20402,0.203859,0.291849}+I{0.0114493}+G4{4.50791}" -nt AUTO --prefix scf_calc`  
`--scfl` is a function using likelihood to calculate the score; previous version used `--scf` that samples from tips. Flag `-m` uses parameters estimated from previous step. 
-->
# Data analysis (to update)


# To-dos
  ## Find tRNA genes in mt and nuclear genome. 
  used aligned primate genome, cropping based on annotations from reference genome (hg38)  
      1. ~~Find the coordinates in annotation files of tRNAs for hg38. There should be 22 for mt and 500 for nuclear genome.~~  
      2. ~~Find the coordinates of coding region in mt and nuclear~~ [annotations](http://hgdownload.soe.ucsc.edu/goldenPath/hg38/multiz30way/alignments/)(`knownCanonical.protNuc.fa.gz`)
      
  ## estimate divergence
  Given a topology (use [phylogeny](https://bds.mpi-cbg.de/hillerlab/120MammalAlignment/Human120way/) in .nh file extracted remaining taxa),
  1. For tRNA, use nuclieotide substition model - Kimura (K80) - to estimate the **total branch lengths** of input alignment (PAML or IQ-Tree would be fine) 
  2. For coding region, use codon substition model to estimate the **total branch lengths** of input alignment (**HyPhy only**)
  3. useful tutorials:  
  calculate dK/dS with [PAML](http://abacus.gene.ucl.ac.uk/software/paml.html) or `FEL` in [HyPhy](https://stevenweaver.github.io/hyphy-site/methods/selection-methods/#fel)  
  example usage of PAML, check `Exercise 1: ML estimation of the dN/dS (ω) ratio by hand`[link](https://isu-molphyl.github.io/EEOB563/computer_labs/lab6/).  
  exmaple of HyPhy [link](http://hyphy.org/methods/other/contrast-fel/). Note that we want **absolute dN and dS, not the ratio**. 

