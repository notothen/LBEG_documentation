# Stacks_documentation

By Enora Geslain on 16/02/21
Reference: Rochette & Catchen (2017), Deriving genotypes from RAD-seq short-read data using Stacks (refered as **"the protocol"** below)

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

4. Do the step 7 and 8 of the protocol. In order to run the *process_radtags* command on the VSC you can use a script looking like ```Example/scripts/process_radtags_trem.pbs``` and you can launch it by writing the following command in your working directory:

```qsub ./scripts/process_radtags_trem.pbs```