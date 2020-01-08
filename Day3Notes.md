#  Day 3
## YUM command line

### yum command

yum --help

yum install

yum update

yum search

yum provides

yum makecache fast

yum clean all

## YUM configuration

### yum repository folder

/etc/yum.repos.d/

yum conf file

/etc/yum.conf

The option in the yum.conf file that breaks the disa stig policy is below

repo_gpgcheck=1


This option should  be set to 0 if you want to pull the meta data from it identifying the pieces of the yum dot repo file

yum repos are in the repo directory and a minimum valid file contains the following


[repository]

name=repository_name

baseurl=repository_url


### example 

[local-extras]

name=local-extras

baseurl=http://repo/extras/

enabled=1

gpgcheck=0



First line must be uniq and the name of the repository that yum will use

Name is the name that will be displayed to the name when listing repository

baseurl is where the repository is located. This can use http, file, or ftp protocol


## LAB Student Laptop

create a Yum repo Config that point to the locally Hosted Class repo

create a yum repo config with the above field and point it at our local repo


sudo vi /etc/yum.repos.d/local.repo 

[local-base]
name=local-base
baseurl=http://repo/base/
enabled=1
gpgcheck=0

[local-rocknsm-2.5]
name=local-rocknsm-2.5
baseurl=http://repo/rocknsm_2_5/
enabled=1
gpgcheck=0

[local-elasticsearch-7.x]
name=local-elasticsearch-7.x
baseurl=http://repo/elasticsearch-7.x/
enabled=1
gpgcheck=0

[local-epel]
name=local-epel
baseurl=http://repo/epel/
enabled=1
gpgcheck=0

[local-extras]
name=local-extras
baseurl=http://repo/extras/
enabled=1
gpgcheck=0

[local-updates]
name=local-updates
baseurl=http://repo/updates/
enabled=1
gpgcheck=0

[local-zerotier]
name=local-zerotier
baseurl=http://repo/zerotier/
enabled=1
gpgcheck=0

[local-virtualbox]
name=local-updates
baseurl=http://repo/virtualbox/
enabled=1
gpgcheck=0

[local-WANdisco-git]
name=local-WANdisco-git
baseurl=http://repo/WANdisco-git/
enabled=1
gpgcheck=0


[localstuff]
name=localstuff
baseurl=http://repo/localstuff/
enabled=1
gpgcheck=0



### in vi :%s/repo/192.168.2.94/g to replace the baseurl by an ip address


sudo yum clean all

sudo yum makecache fast


install yum-utils

sudo yum install yum-utils

cd /srv/

create a local directory

mkdir repos

cd repos


Reposync from a remote repo to the local file system

yum repolist (show repo list)


reposync -l --repoid==repository_name --download_path=/repo/



reposync -l --repoid=local-base --download_path=/srv/repo/ --newest-only --downloadcomps --download-metadata 

reposync -l --repoid=local-Wnndisco-git --download_path=/srv/repo/ --newest-only --downloadcomps --download-metadata 

reposync -l --repoid=local-elasticsearch-7.x --download_path=/srv/repo/ --newest-only --downloadcomps --download-metadata 

reposync -l --repoid=local-epel --download_path=/srv/repo/ --newest-only --downloadcomps --download-metadata

reposync -l --repoid=local-extras  --download_path=/srv/repo/ --newest-only --downloadcomps --download-metadata

reposync -l --repoid=local-rocknsm-2.5 --download_path=/srv/repo/ --newest-only --downloadcomps --download-metadata

reposync -l --repoid=local-updates  --download_path=/srv/repo/ --newest-only --downloadcomps --download-metadata

reposync -l --repoid=virtualbox  --download_path=/srv/repo/ --newest-only --downloadcomps --download-metadata

reposync -l --repoid=local-zerotier --download_path=/srv/repo/ --newest-only --downloadcomps --download-metadata

reposync -l --repoid=localstuff --download_path=/srv/repo/ --newest-only --downloadcomps --download-metadata





## Createrepo

Now that we have all the rpm's located on our box, we can use a tool called createrepo

This tool will creat the needed databases so that yum knows to handle them


   yum install createrepo

create the database file to make local repo usable in the simplest form

 createrepo /repo/<repository_name> 
 
 
 
 
## Spanning & Tapping


aswitch SPAN port

dedicated Tap


LAB - Using Tap

yum install nginx



createrepo -g comps.xml local-base






 







