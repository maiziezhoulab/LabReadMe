# Slurm Usage
## Table of Contents
- [Submitting Jobs](#Submitting-Jobs)
  - [A Quick Example](#A-Quick-Example)
  - [Slurm Header](#Slurm-Header)
  - [Advanced Settings](#Advanced-Settings)
- [Track Your Jobs](#Track-Your-Jobs)
- [Slurm Templates](#Slurm-Templates)
  - [Regular Job](#Regular-Job)
  - [GPU Job](#GPU-Job)
- [Other Resources](#Other-Resources)

## Submitting Jobs

### A Quick Example
Suppose you have a file `submit.slurm` in your current directory, and the content of which is:
```
#!/bin/bash
#
#SBATCH --job-name=List_files_in_current_dir
#
##### Account and Partition ####
#SBATCH --account=maiziezhou_lab
#SBATCH --partition=production
#
##### Resources Required ####
#SBATCH --ntasks=1
#SBATCH --nodes=1
#SBATCH --cpus-per-task=2
#SBATCH --mem=5G
#SBATCH --time=00-01:00:00
#
##### Environment Variable Related ####
#SBATCH --export=ALL
#
##### Output and Error Information ####
#SBATCH --output=test.out
#SBATCH --error=test.err
#
##### Email Notification ####
#SBATCH --mail-user=YOUREMAIL@vanderbilt.edu
#SBATCH --mail-type=END,FAIL

echo HelloWorld
```
By running `sbatch submit.slurm` command, you will submit a job that asks one of the node in the `production` partition to run `echo HelloWorld` command.

As you may have noticed, the content in `submit.slurm` was consisted of two types of lines: some with `#SBATCH` at the beginning and some without. Those with `#SBATCH` were also known as the **header** of a slurm file, which pass essential information about the job to the Slurm workload manager. We will break that down in the following section

```
#!/bin/bash
```
This line is not a slurm header, it just indicates that this file should be interpreted as a bash script

### Slurm Header
#### Give Your Job a Name
```
#SBATCH --job-name=List_files_in_current_dir
```
By using this `--job-name` option, you can assign a name to your job
#### Account and Partition
```
#SBATCH --account=maiziezhou_lab
#SBATCH --partition=production
```
In these two lines, you need to specify which accout you are using and to which partition you want to to submit the job. Different partitions may belong to different owners/labs/groups, have different properties (cpu nodes, gpu nodes etc.) or used for different purpose (production, debug etc.), but in general, a partition just refers to a **subset of nodes in the cluster**. Different accounts can access different partitions, to check which accounts are available to you and which partitions they have access to, just type `slurm_groups` in the command line.
#### Requiring Computing Resources
```
#SBATCH --ntasks=1
#SBATCH --nodes=1
#SBATCH --cpus-per-task=2
#SBATCH --mem=5G
#SBATCH --time=00-01:00:00
```
The block above is about the computing resources you want to allocate to a job. In many situations, you can just use `1` for `--ntasks` and `--nodes`, we will not go that detail into these two parameters, but if you are interested, you can refer to [Slurm Official Documentation](https://slurm.schedmd.com/sbatch.html) or [this post](https://stackoverflow.com/questions/39186698/what-does-the-ntasks-or-n-tasks-does-in-slurm)

%% ReqNodeNotAvail, May be reserved for other job
### Advanced Settings

## Track Your Jobs

## Slurm Templates
### Regular job
```
#!/bin/bash
#
#SBATCH --job-name=YOUR_JOB_NAME
#
##### Account and Partition ####
##Check the available account and partition with command: slurm_groups
##commonly used account and partition pairs:
##submit to production partition: ACCOUNT:maiziezhou_lab PARTITION:production
##submit to maizie node         : ACCOUNT:cgw_maizie     PARTITION:cgw-maizie  
#SBATCH --account=ACCOUNT_NAME
#SBATCH --partition=PARTITION_NAME
#
##### Resources Required ####
###Basic
##Check the time limit for different partition with command: sinfo
##or use : scontrol show node -a NODE_NAME to check the resources on a specific node
#SBATCH --ntasks=1
#SBATCH --nodes=1
#SBATCH --cpus-per-task=CPU_NUMBER (eg:10)
#SBATCH --mem=MEMORY (eg:10G)
#SBATCH --time=TIME_REQUESTED(dd-hh:mm:ss, eg:01-00:00:00)
#
###Advanced
##submit to a specific node/some specific nodes, add this command: #SBATCH --nodelist=NODENAME
#
##### Environment Related ####
#SBATCH --export=ALL
#
##### Output and Error Information ####
#SBATCH --output=OUTFILE_NAME.out
#SBATCH --error=ERRFILE_NAME.err
#
##### Email Notification ####
#SBATCH --mail-user=YOUR_EMAIL_ADDRESS
#SBATCH --mail-type=END,FAIL
```

### GPU job
```
#!/bin/bash
#
#SBATCH --job-name=YOUR_JOB_NAME
#
##### Account and Partition ####
##Check the available account and partition with command: slurm_groups
##commonly used account and partition pairs for GPU tasks:
##submit to turing partition: ACCOUNT:maiziezhou_lab_acc PARTITION:turing <- Recommended
##                        or: ACCOUNT:p_dsi_acc          PARTITION:turing
##submit to pascal partition: ACCOUNT:p_dsi_acc          PARTITION:pascal
#SBATCH --account=ACCOUNT_NAME
#SBATCH --partition=PARTITION_NAME
#
##### Resources Required ####
###Basic
##Check the time limit for different partition with command: sinfo
##or use : scontrol show node -a NODE_NAME to check the resources on a specific node
#SBATCH --ntasks=1
#SBATCH --nodes=1
#SBATCH --cpus-per-task=CPU_NUMBER (eg:5)
#SBATCH --mem=MEMORY (eg:10G)
#SBATCH --time=TIME_REQUESTED(dd-hh:mm:ss, eg:01-00:00:00)
#
###GPU related
#SBATCH --gres=gpu:1
#
##### Environment Related ####
#SBATCH --export=ALL
#
##### Output and Error Information ####
#SBATCH --output=OUTFILE_NAME.out
#SBATCH --error=ERRFILE_NAME.err
#
##### Email Notification ####
#SBATCH --mail-user=YOUR_EMAIL_ADDRESS
#SBATCH --mail-type=END,FAIL
```
