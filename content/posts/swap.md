+++
date = '2024-11-06T22:57:07-08:00'
draft = false
title = 'Create Swap Memory for Linux instance'
categories = ["archives", "System Maintenance"]
tags = ["Linux", "Swap"]
archives = ["archived"]
+++


Prerequisite 
------------
Verify that your file system supports the usage of swap files. For more information, see Swapfile on the BTRFS website. It is a best practice to check the latest file system documentation to confirm that your file system is supported.
Calculate the swap space size

Create a swap file
------------------

Note: It's a best practice to create swap space only on ephemeral storage instance store volumes.

Complete the following steps:

- Run the dd command to create a swap file on the root file system.
  
  Note: In the command, bs is the block size and count is the number of blocks. The size of the swap file is the block size option multiplied by the count option in the dd command. Adjust these values to determine the swap file size.
  
  The block size that you specify must be less than the available memory on the instance, or you receive the memory exhausted error.
  
  In the following example dd command, the swap file is 4 GB (128 MB x 32):

```
$ sudo dd if=/dev/zero of=/swapfile bs=128M count=32
```

- To update the read and write permissions for the swap file, run the following command:

```
$ sudo chmod 600 /swapfile
```

- To set up a Linux swap space, run the following command:

```
$ sudo mkswap /swapfile
```

- To add the swap file to swap space and make the swap file available for immediate use, run the following command:

```
$ sudo swapon /swapfile
```

- To verify that the procedure was successful, run the following command:

```
$ sudo swapon -s
```

- To start the swap file at boot time, edit the /etc/fstab file. Open the file in the editor, and then run the following command:

```
$ sudo vi /etc/fstab
```

- Add the following new line at the end of the file:

```
/swapfile swap swap defaults 0 0
```

- Save the file, and then exit.

Now you can see swap memory shows up with commands like `top`.
