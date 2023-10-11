# Slurm Usage
## Table of Contents
- [Submitting Jobs](#Submitting-Jobs)
  - [A Quick Example](#A-Quick-Example)
  - [Slurm Header](#Slurm-Header)
  - [Advanced Settings](#Advanced-Settings)
  - [Special Notes for Lio System](#Special-Notes-for-Lio-System)
- [Track Your Jobs](#Track-Your-Jobs)
- [Slurm Templates](#Slurm-Templates)
  - [Regular Job](#Regular-Job)
  - [GPU Job](#GPU-Job)
- [Other Resources](#Other-Resources)
- [FAQ](#FAQ)

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

List of available accounts and partitions

| Account | Partition| Notes |
|    :----:   |    :----:   |    :----:   |
| maiziezhou_lab | production | |
| maiziezhou_lab | debug | |
| cgw_maizie | cgw-maizie | Our lab's private node |
| cgw_maizie2 | cgw-maizie2 | Our lab's private node |
| cgw_maizie3 | cgw-maizie3 | Our lab's private node |
| p_dsi | production |  |
| p_dsi | debug |  |
| maiziezhou_lab_acc | turing | GPU partition |
| p_dsi_acc | pascal | GPU partition |
| p_dsi_acc | turing | GPU partition |

**NOTE:**
1. For partition maizie1~3, there's no time limit. maizie nodes are our private node and are hidden by default, you need to add `-a` after `squeue` and `scontrol show node` in order too see them
2. Most of the nodes in turing partition use NVIDIA 2080TI, but some nodes (like gpu0057) have NVIDIA RTX 6000 available. Nodes in pascal partition have NVIDIA TITAN X. Typically, pascal partition has shorter queue time, but the resources allocated to one task are more limited (less CPUs quota for a task for example). Personally, I recommend you to use pascal for interactive GPU task debug/environment configuration, and use turing for production.

#### Requiring Computing Resources
```
#SBATCH --ntasks=1
#SBATCH --nodes=1
#SBATCH --cpus-per-task=2
#SBATCH --mem=5G
#SBATCH --time=00-01:00:00
```
The block above is about the computing resources you want to allocate to your job. 

In many situations, you can use `1` for `--ntasks` and `--nodes`, we will not go that detail into these two parameters, but if you are interested, you can refer to [Slurm Official Documentation](https://slurm.schedmd.com/sbatch.html) or [this post](https://stackoverflow.com/questions/39186698/what-does-the-ntasks-or-n-tasks-does-in-slurm).

`--cpus-per-task` is the number of CPUs allocated to each task, and since we use `--ntasks=1` here, it is the same as the total number of CPUs allocated to this job. `--mem` is the amount of memory allocated to the job, and `--time` specifies the maximum time that the job can run (format is dd-hh:mm:ss).

**Note that there are limits for the resources that could be allocated to a job on ACCRE**, you can check the time limits for different partitions with command `sinfo` or `sacctmgr show qos`, or use `scontrol show node -a [NODE_NAME]` to check the CPU and memory resources on a specific node. Typically, for nodes in `production` partition, the maximum time limits is 14 days, maximum memory is around 1~1.2T and maximum CPU number is around 128; for nodes in GPU partitions like `turing` and `pascal`, the time limit is around 5 days, memory limit is around 80G and CPU limit is around 6. However, these limits may change as ACCRE continues to upgrade, check them with the commands memtioned above to get the most up-to-date values. Nodes under `cgw-maizie` partition are the private nodes of our lab, there's no time limit for these nodes.

Requiring too much resources for your job may result in waiting in the queue list for a long time, therefore, only request the resources that are just enough to complete the job.

#### Environment Variables
```
#SBATCH --export=ALL
```
This is about which enviroment variables you want to use for this job, if it is not given, then the default action is `ALL`, which uses all your environment variables in this job. (this line is not needed actually, it's just the default setting)

#### Output and Error Information
```
#SBATCH --output=test.out
#SBATCH --error=test.err
```
These two lines specify where slurm will redirect the [stdout](https://www.computerhope.com/jargon/s/stdout.htm) and [stderr](https://www.computerhope.com/jargon/s/stderr.htm) information to. In this example, stdout is redirected to file `test.out` and stderr to `test.err`. As this tasks runs `echo HelloWorld` command, you can find `HellowWorld` in `test.out`. In general, these two files are where you can find the printed output and error information of this job.

#### Email Notification
```
#SBATCH --mail-user=YOUREMAIL@vanderbilt.edu
#SBATCH --mail-type=END,FAIL
```
In these two lines, you can tell the slurm how to notifiy you. `--mail-user` is the email that slurm will send notification to, and `--mail-type` is about when will slurm send email. In this example, slurm will send an email to `YOUREMAIL@vanderbilt.edu` when the job **ended** or **failed**. For more options, please refer to [Slurm Official Documentation](https://slurm.schedmd.com/sbatch.html).  

### Advanced Settings
#### Request GPU Resource
```
#SBATCH --gres=gpu:1
```
Add this line to request GPU resources. Note that you have submit to partitions that do have GPU resources.

#### Submit to Specific node(s)
```
#SBATCH --nodelist=[NODENAME]
```
examples:
```
#SBATCH --nodelist=node[01-09]
#SBATCH --nodelist=node01,node02,node03,node04,node05,node07,node08,node09,node10
```
#### Exclude some nodes
```
#SBATCH --exclude=[NODENAME]
```
examples:
```
#SBATCH --exclude=node[01-09]
#SBATCH --exclude=node01,node02,node03,node04,node05,node07,node08,node09,node10
```
**NOTE:** So far I've found that these nodes on ACCRE have some problems activating conda environments:

cn1541, cn1542, cn1554, cn1558

Not sure if this issue was fixed or not

### Special Notes for Lio System

1. If you are submitting jobs under `/lio/lfs`, you need to **login to the node you are submitting to**. For example, if you want to submit a job to maizie2 node (account: cgw_maizie2, partition: cgw-maizie2), and the slurm script is under `/lio/lfs` (or it's subdir), you need to first ssh to maizie2 node, then submit the job. Otherwise the job will fail in a few seconds without any error message. The reason here may be the write permission issue, slurm will create `.out` and `.err` and this will write to lio system.
2. Be careful when your job requires **Write** operation under `/lio/lfs`, this can be quite slow especially you are writing a large number of files. 


## Track Your Jobs

It is recommended to add `echo "SLURM_JOBID: " $SLURM_JOBID` to your slurm script, so that you can record the job ID in your `.out` file

### bash commands
squeue -a -u

rtracejob

sacct --format="JobID,user,account,elapsed, Timelimit,MaxRSS,ReqMem,MaxVMSize,ncpus,ExitCode" -j 42363199

sacct --format=jobid,jobname,account,ntasks,elapsed,user,state -S 082222 -E 082522  -a | grep "maizie" | grep "FAILED" | grep -v ".ex+\|.ba+"

sacct --format="JobId,JobName,CPUTime,MaxRSS,Elapsed" -j
## Slurm Templates
### Regular job
```
#!/bin/bash
#
#SBATCH --job-name=[YOUR_JOB_NAME]
#
##### Account and Partition ####
#SBATCH --account=[ACCOUNT_NAME]
#SBATCH --partition=[PARTITION_NAME]
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

### GPU job
```
#!/bin/bash
#
#SBATCH --job-name=[YOUR_JOB_NAME]
#
##### Account and Partition ####
#SBATCH --account=[ACCOUNT_NAME]
#SBATCH --partition=[PARTITION_NAME]
#
##### Resources Required ####
###Basic
#SBATCH --ntasks=1
#SBATCH --nodes=1
#SBATCH --cpus-per-task=[CPU_NUMBER]
#SBATCH --mem=[MEMORY]
#SBATCH --time=[TIME_REQUESTED]
#
###GPU related
#SBATCH --gres=gpu:1
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

## Other Resources
- [Slurm Official Documentation](https://slurm.schedmd.com/sbatch.html)
- [Introducing Slurm by Princeton](https://researchcomputing.princeton.edu/support/knowledge-base/slurm)
- [CN materials: Slurm作业调度系统使用指南 by 李会民](http://hmli.ustc.edu.cn/doc/userguide/slurm-userguide.pdf)
## FAQ
- **ReqNodeNotAvail, Reserved for maintenance :** The time period you required for this job overlaps with the cluster's schduled maintainance time, try with a shorter time request
