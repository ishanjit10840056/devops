Day 9



Amazon **S3** ( Simple Storage Service )

For a free tier account we can create 100 s3 bucket -> used for object storage

In terms of speed

&nbsp;	EBS > EFS > S3

S3 uses flat file system so that it can be accessible from any other file system

s3 buckets are free but data is chargeable 



Q. How to create S3 bucket

Go to Amazon S3

&nbsp;	create a bucket

&nbsp;	bucket name should be unique

&nbsp;	untick block all access

&nbsp;	disable ACL

&nbsp;	create bucket

&nbsp;	select the bucket

&nbsp;	upload ( add files / folders )

&nbsp;	

select the file -> copy the link from properties and paste -> access will be denied

FULL ACCESS

Go to S3 bucket -> select it -> permission -> ACL -> enable (edit public access list and read tick)


CONTENT BASED ACCESS

or enable a single files ACL - by selecting the file and enabling ACL. (edit permission of the file and in public access tick read)

-----------------------------------------------------------------------------------------------------------------------------------

IF VERSIONING IS ENABLED IN YOUR S3 BUCKET - YOU CAN RETAIN DATA EVEN AFTER DELETING IT.

SO MAKE SURE TO ALWAYS ENABLE VERSIONING WHILE CREATING A BUCKET



TO EDIT AFTER CREATING ->

Select bucket

Properties

bucket versioning

Enable it -> read, write enable



Go to bucket -> delete some data

Now enable show version to see the data deleted

click on the deleted data -> and you can download it 

then upload it

By default the data is stored for 7 days after deleted

-----------------------------------------------------------------------------------------------------------------------------------


Q-  How to use S3 from amazon Linux and upload file from Linux to s3 (steps to create s3 bucket above)


In AWS

create instance

connect -> yum update -y

yum install -y automake fuse-devel gcc-c++ git libcurl-devel libxml2-devel make openssl-devel


Git clone -

git clone https://github.com/s3fs-fuse/s3fs-fuse.git


Move to s3fs-fuse 

cd s3fs-fuse

./autogen.sh

./configure --prefix=/usr --with-openssl

make

sudo make install


to verify package

which s3fs

then cd

In amazon

Go to IAM - USERS

Attach policies

Give him S3full access right

Create it



Go to Users - select your user -> go to security credentials

create access key -> select CLI

download the csv file  or copy the id and password



Create new file to store the passwd

touch /etc/passwd-s3fs

Paste it in the vim /etc/passwd-s3fs ( id:password)



Change the permission of the file

sudo chmod 640 /etc/passwd-s3fs



For mounting to s3 bucket 

s3fs bucket-name /mnt -o passwd\_file=/etc/passwd-s3fs



/mnt is the folder name where the data will be stored


cd /mnt

ls -a (to check the file)

create files in it - it will also reflect in s3

----------------------------------------------------------------------------------



Q. How to do the above step using redhat (less step process)



create a instance and choose redhat in OS - launch instance



sudo su -

cat /etc/os-release (redhat os)



uname -r

yum install wget -y

rpmquery mount-s3



install mount-s3 from aws website

wget -url of mount s3



yum install ./mount-s3.rpm

rmpquery mount-s3



yum install unzip



Install aws cli in the instance (website)

paste the command from website and it will install aws-cli



aws configure

put the access key

and secret key from the zip file saved

region : us-east-1

output : table



it is stored in cd .aws (config \& credentials)



create a mount point

mkdir /mpt



mount-s3 bucketname /mpt/



df -h



cd /mpt 

ll -> you can see all the content



----------------------------------------------------------------------------------------------



Q. Mount s3 bucket with windows

launch a instance with windows OS - select any base version

t3.medium

30gb

enable rdp in inbound rules

launch instance

connect using RDP client



Open the windows -> go to chrome and download Widnows tnt drive (download free trial)

Launch tnt drive

Go to add new drive

add name

give the access and secret key

add new account

save changes



browse amazon s3 bucket

and select your bucket

open it and it will show your files - you can access or create file in it



---------------------------------------------------------------------------------------------------------



Q. Hosting static website on AWS using s3



Go to the s3 bucket -> add files -> pre download a website -> upload it



select properties -> Scroll down go to static website hosting

enable static website hosting 

index document -> index.html

then save changes

copy the url from below too



go to the files select all the files of website -> actions -> make public



then go to the web browser -> paste url and website will be hosted



-----------------------------------------------------------------------------------------------------------



S3 ALSO HAVE CRR (CROSS REGION REPLICATION)

IN PRODUCTION WE CONFIGURE CROSS ZONE LOAD BALANCER
GLOBAL ACCELERATION (OR GLOBAL LOAD BALANCER) FOR CROSS REGION LOAD BALANCER

Types of load balancer
1. Application load balancer
2. Network load balancer
3. Classic load balancer


Target group has pool
POOL is collection of server

Target group makes routing based on users requirement


---------------------------------------------------


Q. Use of load balancer - create 4 instances in different zones - same security group (imp)

Go to load balancing

Create Target group

choose instances
name it
next 
and add the machines - include as pending below
create target group

Now go to -
Load Balancer
Create application load balancer

Name it 
Internet facing (for databse it should be internal)
IPv4

select the zones in which you had made the instances (a,b,c,d)

select the same security group as instance

select http and target group
then create load balancer

Now go to load balancer

wait for it to get active
Now select the load balancer created
Scroll and copy the DNS name and paste it on website

Now to check - go on any machine terminal and systemctl stop httpd

Now refresh the webpage and you will see bad gateway - later in the target group amazon 
will put the machine to unhealthy and stop diverting requests to it

----------------------------------------------------------------------------------------------------

TO ENABLE SESSION STICKINESS

Go to target groups - select your target group
Go to actions - edit target group attributes

Target selection configuration - 
stickiness - turn on -
duration 1 - days
save changes

Now refresh the webpage the server wont change -


