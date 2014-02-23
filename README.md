get_homologues_to_pathway-tools
===============================

Python scripts to convert the .faa output from the software package "get_homologues" back to .gbk file necessary for analyses using Pathway-tools

Get_homologues (http://www.ncbi.nlm.nih.gov/pubmed/24096415) outputs a list file containing all homologous genes shared amongst a set of genomes (input as .gbk files annotated by RAST). The software provides a variety of ad-hoc cutoffs for considering aspects of the pangenome, namely the "core" set of genes (shared between ALL genomes), the shell (shared between most genomes), etc. etc., all the way to the "cloud," which are genes possessed by only one or two genomes in your group. It is a very useful tool on its own, however, I wanted to take the output "core" or "cloud" and predict which biochemical pathways may be present in these levels of similarity. 

To do this, the output generated by get_homologues needs to be reformatted into a .gbk file suitable for import into Pathway-tools (http://bioinformatics.ai.sri.com/ptools/). So, the first script in this repository compiles all of the .faa entries corresponding to genes listed in the .list file generated by get_homologues: "parse_pangenome_matrix.pl." The next script then takes all the information contained in the .faa headers and the AA sequence AND the original .gbk files (in order to pull out the DNA sequence) and creates an amalgamated .gbk file suitable for input into pathway-tools. Pathway-tools uses EC numbers and gene name matches to predict the broad metabolic profile of a genome (or this case, pangenome) and then uses its "hole-filler" software to identify genes missing from certain pathways (which I believe is done by BLASTing for specific enzymes or some other search algorithm). The reason I mention this detail, is b/c the DNA sequence is critical for hole-filling, and is sadly stripped out during get_homologues searching. The major benefit of this script is to pull the nucleic acid sequence back in in the appropriate order (i.e. can handle contigs) and reconstruct a fused pangenome .gbk file.

Overview of Get_homologues to Pathway-tools
===============================
Input RAST annotated .gbk files -> get_homologues (specifically: "parse_pangenome_matrix.pl") -> SCRIPT1: "compile_pangenome_output.py" ->  SCRIPT2: reconstruct_gbk_from_get_homologue_output.py -> To Pathway-tools.

Usage:
===============================
1) ./compile_pangenome_output.py pangenome_matrix_t0__*.list.txt

2) Take the output from 1 and move to the folder containing all of the original .gbk files used for input into get_homologues.

3) ./reconstruct_gbk_from_get_homologue_output.py output.faa

(Please note: this has only been tested with RAST annotated .gbk files. The script should be easily modifiable to handle the output from get_homologues resulting from other .gbk file types (essentially all the date from your original .gbk is parsed into the .faa header and delimited with a "|"). 

(Also note: I am novice-intermediate at programming. There are likely frustrating aspects of reading this code and many inefficiencies. Yet, the script was able to process a .faa file resulting from 32 genomes, containing 65000 entries in 12min). 




