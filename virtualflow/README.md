# Table of Contents

- [Deploy Slurm Cluster](#deploy-slurm-cluster)
- [Install VirtualFlow](#install-virtualflow)
- [Setting Up the Workflow](#setting-up-the-workflow)
- [Preparing the Docking Input Files](#preparing-the-docking-input-files)
- [Preparing the Tools Folder](#preparing-the-tools-folder)
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

* Change to input-files directory

```
[user@machine ligand-library]$ cd ..
[user@machine input-files]$ 

```

* Download docking input files

```
[user@machine input-files]$ wget https://virtual-flow.org/sites/virtual-flow
.org/files/tutorials/docking_files.tar.gz
[user@machine input-files]$  tar -xvzf docking_files.tar.gz
smina_rigid_receptor1/config.txt
receptor/4no7_prot.pdbqt
qvina02_rigid_receptor1/config.txt
smina_rigid_receptor1/
receptor/
qvina02_rigid_receptor1/
[user@machine input-files]$
```

# Preparing the Tools Folder

* Prepare the tools folder

```
[user@machine input-files]$ cd ../tools/
```

* Edit the file _tools/templates/all.ctrl_
  * _batchsystem_: The resource manager which is used by your cluster.
  * _partition_: The partition queue to be used for running the jobs of this workflow/tutorial.
  * _timelimit_: Each partition/queue has normally a time-limit, therefore make sure you don't exceed it.
  
```
[user_google_com@fluid-slurm-gcp-1-login-0 ~]$ vi VFVS-develop/tools/templates/all.ctrl
```

* The _batchsystem_ should already be set to SLURM:

```
batchsystem=SLURM
# Possible values: SLURM, TORQUE, PBS, LSF, SGE
# Settable via range control files: No
```

* Set _partition_ to match the deployment partition name given above:
* Change _“shared”_ to _“partition-1”_ or whatever you named your partition in the deployment

```
partition=partition-1
# Partitions are also called queues in some batchsystems
# Settable via range control files: Yes
```

* Set _timelimit_ (no change):

```
timelimit=7-00:00:00
# Format for slurm: dd-hh:mm:ss
# Format for TORQUE and PBS: hh:mm:ss
# Format for SGE: hh:mm:ss
# Format for LSF: hh:mm
# For all batchsystems: always fill up with two digits per field (used be the job scripts)
# Settable via range control files: Yes
```

# Using Entire Nodes

# Workflow Settings

# Docking Scenario Settings

# Prepare the Job File
