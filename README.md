# Seq_Code
Code for handling sequences 


### Using x86_64 conda on arm64 M2 Chip 
CONDA_SUBDIR=osx-64 conda create -n rosetta python   # create a new environment called rosetta with intel packages.
conda activate rosetta
python -c "import platform;print(platform.machine())"
conda config --env --set subdir osx-64  # make sure that conda commands in this environment use intel packages

### Rename fasta header by filename 
awk '/^>/{print ">" substr(FILENAME,1,length(FILENAME)-6); next} 1' *.fasta

## Seqkit 
seqkit stats -a reads.fasta 

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

## SAMTOOLS
samtools coverage NAME.bam

## Prokka 
prokka --outdir L5_Stock --genus Bordetella --species pertussis --strain L5_Stock --centre XXX L5_Stock_final.fasta

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
