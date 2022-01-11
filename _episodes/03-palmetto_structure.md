---
title: "The structure of the Palmetto Cluster"
questions:
- "What is the structure of the Palmetto Cluster?"
teaching: 15
exercises: 0
objectives:
- compute and login nodes, hardware table, `whatsfree`
keypoints:
- "Palmetto contains more than 2000 interconnected compute nodes"
- "a phase is a group of compute nodes that have the same architecture (CPUs, RAM, GPUs)"
- "a specialized login node runs the SSH server"
---

The computers that make up the Palmetto cluster are called *nodes*. Most of the nodes on Palmetto are *compute nodes*, 
that can perform fast calculations on large amounts of data. There is also a special node called the *login node*; it runs the server, 
which works like the interface 
between the cluster 
and the outside world. The people with Palmetto accounts can log into the server by running a client (such as `ssh`) on their local machines. 
Our client program passes our login credentials to this server, and if we are allowed to log in, the server runs a shell for us. 
Any commands that we enter into this shell are executed not by our own machines, but by the login node.

<img src="../fig/palmetto-structure.png" alt="Structure of the Palmetto Cluster" style="height:350px">

Another special node is the *scheduler*; Palmetto users can get from the
login node to the compute nodes by submitting a request to the scheduler, and the scheduler will assign them to the most appropriate compute node.
Palmetto also has a few so-called "service" nodes, which serve special purposes like transferring code and data to and from the cluster, and hosting web applications.

To see the hardware specifications of the compute nodes, please type

~~~
cat /etc/hardware-table
~~~
{: .bash}


This will print out a text file with the hardware info. Please make sure you type exactly as shown; Linux is case-sensitive space-sensitive, 
and typo-sensitive. The output will look something like this:

~~~
PALMETTO HARDWARE TABLE      Last updated:  Oct 19 2021

PHASE COUNT  MAKE   MODEL    CHIP(0)                CORES  RAM(1)    /local_scratch   Interconnect     GPUs

BIGMEM nodes
 0a     3    HP     DL580    Intel Xeon    7542       24   1.0 TB(2)    99 GB         10ge              0
 0b     5    Dell   R820     Intel Xeon    E5-4640    32   750 GB(2)   740 GB(13)     10ge              0
 0c     1    Dell   R830     Intel Xeon    E5-4627v4  40   1.0 TB(2)   880 GB         10ge              0
 0d     2    Lenovo SR650    Intel Xeon    6240       36   1.5 TB(2)   400 GB(13)     10ge              0
 0e     1    HP     DL560    Intel Xeon    E5-4627v4  40   1.5 TB(2)   881 GB         10ge              0
 0f     1    HPE    DL560    Intel Xeon    6138G      80   1.5 TB(2)   3.6 TB         10ge              0
 0f     1    HPE    DL560    Intel Xeon    6148G      80   1.5 TB(2)   745 GB(13)     10ge              0
 0f     1    HPE    DL560    Intel Xeon    6148G      80   1.5 TB(2)   3.6 TB         10ge              0

C1 CLUSTER (older nodes with interconnect=1g)
 1a   118    Dell   R610     Intel Xeon    E5520       8    31 GB      220 GB         1g                0
 1b    46    Dell   R610     Intel Xeon    E5645      12    92 GB      220 GB         1g                0
 2a    45    Dell   R620     Intel Xeon    E5-2660    16   251 GB      2.7 TB         1g                0
 2b   159    Dell   PE1950   Intel Xeon    E5410       8    15 GB       37 GB         1g                0
 2c    88    Dell   PEC6220  Intel Xeon    E5-2665    16    62 GB      250 GB         1g                0
 3    185    Sun    X2200    AMD   Opteron 2356        8    15 GB      193 GB         1g                0
 4    318    IBM    DX340    Intel Xeon    E5410       8    15 GB      111 GB         1g                0
 5a   266    Sun    X6250    Intel Xeon    L5420       8    31 GB       31 GB         1g                0
 5b     9    Sun    X4150    Intel Xeon    E5410       8    31 GB       99 GB         1g                0
 5c    37    Dell   R510     Intel Xeon    E5640       8    22 GB        7 TB         1g                0
 5d    23    Dell   R520     Intel Xeon    E5-2450    12    46 GB      2.7 TB         1g                0
 6     65    HP     DL165    AMD   Opteron 6176       24    46 GB      193 GB         1g                0

