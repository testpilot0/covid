# Table of Contents

- [Deploy Slurm Cluster](#deploy-slurm-cluster)
- [Install VirtualFlow](#install-virtualflow)
- [Setting Up the Workflow](#setting-up-the-workflow)
- [Preparing the Docking Input Files](#preparing-the-docking-input-files)
- [Preparing the tools Folder](#preparing-the-tools-folder)
- [Using Entire Nodes](#using-entire-nodes)
- [Workflow Settings](#workflow-settings)
- [Docking Scenario Settings](#docking-scenario-settings)
- [Prepare the Job File](#prepare-the-job-file)

# Deploy Slurm Cluster

* Search for SLURM in [GCP Marketplace](https://console.cloud.google.com/marketplace/)

![Search for SLURM](images/image-00.png)

* Select "Fluid-Slurm-GCP"

![Select "Fluid-Slurm-GCP"](images/image-01.png)

* Configure
  * Slurm Login Node
    * Default: 1 instance, n1-standard-4
  * Slurm Controller Node
    * Default: n1-standard-4
  * Slurm Default Compute Partition
    * Default: 10 instances, n1-standard-64 → CHANGE: n1-standard-8 (e2-standard-8?)
    * Partition Name: partition-1

* Deploy

![Deploy "Fluid-Slurm-GCP"](images/image-02.png)

# Install VirtualFlow

* SSH into the SLURM Login Node

![SSH into the SLURM Login Node](images/image-03.png)

* Install VirtualFlow for Virtual Screening (VFVS)
  * wget -O VFVS.tar.gz [https://github.com/VirtualFlow/VFVS/archive/develop.tar.gz](https://github.com/VirtualFlow/VFVS/archive/develop.tar.gz)
  * tar -xvf VFVS_GK.tar
  * git clone [https://github.com/VirtualFlow/VFTools](https://github.com/VirtualFlow/VFTools)

```
[user_google_com@fluid-slurm-gcp-1-login-0 ~]$ ls -l
total 10112
drwxrwxr-x. 4 user_google_com user_google_com      128 Jun  2 20:48 VFTools
drwxrwxr-x. 6 user_google_com user_google_com      173 Apr 23 14:50 VFVS-develop
-rw-rw-r--. 1 user_google_com user_google_com 10351453 Jun  2 20:47 VFVS.tar.gz
```

# Setting Up the Workflow

* VirtualFlow Virtual Screening [tutorial](https://docs.virtual-flow.org/tutorials/-LdE94b2AVfBFT72zK-v/tutorial-2-vfvs-scratch/introduction)
* The target structure in this tutorial is human glucokinase (GK)
 
The files in this tutorial come with two pre-configured docking scenarios:

1. [QuickVina 2](https://academic.oup.com/bioinformatics/article/31/13/2214/195750) with exhaustiveness set to 8
1. [Smina Vinardo](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0155183) with exhaustiveness set to 4

The REAL database of Enamine contains over 1.4 billion commercially available on-demand molecules.

* [https://virtual-flow.org/real-library](https://virtual-flow.org/real-library)

![REAL Library: REAL database of Enamine](images/image-04.png)

* Set Parameters
  * Set Molecular Weight: 250 - 300
  * Set Partition Coefficient: 2.3-3
  * Set Topological Polar Surface Area: 60-80
  * Set Hydrogen Bond Acceptors: 3-5
  * Set Hydrogen Bond Donors: 2-3
  * Set Rotatable Bonds: 0-10

* Download method for tranches: “wget”
  * Click “download” → tranches.sh

* Collection-length file
  * Click “download” → collections.txt

SSH into SLURM Login Node
T![OD: Add screenshot.]()


Upload files to SLURM Login Node
* tranches.sh
* collections.txt

![Upload files to SLURM Login Node](images/image-05.png)

Replace the file tools/templates/todo.all with the file collections.txt

```
[user@machine ~]$ cp collections.txt VFVS-develop/tools/templates/todo.all
```

* Move the file tranches.sh into VFVS_GK/input-files/ligand-library
* Change to that directory
* Source the tranches file

```
[user@machine ~]$ mv tranches.sh VFVS-develop/input-files/ligand-library/
[user@machine ~]$ cd VFVS-develop/input-files/ligand-library/
[user@machine ligand-library]$ source tranches.sh 
```

# Preparing the Docking Input Files

# Preparing the tools Folder

# Using Entire Nodes

# Workflow Settings

# Docking Scenario Settings

# Prepare the Job File
