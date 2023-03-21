* Data preprocessing  
`alignment.phylip` includes 119 spececies and 1 million columns for filtering.  
`tree_119mammal.nwk` is the phylogeny given in the [paper](https://academic.oup.com/gigascience/article/9/1/giz159/5695847#191021819) that published the dataset [dataset](https://bds.mpi-cbg.de/hillerlab/120MammalAlignment/Human120way/)  
  
  
To analysis the data, [IQTREE-2](http://www.iqtree.org/doc/) is used  
- Find the best substituion model  
`~/bin/iqtree2 -s $seqfile -te $treefile -m MF -mset GTR -mfreq F -mrate E,I,G,I+G -nt AUTO --prefix model_selection`  
- Estimate parameters with model selected from previous step  
`~/bin/iqtree2 -s $seqfile -te $treefile -m GTR+F+I+G4 -nt AUTO --prefix parameter_estimation`  
- Calculate the SCF score as criterion for filtering  
`~/bin/iqtree2 -s $seqfile -te $treefile --scfl 1000 -blfix -m "GTR{1.0217,3.18485,0.684926,1.13687,3.35696}+F{0.300271,0.20402,0.203859,0.291849}+I{0.0114493}+G4{4.50791}" -nt AUTO --prefix scf_calc`  
`--scfl` is a function using likelihood to calculate the score; previous version used `--scf` that samples from tips. Flag `-m` uses parameters estimated from previous step. 
