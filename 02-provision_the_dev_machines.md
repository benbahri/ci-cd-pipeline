La machine du developpeurs doit disposer des outils suivants déjà installés:
- git
- nano
- java
- maven

## Manual installation of main packages
### Distributions EL7
installation Git et Nano:
```
sudo yum install -y git nano
```
installation java 8
```sh
yum install java-1.8*
```
installation maven 3.6
```sh
 # Creating maven directory under /opt
 mkdir /opt/maven
 cd /opt/maven
 # downloading maven version 3.6.3
 wget http://mirrors.estointernet.in/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
 tar -xvzf apache-maven-3.6.3-bin.tar.gz
 ```
## Ansible playbook for automatic provisioning
