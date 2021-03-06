# Create Volume group from Physical storage

For this tutorial, you will need AWS ec2 instance (t2.micro should work) and three EBS volumes (8gib) attached to ec2 instance.

## 1 -  Physical Volume

In this section we take RAW Linux storage devices and carve out physical volume from single or multiple RAW device(s).

First we need to verify how many storage devices are visible for ec2 instance by running command below:

```
lsbk
```

Above command will show output similar to this:

```
[ec2-user@ip-172-31-13-254 ~]$ lsblk
NAME    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
xvdc    202:32   0   8G  0 disk
xvda    202:0    0   8G  0 disk
└─xvda1 202:1    0   8G  0 part /
xvdd    202:48   0   8G  0 disk
xvdb    202:16   0   8G  0 disk
```

Notice - we have four devices attached, but one of the device "xvda" already have been provisioned and has filesystem on it.

Now, let's check what physical volumes currently exist by issuing command "pvs"

```
pvs
```

if there is physical volume - you will see output of it.

### 1.1 Create physical group

Create physical volume using non-partitioned disk:

```
sudo pvcreate /dev/xvdc /dev/xvdd /dev/xvdb

```

Verify pv creation was sucessful:

```
pvs
```

Command above will show output of devices attached and size.

```
pvdisplay
```

Command above displays similar output to pvs, but bit with more details.

## 2 -  Create Logical Volume 

Before we create new logical volume - let's verify if there is a existing logical volume group. 

```
lvdisplay
```

In some cases, you will have to alter existing logical volume group and extend the size for consistent growth. For Part 1 of this tutorial we will just create new LVM and add disks from physical volume we created in step 1.1.


