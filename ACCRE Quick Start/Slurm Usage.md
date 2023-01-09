# Slurm Usage
## Table of Contents
1. [Submitting a Job](#Submitting-a-Job)
## Regular job
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

## GPU job
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
## Submitting a Job
