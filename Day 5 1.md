Day 5



Deploy Java based application using Maven via Tomcat Apache



3 types of webserver to host java based application

1. Apache
2. Ngnix
3. Tomcat Apache



Plugins are utility - they are like connectors

Maven are used to build java application (.war file generally for java)(they contain final artifacts)

Deploy using Tomcat







In Jenkins server machine

* install git
* install maven ( yum install maven -y)
* Go to jekins dashboard then plugins
* Install maven integration plugin
* install deploy to container plugin
* install GitHub integration plugin
* Restart Jenkins
* mvn -v -> to check maven directory



MAVEN TOOLS PART ->

* copy java home directory
* Go to Jenkins -> manage Jenkins - tools
* add jdk
* paste java home directory
* remove git
* add maven
* copy maven home directory
* paste maven home directory



BUILD NEW ITEM

* Then new item in Jenkins server --> name and select maven project
* select git - paste git path
* branch main 
* remove snapshot tick
* apply save
* click on build now
* then check for success



In dev-server machine

follow prior steps from day 4

Clone sir's maven java repo to Dev-machine just after installing git

ssh-keygen

connect with GitHub

connect webhook

then clone

then push to your repo



if not pushing - git remote remove origin 

then git remote add origin @sshofmyrepo

then push again



Host new machine

* tomcat-server
* create new security group tomcat
* allow security 8080
* 10Gb -> launch
* install tomcat package from tomcat website
* core - tar.ez(gp2,sha512) -right click copy link
* yum install wget
* wget copiedlink
* tar -xvzf apache-tomcat-9.0.108.tar.gz (extract, verbosity, expand forcefully the file)(gz is extension for zip file)
* mv apache-tomcat.tar.gz /tmp/
* go to cloudstackh step by step install apache tomcat on amazon Linux

====all from the link =====

* cd apache-tomcat-9.0.108
* cd bin
* chmod +x startup.sh
* chmod +x shutdown.sh
* cd
* yum install java\* --> install all java new updates and dependencies
* cd apache
* cd conf/ --> ll
* vim tomcat-users.xml
* shift + G go to bottom
* cursor in between --> paste -> save and exit
* find -name context.html
* vim /webapps/examples/META-INF
* comment out the specified line --> save exit
* vim host-manager -- same and vim manager same
* cd bin/
* ./startup.sh
* take instances public ip:8080
* sign manager app--> admin admin





GO to Jenkins --

set credentials - system - deployer - deployer



builds

configure

add post build action

deploy container war

\*\*/\*.war

context path /



add container tomcat 9

credentials select

ip:8080/ url



save apply



Now go to GitHub 

webapp->index.jsp file

make changes \& commit

go to Jenkins and build now



go to configure and hook trigger









