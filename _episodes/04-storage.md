---
title: "Storage on Palmetto"
questions:
- "How and where can I store my files?"
teaching: 15
exercises: 0
objectives:
- home directory, scratch space
keypoints:
- "users get 100 Gb of backed-up storage in their home directories"
- "they also have access to more than 2 Pb of scratch storage"
- "scratch storage is not backed up, and files left unused for 1 month are deleted"
---

Every Palmetto user gets 100 Gb of storage space; this storage is backed up at the end of every day, and the backup is kept for 42 days. So if you accidentally delete a file that was created more than a day ago, we might be able to restore it. This storage is called *home directory*.

To see how much space you have left in your home directory, please type:

~~~
checkquota
~~~
{: .bash}

Since most of you are new users of Palmetto, you should be using very little storage at the moment.

When you log into Palmetto, you end up in your home directory. To see which directory you are in, type 

~~~
pwd
~~~
{: .bash}

...which stands for "print working directory". It should give you something like

~~~
/home/<your Palmetto username>
~~~
{: .output}

100 Gb might be enough for some, but for people dealing with extensive amounts of data that would not be enough. We also offer the access to *scratch space*, which is about 2 Petabytes in total. Scratch space is not backed up; files that haven't been used for more than 4 months are automatically deleted (and cannot be restored). Scratch storage has been optimized for handling a lot of reading and writing; in particular, if your workflow involvescreating temporary files that will be constantly modified, it is much better to use scratch space than to run your workflow from your home directory (because the process will put a lot of strain on the home directory). We strongly encourage people to use scratch space, but please be aware of its temporary nature. When you get anything that is worth keeping, please back it up, either in your home directory, or on your local machine.

Scratch space is divided into two directories: `scratch1` (1.88 Petabytes) and `scratch2` (188 Terabytes). `scratch1` uses a faster system for more rapid file transfer, and is well suited for jobs with thousands of read/write requests. `scratch2` is better suited for read/write jobs that are performed in parallel.

For ever Palmetto user, their scratch space is located in  `/scratch1/<username>` folder. You can access it with the `cd` ("change directory") command:

~~~
cd /scratch1/<your Palmetto username>
~~~
{: .bash}
 

To go back to your home directory, you can do

~~~
cd /home/<your Palmetto username>
~~~
{: .bash}

There is also a shortcut; to go to your home directory, you can simply type

~~~
cd
~~~
{: .bash}

Here, I will not go into details about Linux commands. Some of you have taken our Linux workshop. There are many online tutorials, my favourite is [this one.](http://linuxcommand.org/lc3_learning_the_shell.php) Please spend some time getting familiar with going between the directories, as well as with copying, moving, and deleting files.

We offer storage space on Palmetto for sale, with the price of $150 per 1 terabyte. This storage is backed up just like your home directory. Please contact us f you are interested in buying storage.
