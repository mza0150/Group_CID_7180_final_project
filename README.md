# Group_CID_7180
# 1  install packages
1.  Set up running environment and install software:
(1) Modify shebang line of setup.py to make it run correctly
(2) $Python3 setup.py build    to build environment 
(3) $python3 setup.py install  to install software needed
(4) $python3 setup.py - - help  can help you run  
2. if not install successfully you can try pip install Run the below from within your $HOME directory on the ASC!!

Use python3 to install pip (a python package/library manager) in your user account
using a python script copied from the biobootcamp directory

cp ~/biobootcamp/get-pip.py . && python3 get-pip.py --user

Confirm that pip is running in your user account and its version is 20.0.2

.local/bin/pip --version

Now we need to add the ~/.local/bin to our $PATH, so....

nano ~/.bashrc.local

And add the following to the bottom of the file (what's the reason for the below?)

export PATH=~/.local/bin:$PATH

Now log out and back into your supercomputer account to pick up the new $PATH. To make sure this is the case,

echo $PATH

And be sure the new ~/.local/bin is at the beginning of your $PATH

Now time to use pip to install packages like:

pip install biopython
pip install reportlab
# 2 Download genome sequence
Use Scripts in Part_1

Welcome to our pipeline for analysis of COVID-19 sequences.

We are starting with the file COVID_Seqs.csv as a reference for accession numbers.

The first step will be to sort the accessions by country for comparison.
SortCut_filter.sh will copy the location and accession data to a separate file,
then copy out accessions from different countries.

--------------outline version-----------
Step 1
Start with COVID_Seqs.csv
Run SortCut_Filter.sh to separate seqs into lists of “location; accession” ($place_sorted.txt)
--also splits up USA into sets of 100 randomized (USA_randsplit_aa)

ls *_sorted* for reference

Run Lines_to_commas.sh to convert location/accession lists to comma-separated lists for download use
Ls DL* for reference

Run Collect_seqs.sh and input name of your DL file to get fasta
Ls -thor *.fasta to make sure it went through

If size seems too big or small, check number of samples with:
cat Other_seqs.fasta | grep ">" | wc –l

Step2: Or download  all complete genomes with fasta format and genbank data NC_045512.2.gb from NCBI:
https://www.ncbi.nlm.nih.gov/labs/virus/vssi/#/virus?SeqType_s=Nucleotide&VirusLineage_ss=SARS-CoV-2,%20taxid:2697049&SLen_i=29000%20TO%2030000

# 3 data preparation
replace non ATCG by N.
sed -e '/^[^>]/s/[^ATGCatgc]/N/g' example_sequence.fasta > example_sequence_processed.fasta

# 4 Get genome sequences general data
You can run with command line:
python3 CID_sequence_description.py example_sequence_processed.fasta
# 5 Output longest orf fasta file
python3 CID_longest_peptide.py example_sequence_processed.fasta
 
# 6 Multiple alignment
You can run with command line:
./multiple_alignment.sh
or run with script 
run_script multiple_alignment.sh 

# 7 Construct COVID 19 genome structure using NC_045512.2.gb and Multiple alignment results

python3 genome_structure.py

# 8 PCA for Cluster analysis based on genome sequences general data using R

R code in PCA.R

# 9 phylogentic tree using NCBI, ETE Toolkit, and Clustalw2
On command line type 'clustalw2'

Selections appear for alignments, creating phylogenetic trees

Phylogenetic Tree is selected

Next, select .fasta or .aln files to upload

After, select type of output file (.ph)

Select type of phylogenetic tree (Neighbor joining)

Input outfile name and run 

Take outfile and view on NCBI or ETE Toolkit

*After ETEToolkit has been debugged, one should be able to use the script to create a .PNG file of the Tree using this on the command line
