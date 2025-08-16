Day 8



create a instance

cat /etc/os-release

yum install httpd -y

cd/var/www/html/

cat > index.html

systemctl start httpd

systemctl enable httpd


cd

curl http://localhost



**Q1. how to increase root volume of a live instance if we have XFS file system.**



go to ebs -> volumes 

select the volume already present -> actions -> modify

size 20GiB



Never take unnecessary IOPS.



Modify the volume.



run blkid



lsblk to see the current disk space and to see the file system that would be xfs ( or ext4)

df -h -> to check the currently utilized space that will be 8Gb ( as the new space has no file system )



aws - expand disk part ( website)



growpart /dev/xvda 1

df -h -> still shows 8gb



then resize file system

xfs\_growfs -d /

df -h -> it shows 20Gb



------------------------------------------------------------------------------------------------------------------------------------------------



XFS PARTITION CAN NEVER DECREASE BUT EXT4 OR EXT3 SYSTEM CAN DECREASE IN SIZE TOO

FSx is used to connect Linux to windows







**Q2. Create partition in disk**



Create a new volume of 5Gb and attach it.



fdisk -l -> list all disks with partition



Two types of partition -> primary \& extended

By default MS DOS partition scheme is used for partition it supports max 2tb disk \& 15 partition

GPT is also a partition scheme -> 8 Hexa byte and 120 partition



fdisk /dev/xvdb



press m for help



then type n

select extended e

partition number -> 4

then press w to save



then run partprobe



fdisk - l to check 



fdisk /dev/xvdb

n -> (add a new partition)

enter

+3G -> enter

w -> write (save) and exit



again for 2Gb



fdisk -l



mkfs.ext4 /dev/xvdb5



mkfs.xfs /dev/xvdb6



mkdir /devops



mount /dev/xvdb5 /devops/



mkdir /data



mount /dev/xvdb6 /data/



df -h -> to check the files



cd /devops/

touch filename.txt



to permanently mount it to put it into fstab like day before



How to remove volume



cd



umount /devops

umount /data



on aws -> select volume -> detach



lsblk -> volume doesnot show naymore



again select the volume on aws and actions -> delete



--------------------------------------------------------------------------------------------



Q2. Replicate data(volume) from one region to another region. ( Ex. N.Virginia to Ohio ) -->imp

Create a new data volume of 5gb

Attach it to running instance 

lsblk

mkfs.ext4 /dev/xvdb

mkdir /data

mount /dev/xvdb /data/

df -h

cd /data/

touch filename.txt{1..100}


SNAPSHOTS ARE STORED IN BACKEND IN AWS S3 ( and its not accessible to us )

SNAPSHOT IS A REGION BASED SERVICE


select the volume from ebs

actions

create snapshot

give description \& click on create snapshot

Now go to Snapshots -> wait for status to be completed


Now select the snapshot -> actions -> copy snapshot -> choose destination - us-east-2 

and click on copy snapshot


Change the region to Ohio & check the snapshot


Now in Ohio region

Create a new instance and launch it as we do with us-east-2a subnet

Connect to the instance in terminal
sudo su -

lsblk -> to check for the disk


Now go to snapshot -> select it -> actions -> create volume from snapshot

Select same zone and create


Now select it from volume - attach it with the instance

Now lsblk

blkid to check for file system

file system is also copied

mkdir /test

mount /dev/xvdb /test/

cd /test

ll

all data will be present

-----------------------------------------------------

umount and delete the volume and snapshots

------------------------------------------------------------------------------------------------

Linux to Linux - Linux to unix - NFS

SMB protocol

-----------------------------------------------------------------------------------------------


EFS in aws is replica of NFS 


Q. Deploy ec2 instance in same region different zone (EFS)


machine 1 : Amazon Linux, us-east-1a, new security group -> LAUNCH



machine 2 : Ubuntu, us-east-1b, existing security group -> LAUNCH



machine 3 : RedHat, us-east 1c ..



Search for EFS -> create file system -> set name(my-efs) -> VPC default -> create file system


Connect all instances in the terminal


**On machine 1:**

rpmquery nfs-utils -> which is by default on amazon Linux

systemctl start nfs-utils

systemctl status nfs-utis


**On machine 2:**

sudo su -

apt update

apt install nfs-common (nfs-common is name of the package in ubuntu)

systemctl start nfs-utils.service


**On machine 3:**

sudo su -

yum install nfs-utils -y



-----------------------------------------------------------------------------

create folder in all 3 instances - mkdir /name



create a security group in AWS and allow port 2049 in inbound rules NFS

Go to instance -> security group -> Inbound rules NFS -> 2049 -> save



Go to EFS - > network and in the zone you have deployed your machines -> remove their default security group and choose your own created.



-Now go to file system ( search for EFS )

select my-efs and attach it

Mount via DNS

sudo 

Come to the instance and paste the DNS client command(In the EFS) in the instances with /foldername --> remove the efs thing from link



create files in the folder of all 3 instances



and check ll -> all the files will be reflected in all 3 machines



------------------------------------------------------------------------



ADD ZONE IN EFS 



Go to EFS -> my-efs-> add mount target and allow new zone if needed



umount /foldername to detach folder



NOW CONNECT VIA IP



select the Mount via IP in efs - attach -> and copy the NFS client link and paste on the instance to connect via IP



----------------------------------------------------------------------------------------------------------------------------------------



Q- Replicate a file system (CRR - cross region replication )



first change region(OHIO) and create a file system there \& also a security group ( allow NFS in inbound rules )

Go to NFS-> select the file system created -> network

Select the security group in 2a-2b-2c 



Go to N-Virginia

Select the existing efs

disable replicate overwrite permission from the edit option of the nfs



Replicate -

existing system

aws region - ohia

browse EFS -> select

create replication

















