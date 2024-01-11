# Seq_Code
Code for handling sequences


## Seqkit 
### Reverse Complement
seqkit seq CN3704.fasta -r -t DNA > CN3704_UKHSA_gidA.fasta

## BLAST 

### Make BLAST database
makeblastdb -in $file -parse_seqids -dbtype nucl
