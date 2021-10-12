# VSC Documentation for LBEG

#### By Henrik Christiansen, last update on 12/10/2021
#### All information available on github: [github.com/Enorya/LBEG_documentation](https://github.com/Enorya/LBEG_documentation/tree/main/vsc) 

## Introduction
For computationally intensive tasks we use the Flemish Supercomputer Cluster (hereafter VSC). Detailed instructions on VSC usage can b
e found [here](https://vlaams-supercomputing-centrum-vscdocumentation.readthedocs-hosted.com/en/latest/index.html)

## Account
Before using the VSC you need to get a VSC account. Following the explanations [here](https://vlaams-supercomputing-centrum-vscdocumentation.readthedocs-hosted.com/en/latest/access/getting_access.html#)

## Credits
When you have an account, you also need credits to run analyses on the VSC. We have a VSC project group called "llp_lbeg" which is used for billing credits. When running jobs you need to specify this group in the PBS header or when requesting interactive jobs: ```-A llp_lbeg```

You also have to be part of the "llp_lbeg" group. Tell Enora, Bart or Henrik your VSC number and they can add you to the group. You will then receive an e-mail invitation.

When you're a member of "llp_lbeg" you can check how many credits we have left like so (when connected to the VSC): ```mam-balance```

If we're about to run out of credits (<5000 credits left) tell Enora or Bart. New project credits can ordered via the KU Leuven Service Catalog "High Performance Computing": [https://icts.kuleuven.be/sc/english/research/HPC](https://icts.kuleuven.be/sc/english/research/HPC)

More specifically, at the time of writing, this is the link to order new credits: [https://admin.kuleuven.be/icts/onderzoek/hpc/extra-project-credits](https://admin.kuleuven.be/icts/onderzoek/hpc/extra-project-credits)

VSC Number, Email of a VSC group admin, Projectname, Number of credits, and information onf financial funding (check with Filip/Bart!) are needed to request new credits.

In addition, every new VSC member receives 2000 introductory credits for free. So, new users should request these [here](https://admin.kuleuven.be/icts/onderzoek/hpc/request-introduction-credits) after setting up their new accound and then use these credits first (and switch to "llp_lbeg" credits after the introductory credits are spent).



## Storage

## Good practices