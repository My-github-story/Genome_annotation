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

## Run prodigal on all of the genomes you have previously downloaded. Using command line tools, find which genome has the highest number of genes. Put all your code into a shell script, and put your code on the repository on Github where you keep your README with the solutions to this assignment.

## Code

```### Directory containing genome files
GENOME_DIR="$HOME/ncbi_dataset/data"  # Use the full path
OUTPUT_DIR="$HOME/ncbi_dataset/data" # Change to desired output path

### Create output directory if it doesn't exist
mkdir -p "$OUTPUT_DIR"

### Variables to track the genome with the highest number of genes
max_genes=0
best_genome=""

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
    
    echo "Genome: $(basename "$genome"), Genes: $gene_count"

    # Check if this genome has the highest number of genes
    if [[ "$gene_count" -gt "$max_genes" ]]; then
        max_genes=$gene_count
        best_genome=$(basename "$genome")
    fi
done

if [[ "$max_genes" -eq 0 ]]; then
    echo "No genes were found in any genomes."
else
    echo "Genome with the highest number of genes: $best_genome with $max_genes genes"
fi```
