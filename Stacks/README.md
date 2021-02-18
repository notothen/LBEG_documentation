# Stacks_documentation

### By Enora Geslain on 16/02/21
### Reference: Rochette & Catchen (2017), Deriving genotypes from RAD-seq short-read data using Stacks (refered as **"the protocol"** below)

## Preparing the working directory and the data
1. Do the steps 1 and 2 of the protocol. In addition to the requested directories, create one named ```scripts```. At the end you should have the same tree structure as the one of the ```Example/``` folder with your raw data looking like the ones in the ```Example/raw/``` folder.

2. Do the step 3 of the protocol. Depending on the technique you used in order to sequenced your samples you can have indexes in addition to your barcodes. If it is the case you should have barcodes files looking like this (without the first line):
```python
Barcode	Index	Sample_name
GCATG	ATCACG	T_eul_PS82_313
AACCA	ATCACG	T_eul_PS82_314
CGATC	ATCACG	T_eul_PS82_315
TCGAT	ATCACG	T_eul_PS82_317
TGCAT	ATCACG	T_sco_PS96_229
CAACC	ATCACG	T_loe_PS96_213
``` 
You can see a complete example in the ```Example/info/``` repertory

3. Do the step 4 of the protocol. You will find an example of a popmap file in ```Example/info/```

## Demultiplexing and filtering (trimming) the reads
4. Do the step 7 and 8 of the protocol. In order to run the *process_radtags* command on the VSC you can use a script looking like ```Example/scripts/process_radtags_trem.pbs``` and you can launch it by writing the following command in your working directory:

```bash
qsub ./scripts/process_radtags_trem.pbs
```
(you will need to run this command as many times as you have libraries)

/!\ **Depending of the number of samples in your libraries this step can be long and heavy thus you might need to change the number of nodes and cores requested as well as the walltime and the memory indicated in the lines 3, 4 and 5 of the script**

5. As suggested in the step 9 of the protocol you can check the proportion of retained reads (and the number of retained reads per sample) in the log file in order to see if some samples should be discarded (because of too low or too high retained reads). You will find it in the ```cleaned/``` directory along with the fastq files of your cleaned reads (= demultiplexed and filtered) like in the ```Example/cleaned/``` folder.

6. **Warning**: If your working with paired-end sequencing data **don't do the step 11** of the protocol. It is no longer necessary because, since the writing of the protocol, an option has been added to specify that the data are paired.

## Working on a subset of samples for parameter testing
7. Do the step 14 of the protocol. I suggest you choose a subset of individuals representative of your entire dataset (like individuals from different locations) and take the ones with a high number of retained reads. You can find an example here: ```Example/info/popmap.test_samples.tsv```

8. Do the steps (i) to (iv) from the step 15 of the protocol (warning: **A** is for **de novo** analysis and **B** for **reference-based** analysis). You will find the scripts in order to run the 9 denovo_map command and the 9 populations command in ```Example/scripts/``` (You can launch them with the same command as the step 4 with ```qsub``` before the name of the script).

(steps (v) and (vi) are not mandatory)

9. Do the steps (vii) and (viii) from the step 15 of the protocol.
/!\ Warning: the explainations in the protocol are longer working. You need to follow this steps instead:
- copy and paste the scripts present in the folder ```/staging/leuven/stg_00026/Useful_scripts/Stacks``` in your own ```scripts/``` folder
- open the script ```Make_plot_number_loci_shared.sh``` and follow the instructions at the beginning of the script (changing the path for the input data and the path to the script ```plot_R_graphs_number_loci_shared.r```)
- open the script ```Make_plot_snps_per_locus.sh``` and do the same manipulation as above
- launch each ```Make_plot_...``` script with the following command:
```bash
bash script_name
```
- look at the 2 graphs generated and choose the better value of the parameters for your dataset

## Running Stacks on the full dataset
10. Do the steps 16 and 17-A-(i) of the protocol.

11. For the rest of the steps (17-A-(ii) to (vii)) you can choose to follow the protocol or to run only one command for the integrality of the steps. Indeed, in the protocol the authors use the commands in a decomposed way: ustacks, cstacks and sstacks but it's the same as running the denovo_map command (it's just easier to parallelize the jobs). If you want to run directly the denovo_map command you can do a script as the one called ```denovo_map_all_trem.pbs``` in the ```Example/scripts/``` folder.

(You can do steps 18 to 20 of the protocol but they are not necessary)

## Filtering genotypes and exporting the data
12. Do the step 21 of the protocol. You can add a lot of different options in order to filter the genotypes or to have different output formats. You will find all these options and their descriptions at this link: https://catchenlab.life.illinois.edu/stacks/comp/populations.php
Some interesting options are:
- --min-mac
- --min-maf
- --write-single-snp
And, of course, choose carefully the file output options in order to have the files you need for your downstream analysis.
You can find a script example in the ```Example/scripts/``` folder, it is named ```populations_all_trem.pbs```