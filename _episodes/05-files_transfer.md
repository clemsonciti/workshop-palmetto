---
title: "Transferring files to and from Palmetto"
teaching: 15
exercises: 0
questions:
- "How can I access the Palmetto cluster from my local machine?"
objectives:
- "Logging into Palmetto from a local Mac / Windows computer."
---

## Windows machines

### using MobaXTerm

For small file transfers, the Windows users can use the built-in function in MobaXTerm. On the left side of the MobaXTerm window, you will see the browser of your Palmetto directory. By default, it points to your home directory: `/home/<your Palmetto username>`. You can point it to any other folder that you have access to, for example, to `/scratch1/<your Palmetto username>`. To upload files *to* Palmetto, use the UP arrow, and to download files *from* Palmetto, use the DOWN arrow.

<img src="../fig/mobaxterm_transfer.png" style="height:350px">

### using WinSCP

For more substantial file transfers, you can use an SCP client, such as WinSCP. You can download it [here](https://winscp.net/eng/download.php). It's intuitive and easy to use. Start it, then click on "New Site", and enter the following information:

File protocol: SCP
Host name: xfer01-ext.palmetto.clemson.edu

You can also specify your Palmetto username and Password. You can click `Save` to save this information. Click on Login, and do the two-factor identification. Note that we connect to Palmetto via `xfer01-ext` rather than `login`. `xfer01-ext` is a special Palmetto node that handles file transfers with computers that are outside of the Palmetto cluster, so it doesn't burden the login node.

If you log in sucessfully, you will see the files on your local machine on the left, and the Palmetto files on the right:

mobaxterm
winscp
filezilla
scp
globus
