# Data preprocessing (to update)
`alignment.phylip` includes 119 spececies and 1 million columns for filtering.  
`tree_119mammal.nwk` is the phylogeny given in the [paper](https://academic.oup.com/gigascience/article/9/1/giz159/5695847#191021819) that published the dataset [dataset](https://bds.mpi-cbg.de/hillerlab/120MammalAlignment/Human120way/)  
  
# Data analysis (to update)
To analysis the data, [IQTREE-2](http://www.iqtree.org/doc/) is used  
- Find the best substituion model  
`~/bin/iqtree2 -s $seqfile -te $treefile -m MF -mset GTR -mfreq F -mrate E,I,G,I+G -nt AUTO --prefix model_selection`  
- Estimate parameters with model selected from previous step  
`~/bin/iqtree2 -s $seqfile -te $treefile -m GTR+F+I+G4 -nt AUTO --prefix parameter_estimation`  
- Calculate the SCF score as criterion for filtering  
`~/bin/iqtree2 -s $seqfile -te $treefile --scfl 1000 -blfix -m "GTR{1.0217,3.18485,0.684926,1.13687,3.35696}+F{0.300271,0.20402,0.203859,0.291849}+I{0.0114493}+G4{4.50791}" -nt AUTO --prefix scf_calc`  
`--scfl` is a function using likelihood to calculate the score; previous version used `--scf` that samples from tips. Flag `-m` uses parameters estimated from previous step. 

# Pipe-line
- use a subset of 120 mammals, focusing on primates -> ~ 1 mb alignment for filtering
- remaining 13 speices  

  `chlSab2, macNem1, macFas5, nasLar1, rhiRox1, rhiBie1, colAng1, ponAbe2, hg38, panTro5, panPan2, calJac3, saiBol1, tarSyr2, otoGar3`  
    where `tarSy2, otoGar3` are outgroupers.
 - attach sCF filtering result
 -
# To-dos
  * Find overlapped tRNA genes in mt in **all species**.  
  example query in [NCBI](https://www.ncbi.nlm.nih.gov/gene/):  
  `genetype trna[Properties] AND source mitochondrion[Properties] AND "annotated genes"[Filter] AND "Homo sapiens"[porgn] AND ("annotated genes"[Filter] AND alive[prop])`  
  (not completely corret yet. NEED TO MODIFY name of taxon `Homo sapiens -> hg38`. Try hg38 -> genome assembly id -> add keywords into query)
  * Find overlapped tRNA genes in nuclear genome in **all speecies** based on previous tRNA gene set. with the same function or just randomly sampled?
  * Get alignment   
  Two options to aligned the tRNA genes: 1. used aligned primate genome, cropping based on annotations from reference genome. 2. aligned sets from previous steps
  * calculate dK/dS with [PAML](http://abacus.gene.ucl.ac.uk/software/paml.html)
  
