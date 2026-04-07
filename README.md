# Genome Eb315ss1

## check the quality of the dataset
 - Use Fast QC

## Use Trimmomatic to trim the ends
 - Use Trimmomatic to trim the ends where Phred scores drop below a threshold (threshold in 20-40 range)

## find the recommended k-mer
 - find the recommended k-mer through Velvet Advisor

## options of assembly
 - run Spades and Velvet (use recommended k-mer as starting point for Velvet) for two options of assembly

## check genome intactness
 - pick the better assembly (decided by n50 and number of contigs) and run it through BUSCO for genome intactness 

## submit through NCBI's website


## Gene Prediction using Augustus and Snap
- Summarize #s of predicted genes from snap and AUGUSTUS
- Include screen shot of IGV browser window showing an example of a gene predicted only by snap
- Include screen shot of IGV browser window showing an example of a gene predicted only by AUGUSTUS
- Include screen shot of gene where snap and AUGUSTUS predict the same exon/intron structure
- Include screen shot of gene where snap and AUGUSTUS predict a different exon/intron structure