C2 CLUSTER (newer nodes with interconnect=FDR)
 7a    42    HP     SL230    Intel Xeon    E5-2665    16    62 GB      240 GB         56g, fdr, 10ge    0
 7b    12    HP     SL250s   Intel Xeon    E5-2665    16    62 GB      240 GB         56g, fdr, 10ge    0
 8a    71    HP     SL250s   Intel Xeon    E5-2665    16    62 GB      900 GB         56g, fdr, 10ge    2 x K20(4)
 8b    57    HP     SL250s   Intel Xeon    E5-2665    16    62 GB      420 GB         56g, fdr, 10ge    2 x K20(4)
 9     72    HP     SL250s   Intel Xeon    E5-2665    16   125 GB      420 GB         56g, fdr, 10ge    2 x K20(4)
10     80    HP     SL250s   Intel Xeon    E5-2670v2  20   125 GB      800 GB         56g, fdr, 10ge    2 x K20(4)
11a    41    HP     SL250s   Intel Xeon    E5-2670v2  20   125 GB      800 GB         56g, fdr, 10ge    2 x K40(6)
11b     3    HP     SL250s   Intel Xeon    E5-2670v2  20   125 GB      800 GB         56g, fdr, 10ge    0
11c    41    Dell   MISC     Intel Xeon    E5-2650v2  16   250 GB      2.7 TB         56g, fdr, 10ge    0
12     29    Lenovo NX360M5  Intel Xeon    E5-2680v3  24   125 GB      800 GB         56g, fdr, 10ge    2 x K40(6)
13     24    Dell   C4130    Intel Xeon    E5-2680v3  24   125 GB      1.8 TB         56g, fdr, 10ge    2 x K40(6)
14     12    HPE    XL1X0R   Intel Xeon    E5-2680v3  24   125 GB      880 GB         56g, fdr, 10ge    2 x K40(6)
15     32    Dell   C4130    Intel Xeon    E5-2680v3  24   125 GB      1.8 TB         56g, fdr, 10ge    2 x K40(6)
16     40    Dell   C4130    Intel Xeon    E5-2680v4  28   125 GB      1.8 TB         56g, fdr, 10ge    2 x P100(8)
17     20    Dell   C4130    Intel Xeon    E5-2680v4  28   124 GB      1.8 TB         56g, fdr, 10ge    2 x P100(8)

C2 CLUSTER (newer nodes without FDR)
19b     4    HPE    XL170    Intel Xeon    6252G      48   372 GB      1.8 TB         56g, 10ge         0

C2 CLUSTER (newest nodes with interconnect=HDR)
18a     2    Dell   C4140    Intel Xeon    6148G      40   372 GB      1.9 TB(13)    100g, hdr, 25ge    4 x V100NV(9)
18b    65    Dell   R740     Intel Xeon    6148G      40   372 GB      1.8 TB        100g, hdr, 25ge    2 x V100(10)
18c    10    Dell   R740     Intel Xeon    6148G      40   748 GB      1.8 TB        100g, hdr, 25ge    2 x V100(10)
19a    28    Dell   R740     Intel Xeon    6248G      40   372 GB      1.8 TB        100g, hdr, 25ge    2 x V100(10)
20     22    Dell   R740     Intel Xeon    6238R      56   372 GB      1.8 TB        100g, hdr, 25ge    2 x V100S(11)
21      2    Dell   R740     Intel Xeon    6248G      40   372 GB      1.8 TB        100g, hdr, 25ge    2 x V100
24a     2    NVIDIA DGXA100  AMD   EPYC    7742      128     1 TB       28 TB        100g, hdr, 100ge   8 x A100(17)
27     34    Dell   R740     Intel Xeon    6258R      56   372 GB      1.8 TB        100g, hdr, 25ge    2 x A100(16)

  *** PBS resource requests are always lowercase ***

If you don't care which GPU MODEL you get (K20, K40, P100, V100, V100S, V100NV), you can specify gpu_model=any
If you don't care which IB you get (FDR or HDR), you can specify interconnect=any

