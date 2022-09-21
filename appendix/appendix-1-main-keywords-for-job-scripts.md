# Main Keywords for Job Scripts

&#x20;**** The method of resource allocation needs to be specified for a desired job by using the appropriate keywords in the job script. The main keywords are presented as follows; users can create a job script file using these keywords. ****&#x20;

&#x20;

**- job-name ( -J, --job-name )**

&#x20;**** Specifies the job name. If the job name is not specified, the name of the script file is adopted as the job name.

&#x20;

**- time ( -t, --time )**

&#x20;**** Indicates the expected job duration; it is recommended to set this slightly longer than the actual expected job duration. If the wall time limit for the partition is exceeded, the job is not submitted. If the job is not completed within the specified time, the SLURM scheduler forcibly terminates the job.

&#x20;

**- partition ( -p, --partition )**

&#x20;**** Specifies the SLURM partition for the job execution. The partition name can be checked with the sinfo command.

&#x20;

**- nodes ( -N, --nodes )**

&#x20; ****  Specifies the number of nodes to allocate for the job.

&#x20;

**- ntasks ( -n, --ntasks )**

&#x20; Specifies the number of processes to allocate for the job.

&#x20;

**- tasks-per-node ( --tasks-per-node )**

&#x20; Specifies the number of processes to allocate per node.

&#x20;

**- input ( -i, --input )**

&#x20; ****  Specifies the standard input.

&#x20;

**- cpus-per-task ( -c, --cpus-per-task )**

&#x20;  ****   Specifies the number of CPUs required for each job task.

&#x20;

**- output ( -o, --output )**

&#x20; ****  Specifies the standard output.

&#x20;   ****    %x: The name specified for "job name" is adopted as the file name.

&#x20;   ****    %j: The "job ID" assigned when submitting a job is used as the file name.

&#x20;   ****    %a: The "job array ID" (index) number is adopted as the file name.

&#x20;   ****    %u: The "user ID" is used as the file name.

&#x20;

**- error ( -e, --error )**

&#x20;  ****   Specifies the standard error.

&#x20;   ****    %x: The name specified for the "job name" is used as the file name.

&#x20;   ****    %j: The "job ID" assigned when submitting a job is used as the file name.

&#x20;   ****    %a: The "job array ID" (index) number is used as the file name.

&#x20;   ****    %u: The "user ID" is used as the file name.

&#x20;

**- dependency ( -d, --dependency )**

&#x20; Sets the job dependency. The job starts after the specified job is completed.

&#x20;

&#x20;

※ Detailed manual: Refer to [http://slurm.schedmd.com/](http://slurm.schedmd.com/)&#x20;

{% hint style="info" %}
2021년 11월 25일에 마지막으로 업데이트되었습니다.
{% endhint %}
