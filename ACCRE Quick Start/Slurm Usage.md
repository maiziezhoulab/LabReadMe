# Slurm Usage
## Table of Contents
- [Submitting Jobs](#Submitting-Jobs)
  - [A Quick Example](#A-Quick-Example)
  - [Slurm Header](#Slurm-Header)
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

ls -lht
```
By running `sbatch submit.slurm` command, you will submit a job that asks one of the node in the `production` partition to run `ls -lht` command.

As you may have noticed, the content in `submit.slurm` was consisted of two parts: one with `#` at the beginning of each line and one without. The first part was also known as the **header** of a slurm file, which passes essential information about the job to the cluster so that the Slurm workload manager could schedule your j

### Slurm Header

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
