# Stacks_documentation

### By Enora Geslain on 16/02/21
### Reference: Rochette & Catchen (2017), Deriving genotypes from RAD-seq short-read data using Stacks (refered as **"the protocol"** below)

## Preparing the working directory and the data
1. Do the steps 1 and 2 of the protocol. In addition to the requested directories, create one named ```scripts```. At the end you should have the same tree structure as the one of the ```Example/``` folder with your raw data looking like the ones in the ```Example/raw/``` folder.

2. Do the step 3 of the protocol. Depending on the technique you used in order to sequenced your samples you can have indexes in addition to your barcodes. If it is the case you should have barcodes files looking like this (without the first line):
```Barcode	Index	Sample_name
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

```qsub ./scripts/process_radtags_trem.pbs```
(you will need to run this command as many times as you have libraries)

/!\ **Depending of the number of samples in your libraries this step can be long and heavy thus you might need to change the number of nodes and cores requested as well as the walltime and the memory indicated in the lines 3, 4 and 5 of the script**

5. As suggested in the step 9 of the protocol you can check the proportion of retained reads (and the number of retained reads per sample) in the log file in order to see if some samples should be discarded (because of too low or too high retained reads). You will find it in the ```cleaned/``` directory along with the fastq files of your cleaned reads (= demultiplexed and filtered) like in the ```Example/cleaned/``` folder.

6. **Warning**: If your working with paired-end sequencing data **don't do the step 11** of the protocol. It is no longer necessary because, since the writing of the protocol, an option has been added to specify that the data are paired.

### Working on a subset of samples for parameter testing
7. Do the steps 
