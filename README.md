## Week 4 + 5: Genome Annotation
## 1. (2 points) If given the amino acid sequence KVRMFTSELDIMLSVNG-PADQIKYFCRHWT*, what is the number of amino acids in the encoded peptide (not including the stop codon)? Additonally, how many bases are contained in the open reading frame of the DNA sequence encoding the amino acids (including the stop codon)?

## command-
```echo "KVRMFTSELDIMLSVNGPADQIKYFCRHWT*" | awk '{amino_acids=length($1)-1; bases=(amino_acids+1)*3; print "Amino acids: " amino_acids ", Bases in ORF: " bases}'```
## Output- 
Amino acids: 30, Bases in ORF: 93

## 2) Run prodigal on one of the genomes you have previously downloaded. Using command line tools, count how many genes were annotated (you can use any of the output formats for this but some are easier than others).

## command-
```$ prodigal -i GCA_000006745.1_ASM674v1_genomic.fna -o prodigal_output.gbk -a proteins.faa -d nucleotides.fna```
```$ grep -c "CDS" prodigal_output.gbk```

## Output-
3594

#### Comment-A CDS refers specifically to the portion of the genome that is translated into a protein, whereas a gene might also include non-coding elements like promoters or untranslated regions (UTRs). Prodigal focuses on coding sequences, so it uses "CDS" to annotate each predicted gene that codes for a protein.

## 3) Run prodigal on all of the genomes you have previously downloaded. Using command line tools, find which genome has the highest number of genes. Put all your code into a shell script, and put your code on the repository on Github where you keep your README with the solutions to this assignment.

## Command to create the shell script and the code
```nano run_prodigal.sh```

## Code

``` javascript

#!/bin/bash

# Directory containing genome files
GENOME_DIR="/home/abazaiea/ncbi_dataset/prodigal_output/"  # Use the full path
OUTPUT_DIR="/home/abazaiea/ncbi_dataset/prodigal_output/" # Change to desired output path

# Create output directory if it doesn't exist
mkdir -p "$OUTPUT_DIR"

# Variables to track the total number of genes and the genome with the highest number of genes
total_genes=0  # Initialize a variable to keep track of the total number of genes across all genomes
max_genes=0    # Initialize a variable to keep track of the maximum number of genes found in a single genome
best_genome=""  # Initialize a variable to store the name of the genome with the highest number of genes

# Loop through all fasta files in the genome directory
for genome in "$GENOME_DIR"/*.fna; do
    # Check if the file exists
    if [[ ! -e "$genome" ]]; then
        echo "No fasta files found in $GENOME_DIR."
        exit 1
    fi

    # Run Prodigal
    prodigal -i "$genome" -o "$OUTPUT_DIR/$(basename "$genome" .fna).gbk" -a "$OUTPUT_DIR/$(basename "$genome" .fna).faa" -f gbk
    
    # Count the number of genes
    gene_count=$(grep -c 'CDS' "$OUTPUT_DIR/$(basename "$genome" .fna).gbk")
    
    # Update total gene count
    total_genes=$((total_genes + gene_count))  # Add the count of genes for the current genome to the total gene count

    # Print the gene count for the current genome
    echo "Genome: $(basename "$genome"), Genes: $gene_count"

    # Check if this genome has the highest number of genes
    if [[ "$gene_count" -gt "$max_genes" ]]; then
        max_genes=$gene_count  # Update max_genes if the current genome has more genes
        best_genome=$(basename "$genome")  # Store the name of the genome with the highest gene count
    fi
done

# Output the total number of genes and the genome with the highest number of genes
if [[ "$total_genes" -eq 0 ]]; then
    echo "No genes were found in any genomes."
else
    echo "Total number of genes across all genomes: $total_genes"
    echo "Genome with the highest number of genes: $best_genome with $max_genes genes"
fi
```

## Output-
Genome with the highest number of genes: GCA_000006745.1_ASM674v1_genomic.fna with 3594 genes
