---
title: "Storage on Palmetto"
teaching: 15
exercises: 0
questions:

objectives:
- "Use `grep` to select lines from text files that match simple patterns."
keypoints:
- "`grep` selects lines in files that match patterns."
- "`--help` is a flag supported by many bash commands, and programs that can be run from within Bash, to display more information on how to use these commands or programs."
---

Every Palmetto user gets 100 Gb of storage space; this storage is backed up at the end of every day, and the backup is kept for 42 days. So if you accidentally delete a file that was reated more than a day ago, we might be able to restore it. This storage is called *home directory*.

To see how much space you have left in your home directory, please type:

~~~
$ checkquota
~~~
{: .bash}

Since most of you are new users of Palmetto, you should be using very little storage at the moment.

When you log into Palmetto, you end up in your home directory. To see which directory you are in, type 

~~~
$ pwd
~~~
{: .bash}

...which stands for "print working directory". It should give you something like

~~~
/home/<your Palmetto username>
~~~
{: .output}

100 Gb might be enough for some, but for people dealing with extensive amounts of data that would not be enough. We also offer the access to *srath space*, which is about 2.5 Petabytes in total. Scratch space is not backed up; files that haven't been used for more than a month are automatically deleted (and cannpt be restored). So we strongly encourage people to use scratch space, but please be aware of its temprary nature. When you get anything that is worth keeping, please back it up, either in your home directory, or on your local machine.

Scratch space is divided into two directories: `scratch1` (1.88 Petabytes) and `scratch2` (188 Terabytes). 


home storage: backed up, 100 gb limit
checkquota
scratch storage
buying storage
