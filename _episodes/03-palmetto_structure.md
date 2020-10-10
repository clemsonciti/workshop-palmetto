---
title: "The structure of the Palmetto Cluster"
teaching: 15
exercises: 0
questions:
- "How can I access the Palmetto cluster from my local machine?"
objectives:
- "Logging into Palmetto from a local Mac / Windows computer."
---

Basic structure of the cluster:

<img src="../fig/palmetto-structure.png" alt="Structure of the Palmetto Cluster" style="height:350px">

The computers that make up the Palmetto cluster are called *nodes*. Most of the nodes on Palmetto are *compute nodes*, 
that can perform fast calculations on large amounts of data. There is also a special node called the *login node*; it runs the server, 
which works like the interface 
between the cluster 
and the outside world. The people with Palmetto accounts can log into the server by running a client (such as `ssh`) on their local machines. 
Our client program passes our login credentials to this server, and if we are allowed to log in, the server runs a shell for us. 
Any commands that we enter into this shell are executed not by our own machines, but by the login node.

Another special node is the *scheduler*; Palmetto users can get from the
login node to the compute nodes by submitting a request to the scheduler, and the scheduler will assign them to the most appropriate compute node.
Palmetto also has a few so-called "service" nodes, which serve special purposes like transfering code and data to and from the cluster, and hsting web applications.

To see the hardware specifications of the compute nodes, please type

~~~
$ cat /etc/hardware-table
~~~
{: .bash}


This will print out a text file with the hardware info. Please make sure you type exactly as shown; Linux is case-sensitive space-sensitive, 
and typo-sensitive. The output will look something like this:

~~~
[gyourga@login001 ~]$ cat /etc/hardware-table

PALMETTO HARDWARE TABLE      Last updated:  Jun 15 2020

PHASE COUNT  MAKE   MODEL    CHIP(0)                CORES  RAM(1)    /local_scratch   Interconnect     GPUs  SSD

 0      6    HP     DL580    Intel Xeon    7542       24   505 GB(2)    99 GB         1ge               0     0
 0      5    Dell   R820     Intel Xeon    E5-4640    32   750 GB(2)   740 GB(12)     10ge              0     0
 0      1    HP     DL560    Intel Xeon    E5-4627v4  40   1.5 TB(2)   881 GB         10ge              0     0
 0      1    Dell   R830     Intel Xeon    E5-4627v4  40   1.0 TB(2)   880 GB         10ge              0     0
 0      1    HPE    DL560    Intel Xeon    6138G      80   1.5 TB(2)   3.6 TB         10ge              0     0
 0      1    HPE    DL560    Intel Xeon    6148G      80   1.5 TB(2)   3.6 TB         10ge              0     0
 0      1    HPE    DL560    Intel Xeon    6148G      80   1.5 TB(2)   745 GB(12)     10ge              0     0

 1a   116    Dell   R610     Intel Xeon    E5520       8    31 GB      220 GB         1g                0     0
 1b    40    Dell   R610     Intel Xeon    E5645      12    92 GB      220 GB         1g                0     0
 2a    30    Dell   R620     Intel Xeon    E5-2660    16   375 GB      2.7 TB         1g                0     0
 2b   134    Dell   PE1950   Intel Xeon    E5410       8    15 GB       37 GB         1g                0     0
 3    224    Sun    X2200    AMD   Opteron 2356        8    15 GB      193 GB         1g                0     0
 4    323    IBM    DX340    Intel Xeon    E5410       8    15 GB      111 GB         1g                0     0
 5a   310    Sun    X6250    Intel Xeon    L5420       8    31 GB       31 GB         1g                0     0
 5b     9    Sun    X4150    Intel Xeon    E5410       8    31 GB       99 GB         1g                0     0
 5c    19    Dell   R510     Intel Xeon    E5640       8    22 GB        7 TB         1g                0     0
 5d    20    Dell   R520     Intel Xeon    E5-2450    12    46 GB      2.7 TB         1g                0     0
 6     66    HP     DL165    AMD   Opteron 6176       24    46 GB      193 GB         1g                0     0

 7a    42    HP     SL230    Intel Xeon    E5-2665    16    62 GB      240 GB         56g, fdr, 10ge    0     0
 7b    12    HP     SL250s   Intel Xeon    E5-2665    16    62 GB      240 GB         56g, fdr, 10ge    2(3)  0
 8a    71    HP     SL250s   Intel Xeon    E5-2665    16    62 GB      900 GB         56g, fdr, 10ge    2(4)  300 GB(7)
 8b    57    HP     SL250s   Intel Xeon    E5-2665    16    62 GB      420 GB         56g, fdr, 10ge    2(4)  0
 8c    88    Dell   PEC6220  Intel Xeon    E5-2665    16    62 GB      350 GB         56g, fdr, 10ge    0     0
 9     72    HP     SL250s   Intel Xeon    E5-2665    16   126 GB      420 GB         56g, fdr, 10ge    2(4)  0
