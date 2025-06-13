# SLURM Job Submission on Rocky9 Guide

To run jobs on our cluster, please follow these steps:

## 1. Check Available Cluster Resources

Before submitting your job, use the `slurm_resources` utility to see which resources are currently available.  
Update your SLURM script accordingly based on this information.

```bash
slurm_resources
```

```
Accounts to use for accessing the batch (default) partition:

Account         
----------------
cgg             
maiziezhou_lab  


Accounts and GPU types for accessing the batch_gpu partition:

Account              GPU Type                      GPU Limit
-------------------- ---------------------------- ----------
cgg_acc              nvidia_titan_x                        8
maiziezhou_lab_acc   nvidia_geforce_rtx_2080_ti           51
maiziezhou_lab_acc   nvidia_rtx_a6000                      2
maiziezhou_lab_acc   nvidia_titan_x                       16
maiziezhou_lab_acc   quadro_rtx_6000                       6

You have access to accelerated GPU resources in the batch_gpu partition.
As a usage example, if you wanted to request 2 GPUs of type "nvidia_titan_x"
for a job with account "cgg_acc" on the partition "batch_gpu",
then you would add the following lines to your SLURM script:

#SBATCH --account=cgg_acc
#SBATCH --partition=batch_gpu
#SBATCH --gres=gpu:nvidia_titan_x:2

Note that the "GPU Limit" column on the table above lists is the burst
limit for that GPU type for the specified account. For example, the
scheduler will not allow more than 8 GPUs of type
"nvidia_titan_x" to be used simulatenously by account "cgg_acc".
If more GPUs of that type are requested by that account, the scheduler
will not allow them to run until other running jobs have completed.
You may not request more than the GPU limit for a single job.


Accounts and QOSs for accessing the interactive partition:

Account              QOS                   CPU limit  Memory Limit (GiB)
-------------------- -------------------- ---------- -------------------
cgg_int              debug_int                    80               960.0
maiziezhou_lab_int   debug_int                    80               960.0
maiziezhou_lab_int   maiziezhou_lab_int           64              1002.0

Use the "qosstate [QOS_NAME]" command to get current usage for
the QOS specified by QOS_NAME with a breakdown of usage by user
and slurm account.

Note that the "debug_int" QOS is a special QOS available to all
cluster users for quick debugging of slurm scripts. On this special
QOS there are additional restrictions of a maximum wall clock time
of 00:30:00 and each user can use a maximum of cpu=16,mem=192G at
one time.

As an example, to submit a job to the interactive partition using
the account "cgg_int" and QOS "debug_int" you would add the following
lines to your slurm script:

#SBATCH --account=cgg_int
#SBATCH --partition=interactive
#SBATCH --qos=debug_int
```
 
## 2. Check QOS Status (if Applicable)
If you want to use a Quality of Service (QOS) queue, check the current usage and available resources:

```bash
qosstate maiziezhou_lab_int
```

```
Live interactive QOS usage report for QOS available to user: yuanw2

QOS                     CPUs (used/limit)   Memory GiB (used/limit)
===================================================================
maiziezhou_lab_int                15 / 64            200.0 / 1002.0


Breakdown of maiziezhou_lab_int usage by slurm account and user

Account              User           CPUs      Memory GiB
========================================================
maiziezhou_lab_int
                     yuanw2           15           200.0
```
Note:
Make sure your resource requests do not exceed the available quota. 
Use `qosstate --user [USER]` to check USER's usage. 

## 3. Prepare and Submit Your Job

Prepare your SLURM batch script (your_script.slurm) as usual.
Submission works the same as on CentOS 7:

```bash
sbatch your_script.slurm
```
Here is an example of SLURM script template of using QOS:
```
#!/bin/bash
#
#SBATCH --job-name=[YOUR_JOB_NAME]
#
##### Account and Partition ####
#SBATCH --account=[ACCOUNT_NAME]
#SBATCH --partition=interactive
#SBATCH --qos=[PARTITION_NAME]
#
##### Resources Required ####
###Basic
#SBATCH --ntasks=1
#SBATCH --nodes=1
#SBATCH --cpus-per-task=[CPU_NUMBER]
#SBATCH --mem=[MEMORY]
#SBATCH --time=[TIME_REQUESTED]
#
###Advanced
##submit to a specific node/some specific nodes, add this command: #SBATCH --nodelist=[NODENAME]
#
##### Output and Error Information ####
#SBATCH --output=[OUTFILE_NAME].out
#SBATCH --error=[ERRFILE_NAME].err
#
##### Email Notification ####
#SBATCH --mail-user=[YOUR_EMAIL_ADDRESS]
#SBATCH --mail-type=END,FAIL

echo "SLURM_JOBID: " $SLURM_JOBID
```

## 4. Use salloc to run job on the background

If you would like your jobs to run on the background, here is an command to request resource for QOS: 

```
ssh -t YOUR_VUNETID0@login.accre.vu "salloc --nodes=1 --ntasks=1 --cpus-per-task
 =10 --mem=64G --time=01-00:00:00 --partition=interactive --account=maiziezhou_lab_int --qos=maiziezhou_lab_int"
```
