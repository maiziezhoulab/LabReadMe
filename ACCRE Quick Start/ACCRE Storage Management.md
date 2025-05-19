# ACCRE Storage Management


## Moniter Storage Usage
Total storage used for our account: `accre_storage`

Scan the storage usage for a specific directory: `du -shc [DIRECTORY TO CHECK]`

For more information, please check the documentation of `du` command.

Slurm script for scanning specific directories:
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

dir_to_scan=[DIRECTORY TO CHECK]
report=storage_report_$(date +'%m_%d_%Y').txt
touch ${report}

dir=$(ls -l ${dir_to_scan} | awk '/^d/ {print $NF}')
for i in $dir
do
        echo "=========================================" >> $report
        echo Storage report of $i: >> $report
        echo Storage usage: >> $report
        du -shc ${dir_to_scan}/$i >> $report
        echo "inodes (number of files):" >> $report
        ls -lR ${dir_to_scan}/$i | grep "^-" | wc -l >> $report
        echo "=========================================" >> $report
        echo >> $report
done
chmod 740 ${report}
```


## Backup Files to Lio
Please also check this [section](Slurm%20Usage.md#special-notes-for-lio-system)
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

current=[SOURCE DIRECTORY]
target=[TARGET DIRECTORY]

mkdir -p ${target}

find ${current}/ -type f -exec md5sum "{}" + | sed "s@${current}/@@g" > ${target}/files.md5

cp -r ${current}/* ${target}

cd ${target}
md5sum -c files.md5 > md5check.out
```

Before deleting the original files, please check the md5check.out under target directory to see if any files failed to pass the MD5 check!
