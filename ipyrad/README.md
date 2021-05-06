# ipyrad_documentation

### By Enora Geslain on 29/04/21
### Reference: https://ipyrad.readthedocs.io and Eaton DAR & Overcast I. "ipyrad: Interactive assembly and analysis of RADseq datasets." Bioinformatics (2020).
### You can find the script and data examples at this link: https://github.com/Enorya/LBEG_documentation/tree/main/ipyrad

# ipyrad analysis

## Preparing the working directory and the data
1. Create a new directory with a meaningful name (like "Trematomus_analysis_ipyrad") in order to put all your analysis with ipyrad in it.
2. Move in that new directory and create 4 folders: "scripts", "raw", "info" and a folder with a meaningful name in order to put your results (like "trematomus_results"). 
3. If you are working with paired-end data make sure that the name of your raw files have the exact mentions `_R1_` and `_R2_` to differentiate forward and reverse reads (You will see examples in the folder `Example/raw/`). Otherwise the software is not going to differentiate the reads.
4. Depending on the barcoding technique you used you will need to create either 1 file or more files containing the barcodes. Look at the documentation in this [location](https://ipyrad.readthedocs.io/en/latest/5-demultiplexing.html#barcodes-file) to find out how to create your barcodes file(s). When they will be created put them in the `info` folder.
For the example dataset you will see that there is 8 different libraries and in each library the same set of double barcodes was used. Because of that the demultiplexing was done in 2 steps for each library. First, the reads were demultiplexed using the inline barcodes (8 different) and then, they were demultiplexed again using, this time, the i7 index present in the line preceding the read's sequence (12 differents). 

## Connecting to the miniconda3 environment (to be able to use ipyrad)
1. `source activate /staging/leuven/stg_00026/Softwares/miniconda3/`

## Creating the parameter file (params file) and change it
1. Use the command `ipyrad -n meaningful_name` (this name will be used to name your outputs).
2. Open your new params file (it will have a name such as "params-meaningful_name.txt" and look like the one in the `Example` folder) with your favourite text editor (nano for example).
3. You now need to change the parameters at your convenience. Some parameters are mandatory, [here](https://ipyrad.readthedocs.io/en/latest/7-outline.html#seven-steps) you will see the mandatory parameters for each step of the pipeline marked with an asterisk.
For example, we want to do a *De novo* analysis so we are going to put "denovo" to the fifth parameter line.
At the beginning it can give the impression that there is too much parameters but don't panic! You can just let the default parameters for your first run and you will have the opportunity to do multiple other runs changing the parameters values.

## Running the analysis
You have 2 possibilities:
1. You can run the complete pipeline using the command `ipyrad -p params-meaningful_name.txt -s 1234567 -c 108 --MPI`
2. You can run the pipeline step by step giving the number of the step that you want to launch `ipyrad -p params-meaningful_name.txt -s 1 -c 108 --MPI`
You can notice the options `--MPI` and `-c 108`, those first option indicate that you are going to parallelize your job (doing different steps at the same moment) and the second give the number of cores you will use in order to parallelize this job (it should correspond to the number of cores you have allocated for your job on the cluster)
You will find a script example in the folder `Example/scripts/` 

*I would recommend to do first the step 1 and 2, do some statistics on your samples in order to exclude some if needed and then go for the rest of the steps together*

# Branching your analysis

## Branching in order to redo some analysis changing some parameters
1. Using the command `ipyrad -p params-meaningful_name.txt -b new_meaningful_name`
2. Change some parameters in your new params file
3. Run the pipeline again from any step you want but add --force at the end of the command like this: `ipyrad -p params-new_meaningful_name.txt -s 567 -c 108 --MPI --force`

## Branching in order to redo some analysis excluding some individuals
You have different possibilities depending on the number of samples you want to keep:
1. You want to keep a very few number of samples, give the name of the samples you want to keep when creating the new params file, like this: `ipyrad -p params-meaningful_name.txt -b new_meaningful_name 1A0 1B0 1C0` (here 1A0, 1B0 and 1C0 are three different samples)
2. You want to exclude a very few number of samples, give the name of the samples you want to exclude and add a dash when creating the new params file, like this: `ipyrad -p params-meaningful_name.txt -b new_meaningful_name - 1A0 1B0 1C0`
3. You want to keep a very large number of samples, give a text file containing a list of the samples you want to keep which should look like this (you can find an example in the folder `Example/info/`):
```bash
1A0
1B0
1C0
```
Then you can create a new params file with the branching method like this: `ipyrad -p params-meaningful_name.txt -b new_meaningful_name list_of_samples.txt`

**/!\ Warning**: if you didn't change the parameter "project dir" the results of your new analysis will be in the a new directory (named after your assembly name like "new_meaningful_name" here) inside your project directory

# 