(0) CHIP has 3 resources:   chip_manufacturer, chip_model, chip_type
(1) Leave 2 or 3GB for the operating system when requesting memory in PBS jobs
(2) Specify queue "bigmem" to access the large memory machines
(4) 2 NVIDIA Tesla K20m cards per node, use resource request "ngpus=[1|2]" and "gpu_model=k20"
(6) 2 NVIDIA Tesla K40m cards per node, use resource request "ngpus=[1|2]" and "gpu_model=k40"
(8) 2 NVIDIA Tesla P100 cards per node, use resource request "ngpus=[1|2]" and "gpu_model=p100"
(9) 4 NVIDIA Tesla V100 cards per node with NVLINK2, use resource request "ngpus=[1|2|3|4]" and "gpu_model=v100nv"
(10) 2 NVIDIA Tesla V100 cards per node, use resource request "ngpus=[1|2]" and "gpu_model=v100"
(11) 2 NVIDIA Tesla V100S cards per node, use resource request "ngpus=[1|2]" and "gpu_model=v100s"
(12) Phase18a nodes use NVMe storage for /local_scratch.
(13) local_scratch is housed entirely on SSD
(15) phase21 has a virtually segmented GPU, available GPU is Tesla V100 with 8GB VRAM
(16) 2 NVIDIA A100 cards per node, use resource request "ngpus=[1|2]" and "gpu_model=a100"
(17) 8 NVIDIA A100 cards per node, use resource request "ngpus=[1..8]" and "gpu_model=dgxa100"
~~~

We have more than 2,000 compute nodes. They are grouped into phases; all nodes within a phase have the same hardware specifications. 
The compute nodes in Phase 0 
have very large amount of RAM, up to 1.5 Tb. The nodes in phases 1 to 6 are connected to each other with 1g Ethernet connection; they have at least 8
CPUs and at least 15 Gb of RAM. Nodes in phases 7 and up are connected with *InfiniBand connection*, which is much faster than Ethernet. They are, on average, more powerful than the 1g nodes: they have at least 16 CPUs and at least 62 Gb of RAM. Most of them also have GPUs (videocards); they are typically not used for video processing, but rather for some computation-heavy procedures such as machine learning applications. About 600 compute nodes on Palmetto have GPUs. The InfiniBand nodes are more popular than the 1g nodes, so we have stricter limits on their use: one can use the 1g nodes for up to 168 hours at a time, whereas one can use an InfiniBand node for up to 72 hours.

To see which nodes are available at the moment, you can type 

~~~
whatsfree
~~~
{: .bash}

You will see something like this:

~~~
TOTAL NODES: 2205  TOTAL CORES: 35856  NODES FREE: 1278   NODES OFFLINE: 16   NODES RESERVED: 0

BIGMEM nodes
PHASE 0a   TOTAL =   3  FREE =   3  OFFLINE =   0  TYPE = Bigmem node 24 cores and 1TB RAM
PHASE 0b   TOTAL =   5  FREE =   4  OFFLINE =   1  TYPE = Bigmem node 32 cores and 750GB RAM
PHASE 0c   TOTAL =   1  FREE =   0  OFFLINE =   0  TYPE = Bigmem node 40 cores and 1TB RAM
PHASE 0d   TOTAL =   2  FREE =   1  OFFLINE =   0  TYPE = Bigmem node 36 cores and 1.5TB RAM
PHASE 0e   TOTAL =   1  FREE =   0  OFFLINE =   1  TYPE = Bigmem node 40 cores and 1.5TB RAM
PHASE 0f   TOTAL =   3  FREE =   0  OFFLINE =   0  TYPE = Bigmem node 80 cores and 1.5TB RAM

