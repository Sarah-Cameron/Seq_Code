# Seq_Code
Code for handling sequences


## Seqkit 

### Fastq to Fasta 
eqkit fq2fa L6_Illumina_Trimmed.fastq -o L6_Illumina_Trimmed.fasta

### Replace all sequence IDs with unique number in sequence 
seqkit replace -p .+ -r "seq_{nr}"

### Reverse Complement
seqkit seq CN3704.fasta -r -p -t DNA > CN3704_UKHSA_rev.fasta

### Remove Gaps
seqkit seq -g NAME.fasta > NEW_NAME.fasta 

### Extract a only fasta files from given list: 
seqkit grep -f yagA.txt pangenome_reference.fa -o yagA_Reads.fasta

## Medaka 

### Annotate a vcf file with depth of reference and alternative reads for each SNP (fwd and rev for each):
medaka tools annotate --dpsp ./medaka/medaka.sorted.vcf L10_Stock_Assembly.fasta ./medaka/calls_to_ref.bam ./medaka/L10_Broth_SNPs.vcf

## FastP
fastp -i Sp_clone_S2_S73_R1_001.fastq -I Sp_clone_S2_S73_R2_001.fastq -g -f 20 -F 20 -t 5 -T 5 -o S2_R1.fastq -O S2_R2.fastq


## Prokka 

## BLAST 

### Make BLAST database
makeblastdb -in $file -parse_seqids -dbtype nucl

### BLAST Loop
#!/bin/bash
#Loop for making each genome into a database
files=./Raw_Reads/SRR12168673.fasta
for file in $files
do
makeblastdb -in $file -parse_seqids -dbtype nucl;
done

#Loop for conducting BLAST search in each database
for database in ./Raw_Reads/SRR12168673.fasta; do
blastn \
-query First_40_IS481.txt \
-db $database \
-max_target_seqs 50000 \
-outfmt "6 std sstrand" \
-out $database.BLAST.Result.First.txt;
done
