---
title: "Running a batch job"
teaching: 15
exercises: 0
questions:
- "How do I run my computations on a compute node on the background?"
objectives:
- "Use `grep` to select lines from text files that match simple patterns."
keypoints:
- "`grep` selects lines in files that match patterns."
- "`--help` is a flag supported by many bash commands, and programs that can be run from within Bash, to display more information on how to use these commands or programs."
---

Interactive jobs are great if you need to do something quick, or perhaps visualize some data. If you have some code which runs for seven hours, interactive jobs are not a great idea. Please keep in mind that an interactive job gets killed if you close the SSH connetion. So for example, you connect to Palmetto from your laptop, start an interactive job, but then your laptop runs out of battery power and you can't find your charger. SSH client quits, and your interactive job is killed. 

If you have some truly serious, multi-hour computation project (and that's what Palmetto is really good for), a better idea is to run it on the background. This is called a *batch job*. You submit it in a fashin which is conceptually similar to an interactive job, but then it runs on the compute node on the background until it's over. If it needs to take two days, it takes two days. You can quit the SSH client or close your laptop, it won't affect the batch job.

To submit a batch job, we usually create a separate file called a *PBS script*. This file tells the scheduler about the resoures we want to request, and then what should be done once we get on a compute node. 

Let us go through an example. We will create a Matlab script, and then create a


bigmatrix.m
bigmatrix.sh
qsub bigmatrix.sh
qstat -u username
qstat -f job_id
