# ipyrad_documentation

### By Enora Geslain on 29/04/21
### Reference: https://ipyrad.readthedocs.io and Eaton DAR & Overcast I. "ipyrad: Interactive assembly and analysis of RADseq datasets." Bioinformatics (2020).
### You can find the script and data examples at this link: https://github.com/Enorya/LBEG_documentation/tree/main/ipyrad

# *De novo* analysis

## Preparing the working directory and the data
1. Create a new directory with a meaningful name (like "Trematomus_analysis_ipyrad") in order to put all your analysis with ipyrad in it.
2. Move in that new directory and create 4 folders: "scripts", "raw", "info" and a folder with a meaningful name in order to put your results (like "trematomus_results"). 
3. If you are working with paired-end data make sure that the name of your raw files have the exact mentions `_R1_` and `_R2_` to differentiate forward and reverse reads (You will see examples in the folder `Example/raw/`). Otherwise the software is not going to differentiate the reads.
3. If you are working with paired-end data make sure that the name of your raw files have the exact mentions `_R1_` and `_R2_` to differentiate forward and reverse reads (You will see examples in the folder `Example/raw/`). Otherwise the software is not going to differentiate the reads.
4. Depending on the barcoding technique you used you will need to create either 1 file or more files containing the barcodes. Look at the documentation in this location https://ipyrad.readthedocs.io/en/latest/5-demultiplexing.html#barcodes-file to find out how to create your barcodes file(s). When they will be created put them in the `info` folder.
For the example dataset you will see that there is 8 different libraries and in each library the same set of double barcodes was used. Because of that the demultiplexing was done in 2 steps for each library. First, the reads were demultiplexed using the inline barcodes (8 different) and then, they were demultiplexed again using, this time, the i7 index present in the line preceding the read's sequence (12 differents). 

## Connecting to the miniconda3 environment (to be able to use ipyrad)
1. `source activate /staging/leuven/stg_00026/Softwares/miniconda3/`

## Creating the parameter file (params file) and change it
1. Use the command `ipyrad -n meaningful_name` (this name will be used to name your outputs).
2. Open your new params file (it will have a name such as "params-meaningful_name.txt" and look like the one in the `Example` folder) with your favourite text editor (nano for example).
3. You now need to change the parameters at your convenience. Some parameters are mandatory, [here](https://ipyrad.readthedocs.io/en/latest/7-outline.html#seven-steps) you will see the mandatory parameters for each step of the pipeline marked with an asterisk.
