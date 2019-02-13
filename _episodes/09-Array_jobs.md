---
title: "Submitting array jobs"
teaching: 15
exercises: 15
questions:
- "FIXME"
objectives:
- "Be able to submit, monitor and cancel array jobs applicable for several input scenarios"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

* Why
* Scenarios / examples
* Distributed computing
* `--array=1-10` 
* simple example 

```
#!/bin/bash
##FIXME include additional opts here
#SBATCH --array=1-10

sleep 120 
hostname
echo $SLURM_TASK_ID
```
* complex example (real data crunching)
* `squeue, sacct, scancel`

{% include links.md %}
