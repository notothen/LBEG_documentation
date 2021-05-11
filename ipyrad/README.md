# ipyrad_documentation

### By Enora Geslain on 29/04/21
### Reference: https://ipyrad.readthedocs.io and Eaton DAR & Overcast I. "ipyrad: Interactive assembly and analysis of RADseq datasets." Bioinformatics (2020).
### You can find the script and data examples at this link: https://github.com/Enorya/LBEG_documentation/tree/main/ipyrad

# ipyrad analysis

## Preparing the working directory and the data
1. Create a new directory with a meaningful name (like "Trematomus_analysis_ipyrad") in order to put all your analysis with ipyrad in it.
2. Move in that new directory and create 4 folders: "scripts", "raw", "info" and a folder with a meaningful name in order to put your results (like "trematomus_results"). 
3. If you are working with paired-end data make sure that the name of your raw files have the exact mentions `_R1_` and `_R2_` to differentiate forward and reverse reads (You will see examples in the folder `Example/raw/`). Otherwise the software is not going to differentiate the reads.
4. Depending on the barcoding technique you used you will need to create either 1 file or more files containing the barcodes. Look at the documentation in this [location](https://ipyrad.readthedocs.io/en/latest/5-demultiplexing.html#barcodes-file) to find out how to create your barcodes file(s). When they will be created put them in the `info` folder. Take a look at the [demultiplexing with i7](https://github.com/Enorya/LBEG_documentation/tree/main/ipyrad#demultiplexing-with-i7) part of this document if you need to use this technique.
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

# Demultiplexing with i7

## Open jupyter-notebook
1. Connect to the VSC using NoMachine
2. Open a terminal and begin an interactive session like this: `qsub -I -l nodes=1:ppn=10 -l walltime=3:00:00 -l pmem=10gb -A llp_lbeg`
3. Move to the directory you want to work in: `cd /path/to/your/folder`
4. Activate the conda environment in order to run ipyrad and jupyter-notebook like this: `conda activate /staging/leuven/stg_00026/Softwares/miniconda3`
5. Launch jupyter-notebook with: `jupyter-notebook --ip name_of_the_core --port your_vsc_number` (when looking at your terminal you will see something like `vsc34088@r22i27n08` where "name_of_the_core" corresponds to "r22i27n08" and "your_vsc_number" corresponds to "34088")
6. Some text will appear, that's normal, don't panic! At the end you will see something like: 
```bash
To access the notebook, open this file in a browser:
        file:///vsc-hard-mounts/leuven-user/340/vsc34088/.local/share/jupyter/runtime/nbserver-4368-open.html
```
copy the link and paste it in chrome browser, this will open the jupyter-notebook.
7. You will see the different folders of files of your working directory and click on the button called "IPython Clusters" in the top left of your screen. In the column "# of engines" put the same number of cores you asked for your interactive session and click "Start"
8. Move back to "Files" and create a new "Python 3 notebook" clicking on the button "New" in the top roght of the screen.
You are now in a jupyter-notebook file!

## Demultiplex your i7 index
1. Import the different modules needed in the first cell like this:
```python
## imports
import ipyrad as ip
import ipyrad.analysis as ipa
import ipyparallel as ipp
import toyplot
```
**Tip**: To execute the cell rapidly you can press *Ctrl + Enter*

2. You can add a new cell and verify on how many cores you are running with this:
```python
ipyclient = ipp.Client()
print("Connected to {} cores".format(len(ipyclient)))
```
3. Add a new cell and do the different steps for the demultiplexing: setting up the parameters (assembly name, project dir, path of the fastq files, path of the barcode file, datatype), setting hackers params to demultiplex on i7 index, run the demultiplexing.
```python
# demux trem i7s to plate1 and plate2
all_trem = ip.Assembly("trem_lib3_inl3_i7")
all_trem.params.project_dir = "/staging/leuven/stg_00026/enora/trem_ipyrad/new_trem/demux_i7"
all_trem.params.raw_fastq_path = "/staging/leuven/stg_00026/enora/trem_ipyrad/new_trem/new_trem_lib3_fastqs/inline3_R*_.fastq.gz"
all_trem.params.barcodes_path = "/staging/leuven/stg_00026/enora/trem_ipyrad/info/trem_i7_barcodes_inline3_lib3.txt"
all_trem.params.datatype = "pairddrad"

# important: set hackers params to demux on i7
all_trem.hackersonly.demultiplex_on_i7_tags = True
all_trem.hackersonly.merge_technical_replicates = True

# run step 1 to demux
all_trem.run("1", ipyclient=ipyclient, force=True)
```
You will find an example file in the folder `Example/scripts/` (**Warning**: open it using jupyter-notebook otherwise it will not be readable)

# Branching your analysis

## Branching in order to redo some analysis changing some parameters
1. Using the command `ipyrad -p params-meaningful_name.txt -b new_meaningful_name`
2. Change some parameters in your new params file
3. Run the pipeline again from any step you want but add --force at the end of the command like this: `ipyrad -p params-new_meaningful_name.txt -s 567 -c 108 --MPI --force`

## Branching in order to redo some analysis selectioning/excluding some individuals
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

# ipyrad analysis toolkit

You can use the analysis toolkit of ipyrad with any kind of VCF files but in order to use them you first need to convert those files into hdf5 format.

## Converting VCF files into hdf5 files
1. Open a jupyter-notebook in your working directory following the steps [here](https://github.com/Enorya/LBEG_documentation/tree/main/ipyrad#open-jupyter-notebook)
2. Import the different modules needed in the first cell:
```python
import ipyrad.analysis as ipa
import pandas as pd
```
3.Compress your VCF file using those commands in a second cell:
```python
%%bash

# compress the VCF file if not already done (creates .vcf.gz)
bgzip data.vcf

# recompress the final file (create .vcf.gz)
bgzip data.cleaned.vcf
```
4. Convert your file to hdf5 in a third cell:
```python
# init a conversion tool
converter = ipa.vcf_to_hdf5(
    name="meaningful_name",
    data="/path/to/your/file.vcf.gz",
    ld_block_size=20000,
)

# run the converter
converter.run()
```
You will find an example file in the folder `Example/ipyrad_analysis_toolkit/conversion/` (**Warning**: open it using jupyter-notebook otherwise it will not be readable)

## PCA analysis
1. Open a jupyter-notebook in your working directory following the steps [here](https://github.com/Enorya/LBEG_documentation/tree/main/ipyrad#open-jupyter-notebook). If your data are in VCF format you need to convert them into hdf5 using those [explanations](https://github.com/Enorya/LBEG_documentation/tree/main/ipyrad#converting-vcf-files-into-hdf5-files)
2. Import the different modules needed in the first cell:
```python
import ipyrad.analysis as ipa
import pandas as pd
import toyplot
import toyplot.pdf
```
3. In the second cell, setup the path to your vcf file converted to hdf5 format:
```python
# the path to your .snps.hdf5 database file
data = "/staging/leuven/stg_00026/enora/trem_ipyrad/new_trem/trem_default_popfile_outfiles/trem_default_popfile.snps.hdf5"
```
4. If you want to separate your samples into different populations, you will need to group your individuals in the third cell like this:
```python
# group individuals into populations
    imap = {
        "bor": ["P_bor_JRI_01", "P_bor_KGI_03"],
	"ber": ["T_ber_JRI_02", "T_ber_JRI_03", "T_ber_JRI_04", "T_ber_JRI_05", "T_ber_JRI_06_RS", "T_ber_JRI_06_b_RS"],
	"eul": ["Standart_2_lib1", "Standart_2_lib2", "Standart_2_lib3", "Standart_2_lib4", "Standart_2_lib5"],
	"tok": ["T_loe_PS96_369", "T_loe_PS96_370", "T_loe_PS96_371", "T_tok_PS96_173", "T_tok_PS96_345"],
    }
```
You also have the opportunity to require a minimum percentage of samples that should have data in each group like this:
```python
# require that 50% of samples have data in each group
minmap = {i: 0.5 for i in imap}
```
5. You now need to launch the PCA analysis like this:
```python
# init pca object with input data and (optional) parameter options
    pca = ipa.pca(
        data=data,
        imap=imap,
        minmap=minmap,
        mincov=0.75,
        impute_method="sample",
    )
```
The mincov parameter is optional you can have more informations on it [here](https://ipyrad.readthedocs.io/en/stable/API-analysis/cookbook-pca.html#Enter-data-file-and-params). You can also change the impute_method, there is three methods described at this [link](https://ipyrad.readthedocs.io/en/stable/API-analysis/cookbook-pca.html#Subsampling-with-replication).

6. Run the PCA using this command:
```python
pca.run()
```
You have the possibility to write the PC axes to a CSV file following the instructions [here](https://ipyrad.readthedocs.io/en/stable/API-analysis/cookbook-pca.html#Run-PCA).

7. Draw the results using:
```python
pca.draw(0, 1);
```
This command will plot the PCA results for the first 2 axes (numerotation of the axes begin at 0).

8. The PCA analysis of ipyrad toolkit take one random SNPs on each locus for the plotting in order to have unlinked SNPs. Adding some parameters can replicate the analysis on different sets of random SNPs like this:
```python
pca.run(nreplicates=25, seed=12345);
pca.draw(0, 1);
```
This commands will generate a plot with 25 replicates of the PCA (for the seed you can write any random number, it is just a marker for the analysis).

9. Finally, you can save your plot in PDF format using:
```python
pca.draw(0, 1, outfile="PCA-trem_default_axes0-1.pdf");
```
You will find an example file in the folder Example/ipyrad_analysis_toolkit/pca/ (Warning: open it using jupyter-notebook otherwise it will not be readable)

## Treemix analysis
1. Open a jupyter-notebook in your working directory following the steps [here](https://github.com/Enorya/LBEG_documentation/tree/main/ipyrad#open-jupyter-notebook) but instead of connecting to the miniconda3 environment, connect to the treemix environment with this command: `source activate /staging/leuven/stg_00026/Softwares/miniconda3/envs/treemix/`.
If your data are in VCF format you need to convert them into hdf5 using those [explanations](https://github.com/Enorya/LBEG_documentation/tree/main/ipyrad#converting-vcf-files-into-hdf5-files).
2. Import the different modules needed in the first cell:
```python
import ipyrad.analysis as ipa
import toytree
import toyplot
import toyplot.pdf
```
3. Do step 3 and 4 as with the [PCA analysis](https://github.com/Enorya/LBEG_documentation/tree/main/ipyrad#pca-analysis)
4. You now need to launch the PCA analysis like this:
```python
tmx = ipa.treemix(
    data=data,
    imap=imap,
    minmap=minmap,
    seed=123456,
    root="bor",
    m=2,
)
```
The m parameter corresponds to the number of migrations you are allowing for your analysis. You will end up with a plot looking like:
<img src="https://github.com/Enorya/LBEG_documentation/blob/main/ipyrad/images/treemix_plot_example.png" alt="treemix plot"/>

5. Run the Treemix analysis using this command:
```python
tmx.run()
```
6. Draw the resulting tree:
```python
tmx.draw_tree()
```
7. Draw the covariance matrix:
```python
tmx.draw_cov()
```
8. In order to find the best value for the number of migration events `m` first initialize your treemix analysis:
```python
tmx = ipa.treemix(
    data=data,
    imap=imap,
    minmap=minmap,
    seed=1234,
    root="bor",
)
```
Then iterate your run with different values of `m`:
```python
tests = {}
nadmix = [0, 1, 2, 3, 4, 5, 6]

for adm in nadmix:
    tmx.params.m = adm
    tmx.run()
    tests[adm] = tmx.results.llik
```
Finally, plot the likelihood for these different values of `m`:
```python
toyplot.plot(
    nadmix,
    [tests[i] for i in nadmix],
    width=350,
    height=275,
    stroke_width=3,
    xlabel="n admixture edges",
    ylabel="ln(likelihood)",
);
```
You will end up with a plot like the following one, all you have to do is choose the value at which the curve starts to flatten out.
<img src="https://github.com/Enorya/LBEG_documentation/blob/main/ipyrad/images/likelihood_for_m_plot.png" alt="likelihood for different values of m"/>

9. You can then run again your analysis with the correct value but also replicating on different set of random SNPs like this:
```python
# initialize a gridded canvas to plot trees on
canvas = toyplot.Canvas(width=600, height=700)

# iterate on different set of random SNPs (9 here)
for i in range(9):

    # init a treemix analysis object with a random seed
    tmx = ipa.treemix(
        data=data,
        imap=imap,
        minmap=minmap,
        root="bor",
        global_=True,
        m=3,
        quiet=True
    )

    # run model fit
    tmx.run()

    # select a plot grid axis and add tree to axes
    axes = canvas.cartesian(grid=(3, 3, i))
    tmx.draw_tree(axes)
```
10. Finally, you can save your plot in PDF format using:
```python
toyplot.pdf.render(canvas, "treemix-m3.pdf")
```
You will find an example file in the folder Example/ipyrad_analysis_toolkit/treemix/ (Warning: open it using jupyter-notebook otherwise it will not be readable)