10     80    HP     SL250s   Intel Xeon    E5-2670v2  20   126 GB      800 GB         56g, fdr, 10ge    2(4)  0
11a    40    HP     SL250s   Intel Xeon    E5-2670v2  20   126 GB      800 GB         56g, fdr, 10ge    2(6)  0
11b     4    HP     SL250s   Intel Xeon    E5-2670v2  20   126 GB      800 GB         56g, fdr, 10ge    0     0
12     30    Lenovo NX360M5  Intel Xeon    E5-2680v3  24   126 GB      800 GB         56g, fdr, 10ge    2(6)  0
13     24    Dell   C4130    Intel Xeon    E5-2680v3  24   126 GB      1.8 TB         56g, fdr, 10ge    2(6)  0
14     12    HPE    XL1X0R   Intel Xeon    E5-2680v3  24   126 GB      880 GB         56g, fdr, 10ge    2(6)  0
15     32    Dell   C4130    Intel Xeon    E5-2680v3  24   126 GB      1.8 TB         56g, fdr, 10ge    2(6)  0
16     40    Dell   C4130    Intel Xeon    E5-2680v4  28   126 GB      1.8 TB         56g, fdr, 10ge    2(8)  0
17     20    Dell   C4130    Intel Xeon    E5-2680v4  28   126 GB      1.8 TB         56g, fdr, 10ge    2(8)  0
18a     2    Dell   C4140    Intel Xeon    6148G      40   372 GB      1.9 TB(12)    100g, hdr, 10ge    4(9)  0
18b    65    Dell   R740     Intel Xeon    6148G      40   372 GB      1.8 TB        100g, hdr, 25ge    2(10) 0
18c    10    Dell   R740     Intel Xeon    6148G      40   748 GB      1.8 TB        100g, hdr, 25ge    2(10) 0
19a    28    Dell   R740     Intel Xeon    6248G      40   372 GB      1.8 TB        100g, hdr, 25ge    2(10) 0
19b     4    HPE    XL170    Intel Xeon    6252G      48   372 GB      1.8 TB         56g, fdr, 10ge    0     0

  *** PBS resource requests are always lowercase ***

(0) CHIP has 3 resources:   chip_manufacturer, chip_model, chip_type
(1) Leave 2 or 3GB for the operating system when requesting memory in PBS jobs
(2) Specify queue "bigmem" to access the large memory machines, only ncpus and mem are valid PBS resource requests
(3) 2 NVIDIA Tesla M2075 cards per node, use resource request "ngpus=[1|2]" and "gpu_model=m2075"
(4) 2 NVIDIA Tesla K20m cards per node, use resource request "ngpus=[1|2]" and "gpu_model=k20"
(5) 2 NVIDIA Tesla M2070-Q cards per node, use resource request "ngpus=[1|2]" and "gpu_model=m2070q"
(6) 2 NVIDIA Tesla K40m cards per node, use resource request "ngpus=[1|2]" and "gpu_model=k40"
(7) Use resource request "ssd=true" to request a chunk with SSD in location /ssd1, /ssd2, and /ssd3 (100GB max each)
(8) 2 NVIDIA Tesla P100 cards per node, use resource request "ngpus=[1|2]" and "gpu_model=p100"
(9) 4 NVIDIA Tesla V100 cards per node with NVLINK2, use resource request "ngpus=[1|2|3|4]" and "gpu_model=v100nv"
(10)2 NVIDIA Tesla V100 cards per node, use resource request "ngpus=[1|2]" and "gpu_model=v100"
(11)Phase18a nodes contain NVMe storage for /local_scratch.
(12)local_scratch is housed entirely on SSD
~~~






hardware table
whatsfree
Login and compute nodes
node ownership
condominium model