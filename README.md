# Seq_Code
Code for handling sequences


## Seqkit 
### Reverse Complement
seqkit seq CN3704.fasta -r -p -t DNA > CN3704_UKHSA_rev.fasta

### Remove Gaps
seqkit seq -g NAME.fasta > NEW_NAME.fasta 

## Prokka 



## BLAST 

### Make BLAST database
makeblastdb -in $file -parse_seqids -dbtype nucl
