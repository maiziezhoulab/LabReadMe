# SLURM Job Submission on Rocky9 Guide

To run jobs on our cluster, please follow these steps:

## 1. Check Available Cluster Resources

Before submitting your job, use the `slurm_resources` utility to see which resources are currently available.  
Update your SLURM script accordingly based on this information.

```bash
slurm_resources
```
 
## 2. Check QOS Status (if Applicable)
If you want to use a Quality of Service (QOS) queue, check the current usage and available resources:

```bash
qosstate maiziezhou_lab_int
```

Note:
Make sure your resource requests do not exceed the available quota.

## 3. Prepare and Submit Your Job

Prepare your SLURM batch script (your_script.slurm) as usual.
Submission works the same as on CentOS 7:

```bash
sbatch your_script.slurm
```