C1 CLUSTER (older nodes with interconnect=1g)
PHASE 1a   TOTAL = 118  FREE =  48  OFFLINE =   0  TYPE = Dell   R610    Intel Xeon  E5520,      8 cores,  31GB, 1g
PHASE 1b   TOTAL =  46  FREE =  45  OFFLINE =   0  TYPE = Dell   R610    Intel Xeon  E5645,     12 cores,  94GB, 1g
PHASE 2a   TOTAL =  45  FREE =   3  OFFLINE =   1  TYPE = Dell   R620    Intel Xeon  E5-2660    16 cores, 251GB, 1g
PHASE 2b   TOTAL = 159  FREE =  90  OFFLINE =   0  TYPE = Dell   PE1950  Intel Xeon  E5410,      8 cores,  15GB, 1g
PHASE 2c   TOTAL =  88  FREE =  82  OFFLINE =   1  TYPE = Dell   PEC6220 Intel Xeon  E5-2665,   16 cores,  62GB, 1g
PHASE 3    TOTAL = 185  FREE =  40  OFFLINE =   0  TYPE = Sun    X2200   AMD Opteron 2356,       8 cores,  15GB, 1g
PHASE 4    TOTAL = 318  FREE = 284  OFFLINE =   0  TYPE = IBM    DX340   Intel Xeon  E5410,      8 cores,  15GB, 1g
PHASE 5a   TOTAL = 266  FREE = 249  OFFLINE =   4  TYPE = Sun    X6250   Intel Xeon  L5420,      8 cores,  30GB, 1g
PHASE 5b   TOTAL =   9  FREE =   8  OFFLINE =   1  TYPE = Sun    X4150   Intel Xeon  E5410,      8 cores,  15GB, 1g
PHASE 5c   TOTAL =  37  FREE =  14  OFFLINE =   0  TYPE = Dell   R510    Intel Xeon  E5460,      8 cores,  23GB, 1g
PHASE 5d   TOTAL =  23  FREE =  10  OFFLINE =   0  TYPE = Dell   R520    Intel Xeon  E5-2450    12 cores,  46GB, 1g
PHASE 6    TOTAL =  65  FREE =  36  OFFLINE =   0  TYPE = HP     DL165   AMD Opteron 6176,      24 cores,  46GB, 1g

C2 CLUSTER (newer nodes with interconnect=FDR)
PHASE 7a   TOTAL =  42  FREE =   0  OFFLINE =   0  TYPE = HP     SL230   Intel Xeon  E5-2665,   16 cores,  62GB, FDR, 10ge
PHASE 7b   TOTAL =  12  FREE =   0  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores,  62GB, FDR, 10ge
PHASE 8a   TOTAL =  71  FREE =  40  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores,  62GB, FDR, 10ge, K20
PHASE 8b   TOTAL =  57  FREE =  57  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores,  62GB, FDR, 10ge, K20
PHASE 9    TOTAL =  72  FREE =  66  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores, 125GB, FDR, 10ge, K20
PHASE 10   TOTAL =  80  FREE =  22  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2670v2, 20 cores, 125GB, FDR, 10ge, K20
PHASE 11a  TOTAL =  41  FREE =  41  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2670v2, 20 cores, 125GB, FDR, 10ge, K40
PHASE 11b  TOTAL =   3  FREE =   3  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2670v2, 20 cores, 125GB, FDR, 10ge, Phi
PHASE 12   TOTAL =  29  FREE =   0  OFFLINE =   0  TYPE = Lenovo MX360M5 Intel Xeon  E5-2680v3, 24 cores, 125GB, FDR, 10ge, K40
PHASE 13   TOTAL =  24  FREE =   3  OFFLINE =   0  TYPE = Dell   C4130   Intel Xeon  E5-2680v3, 24 cores, 125GB, FDR, 10ge, K40
PHASE 14   TOTAL =  12  FREE =   6  OFFLINE =   1  TYPE = HP     XL190r  Intel Xeon  E5-2680v3, 24 cores, 125GB, FDR, 10ge, K40
PHASE 15   TOTAL =  32  FREE =  32  OFFLINE =   0  TYPE = Dell   C4130   Intel Xeon  E5-2680v3, 24 cores, 125GB, FDR, 10ge, K40
PHASE 16   TOTAL =  40  FREE =   6  OFFLINE =   3  TYPE = Dell   C4130   Intel Xeon  E5-2680v4, 28 cores, 125GB, FDR, 10ge, P100
PHASE 17   TOTAL =  20  FREE =  17  OFFLINE =   0  TYPE = Dell   C4130   Intel Xeon  E5-2680v4, 28 cores, 124GB, FDR, 10ge, P100

