# VSC Documentation for LBEG

#### By Henrik Christiansen, last update on 12/10/2021
#### All information available on github: [github.com/Enorya/LBEG_documentation](https://github.com/Enorya/LBEG_documentation/tree/main/vsc) 

## Introduction
For computationally intensive tasks we use the Flemish Supercomputer Cluster (hereafter VSC). Detailed instructions on VSC usage can be found [here](https://vlaams-supercomputing-centrum-vscdocumentation.readthedocs-hosted.com/en/latest/index.html).

## Account
Before using the VSC you need to get a VSC account. Following the explanations [here](https://vlaams-supercomputing-centrum-vscdocumentation.readthedocs-hosted.com/en/latest/access/getting_access.html#).

## Credits
When you have an account, you also need credits to run analyses on the VSC. We have a VSC project group called "llp_lbeg" which is used for billing credits. When running jobs you need to specify this group in the PBS header or when requesting interactive jobs: ```-A llp_lbeg```

You also have to be part of the "llp_lbeg" group. Tell Enora, Bart or Henrik (the project group leaders) your VSC number and they can add you to the group. You will then receive an e-mail invitation. For project group leaders: adding/removing users or changing the project group lead can be done in the [VSC account centrum](https://account.vscentrum.be) under "Edit group".

When you're a member of "llp_lbeg" you can check how many credits we have left like so (when connected to the VSC): ```mam-balance```

If we're about to run out of credits (<5000 credits left) tell Enora or Bart. New project credits can be ordered via the KU Leuven Service Catalog "High Performance Computing": [https://icts.kuleuven.be/sc/english/research/HPC](https://icts.kuleuven.be/sc/english/research/HPC)

More specifically, at the time of writing, this is the link to order new credits: [https://admin.kuleuven.be/icts/onderzoek/hpc/extra-project-credits](https://admin.kuleuven.be/icts/onderzoek/hpc/extra-project-credits)

VSC Number, Email of a VSC group admin, Projectname, Number of credits, and information onf financial funding (check with Filip/Bart!) are needed to request new credits.

In addition, every new VSC member receives 2000 introductory credits for free. So, new users should request these [here](https://admin.kuleuven.be/icts/onderzoek/hpc/request-introduction-credits) after setting up their new account and then use these credits first (and switch to "llp_lbeg" credits after the introductory credits are spent).

The VSC sends out a monthly project users overview to the project leaders (currently Enora, Bart, Henrik) with information on how many jobs each user ran and how many credits each users used, as well as the total amount of jobs and credits for the group.

## Storage

We have two storage folders on the VSC:

- ```/staging/leuven/stg_00026```

- ```/archive/leuven/arc_00026```

These localities differ in the sense that ```staging``` is the "normal" working space (in addition to your personal home, data, and scratch folders). The ```archive``` directory is for long-term storage of raw data/final output files and **not** for ongoing work (this directory is slower to work with & more expensive!).

Storage space on the VSC is costly (much more expensive than credits), so use it wisely!

The VSC now bills ```staging``` usage on a monthly basis and sends out monthly overviews on the space used to the project lead(ers). The ```archive``` directory is still billed annually. New space can be ordered [here](https://admin.kuleuven.be/icts/onderzoek/hpc/extend_storage).

## Good practices/first things to do for troubleshooting

If you're completely new at working with the VSC (and with a command line), consider following a VSC (and/or Linux) introduction course (these are regularly offered by the VSC staff - check their [website](https://www.vscentrum.be/training)) and/or ask someone experienced to guide/teach you.

Start with easy tasks and if you run into problems check the [VSC documentation pages](). A good way to troubleshoot is also to go back to a simpler version and try that (e.g. when tryin to submit a job with a relatively long PBS script and it doesn't work, go back to a simpler PBS and if that works, add more things step by step to figure out where the problem is).

Also consider this:

- if you can't login, check your keys! if you uploaded a new public key wait for up to a day for it to work!

- if you still can't login, check if the cluster is down: [statuspagina](https://status.kuleuven.be/hpc.php)

- if your jobs don't run, check if the cluster is down: [statuspagina](https://status.kuleuven.be/hpc.php)

- if the cluster is online, but your jobs don't run, check if we have enough credits: ```mam-balance```

- if your job still doesn't run very carefully check your PBS header and maybe consider submitting a test job with minimal content and minimal PBS requests
