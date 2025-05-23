#MIM #setup #arch
this man pages assumes a fresh new linux install as it offers itself to good sequential explanation of certain concepts, but these can be taken and easily adapted to work on any other environment.

setting up your storage is a crucial step towards a working system, this can be divided in smaller increments:

-Drive partitioning - 1.1
-Drive formatting - 1.2
-Drive mounting - 1.3
-fstab file generation and setup - 1.4


### Drive partitioning 1.1

fdisk is a popular utility to partition disks and heavily recommended by the archwiki, i believe however that cfdisk is a much more user friendly experience, at least for new users.

these need to be formatted in ext4 (preferably) and only then can you mount then onto the system. this is because the partition needs a filesystem label in order to be read by the machine. the most commonplace label is GPT.

### Drive Formatting 1.2

to format a partition sda1 located on drive sda we would use the mkfs command as follows 
```
mkfs.ext4 /dev/sda1
```

### Drive Mounting 1.3

and assuming this is an external data drive we could mount it afterwards with 

```
mount /dev/sda1 /mnt/data
```

sudo is required most times when doing this on a day to day basis.
also, the directory must exist prior to using it as a mounting point.
-t can be used to specify the type of filesystem used by the drive, as such:

```
mkdir /mnt/data
sudo mount -t ntfs /dev/sda1/ mnt/data
```

### Drive Mounting 1.4

to mount on boot edit the FSTAB file in /etc/  as such. 

```
#<file system>    <mount point>  <type>  <options>  <dump><pass>
UUID/LABEL=number  /dev/mnt       ext4    defaults     0     0 
```

### 1.4.1 Labels

the use of labels is recommended for future proofing, as UUID sometimes change between boots. 
labels can be eassily checked with the blkid command, as such: 

```
blkid /dev/PATH TO PARTITION 
```
if the partition is not specified, according to the manual blkid will provide the system label for the current partition, however this behavior is not consistent. 
Path should always be specified for consistent and non confusing results. 

labels can be assigned with the e2label command, for example, on a drive sda, with partition sda1 we would do:

```
e2label /dev/sda1 NAMEOFDRIVE
```

#### 1.4.2 Labels for NTFS drives

for NTFS file systems, ntfslabel would be used instead 

```
ntfslabel /dev/sdb1 YOURLABEL
```

After properly partitioning, formatting and mounting the drive, the required permissions are needed to write onto it. 
In the case of drives the word permissions is misleading, as one can have read write and execute permissions on a drive and yet be unable to write onto it. 
What we need to do is set the current user as the owner of the mount point directory, using :

```
chown jorge:jorge /mnt/data
```

the directory is listed, **NOT** the driver partition. much like chmod, chown works with filesystems.