C2 CLUSTER (newest nodes with interconnect=HDR except for phase19b,21,22)
PHASE 18a  TOTAL =   2  FREE =   0  OFFLINE =   0  TYPE = Dell   C4140   Intel Xeon  6148G,     40 cores, 372GB, HDR, 10ge, V100nv
PHASE 18b  TOTAL =  65  FREE =   1  OFFLINE =   0  TYPE = Dell   R740    Intel Xeon  6148G,     40 cores, 372GB, HDR, 25ge, V100
PHASE 18c  TOTAL =  10  FREE =   0  OFFLINE =   0  TYPE = Dell   R740    Intel Xeon  6148G,     40 cores, 748GB, HDR, 25ge, V100
PHASE 19a  TOTAL =  28  FREE =   1  OFFLINE =   0  TYPE = Dell   R740    Intel Xeon  6248G,     40 cores, 372GB, HDR, 25ge, V100
PHASE 19b  TOTAL =   4  FREE =   1  OFFLINE =   0  MUSC TYPE = HPE XL170 Intel Xeon  6252G,     48 cores, 372GB,      10ge
PHASE 20   TOTAL =  22  FREE =   0  OFFLINE =   0  TYPE = Dell   R740    Intel Xeon  6238R,     56 cores, 372GB, HDR, 25ge, V100S
PHASE 21   TOTAL =   2  FREE =   0  OFFLINE =   0  TYPE = Dell   R740    Intel Xeon  6248G,     40 cores, 372GB,      10ge
PHASE 22   TOTAL =  18  FREE =  16  OFFLINE =   2  UNAVAILABLE  Dell C8220 Intel Xeon 6238r     20 cores, 250GB,      10ge

DGX NODES
PHASE 24a  TOTAL =   2  FREE =   2  OFFLINE =   0  TYPE = NVIDIA DGXA100 AMD   EPYC  7742,      128 cores, 990GB, HDR, 25ge, A100

SKYLIGHT CLUSTER (Mercury Consortium)
PHASE 25a  TOTAL =  22  FREE =   0  OFFLINE =   0  TYPE = ACT            Intel Xeon  E5-2640v4, 20 cores,  125GB,  1ge
PHASE 25b  TOTAL =   3  FREE =   0  OFFLINE =   0  TYPE = ACT            Intel Xeon  E5-2680v4, 28 cores,  503GB,  1ge
PHASE 25c  TOTAL =   6  FREE =   6  OFFLINE =   0  TYPE = ACT            Intel Xeon  E5-2640v4, 20 cores,   62GB,  1ge, GTX1080
PHASE 25d  TOTAL =   2  FREE =   0  OFFLINE =   0  TYPE = ACT            Intel Xeon  E5-2640v4, 20 cores,  125GB,  1ge, P100
PHASE 26a  TOTAL =  24  FREE =   0  OFFLINE =   1  TYPE = Dell R640      Intel Xeon  6230R,     52 cores,  754GB, 25ge
PHASE 26b  TOTAL =   5  FREE =   0  OFFLINE =   0  TYPE = Dell R640      Intel Xeon  6230R,     52 cores, 1500GB, 25ge
PHASE 26c  TOTAL =   6  FREE =   0  OFFLINE =   0  TYPE = Dell DSS840    Intel Xeon  6230R,     52 cores,  380GB, 25ge, RTX6000

C2 CLUSTER New nodes with A100 GPUs
PHASE 27   TOTAL =  34  FREE =   0  OFFLINE =   0  TYPE = Dell   R740    Intel Xeon  6258R,     56 cores, 372GB, HDR, 25ge, A100

NOTE: PBS resource requests must be LOWER CASE.
      Your job will land on the oldest phase that satisfies your PBS resource requests.
      Also run "checkqueuecfg" to find out the queue limits on number of running jobs permitted per user in each queue.
~~~

This table shows the amount of *completely free* nodes per each phase; a node which has, for example, 8 cores, but only 4 of them are used, would not be counted as "free". So this table is a conservative estimate. Note that there are a lot more free nodes in the 1g phases, compared to the InfiniBand phases. It is a good idea to run `whatsfree` when you log into Palmetto, to get a picture of how busy the cluster is. This picture can change pretty drastically depending on the time of the day and the day of the week.

