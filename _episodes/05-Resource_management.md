---
title: "Resource Management"
teaching: 15
exercises: 15
questions:
- "How much time and CPUs do I need for my job?"
objectives:
- "Understand/demonstrate how to request appropriate resources for a job"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

### Resource requests

One thing that is absolutely critical when working on an HPC system is specifying the resources 
required to run a job. This allows the scheduler to find the right time and place to schedule our 
job. If you do not specify requirements (such as the amount of time you need), you will likely be
stuck with your site's default resources, which is probably not what we want.

The following are several key resource requests:

* `-n <nnodes>` - how many nodes does your job need? 

* `-c <ncpus>` - How many CPUs does your job need?

* `--mem=<megabytes>` - How much memory on a node does your job need in megabytes? You can also
  specify gigabytes using by adding a little "g" afterwards (example: `--mem=5g`)

* `--time <days-hours:minutes:seconds>` - How much real-world time (walltime) will your job take to
  run? The `<days>` part can be omitted.

Note that just *requesting* these resources does not make your job run faster! We'll talk more 
about how to make sure that you're using resources effectively in a later episode of this lesson.

> ## Submitting resource requests
>
> Submit a job that will use 2 CPUs, 4 gigabytes of memory, and 5 minutes of walltime.
{: .challenge}

> ## Job environment variables
>
> When SLURM runs a job, it sets a number of environment variables for the job. One of these will
> let us check our work from the last problem. The `SLURM_CPUS_PER_TASK` variable is set to the
> number of CPUs we requested with `-c`. Using the `SLURM_CPUS_PER_TASK` variable, modify your job
> so that it prints how many CPUs have been allocated.
{: .challenge}

Resource requests are typically binding. If you exceed them, your job will be killed. Let's use
walltime as an example. We will request 30 seconds of walltime, and attempt to run a job for two
minutes.

~~~
#!/bin/bash
#SBATCH -t 0:0:30

echo 'This script is running on:'
hostname
sleep 120
~~~

Submit the job and wait for it to finish. Once it is has finished, check the log file.

~~~
sbatch example-job.sh
watch -n 60 squeue -u yourUsername
cat slurm-38193.out
~~~
{: .language-bash}
~~~
This job is running on:
gra533
slurmstepd: error: *** JOB 38193 ON gra533 CANCELLED AT 2017-07-02T16:35:48 DUE TO TIME LIMIT ***
~~~
{: .output}

Our job was killed for exceeding the amount of resources it requested. Although this appears harsh,
this is actually a feature. Strict adherence to resource requests allows the scheduler to find the
best possible place for your jobs. Even more importantly, it ensures that another user cannot use
more resources than they've been given. If another user messes up and accidentally attempts to use
all of the CPUs or memory on a node, SLURM will either restrain their job to the requested resources
or kill the job outright. Other jobs on the node will be unaffected. This means that one user cannot
mess up the experience of others, the only jobs affected by a mistake in scheduling will be their
own.


* Requirements (with  example)
* Headers in sbatch file (mem,  nodes, CPUs, etc.)
* sacct to access job information (example)
* request resources based on job example
* Exercise: re-submit previous jobs with good/poor resource requests

* Exercise: How would I use this in my current project? What would be the first 3 steps in the process? How do I get help?

{% include links.md %}


