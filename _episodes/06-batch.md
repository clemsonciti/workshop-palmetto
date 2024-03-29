---
title: "Running a batch job"
teaching: 15
exercises: 0
questions:
- "How do I run my computations on a compute node on the background?"
objectives:
- PBS scripts, `qstat`, `checkqueuecfg`, `nano`
keypoints:
- "batch jobs don't require interaction with the user and run on the compute nodes on the background"
- "to submit a batch job, users need to provide a PBS script which is passed to the scheduler"
- "jobs are assigned to queues, according to the amount of requested resources"
- "different queues have different limits on the walltime and the number of parallel jobs"
---

Interactive jobs are great if you need to do something quick, or perhaps visualize some data. If you have some code which runs for seven hours, interactive jobs are not a great idea. Please keep in mind that an interactive job gets killed if you close the SSH connection. So for example, you connect to Palmetto from your laptop, start an interactive job, but then your laptop runs out of battery power and you can't find your charger. SSH client quits, and your interactive job is killed. 

If you have some truly serious, multi-hour computation project (and that's what Palmetto is really good for), a better idea is to run it on the background. This is called a *batch job*. You submit it in a fashion which is conceptually similar to an interactive job, but then it runs on the compute node on the background until it's over. If it needs to take two days, it takes two days. You can quit the SSH client or close your laptop, it won't affect the batch job.

To submit a batch job, we usually create a separate file called a *PBS script*. This file asks the scheduler for specific resources, and then specifies the actions that will be done once we get on a compute node. 

Let us go through an example. We will use bath mode to compute the first eigenvalue of a large matrix. We will create two scripts: a Matlab script which does the computation, and a PBS script which will execute the Matlab script on a compute node in batch mode.

Palmetto has a simple text editor which is called `nano`. It doesn't offer any fancy formatting, but it suffices for ceating and editing simple texts. Let's go to our home directory and create the Matlab script:

~~~
cd
nano bigmatrix.m
~~~
{: .bash}

This will open the `nano` text editor:

<img src="../fig/nano_empty.png" style="height:350px">

Inside the editor, type this:
~~~
a = randn (5000, 5000);
[v,d] = eig (a); 
fprintf ('first eigenvalue = %.5f\n', d(1,1));
~~~
Instead of typing, you can copy the text from the Web browser and paste it into `nano`. Windows users can paste with `Shift`+`Ins` (or by right-clicking the mouse). Mac users can paste with `Cmd`+`V`. At the end, your screen should look like this:

<img src="../fig/nano_matlab.png" style="height:350px">

To save it, press `Ctrl`+`O`, and hit enter. To exit the editor, press `Ctrl`+`X`. To make sure the text is saved properly, print it on screen using the `cat` command:

~~~
cat bigmatrix.m
~~~
{: .bash}

Now, let's create the PBS script:

~~~
nano bigmatrix.sh
~~~
{: .bash}

Inside the `nano` text editor, type this (or paste from the Web browser):

~~~
#!/bin/bash
#
#PBS -N bigmatrix
#PBS -l select=1:ncpus=10:mem=10gb:interconnect=1g
#PBS -l walltime=0:30:00
#PBS -o output.txt
#PBS -j oe

cd $HOME
module load matlab/2020a
matlab -r "bigmatrix"
~~~

Let's go through the script, line by line. The first cryptic line says that it's a script that is executed by the Linux shell. The next line is empty, followed by five lines that are the instructions to the scheduler (they start with `#PBS`):

- `-N` specifiies the name of the job (could be anything, I called it `bigmatrix` for the sake of consistency)
- the first `-l` line is the specification of resources: one node, ten CPUs, ten Gb of RAM, 1g interconnect
- the second `-l` line is the amount of walltime (thirty minutes);
- `-o` specifies the name of the output file where the Matlab output will be printed;
- `-j oe` means "join output and error", which is, if any errors happen, they will be written into `output.txt`.

The rest is the instructions what to do once we get on the compute node that satisfies the request we provided in `-l`: go to the home directory, load the Matlab module, and execute the Matlab script called bigmatrix.m that we have created. Save the PBS script and exit `nano` (`Ctrl`+`O`, `ENTER`, `Ctrl`+`X`). 

A very common question is how much walltime we should ask for. It's a tricky question beause there is no way of knowing how much time you will need until you actually try it. My rule of thumb is: make a rough guess, and ask for twice as much. The `bigmatrix.m` script takes at most 15 minutes (usually it runs under five minutes), so I ask for half an hour.

Now, let's submit our batch job!

~~~
qsub bigmatrix.sh
~~~
{: .bash}

We use the same command `qsub` that we have previously used for an interactive job, but now it's much simpler, because all the hard work went into creating the PBS shell script `bigmatrix.sh` and `qsub` reads all the necessary information from there. If the submission was successful, it will give you the job ID, for example:

~~~
632585.pbs02
~~~
{: .output}

We can monitor the job's progress with the `qstat` command. This is an example to list all jobs that are currently executed by you:

~~~
qstat -u <your Palmetto username>
~~~
{: .bash}

You should see something like this:

~~~
pbs02:
                                                            Req'd  Req'd   Elap
Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
--------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
632585.pbs02    gyourga  c1_sing* bigmatrix  24385*   1  10   10gb 00:30 R 00:00
~~~
{: .output}

You see the job ID, your Palmetto username, the name of the queue (more on that later), the name of the job (`bigmatrix`), the resources requested (1 node, 10 CPUs, 10 gb of RAM, half an hour of walltime). The letter `R` means that the job is running (`Q` means "queued", and `F` means "finished"), and then it shows for how long it's been running (it basically just started).

Wait a little bit and do `qstat` again (you can hit the `UP` arrow to show the previous command). `Elap time` should now be a bit longer. The script should take five minutes or so to execute. If you enter `qstat -u <your Palmetto username>` and the list is empty, then congratulations, we are done!

If everything went well, you should now see the file `output.txt`. Let's print it on screen:

~~~
cat output.txt
~~~
{: .bash}

~~~
MATLAB is selecting SOFTWARE OPENGL rendering.

                            < M A T L A B (R) >
                  Copyright 1984-2020 The MathWorks, Inc.
              R2020a Update 1 (9.8.0.1359463) 64-bit (glnxa64)
                               April 9, 2020


To get started, type doc.
For product information, visit www.mathworks.com.

>> >> >> first eigenvalue = -64.79945
~~~
{: .output}

Your first eigenvalue might be different because it's a random matrix.

Another way to use `qstat` is to list the information about a particular job. Here, instead of `-u`, we use the `-xf` option, followed by the Job ID:

~~~
qstat -xf 632585
~~~
{: .bash}

This will give you a lot of information about the job, which is really useful for debugging. If you have a problem and you need our help, it is very helpful to us if you provide the job ID so we can do `qstat -xf` on it and get the job details.

How many jobs can you run at the same time? It depends on how much resources you ask for. If each job asks for a small amount of resources, you can do a large amount of jobs simultaneously. If each job needs a large amount of resources, only a few of them can be running simultaneously, and the rest of them will be waiting in the queue until the jobs that are running are completed. This is a way to ensure that Palmetto is used fairly.

These limits of the number of simultaneous jobs is not carved in stone, but it changes depending on how much Palmetto is used at the moment. To see the current queue configuration, you can execute this command (note that it only works on the login node):

~~~
checkqueuecfg
~~~
{: .bash}

This script produces a lot of output. Here's the first few lines:
~~~
1G QUEUES     min_cores_per_job  max_cores_per_job   max_mem_per_queue  max_jobs_per_queue   max_walltime
c1_solo                       1                  1             20000gb                2000      336:00:00
c1_single                     2                 24             30000gb                 250      336:00:00
c1_tiny                      25                128            102400gb                 100      336:00:00
c1_small                    129                512             81920gb                  20      336:00:00
c1_medium                   513               2048             81920gb                   5      336:00:00
c1_large                   2049               4096             65536gb                   2      336:00:00

IB QUEUES     min_cores_per_job  max_cores_per_job   max_mem_per_queue  max_jobs_per_queue   max_walltime
c2_single                     1                 56             10000gb                  25       72:00:00
c2_tiny                      57                200             32000gb                  10       72:00:00
c2_small                    201                512             21504gb                   3       72:00:00
c2_medium                   513               2048             32768gb                   2       72:00:00
c2_large                   2049               4096             32768gb                   1       72:00:00

c2_fdr_single                  1                 56             70000gb                 175       72:00:00
c2_fdr_tiny                   57                200            112000gb                  35       72:00:00
c2_fdr_small                 201                512             21504gb                   3       72:00:00
c2_fdr_medium                513               2048             32768gb                   2       72:00:00
c2_fdr_large                2049               4096             32768gb                   1       72:00:00

c2_hdr_single                  1                 56              6000gb                   5       72:00:00
c2_hdr_tiny                   57                200              9600gb                   3       72:00:00
c2_hdr_small                 201                512             14336gb                   2       72:00:00
c2_hdr_medium                513               2048             16384gb                   1       72:00:00
c2_hdr_large                2049               4096             32768gb                   1       72:00:00

~~~
{: .output}

One thing to note is that 1g nodes have maximum walltime of 336 hours (two weeks), and InfiniBand (hdr and fdr) nodes have maximum walltime of 72 hours (three days). Since the GPUs are only installed on the InfiniBand nodes, any job that asks for a GPU will also be subject to 72-hour limit. The maximum number of simultaneous jobs really depends on how much CPUs and memory you are asking; for example, for 1 node, 10 CPUs and 10 Gb of RAM (what we asked for in our `bigmatrix` job), we can run 250 jobs on 1g nodes (queue name `c1_single`), but only 25 jobs on InfiniBand nodes (queue name `c2_single`). This number changes day to day, depending on how busy the cluster is --on busy days, this number is lowered so more people have a chance to run their jobs on Palmetto.
