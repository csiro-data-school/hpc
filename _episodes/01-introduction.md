---
title: 'Introduction - Why HPC?'
teaching: 15
exercises: 5
questions:
  - 'Why would I be interested in High Performance Computing (HPC)?'
  - 'What can I expect to learn from this course?'
objectives:
  - 'Be able to describe what an HPC system is'
  - 'Identify how an HPC system could benefit you.'
keypoints:
  - 'High Performance Computing (HPC) typically involves connecting to very large computing systems
    elsewhere in the world.'
  - 'These other systems can be used to do work that would either be impossible or much slower or
    smaller systems.'
  - 'The standard method of interacting with such systems is via a command line interface.'
---

Frequently, research problems that use computing can outgrow the desktop or laptop computer where
they started:

- A statistics student wants to cross-validate their model. This involves running the model 1000
  times -- but each run takes an hour. Running on their laptop will take over a month!

- A genomics researcher has been using small datasets of sequence data, but soon will be receiving
  a new type of sequencing data that is 10 times as large. It's already challenging to open the
  datasets on their computer -- analyzing these larger datasets will probably crash it. The memory
  requirements are just too high.

- A technician would like to run a comprehensive data analysis across a dozen different samples.
  Each sample will take a significant time to process. However, each sample is independent. So,
  what if all samples could be processed by different computers, all at the same time, in
  parallel?

These problems can be solved by access to multiple computers at the same time, with perhaps more
processors, more memory, and more available storage than a local laptop or desktop computer.  
This is the offering of High Performance Computing.

> # And what do you do?
>
> Talk to your neighbour, office mate or [rubber duck](https://rubberduckdebugging.com/) and
> reflect on examples from your experience where a data analysis would be unfeasible or
> inconvenient if performed on your own computer.
> How could more computing help you do more or better research?
> {: .challenge }

# The use of HPC

## A standard Laptop for standard tasks

Today, people coding or analysing data typically work with laptops.

{% include figure.html url="" max-width="10%" file="/fig/200px-laptop-openclipartorg-aoguerrero.svg"
 alt="A standard laptop" caption="" %}

Let's dissect what resources programs running on a laptop require:

- the keyboard and/or touchpad is used to tell the the computer what to do (**Input**)
- the internal computing resources **Central Processing Unit** and **Memory** perform calculation
- the display depicts progress and results (**Output**)

Schematically, this can be reduced to the following:

{% include figure.html max-width="30%" file="/fig/Simple_Von_Neumann_Architecture.svg"
alt="Schematic of how a computer works" caption="" %}

## When tasks take too long

When the task to solve become more heavy on computations, the operations are typically out-sourced
from the local laptop or desktop to elsewhere. Have you ever asked Google Maps for directions?
The capabilities of your laptop are typically not enough to calculate such routes so spontaneously.
So you use a website, which in turn runs on a server that is almost certaintly not in the same
room as you are.

{% include figure.html url="" max-width="10%" file="/fig/servers-openclipartorg-ericlemerdy.svg"
alt="A rack half full with servers" caption="" %}

Note here, that a server is generally a noisy computer mounted into a rack cabinet which in turn
resides in a data center. The internet made it possible that these data centers do not require to
be nearby your laptop. What people call **the cloud** is mostly a web-service where you can rent
such servers by providing your credit card details and by clicking together the specs of this
remote resource.

The server itself has no direct display or input methods attached to it. But most importantly, it
has much more storage, memory and compute capacity than your laptop will ever have. In any case,
you need a local device (laptop, workstation, mobile phone or tablet) to interact with this remote
machine, people typically call 'a server'.

## When one server is not enough

If the computational task or analysis to complete is daunting for a single server, larger
agglomerations of servers are used. These go by the name of clusters or super computers.

{% include figure.html url="" max-width="10%"
file="/fig/serverrack-openclipartorg-psteinb-basedon-ericlemerdy.svg" alt="A rack with servers"
caption="" %}

When interacting with such systems, providing input data, code, instructions, options, etc.,
a GUI-style (point and click) interface is often discarded in favor of using a command line
interface. This imposes a double paradigm shift for prospective users:

1. Getting used to working with the command line
2. Getting used to working with a distributed set of computers (called nodes)

> # I've never used a server, have I?
>
> Take a minute and think about which of your daily interactions with a computer may require a
> remote server or even cluster to provide you with results.
> {: .challenge }

{% include links.md %}
