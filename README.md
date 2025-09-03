# Seq_Code
Code for handling sequences 

### Move files with same extension from subdirectories:
find . -name \*.xls -exec cp {} newDir \;

### Allow app downloaded from internet
xattr -d com.apple.quarantine -r Ziplign.app

### Using x86_64 conda on arm64 M2 Chip 
CONDA_SUBDIR=osx-64 conda create -n rosetta python   # create a new environment called rosetta with intel packages.
conda activate rosetta
python -c "import platform;print(platform.machine())"
conda config --env --set subdir osx-64  # make sure that conda commands in this environment use intel packages

### Rename fasta header by filename 
awk '/^>/{print ">" substr(FILENAME,1,length(FILENAME)-6); next} 1' *.fasta

## Seqkit 
seqkit stats -a reads.fasta 

### Get Contig Lengths
seqkit fx2tab --length --name --header-line  foo.fasta

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

### Extract contig lengths 
seqkit fx2tab -n -l *.fasta > contig_lengths.tsv

## Linux Files 

### Extract 2nd column from .txt file
awk '{print $2}' file1 > file2

### Remove double quotes from file 
tr -d '"' <infile

### Extract all file types to a separate directory 
find . -name \*.xls -exec cp {} newDir \;

### Compress directory with tar 
tar czf name_of_archive_file.tar.gz name_of_directory_to_tar

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

## Making Fake Illumina Reads - art_modern
opt/build_release/art_modern --mode wgs --lc pe --i-file SPARK_1074_C1_Dup_5.fasta --o-fastq opt/build_release/SPARK_1074_50x_DUP_5.fastq --qual_file_1 data/Illumina_profiles/HiSeqXtruSeqL150R1.txt --qual_file_2 data/Illumina_profiles/HiSeqXtruSeqL150R2.txt --read_len 150 --parallel 4 --i-fcov 50 --pe_frag_dist_mean 300 --pe_frag_dist_std_dev 50

## Scoary 2
scoary2 --genes gene_presence_absence.Rtab --gene-data-type 'gene-count:\t' --traits total_resistance.csv --trait-data-type gaussian:kmeans:,  --outdir scoary_total_out --n-permut 1000 
