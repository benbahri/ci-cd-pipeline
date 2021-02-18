## Installer Ansible sur CentOS 7 / RHEL 7 / Ubuntu 18.04 / 16.04 et Debian 9


### Configuration de la machine de contrôle

1. Pour installer Ansible, nous devrons [***activer le référentiel EPEL sur
CentOS 7 / RHEL7***](https://www.itzgeek.com/how-tos/linux/centos-how-tos/enable-epel-repository-for-centos-7-rhel-7.html) .

```
### CentOS 7 ###

yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

### RHEL 7 ###

subscription-manager repos --enable rhel-7-server-ansible-2.6-rpms

### Ubuntu 18.04 / Ubuntu 16.04 ###

sudo apt-get update
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update

### Debian 9 ###

sudo apt-get install dirmngr
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys
93C4A3FD7BB9C367
echo "deb http://ppa.launchpad.net/ansible/ansible/ubuntu trusty main" | sudo tee -a /etc/apt/sources.list.d/ansible.list
sudo apt-get update
```
2. Installez Ansible.

```
### CentOS 7 / RHEL 7 & Fedora 28 ###

yum install -y ansible

### Ubuntu 18.04 / 16.04 & Debian 9 ###

sudo apt-get install -y ansible
```
Une fois Ansible installé, vérifiez la version d'Ansible en exécutant la
commande ci-dessous.
```
ansible --version
```
**Output:**
```
ansible 2.9.16

config file = /etc/ansible/ansible.cfg

configured module search path =
[u'/home/vagrant/.ansible/plugins/modules',
u'/usr/share/ansible/plugins/modules']

ansible python module location =
/usr/lib/python2.7/site-packages/ansible

executable location = /usr/bin/ansible

python version = 2.7.5 (default, Apr 2 2020, 13:16:51) [GCC 4.8.5
20150623 (Red Hat 4.8.5-39)]
```
### Configurer les nœuds gérés

Les machines clientes doivent au moins avoir Python 2 (version 2.6 ou
ultérieure) ou Python 3 (version 3.5 ou ultérieure).
```
### CentOS 7 / RHEL 7 & Fedora ###

yum install -y python

### Ubuntu 18.04 / 16.04 & Debian 9 ###

sudo apt-get install -y python
```
### SELinux ( CentOS / RHEL / Fedora )

Si SELinux est activé sur les nœuds gérés, vous devrez installer le
package ci-dessous sur les nœuds avant d'utiliser les fonctionnalités
`copy` / `file` / `template` dans Ansible.
```
yum install -y libselinux-python
```
## Authentification SSH

Comme indiqué précédemment, Ansible utilise *OpenSSH* pour la
communication à distance. Ansible prend en charge l' authentification
sans **mot de passe** et par **mot de passe** pour exécuter des
commandes sur les nœuds gérés.

### Authentification par clé SSH (*authentification sans mot de passe*)

Lorsqu'il s'agit d'authentification **ssh**, par défaut, il utilise des clés
ssh (authentification sans mot de passe) pour s'authentifier auprès de
la machine distante.

Une fois que vous avez configuré la communication sans mot de passe,
vérifiez-la.
```
$ ssh vagrant@192.168.33.100
$ ssh vagrant@192.168.33.10
```
Vous devriez maintenant pouvoir vous connecter à la machine distante
sans mot de passe.

### Authentification par mot de passe

L'authentification par mot de passe peut également être utilisée si
nécessaire en fournissant l'option **--ask-pass**. Cette option
nécessite **sshpass** sur la machine de contrôle.
```
### CentOS 7 / RHEL 7 et Fedora ###

yum install -y sshpass

### Ubuntu 18.04 / 16.04 & Debian 9 ###

sudo apt-get update
sudo apt-get install -y sshpass
```
Dans ce lab, on utilise une communication sans mot de passe
entre le nœud de contrôle et les nœuds gérés. Nom d'utilisateur
du serveur Ansible = Nom d'utilisateur du nœud géré racine = `vagrant`

## Créer un inventaire Ansible

Modifiez (ou créez) le fichier **/etc/ansible/hosts** . Ce fichier
contient l'inventaire des hôtes distantes auxquelles Ansible se connectera
via SSH pour les gérer.
```
### CentOS 7 / RHEL 7 & Fedora ###

vi /etc/ansible/hosts

### Ubuntu 18.04 / 16.04 & Debian 9 ###

sudo nano /etc/ansible/hosts
```
Mettez un ou plusieurs systèmes distants et groupez-les. Ici, on
ajoute les deux machines au groupe `dev`.

Les groupes sont utilisés pour classer les systèmes pour un usage
particulier. Si vous ne spécifiez aucun groupe, ils agiront comme des
hôtes non groupés.
```
[dev]
192.168.33.100
[jenkins]
192.168.33.10
...
```
