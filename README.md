## Week 4 + 5: Genome Annotation
## 1. (2 points) If given the amino acid sequence KVRMFTSELDIMLSVNG-PADQIKYFCRHWT*, what is the number of amino acids in the encoded peptide (not including the stop codon)? Additonally, how many bases are contained in the open reading frame of the DNA sequence encoding the amino acids (including the stop codon)?

## command-
```echo "KVRMFTSELDIMLSVNGPADQIKYFCRHWT*" | awk '{amino_acids=length($1)-1; bases=(amino_acids+1)*3; print "Amino acids: " amino_acids ", Bases in ORF: " bases}'```
## Output- 
Amino acids: 30, Bases in ORF: 93

## Run prodigal on one of the genomes you have previously downloaded. Using command line tools, count how many genes were annotated (you can use any of the output formats for this but some are easier than others).

## command-
```$ prodigal -i GCA_000006745.1_ASM674v1_genomic.fna -o prodigal_output.gbk -a proteins.faa -d nucleotides.fna```
```$ grep -c "CDS" prodigal_output.gbk```

## Output-
3594

