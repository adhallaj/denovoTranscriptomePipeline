# ASC-Transcriptome
Pipeline for assembly and annotation of a de-novo transcriptome using the Alabama SuperComputer.

# Notes before starting:

All .sh scripts need to have the > chmod +x command executed on them to be able to run via run_script on the ASC.

# 1. Removing Adapters from Raw Reads with CutAdapt
Adapters are removed using the program Cutadapt. The adapter sequences should be available in the data provided by the sequencing provider.
There are two adapters (and two raw data .fq.gz files) because this transcriptome was sequenced using Illumina paired-end sequencing.

5' Adapter:

5'-AATGATACGGCGACCACCGAGATCTACACTCTTTCCCTACACGACGCTCTTCCGATCT-3'

3' Adapter:

5'-GATCGGAAGAGCACACGTCTGAACTCCAGTCACATCACGATCTCGTATGCCGTCTTCTGCTTG-3'

Ran Cutadapt with the following parameters:
4:00:00 run time, medium queue, 16gb memory, 16 cores, dmc

# 2. Assembling Raw Reads into a Transcriptome with Trinity
Now there are two files of sequences, adapter free, that need to be assembled. This assembly is done via Trinity. Both files will be combined and assembled in an output .fasta file specified by the flag --output.

The sequences are now a proper assembled transcriptome, but will need to be searched for open reading frames.

# 3. Identifying Open Reading Frames (ORFs) in the Assembled Transcriptome using Transdecoder

Transdecoder outputs four files:

transcripts.fasta.transdecoder.pep : peptide sequences for the final candidate ORFs; all shorter candidates within longer ORFs were removed.
transcripts.fasta.transdecoder.cds : nucleotide sequences for coding regions of the final candidate ORFs
transcripts.fasta.transdecoder.gff3 : positions within the target transcripts of the final selected ORFs
transcripts.fasta.transdecoder.bed : bed-formatted file describing ORF positions, best for viewing using GenomeView or IGV.

The .cds file is the identified ORFS in the assembled transcriptome, which includes duplicates.

# 4. Removing Duplicate ORFs using CD-HIT
