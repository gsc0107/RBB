# RBB
Reciprocal Best Blast of GOS database
========

RBB is a program designed to perform a Reciprocal Best Blast against a given BLAST formatted database. This code has been optimized to work with the Global Ocean Sampling Expedition [GOS] (http://www.jcvi.org/cms/research/projects/gos/overview) and [Tara] (http://ocean-microbiome.embl.de/companion.html) datasets. 

INSTALLATION
------------  

RBB was written in Python 3. Python was installed using the [Anaconda] (https://docs.continuum.io/anaconda/install) free distribution.

RBB also requires [BioPython] (http://biopython.org/wiki/Documentation) to be installed.

RBB also requires the [BLAST+ suite] (https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE_TYPE=BlastDocs&DOC_TYPE=Download) to be installed and on your local PATH. NCBI BLAST+ tools can be found at: 

  * BLAST+: architecture and applications. C. Camancho et al. *BMC Bioinformatics* 2009, 10:421.DOI:http://dx.doi.org/10.1186/1471-2105-10-421

The RBB repository also includes a shell script for use on unix systems to create a database and conduct a BLAST. This author recommends not running a large BLAST on your local computer as the memory requirements can get quite large for these databases.

USAGE
-----

RBB has 4 main steps:

For examples, please see example walkthrough.md

#1
Make a BLAST database from a file containing fasta sequences using the makeblastdb command from the BLAST+ suite.

This requires **1 input**:
  * *input fasta file* -- This is a file containing all fasta sequences to be used to make the database to be queried against.

        makeblastdb -dbtype nucl -in *input_fasta_file* -parse_seqids -hash_index

Use BLASTn to search a query sequence against the newly created database. The outfmt is set to 6 as we will add headers in a future step.

This requires **3 inputs**:

  * *query sequence* -- This is the fasta formatted sequence you wish to blast against the database 
  * *new_db* -- This is the newly created BLAST formatted database created above.
  * *result_file* -- This will be the name of the file the BLAST results will print to.

        blastn -query *query sequence* -task blastn -db *new_db* - out *result_file* -outfmt 6

#2
Get fasta sequences back from top hits retured from Step 1. For this, use the **get_fastas.py** script.

This script has **3 Required** flags:
  * *--F* -- BLAST results from step 1. It is important that the format for this output is tab delimited without headers (option 6)
  * *--directory* -- Path to directory that you would like your output files sent to. Program will create a folder within called **RBB**
  * *--db* -- BLAST database that was created in step 1. This database should be the exact same as the one queried against in step 1.

Example of this code:

        python.exe get_fastas.py --F \..\..\BLASTRESULT.txt --d \..\..\RBB_outputs --db \..\..\blastdb.fna
 
This will provide **2 output files**:

  * *topnames.txt* -- File contains the names of the top BLAST results
  * *tophits.txt* -- File contains the sequences of the top hits 

This will also print the path to these files, and print the number of sequence objects in your database.

#3 
BLAST query sequence against another database (i.e. RefSeq)
Use the same BLASTn script from **step 1** but change the database.
The important thing here is that the output is in fasta format.

#4
Use RBH to compare the two outputs. This Repository is hosted on GitHub and can be found [here] (https://github.com/peterjc/galaxy_blast/tree/master/tools/blast_rbh).

####Citation

Per the creator/host of the RBH script, Peter Cock, Please cite the following paper: 
NCBI BLAST+ integrated into Galaxy. P.J.A. Cock, J.M. Chilton, B. Gruening, J.E. Johnson, N. Soranzo *GigaScience* 2015, 4:39 DOI: ttp://dx.doi.org/10.1186/s13742-015-0080-7

This code was optimized to run using Python 2.7, however, this author suggests upgrading the script for use in Python 3.5.
To do this use the 2to3.py script that is automatically installed with Anaconda. For more information please visit [here] (https://docs.python.org/2.7/library/2to3.html).

To convert script from Python version 2.7 to 3.5, use the following script:

         python.exe \..\Anaconda3\Tools\scripts\2to3.py -w python.py

#5 
Future Directions will be to include a fragment recruitment script to produce graphical representations of data.
