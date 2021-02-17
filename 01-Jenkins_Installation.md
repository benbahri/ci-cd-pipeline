# Installer Jenkins sur CentOS / 7
Jenkins est un programme indépendant basé sur Java, prêt à être exécuté, avec des packages pour Windows, Mac OS X et d'autres systèmes d'exploitation de type Unix.
En tant que serveur d'automatisation extensible, Jenkins peut être utilisé comme un simple serveur CI ou transformé en hub de livraison continue pour n'importe quel projet.


### Conditions préalables
1. VM CentOS/7
   - Avec accès Internet
   - Port «8080» ouvert à Internet
1. Java v1.8.x

## Installer Java
1. Nous utiliserons open java pour notre démo, obtenez la dernière version sur http://openjdk.java.net/install/
```sh
yum install java-1.8*
#yum -y install java-1.8.0-openjdk
```

1. Confirmez la version Java et définissez  JAVA_HOME
```sh
java -version
find /usr/lib/jvm/java-1.8* | head -n 3

JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.212.b04-1.el8_0.x86_64
export JAVA_HOME
PATH=$PATH:$JAVA_HOME
 # To set it permanently update your .bash_profile
vi ~/.bash_profile
```
*La sortie devrait être quelque chose comme ça,*
```sh
[root@~]# java -version
openjdk version "1.8.0_151"
OpenJDK Runtime Environment (build 1.8.0_151-b12)
OpenJDK 64-Bit Server VM (build 25.151-b12, mixed mode)
```

## Installer Jenkins
Vous pouvez installer jenkins en utilisant le rpm ou en configurant le dépôt. Nous allons mettre en place le dépôt afin de pouvoir le mettre à jour facilement à l'avenir.
1. Obtenez la dernière version de jenkins sur https://pkg.jenkins.io/redhat-stable/ et l'installez
```sh
yum -y install wget
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum -y install jenkins
```
### Démarrer Jenkins
```sh
# Start jenkins service
systemctl start jenkins

# Setup Jenkins to start at boot,
systemctl enablejenkins
```

### Accéder à Jenkins
Par défaut, jenkins fonctionne sur le port `8080`, vous pouvez accéder à jenkins par
```sh
http://YOUR-SERVER-IP:8080
```  
#### Configurer Jenkins
- Le nom d'utilisateur par défaut est `admin`
- Prenez le mot de passe par défaut
- Emplacement du mot de passe: `/var/lib/jenkins/secrets/initialAdminPassword`
- Installation du plugin `Skip`; _Nous pouvons le faire plus tard_
- Changer le mot de passe administrateur
   - `Admin`>` Configurer`> `Mot de passe`
- Configurer le `java` path
  - `Manage Jenkins`>` Global Tool Configuration`> `JDK`
- Créez un autre identifiant d'utilisateur administrateur

### Tester les jobs Jenkins
1. Créer un «new item»
1. Entrez un nom d'élément - `My-First-Project`
   - Choisissez le projet `Freestyle`
1. Sous la section Build
   Execute shell: echo "Welcome to Jenkins Demo"
1. Sauvegardez votre travail
1. Build job
1. Vérifiez la "console output"
