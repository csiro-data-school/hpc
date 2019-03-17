---
title: "Components of HPC"
teaching: 30
exercises: 30
questions:
- "What makes up a HPC system?"
- "How may I find what resources are available?"
objectives:
- "Understand the key components that make up the HPC"
keypoints:
- "The HPC is made up many computers, referred to as nodes."
- "Nodes have various CPU and memory resources."
- "Nodes are grouped together into partitions."
- "Each partition has a set maximum time-limit allowed for jobs on its nodes."
- "A job sheduling system administers the entire system."
- "The scheduler assigns jobs to nodes, and maintains a queue of waiting jobs."
- "The login node is largely used to interact with the scheduler."
- "`sinfo` can list partitions and nodes available."
---

## The login node

So, you've logged into a HPC system and you're currently interacting with a computer we call the 
"login node". This computer acts as a gateway to a larger system. It's the computer that you'll 
largely interact with, but as all active users are interacting with it at once, it's not to be 
used for any processes that are too intensive or run for more than a moment. In fact, depending 
on the system, any process that runs for too long on the login node will be automatically killed.
Lets learn some things about the login node computer.

> ## Interrogate the login node 
>  
> 1. Run the following command, to report how many CPUs a computer has
> ~~~
> nproc --all
> ~~~
> {: .language-bash}
> 2. Run the following command, which reports available memory (RAM) in gigabytes (-g)
> ~~~
> free -g
> ~~~
> {: .language-bash}
> How does this compare to your own laptop/desktop computer?
> (On Windows, ctrl+shift+esc, click 'More details', then 'Performance' tab)
{: .challenge}

## Nodes of the larger HPC
  
The larger HPC is made up of a great number of networked computers. We call each computer a
"node" of the HPC system. Each node will have its own set of Central Processing Units (CPUs) 
and memory (RAM), but shares access to the same file paths as the rest of the system. This 
allows each node to run independent tasks, while reading the same input files and writing to
the same output directories. A CPU within a compute node, is often referred to as a "core".
  
> ## Other node types
>
> There are two other types of nodes, used in some HPC systems, that we won't be making use of
> in this tutorial.
> 1. Shared memory nodes: Rather than being independent, can access a shared memory pool and 
> bring all CPUs from multiple nodes together to solve a problem in a massively parallel way. 
> Similar to how any single computer can make use of multiple CPUs, but more complex in 
> implementation.
> 2. GPU nodes: Some systems focus on Graphical Processing Units (GPUs) rather than CPUs. GPUs
> started out as a way to quickly render three-dimensional data, e.g. for video games. However,
> as they became more powerful they also became more generalised in purpose. They are especially
> good at parallelisation of simple mathematical calculations over streams of data, e.g. vectors.
{: .callout}
  
Lets learn about the computing nodes available on this HPC.

> ## List nodes of the HPC
>  
> Run the following command, to report a list of Nodes on the HPC system, in long (detailed) 
> format. We'll pipe the command to another command "more" to limit output to one screens fill at
> a time.
> ~~~
> sinfo --Node --long | more
> ~~~
> {: .language-bash}
{: .challenge}

We have a list of nodes. Note the number of CPUs and memory (in Megabytes) available on each. 
Each node will have a state, usually "Idle" or "Allocated" (in use). Also of note, each node
belongs to a "partition". A "partition" is a named grouping nodes of the same type and usage
settings.

> ## List nodes of the HPC 2
>  
> We can add a sort option to the sinfo command to find the nodes with the most memory:
> ~~~
> sinfo --Node --long --sort="-memory" | head
> ~~~
> {: .language-bash}
> Or most CPUs:
> ~~~
> sinfo --Node --long --sort="-cpus" | head
> ~~~
> {: .language-bash}
> (Note: The "-" sign in front of "memory" or "cpus" signals descending order sort.)
> (Note 2: The memory field is being truncated for the 3Tb nodes. Can be better viewed with:)
> ~~~
> sinfo --Node --sort="-memory" --Format="nodelist,partition,cpus,memory" | head
> ~~~
> {: .language-bash}
> We can also limit the list to nodes with a certain state:
> ~~~
> sinfo --Node --long --states="idle" | more
> ~~~
> {: .language-bash}
{: .challenge}


> ## Count nodes of the HPC - Challenge
>  
> Use the `sinfo` command along with `wc -l` ("word count" output for number of lines) to...
> 1. Count how many nodes the HPC system has.
> 2. Count how many nodes are currently "idle".
> 3. Count how many nodes are currently "allocated".
> 
> > ## Solution
> >
> > ~~~
> > sinfo --Node --noheader | wc -l
> > sinfo --Node --noheader --states="idle" | wc -l
> > sinfo --Node --noheader --states="allocated" | wc -l
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}
  
## Partitions 

As mentioned, each node belongs to a "partition", a named grouping nodes of the same type and 
usage settings (e.g. maximum allocation time allowed per job/task).

> ## List partitions of the HPC
>  
> The sinfo command can list partition focused information:
> ~~~
> sinfo --summarize
> ~~~
> {: .language-bash}
> Or to better highlight partition differences:
> ~~~
> sinfo --Format="partition,cpus,memory,nodes,time"
> ~~~
> {: .language-bash}
{: .challenge}

Each partition is made up of a collection of nodes, with the same number of cores per node and 
same amount of memory per node. Each partition also has a time limit setting- the maximum time
that any one job/task is allowed to run on any one node of that partition. 
An assortment of different "shaped" partitions allows the system to handle different types of
tasks efficiently. For example, there are many nodes with lower amounts of memory and a shorter
time limit, for running lots of smaller jobs simultaneously, and a smaller number of nodes with
a very high amount of available memory and a time limit in weeks, for running much larger/longer
jobs.
  
  
## The scheduler
  
Tying the system together is the job scheduler- software that monitors the state of the entire 
system, administers the system, takes requests to run jobs from users and allocates suitable free
compute nodes to them, enforces job time limits, etc.. Jobs waiting for suitable compute nodes to 
become available wait in a job queue, which is also managed by the scheduler.

The scheduler needs to make sensible decisions when allocating jobs to nodes. For example, 
a job flagged as only taking 1 hour to run and only using 4Gb of memory, should probably not be 
allocated to the nodes intended for large memory and long time-limit jobs, even if available
(as then it would not be available when a suitable large job came in otherwise).
  
Our system uses the [Slurm Workload Manager](https://slurm.schedmd.com/) as job scheduler.
The "sinfo" command we used previously was a part of this system.

Another Slurm system command we'll be making more use of later is `squeue`, which lists all
jobs currently queued to run, or running, on the HPC.

> ## Resource slotting exercise
>  
> Divide up a group to roleplay as: one job scheduler, multiple computer nodes and one or more 
> users, to simulate the interaction between the HPC components. A user should ask the scheduler
> to run jobs, the scheduler assigns those jobs to non-busy nodes, nodes do work and report
> back to scheduler, who reports back to user.
{: .challenge}

{% include links.md %}

