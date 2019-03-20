---
title: "Submit a batch Job"
teaching: 15
exercises: 15
questions:
- "How do I submit a job to the batch system?"
objectives:
- "Understand how to work with batch system on the HPC"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

## Running a batch job

The most basic use of the scheduler is to run a command non-interactively. Any command (or series 
of commands) that you want to run on the cluster is called a *job*, and the process of using a
scheduler to run the job is called *batch job submission*.

In this case, the job we want to run is just a shell script. Let's create a demo shell script to 
run as a test.

> ## Creating our test job
> 
> Using your favourite text editor, create the following script, save it as `example-job.sh`.
> ~~~
>#!/bin/bash
>
> echo 'This script is running on:'
> hostname
> sleep 120
> ~~~
> {: .language-bash}
> Run the script using 
> ~~~
> bash example-job.sh
> ~~~
> {: .language-bash}
> Does it run on the cluster or just our login node?
{: .challenge}

If you completed the previous challenge successfully, you probably realise that there is a
distinction between running the job through the scheduler and just "running it". To submit this job
to the scheduler, we use the `sbatch` command.

~~~
sbatch example-job.sh
~~~
{: .language-bash}
~~~
Submitted batch job 36855
~~~
{: .output}

And that's all we need to do to submit a job. Our work is done -- now the scheduler takes over and
tries to run the job for us. While the job is waiting to run, it goes into a list of jobs called 
the *queue*. To check on our job's status, we check the queue using the command `squeue`.

~~~
squeue -u yourUsername
~~~
{: .language-bash}
~~~
JOBID USER         ACCOUNT     NAME           ST REASON START_TIME         TIME TIME_LEFT NODES CPUS
36856 yourUsername yourAccount example-job.sh R  None   2017-07-01T16:47:02 0:11 59:49     1     1
~~~
{: .output}

We can see all the details of our job, most importantly that it is in the "R" or "RUNNING" state.
Sometimes our jobs might need to wait in a queue ("PENDING") or have an error. The best way to check
our job's status is with `squeue`. Of course, running `squeue` repeatedly to check on things can be
a little tiresome. To see a real-time view of our jobs, we can use the `watch` command. `watch`
reruns a given command at 2-second intervals. This is too frequent, and will likely upset your system
administrator. You can change the interval to a more resonable value, for example 60 seconds, with the
`-n 60` parameter. Let's try using it to monitor another job.

~~~
sbatch example-job.sh
watch -n 60 squeue -u yourUsername
~~~
{: .language-bash}

You should see an auto-updating display of your job's status. When it finishes, it will disappear
from the queue. Press `Ctrl-C` when you want to stop the `watch` command.

## Customising a job

The job we just ran used all of the scheduler's default options. In a real-world scenario, that's
probably not what we want. The default options represent a reasonable minimum. Chances are, we will
need more cores, more memory, more time, among other special considerations. To get access to these
resources we must customise our job script.

Comments in UNIX (denoted by `#`) are typically ignored. But there are exceptions. For instance the
special `#!` comment at the beginning of scripts specifies what program should be used to run it
(typically `/bin/bash`). Schedulers like SLURM also have a special comment used to denote special
scheduler-specific options. Though these comments differ from scheduler to scheduler, SLURM's
special comment is `#SBATCH`. Anything following the `#SBATCH` comment is interpreted as an
instruction to the scheduler.

Let's illustrate this by example. By default, a job's name is the name of the script, but the 
`--job-name=` option can be used to change the name of a job.

Submit the following job (`sbatch example-job.sh`):

~~~
#!/bin/bash
#SBATCH --job-name=new_name

echo 'This script is running on:'
hostname
sleep 120
~~~

~~~
squeue -u yourUsername
~~~
{: .language-bash}
~~~
JOBID USER         ACCOUNT     NAME     ST REASON   START_TIME TIME TIME_LEFT NODES CPUS
38191 yourUsername yourAccount new_name PD Priority N/A        0:00 1:00:00   1     1
~~~
{: .output}

Fantastic, we've successfully changed the name of our job!

> ## Setting up email notifications
> 
> Jobs on an HPC system might run for days or even weeks. We probably have better things to do than
> constantly check on the status of our job with `squeue`. Looking at the
> [online documentation for `sbatch`](https://slurm.schedmd.com/sbatch.html) (you can also google
> "sbatch slurm"), can you set up our test job to send you an email when it finishes?
> 
> Hint: you will need to use the `--mail-user` and `--mail-type` options.
{: .challenge}


## Cancelling a job

Sometimes we'll make a mistake and need to cancel a job. This can be done with the `scancel`
command. Let's submit a job and then cancel it using its job number.

~~~
sbatch example-job.sh
squeue -u yourUsername
~~~
{: .language-bash}
~~~
Submitted batch job 38759

JOBID USER         ACCOUNT     NAME           ST REASON   START_TIME TIME TIME_LEFT NODES CPUS
38759 yourUsername yourAccount example-job.sh PD Priority N/A        0:00 1:00      1     1
~~~
{: .output}

Now cancel the job with it's job number. Absence of any job info indicates that the job has been
successfully cancelled.

~~~
scancel 38759
squeue -u yourUsername
~~~
{: .language-bash}
~~~
JOBID  USER  ACCOUNT  NAME  ST  REASON  START_TIME  TIME  TIME_LEFT  NODES  CPUS
~~~
{: .output}

> ## Cancelling multiple jobs
>
> We can also all of our jobs at once using the `-u` option. This will delete all jobs for a
> specific user (in this case us). Note that you can only delete your own jobs.
>
> Try submitting multiple jobs and then cancelling them all with `scancel -u yourUsername`.
{: .challenge}

## Other types of jobs

Up to this point, we've focused on running jobs in batch mode. SLURM also provides the ability to
run tasks as a one-off or start an interactive session.

There are very frequently tasks that need to be done semi-interactively. Creating an entire job
script might be overkill, but the amount of resources required is too much for a login node to
handle. A good example of this might be building a genome index for alignment with a tool like
[HISAT2](https://ccb.jhu.edu/software/hisat2/index.shtml). Fortunately, we can run these types of
tasks as a one-off with `srun`.

`srun` runs a single command on the cluster and then exits. Let's demonstrate this by running the
`hostname` command with `srun`. (We can cancel an `srun` job with `Ctrl-c`.)

~~~
srun hostname
~~~
{: .language-bash}
~~~
gra752
~~~
{: .output}

`srun` accepts all of the same options as `sbatch`. However, instead of specifying these in a
script, these options are specified on the command-line when starting a job. To submit a job that
uses 2 CPUs for instance, we could use the following command:

~~~
srun --cpus-per-task=2 echo "This task will use 2 CPUs."
~~~
{: .language-bash}
~~~
This task will use 2 CPUs.
~~~
{: .output}

Typically, the resulting shell environment will be the same as that for `sbatch`.

### Interactive jobs

Sometimes, you will need a lot of resource for interactive use. Perhaps it's our first time running
an analysis or we are attempting to debug something that went wrong with a previous job.
Fortunately, SLURM makes it easy to start an interactive job with `srun`:

TO DO : salloc??  sinteractive??

## Job output

### Default slurm-out files

### Application directed out files 

TO DO
  * sacct


{% include links.md %}


