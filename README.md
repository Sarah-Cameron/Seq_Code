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

## FastP
fastp -i Sp_clone_S2_S73_R1_001.fastq -I Sp_clone_S2_S73_R2_001.fastq -g -f 20 -F 20 -t 5 -T 5 -o S2_R1.fastq -O S2_R2.fastq

## Prokka 



## BLAST 

### Make BLAST database
makeblastdb -in $file -parse_seqids -dbtype nucl
