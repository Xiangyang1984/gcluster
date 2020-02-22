![](https://github.com/Xiangyang1984/gcluster_tool-an-old-version/blob/master/test_data/figure_160_final.png)  

# Gcluster_v2.0.1
***
Gcluster is a simple-to-use tool for visualizing and comparing genome contexts for numerous genomes. It is freely available at http://www.microbialgenomic.com/Gcluster_tool.html and https://github.com/Xiangyang1984/Gcluster under an open source GPLv3 license. It is a stand-alone Perl application, which requires MCL, NCBI BLAST+ and several Perl Modules (e.g. GD, GD::SVG) to be installed before use.


[Installation](#installation)

[Usage](#usage)




[回到顶部](#readme)
## Installation
***
Gcluster is a Perl script which doesn't need compilation. But before running, Gcluster needs to pre-install several Perl modules and three extra programs. In addition, the paths of those three programs within Gcluster.pl and interested_gene_generation.pl must be set. There are two ways to install the Gcluster.

### **Option 1**: Installing the Gcluster via conda
We build a bioconda package for Gcluster, users are recommended to install the [conda](https://www.anaconda.com), then to install this package with the following command:

	$ conda install -c bioconda gcluster

Once installation finished, the absolute paths for mcl, blastp and makeblastdb are auto-configured well for Gcluster.pl and interested_gene_generation.pl, so uers should now be able to run Gcluster.

### **Option 2**: Installing the Gcluster from source code
Gcluster is available at https://github.com/xiangyang1984/Gcluster.git. Installation Gcluster can be accomplished by downloading the code and then following the steps below.
#### Step 1: Download source code
Download Gcluster，and put the Gcluster directory into your PATH with the following command：
```	
$ git clone https://github.com/xiangyang1984/Gcluster.git
$ export PATH=/path/to/Gcluster/:$PATH
```

#### Step 2: Perl modules installation
The Gcluster requires Perl as well as Perl modules including GD; GD::SVG, SVG; threads, File::Basename, FindBin, File::Spec, lib, Getopt::Long, Math::BigFloat, Storable, vars, Bio::SeqIO, Bio::Tree::NodeI, Bio::TreeIO.

These can be installed with cpan using:

	$ sudo cpan install GD GD::SVG SVG threads File::Basenamey FindBin lib Getopt::Long Math::BigFloat Storable vars Bio::SeqIO Bio::Tree::NodeI Bio::TreeIO


#### Step 3: Programs installation
Additional software dependencies for the pipeline are as follows:

* makeblastdb and blastp  
Both of them come from NCBI BLAST+, available at https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/

* mcl (Markov Clustering algorithm)  
This software is available at http://micans.org/mcl/

***Please set the absolute path for three programs within "Gcluster.pl", as in the following example:

	* *my $blastp        = "/usr/bin/blastp";*
	* *my $makeblastdb   = "/usr/bin/makeblastdb";*
	* *my $MCL           = "/usr/bin/mcl";*

***Please set the absolute path for three programs within "interested_gene_generation.pl", as in the following example:

	* *my $blastp        = "/usr/bin/blastp";*
	* *my $makeblastdb   = "/usr/bin/makeblastdb";*

### Test the Gcluster with example data
Once Gcluster installation finished, a small dataset in the **./test_data** directory can be used to test whether Gcluster (for **Gcluster.pl** and **interested_gene_generation.pl**) can run on your system (**Linux/MacOSX**) successfully or not using the **test.pl** script as below:

	$ perl ./test.pl
	
	Test-step1: Begin test Gcluster.pl...
	################################################################

	Tue Feb 18 13:16:12 2020: Gcluster.pl start...

	GenBank_extraction_percent: 11...22...33...44...55...66...77...88...100...done
	Step 1-1: Extract all predicted proteomic sequences of each genebank file in folder, and transformat protein sequence into a ID-sequence hash

	Step 1-2: Extract all feature table of each genebank file in folder

	Step 2: Generate a sub-TFT table file containing the gene information flanking the interested gene

	Step 3: Extract the interested gene related sub-proteomic sequences according the sub-TFT table files
	Blast_perform_percent: 11...22...33...44...55...66...77...88...100...done

	Step 4: Obtain the homologous gene clusters information

	Step 5(-PNG): Be going to create a size of 992.575x655 PNG image

	Step 6(-PNG): PNG format was chosen to map

	Step 7(-PNG): PNG Figure will be generated, please wait!!!

	Step 5(-SVG): Be going to create a size of 932.735x655 SVG image

	Step 6(-SVG): SVG format was chosen to map

	Step 7(-SVG): SVG Figure will be generated, please wait!!!
	Tue Feb 18 13:16:27 2020: Finished!
	################################################################
	Ok, Gcluster.pl works success!


	
	Test-step2: Begin test interested_gene_generation.pl...
	################################################################

	Tue Feb 18 13:16:27 2020: interested_gene_generation.pl start...

	Genbank_extraction: 1
	Genbank_extraction: 2
	Genbank_extraction: 3
	Genbank_extraction: 4
	Genbank_extraction: 5
	Genbank_extraction: 6
	Genbank_extraction: 7
	Genbank_extraction: 8
	Genbank_extraction: 9
	Blast_perform: 1
	Blast_perform: 2
	Blast_perform: 3
	Blast_perform: 4
	Blast_perform: 5
	Blast_perform: 6
	Blast_perform: 7
	Blast_perform: 8
	Blast_perform: 9
	Warning: [blastp] Examining 5 or more matches is recommended
	Warning: [blastp] Examining 5 or more matches is recommended
	Warning: [blastp] Examining 5 or more matches is recommended
	Warning: [blastp] Examining 5 or more matches is recommended
	Warning: [blastp] Examining 5 or more matches is recommended
	Warning: [blastp] Examining 5 or more matches is recommended
	Warning: [blastp] Examining 5 or more matches is recommended
	Warning: [blastp] Examining 5 or more matches is recommended
	Warning: [blastp] Examining 5 or more matches is recommended
	Tue Feb 18 13:17:01 2020: Finished!
	################################################################
	Ok, interested_gene_generation.pl works success!

The Warning happens because the user database has less than 5 sequences in it. Once all tests have passed then you are ready to start using the Gcluster.  

## Usage
***
The brief overview of running the Gcluster is as follows:

[Preperation of input data](#preperation of input data）

[Running Gcluster.pl](#running-Gcluster.pl）

[Preperation of input data](#preperation-of-input-data)

[Running Gcluster](#running-gcluster)

[Running Gcluster.pl](#running-gcluster.pl)

[Running Gcluster](#running-Gcluster)


[Customization of the figure](#customization-of-the-figure)

### Preperation of input data

### step 1: Preperation of input data
#### Genbank_file_directory (mandatory option)
Genbank_file_directory : A directory containing annotated genomes as Genbank format file (e.g. [test_data/gbk](https://github.com/Xiangyang1984/Gcluster/tree/master/test_data/gbk)). For large number of genomes, users are recommended to download using [Aspera](https://downloads.asperasoft.com/), a high-speed file transfer tool. 

#### interested_gene_file (mandatory option)
interested_gene_file: A list of the interested gene, in which each row contains a locus tag of the interested gene for individual genome. 

A gene of interest file generated looks like:

```
THIARS_RS06055	#Thiomonas_delicata_DSM_16361
THIX_RS16425	#Thiomonas_sp._X19
AX2_RS10405	#Achromobacter_xylosoxidans_NBRC_15126_ATCC_27061
KUC_RS10495	#Halomonas_boliviensis_LC1
KYC_RS14580	#Achromobacter_arsenitoxydans_SY8
...
```
*Users are recommended  to use interested_gene_generation.pl in Gcluster package to obtain a list of locus tag of interested genes based on a local blastp analysis using multiple threads:* 

```perl
$ perl interested_gene_generation.pl -dir test_data/Genbank_file_directory -db test_data/aioB.fasta
```
it would generate a output file named (e.g. [test_data/interested_gene_name.txt](https://github.com/Xiangyang1984/Gcluster/blob/master/test_data/interested_gene_name.txt)). In this file, the blast hits are listed for each genome per row; the best hit (top hit) was used as gene of interest for each genome, and the other none-top hits are also listed followed by "#".

aioB.fasta is a blast database file in FASTA format, which contains at least one protein sequence homologous to the gene of interest.
```
$ cat test_data/aioB.fasta
>YO5_RS17935
MSQFKDRVPLPPIDAQKTNMACHFCIVGCGYHVYKWPANKEGGRAPEQNALNVDFTRQVPPMQITMTPAMVNRIKDNDGSEHNIMIIPDKECEVNKGLSSTRGGQMASIMYSENTPIGERRLKVPMLYTGDDWIETTWQQSMDIYAGLTKRILDEDGPEQILFNLFDHGGAGGGFENTWATGKLIFSGIGTPMVRIHNRPAYNSECHATRDMGVGELNNSYEDAELADVLISIGNNPYESQTNYFLAHWVPNLQGATTGKKKERYPGESFAKAKIIFVDPRRTISVDISETVAGKDHVLHLAINPGTDTALFNGLLTYVVEKGWQDDEFIQNHTTGFDDTLASNKLSLSECSVITGITEDDLRKAAEWAYQPKESGHAPRTMHAYEKGVIWGNDNYRIQSSIVNLVLATHNVGRRGTGVVRMGGHQEGYVRPPYPGGRPAPYIDQEIIKNNGMMLTVWACNAFQTTVNAETYREAVKRRANIVNQALAKARGASTEQLINIIYDAVKNQGGLYLVDIDLYRTKFADSSHMLLPAAHPGEMNLTSMNGERRLRLSERFMDPPGIAKADCMIAADMANALKRLYEGEGNTEMAQRFSGFDWQSEEDSFNDGFRMAHEKEIDSQGGPTGHLATYERLRAAGTNGVQLPIKEYRDGKLIGTEILYSDNTFDTDDGKAHFQPSPWNGFPAVIEAQQKNHAFWINNGRTNHIWQSAYHDQHLSFRKGRFPMAPLEINPEDAAQLGIAAGDIVEIYNDYGATYAMAYPEPDIKRGQVFMMFGYPNGVQGDTVSEWTDRNVIPYYKGAWADIRKVGENEAYKHSVSFKRRRYS
```
#### phylogenetic_file (optional option) 
A Newick format phylogenetic tree is used by Gcluster to automatically accociate the genomes with their phylogeny. It should be noted that all nodes name in provided tree must completely match with the genbank files name of all genomes. Gcluster provides a perlscript ([script/extract_rRNA_dir.pl](https://github.com/Xiangyang1984/Gcluster/blob/master/script/extract_rRNA_dir.pl)) directory for batch extraction of 16S rRNA gene sequences, which can be used to build a 16S rRNA tree using software like [MEGA](https://www.megasoftware.net/) .
#### strain_reorder_file (optional option)
A two-column tab-delimited text file is used to sort genomes from up to down accoding to users requirement. Each row must consist of a strain name followed by the numerical order that is used for sorting genomes. It should be noted that all strains name must completely match with the genbank files name of all genomes. Gcluster needs a "strain_reorder_file" or a "phylogenetic_file", but not both at the same time. 

A example of the strain_reorder_file looks like:
|strain\_name | order|
|- | -|
|Thiomonas_sp.\_FB-Cd| 1|
|Thiomonas_sp.\_X19| 2|
|Thiomonas_delicata_DSM\_16361| 3|
|Thiomonas_intermedia_ATCC_15466| 4|
|Thiomonas_sp.\_B1| 5|
|Thiomonas_sp.\_ACO7| 6|
|Thiomonas_intermedia\_K12| 8|
|Thiomonas_arsenitoxydans\_3As| 7|
|Thiomonas_sp.\_ACO3| 9|

### step 2: Running Gcluster
### Running Gcluster

#### Example 1: a simple mode to visualize genome contexts for genomes

	$ perl Gcluster.pl -dir ./test_data/gbk -gene ./test_data/interested_gene_name.txt -o ./out_directory
		Runs Gcluster.pl using the input gbk files under ./test_data/gbk and interested_gene_name.txt as interested_gene_file.
		Places data in ./out_directory. Gets other parameters using default value.

#### Example 2: A Newick format tree file is used by Gcluster to automatically accociate the genomic context with their phylogeny

	$ perl Gcluster.pl -dir ./test_data/gbk -gene ./test_data/interested_gene_name.txt -tree ./test_data/16S_rRNA_tree.nwk -o out_directory
		Runs Gcluster.pl using the input gbk files under ./test_data/gbk, interested_gene_name.txt as interested_gene_file, and
		16S_rRNA_tree.nwk as phylogenetic_file. Places data in ./out_directory. Gets other parameters using default value.

#### Example 3: A two-column tab-delimited text file is used to sort genomes from up to down accoding to users requirement

	$ perl Gcluster.pl -dir ./test_data/gbk -gene ./test_data/interested_gene_name.txt -srf temp_strain_reorder_file -o out_directory -n 100
		Runs Gcluster.pl using the input gbk files under ./test_data/gbk, interested_gene_name.txt as interested_gene_file, and
		temp_strain_reorder_file as strain_reorder_file. Places data in ./out_directory. Gets other parameters using default
		value.

#### Example 4: use multiple threads, and set the number of the flanking gene of interest to show

	$ perl Gcluster.pl -dir ./test_data/gbk -gene ./test_data/interested_gene_name.txt -tree ./test_data/16S_rRNA_tree.nwk -o out_directory -m 4 -n 100
		Runs Gcluster.pl using the input gbk files under ./test_data/gbk, interested_gene_name.txt as interested_gene_file, and
		16S_rRNA_tree.nwk as phylogenetic_file. Places data in ./out_directory. 4 threads are used, 100 genes flanked gene of interest are set to show, and other parameters use default value.

#### Example 5: Jump to generate a collection of sub-TFT tables and perform homologous gene analysis (Default: F). Skips sequences extraction and TFT file generation. 

	$ perl Gcluster.pl -dir ./test_data/gbk -gene ./test_data/interested_gene_name.txt -o out_directory -sub_TFT T

#### Example 5: Jump to map generation. Generation of a collection of sub-TFT tables and homologous gene clusters has already been done. 

*perl Gcluster.pl -dir [genbank_file_directory] -gene [interested_gene_file] -o [Gcluster_output_directory] -map [start_at_map]*

	$ perl Gcluster.pl -dir ./test_data/gbk -gene ./test_data/interested_gene_name.txt -o out_directory -sub_TFT T
	
### Customization of the figure

	This parameter is very useful to customize the map quickly.After creat a figure for genomes, The user can customize figure using option "-map/--start_at_sub_map":

	* adjust the margins, the interval between two neighboring genomes, the text size, the gene length and width, the scale, the rotation angle of gene labels, the order of genome contexts. 

	* Revising the gene label
users can revise the gene label by directly edition of the locus_tag in sub_TFT file or all_orthomcl.out. 

for exzample:
Exzample for editing the locus_tag in sub_TFT file:

	3072251	3070956	CDS	THI_RS14520	molybdopterin molybdenumtransferase MoeA

_Revised to:_

	3072251	3070956	CDS	**moeA;**THI_RS14520	molybdopterin molybdenumtransferase MoeA
all genes homologous to THI_RS14520 will have gene lable "moeA" in output figure if option "--unification_lable" set to "T".

Exzample for editing the locus_tag in all_orthomcl.out file:

	homologous_gene_cluster_8(5 genes,5 taxa): ACO3_RS13890 ACO7_RS14160 THICB1_RS17625 THIX_RS16470 THI_RS14520

_revised to:_

	homologous_gene_cluster_8(5 genes,5 taxa): ACO3_RS13890 ACO7_RS14160 THICB1_RS17625 THIX_RS16470 **moeA;**THI_RS14520
all genes homologous to THI_RS14520 will have gene lable "moeA" in output figure if option "--unification_lable" set to "T".


* Using yourself homologous gene clusters
by instead of "all_orthomcl.out" created by Gcluster, users can supply homologous gene clusters from their own OrthoMCL output using the current OrthoMCL release which uses a SQL database for homologue grouping. it should be noted that the homologous gene clusters file must renamed with suffix "out". (e.g. *group.out*)

 perl Gcluster.pl -dir [genbank_file_directory] -gene [interested_gene_file] -tree [phylogenetic_file] -o [Gcluster_output_directory] -m [multiple_threads] -n [flanking_gene_number] [Options]
 

### Detailed explanations for ARGUMENTS in Gcluster.pl

```
--------------
    REQUIRED ARGUMENTS:
    ~~~~~~~~~~~~~~~~~~~
       -dir, --genbank_file_directory
             A Directory containing annotated genomes as Genbank format file. To avoid a mistake, genome names cannot use special character, such as space, equal. For large number of genomes, users are recommended to download using Aspera, a high-speed file transfer tool (https://downloads.asperasoft.com/). If enough rgb colors provided or not to color homologous genes, there are no limitation to the number of genomes and the number of genes flanking gene of interest for Gcluster.
       -gene, --interested_gene_file
             A list of the interested gene, in which each line contains a locus tag of the interested gene for individual genome. Users are recommended to use "interested_gene_generation.pl" in Gcluster package for generation this file. In this situation, user needs to provide a blast database file in FASTA format, which contains at least one protein sequence homologous to the gene of interest. To map genome contexts to a given phylogeny or to order the genome contexts using a "strain_reorder_file", only one gene of interest is allowed to provide for each genome.
             For example:
                 AX2_RS10405	#arsenite_oxidase_large_subunit;Achromobacter_xylosoxidans_NBRC_15126_ATCC_27061
                 KUC_RS10495	#arsenite_oxidase_large_subunit;Halomonas_boliviensis_LC1
                 KYC_RS14580	#arsenite_oxidase_large_subunit;Achromobacter_arsenitoxydans_SY8
                 ...
    OPTIONAL ARGUMENTS:
    ~~~~~~~~~~~~~~~~~~~
       -o, --Gcluster_output_directory
             An output directory holding all the generated files by Gcluster.pl. if this option is not set, Gcluster will create a directory named "Gcluster_workplace" in the bin directory from where Gcluster.pl was invoked.  
       -tree, --phylogenetic_file
             A Newick format tree file is used by Gcluster to automatically accociate the genomes with their phylogeny. Meanwhile,  Gcluster will output a file named "temp_strain_reorder_file", which contains the order information of genomes in tree from up to down. It should be noted that all nodes name in provided tree must completely match with the genbank files name of all genomes. 
             Gcluster provides a perlscript in "Gcluster/script" directory for batch extraction of 16S rRNA gene sequences, which can be used to build a 16S rRNA tree using software like MEGA (https://www.megasoftware.net/). 
       -topology, --show_tree_topology
             Display the tree topology, which is obtained from the tree file (Default: T).
       -branch, --show_tree_branch
             Draw tree using tree branch length, which is obtained from the tree file (Default: F).
       --x_step
             Draw tree using xstep instead of tree branch length (Default: 10).
       -bootstrap, --show_tree_bootstrap
             Display the tree bootstrap value, which is obtained from the tree file (Default: F).
       -srf, --strain_reorder_file
             A two-column tab-delimited text file is used to sort genomes from up to down accoding to users requirement. Each row must consist of a strain name followed by the numerical order that is used for sorting genomes. It should be noted that all strains name must completely match with the genbank files name of all genomes. Gcluster needs a "strain_reorder_file" or a "phylogenetic_file", but not both at the same time. 
             For example:
                 Achromobacter_xylosoxidans_NCTC10807	1
                 Achromobacter_xylosoxidans_NBRC_15126_ATCC_27061	2
                 Achromobacter_xylosoxidans_NCTC10808	9
                 Achromobacter_sp._2789STDY5608623	5
                 Achromobacter_sp._2789STDY5608621	4
                 Alcaligenes_faecalis_subsp._faecalis_NCIB_8687	10
                 Achromobacter_xylosoxidans_DPB_1	7
                 Achromobacter_xylosoxidans_B_1	8
                 Achromobacter_piechaudii_HLE	3
                 Achromobacter_marplatensis_B2	6
                 ...	...
       -n, --flanking_gene_number
             Number of genes flanking gene of interest are set to show. If enough rgb colors provided or not to color homologous genes, there are no limitation to the number of genomes and the number of genes flanking gene of interest (Default: 10).
       -color_f, --gene_color_filled
             Color was used to fill homologous gene clusters or gene families of interest (Default: T), if choose F, all of the genes were filled with the color customized by "gene_no_color_filled" parameter).
       -pso, --percent_strain_homologouscluster_color
             Only color certain homologous gene clusters, in which the holding number of different genomes exceeds the threshold number (Default: 0). This is measured using (X/Y)*100. In this formula, X denotes the number of different genomes in a set of homologous gene cluster, and Y denotes the total number of genomes. This parameter is useful when no enough rgb colors are provided in colors_configure_file under color_configure direcoty. Users could to reduce the number of colors used by setting a high value.  
       -c_color_b, --cds_color_border
             To color the border of the CDS genes (Default: black), users can choose from blue, black, red, white, gray, dgray.
       -p_color_b, --pseudo_gene_color_border
             To color the border of the Pseudo genes (Default: dgray), users can choose from blue, black, red, white, gray, dgray.
       -r_color_b, --RNA_gene_color_border
             To color the border of the RNA (tRNA, rRNA) genes (Default: red), users can choose from blue, black, red, white, gray, dgray.
       -no_color_f, --gene_no_color_filled
             To fill uniqe genes (including RNA genes), pseudo genes, and homologous gene clusters not meeting the criteria set by "percent_strain_homologouscluster_color" parameter with a single color (Default: white), users can choose from blue, black, red, white, gray, dgray.
       -l, --arrow_relative_Length    
             Set the relative length of the gene arrow (Default: 4).
       -w, --arrow_relative_Height
             Set the relative Height of the gene arrow (Default: 6).
       -scale, --figure_Scale_up_multiple
             Adjust gene length through zooming (Default: 0.5).
       -s_Y, --strain_name_shift_Y
             Set the offset along Y-axis for strain names (Default: 0).
       -g_Y, --gene_label_shift_Y
             Set the offset along Y-axis for gene labels (Default: 2).
       -dis, --distance_between_two_genomes
             Set the distance between two genome contexts in Y-axis (Default: 70).
       -up, --up_shift
             Set the top margin of image in pixels (Default: 10).
       -down, --down_shift
             Set the bottom margin of image in pixels (Default: 20).
       -left, --left_shift
             Set the left margin of image in pixels (Default: 10).
       -right, --right_shift
             Set the right margin of image in pixels (Default: 20).
       -label, --show_label
             Display the gene label (gene Locus Tag or genename) (Default: T).
       -ul, --unification_label
             Unify gene lable for homologous gene cluster (Default: T). Among a set of homologous gene cluster, if a gene is annotated with a name X, all other genes will be labeled with X.
       -family, --font_family
             Set font family for the genome name and the gene label, e.g. Times New Roman, Arial, Verdana and so on (Default: Times New Roman). Users are suggested to choose font family listed in metrcis module, or causing a miscalculation of string width for genome name in SVG-format map.
       -style, --font_style
             Set font style for the genome name and the gene label, e.g. Normal, Bold, Italic (Default: Normal).
       -size, --label_font_size
             Set font size for gene label (Default: 6).
       -color, --label_font_color
             Set font color for gene label (Default: dgray).
       -i_color, --interested_gene_label_font_color
             Customize gene label color for gene of interest (Default: red), users can choose from blue, black, red, white, gray, dgray.
       -r, --rotate_gene_label (Default: 30)
             Rotate the angle of the gene label, e.g. 30, 45, 135 and so on.
       --strain_name_font_size
             Set font size for genome name (Default: 12).   
       --Strain_name_font_color
             set font color for genome name (Default: black). Users can choose from blue, black, red, white, gray, dgray.
       -Bst, --homologous_gene_cutoff
             Array to set blast parse cutoff: E-value, Identify, Coverage, Match_length (Default: E-value=1-e5, Identify=0, Coverage=50%, Match_length=0).
       -m, --multiple_threads
             Numbers of thread to use (Default: 1).
       -SVG, --SVG_image
             Create SVG format figure (Default: T).
       -PNG, --PNG_image
             Create PNG format figure (Default: T).
       -sub_TFT, --start_at_sub_TFT (Default: F)
             Jump to generate a collection of sub-TFT tables and perform homologous gene analysis (Default: F). Skips sequences extraction and TFT file generation.  
       -map, --start_at_map
             Jump to map generation (Default: F). Generation of a collection of sub-TFT tables and homologous gene clusters has already been done. This parameter is very useful to customize the map quickly. It should be noted that there's no sense to reset "flanking_gene_number" parameter if this parameter set to "T".
             Importantly, at this step, users can revise the gene label by directly edition of the locus_tag in sub_TFT file or all_orthomcl.out. In sub_TFT files and all_orthomcl.out file, there are two forms of gene locus tag, (1) "Locus_Tag", in this case, no genename is defined for a gene; (2) "GeneName;Locus_Tag", in this case, genename is given for a gene. For the first form, user can revise gene label by addition of a genename followed by a semicolon in the front of the Locus_Tag. For the second form, user can revise gene label by modification of the genename.
       -h, --help
             Show this message.

```
