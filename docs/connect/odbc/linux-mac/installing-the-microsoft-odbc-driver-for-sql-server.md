---
title: Installer Microsoft ODBC Driver for SQL Server (Linux)
description: Découvrez comment installer Microsoft ODBC Driver for SQL Server sur des clients Linux pour permettre la connectivité de base de données.
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver, installing
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 626565382328ff9ad698f3b4bd11ebbe4a81f689
ms.sourcegitcommit: 98acedd435aecfda1b3c4c23d3f0c3c1a12682a4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102532301"
---
# <a name="install-the-microsoft-odbc-driver-for-sql-server-linux"></a>Installer Microsoft ODBC Driver for SQL Server (Linux)

Cet article explique comment installer Microsoft ODBC Driver for SQL Server sur Linux. Il contient également des instructions pour les outils en ligne de commande facultatifs pour SQL Server (`bcp` et `sqlcmd`) et les en-têtes de développement unixODBC.

Cet article fournit des commandes pour installer le pilote ODBC à partir de l’interpréteur de commandes bash. Si vous souhaitez télécharger les packages directement, consultez [Télécharger ODBC Driver for SQL Server](../download-odbc-driver-for-sql-server.md).

## <a name="microsoft-odbc-17"></a><a id="17"></a> Microsoft ODBC 17

Les sections suivantes expliquent comment installer le pilote Microsoft ODBC 17 à partir de l’interpréteur de commandes bash pour différentes distributions de Linux.

- [Alpine Linux](#alpine17)
- [Debian](#debian17)
- [Red Hat Enterprise Linux et Oracle](#redhat17)
- [SUSE Linux Enterprise Server](#suse17)
- [Ubuntu](#ubuntu17)

> [!IMPORTANT]
> Si vous avez installé le package `msodbcsql` v17 qui n’a pas été disponible longtemps, vous devez le supprimer avant d’installer le package `msodbcsql17`. Cette opération évitera les conflits. Le package `msodbcsql17` peut être installé côte à côte avec le package `msodbcsql` v13.

### <a name="alpine-linux"></a><a id="alpine17"></a> Alpine Linux

```bash
#Download the desired package(s)
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.1.1-1_amd64.apk
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/mssql-tools_17.7.1.1-1_amd64.apk


#(Optional) Verify signature, if 'gpg' is missing install it using 'apk add gnupg':
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.1.1-1_amd64.sig
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/mssql-tools_17.7.1.1-1_amd64.sig

curl https://packages.microsoft.com/keys/microsoft.asc  | gpg --import -
gpg --verify msodbcsql17_17.7.1.1-1_amd64.sig msodbcsql17_17.7.1.1-1_amd64.apk
gpg --verify mssql-tools_17.7.1.1-1_amd64.sig mssql-tools_17.7.1.1-1_amd64.apk


#Install the package(s)
sudo apk add --allow-untrusted msodbcsql17_17.7.1.1-1_amd64.apk
sudo apk add --allow-untrusted mssql-tools_17.7.1.1-1_amd64.apk
```

> [!NOTE]
> Le pilote version 17.5 ou ultérieure est requise pour la prise en charge d’Alpine.

### <a name="debian"></a><a id="debian17"></a> Debian

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#Debian 8
curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Debian 9
curl https://packages.microsoft.com/config/debian/9/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Debian 10
curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
# optional: kerberos library for debian-slim distributions
sudo apt-get install libgssapi-krb5-2
```

> [!NOTE]
> Vous pouvez remplacer la variable d’environnement 'ACCEPT_EULA' par la variable debconf 'msodbcsql/ACCEPT_EULA' : `echo msodbcsql17 msodbcsql/ACCEPT_EULA boolean true | sudo debconf-set-selections`

### <a name="red-hat-enterprise-server-and-oracle-linux"></a><a id="redhat17"></a> Red Hat Enterprise Server et Oracle Linux

```bash
sudo su

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#RedHat Enterprise Server 6
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo

#RedHat Enterprise Server 7 and Oracle Linux 7
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo

#RedHat Enterprise Server 8 and Oracle Linux 8
curl https://packages.microsoft.com/config/rhel/8/prod.repo > /etc/yum.repos.d/mssql-release.repo

exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="suse-linux-enterprise-server"></a><a id="suse17"></a> SUSE Linux Enterprise Server

```bash
sudo su
curl -O https://packages.microsoft.com/keys/microsoft.asc
rpm --import microsoft.asc

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#SUSE Linux Enterprise Server 11 SP4
#Ensure SUSE Linux Enterprise 11 Security Module has been installed 
zypper ar https://packages.microsoft.com/config/sles/11/prod.repo

#SUSE Linux Enterprise Server 12
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo

#SUSE Linux Enterprise Server 15
zypper ar https://packages.microsoft.com/config/sles/15/prod.repo
#(Only for driver 17.3 and below)
SUSEConnect -p sle-module-legacy/15/x86_64

exit
sudo ACCEPT_EULA=Y zypper install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="ubuntu"></a><a id="ubuntu17"></a> Ubuntu

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#Ubuntu 16.04
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 18.04
curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 20.04
curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 20.10
curl https://packages.microsoft.com/config/ubuntu/20.10/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

> [!NOTE]
> - Le pilote version 17.2 ou ultérieure est requise pour la prise en charge d’Ubuntu 18.04.
> - Le pilote version 17.3 ou ultérieure est requise pour la prise en charge d’Ubuntu 18.10.

> [!NOTE]
> Vous pouvez remplacer la variable d’environnement 'ACCEPT_EULA' par la variable debconf 'msodbcsql/ACCEPT_EULA' : `echo msodbcsql17 msodbcsql/ACCEPT_EULA boolean true | sudo debconf-set-selections`

## <a name="previous-versions"></a>Versions précédentes

Les sections suivantes fournissent des instructions pour l’installation des versions précédentes du pilote Microsoft ODBC sur Linux. Les versions du pilote suivantes sont abordées :

- [Pilote Microsoft ODBC 13.1 for SQL Server](#13.1)
- [Pilote Microsoft ODBC 13 for SQL Server](#13)
- [Pilote Microsoft ODBC 11 for SQL Server](#11)

## <a name="odbc-131"></a><a id="13.1"></a> ODBC 13.1

Les sections suivantes expliquent comment installer le pilote Microsoft ODBC 13.1 à partir de l’interpréteur de commandes bash pour différentes distributions de Linux.

### <a name="debian-8"></a>Debian 8

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="redhat-enterprise-server-6"></a>RedHat Enterprise Server 6

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="redhat-enterprise-server-7"></a>RedHat Enterprise Server 7

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="suse-linux-enterprise-server-11"></a>SUSE Linux Enterprise Server 11

```bash
sudo su
zypper ar https://packages.microsoft.com/config/sles/11/prod.repo
exit
sudo ACCEPT_EULA=Y zypper install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

```bash
sudo su
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo
exit
sudo ACCEPT_EULA=Y zypper install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="ubuntu-1510"></a>Ubuntu 15.10

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/15.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="ubuntu-1610"></a>Ubuntu 16.10

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

## <a name="odbc-13"></a><a id="13"></a> ODBC 13

Les sections suivantes expliquent comment installer le pilote Microsoft ODBC 13 à partir de l’interpréteur de commandes bash pour différentes distributions de Linux.

### <a name="redhat-enterprise-server-6"></a>RedHat Enterprise Server 6

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum update
sudo yum remove unixODBC #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
sudo yum install unixODBC-utf16-devel #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="redhat-enterprise-server-7"></a>RedHat Enterprise Server 7

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum update
sudo yum remove unixODBC #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
sudo yum install unixODBC-utf16-devel #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="ubuntu-1510"></a>Ubuntu 15.10

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/15.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql=13.0.1.0-1 mssql-tools=14.0.2.0-1
sudo apt-get install unixodbc-dev-utf16 #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql=13.0.1.0-1 mssql-tools=14.0.2.0-1
sudo apt-get install unixodbc-dev-utf16 #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

```bash
sudo su 
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo 
zypper update 
sudo ACCEPT_EULA=Y zypper install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
zypper install unixODBC-utf16-devel
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="offline-installation"></a>Installation hors connexion

Si vous préférez/nécessitez que [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 soit installé sur un ordinateur sans connexion Internet, vous devez résoudre manuellement les dépendances de package. [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 a les dépendances directes suivantes :
- Ubuntu : libc6 (>= 2.21), libstdc++6 (>= 4.9), libkrb5-3, libcurl3, openssl, debconf (>= 0.5), unixodbc (>= 2.3.1-1)
- Red Hat : ```glibc, e2fsprogs, krb5-libs, openssl, unixODBC```
- SUSE : ```glibc, libuuid1, krb5, openssl, unixODBC```

Chacun de ces packages a ses propres dépendances, qui peuvent ou non être présentes sur le système. Pour une solution générale à ce problème, reportez-vous à la documentation du gestionnaire de package de votre distribution : [Redhat](https://wiki.centos.org/HowTos/CreateLocalRepos), [Ubuntu](https://unix.stackexchange.com/questions/87130/how-to-quickly-create-a-local-apt-repository-for-random-packages-using-a-debian) et [SUSE](https://en.opensuse.org/Portal:Zypper)

Il est également courant de télécharger manuellement tous les packages dépendants et de les placer ensemble sur l’ordinateur d’installation, puis d’installer manuellement chaque package tour à tour, en terminant par le package [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13.

#### <a name="redhat-linux-enterprise-server-7"></a>Redhat Linux Enterprise Server 7

- Téléchargez le dernier package `msodbcsql` `.rpm` à partir de [https://packages.microsoft.com/rhel/7/prod/](https://packages.microsoft.com/rhel/7/prod/).
- Installez les dépendances et le pilote.
  
```bash
yum install glibc e2fsprogs krb5-libs openssl unixODBC unixODBC-devel #install dependencies
sudo rpm -i  msodbcsql-13.1.X.X-X.x86_64.rpm #install the Driver
```

#### <a name="ubuntu-1604"></a>Ubuntu 16.04

- Téléchargez le dernier package `msodbcsql` `.deb` à partir de [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/).
- Installez les dépendances et le pilote.

```bash
sudo apt-get install libc6 libstdc++6 libkrb5-3 libcurl3 openssl debconf unixodbc unixodbc-dev #install dependencies
sudo dpkg -i msodbcsql_13.1.X.X-X_amd64.deb #install the Driver
```

#### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

- Téléchargez le dernier package `msodbcsql` `.rpm` à partir de [https://packages.microsoft.com/sles/12/prod/](https://packages.microsoft.com/sles/12/prod/).
- Installez les dépendances et le pilote.

```bash
zypper install glibc, libuuid1, krb5, openssl, unixODBC unixODBC-devel #install dependencies
sudo rpm -i  msodbcsql-13.1.X.X-X.x86_64.rpm #install the Driver
```

Une fois l’installation du package terminée, vous pouvez vérifier que [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 peut trouver toutes ses dépendances en exécutant ldd et en inspectant sa sortie pour déterminer les bibliothèques manquantes :

```bash
ldd /opt/microsoft/msodbcsql/lib64/libmsodbcsql-*
```

## <a name="odbc-11"></a><a id="11"></a> ODBC 11

Les sections suivantes expliquent comment installer le pilote Microsoft ODBC 11 sur Linux. Avant d’utiliser le pilote, installez le Gestionnaire de pilotes unixODBC. Pour plus d’informations, consultez [Installation du Gestionnaire de pilotes](../../../connect/odbc/linux-mac/installing-the-driver-manager.md).

### <a name="installation-steps"></a>Étapes d’installation  

> [!IMPORTANT]  
> Ces instructions font référence à `msodbcsql-11.0.2270.0.tar.gz`, qui est le fichier d’installation pour Red Hat Linux. Si vous installez la préversion pour SUSE Linux, le nom du fichier est `msodbcsql-11.0.2260.0.tar.gz`.  
  
Pour installer le pilote :

1. Assurez-vous de disposer d’une autorisation d’accès à la racine.  

2. Accédez au répertoire où le téléchargement a placé le fichier `msodbcsql-11.0.2270.0.tar.gz`. Assurez-vous de disposer du fichier \*.tar.gz correspondant à votre version de Linux. Pour extraire les fichiers, exécutez la commande suivante : `tar xvzf msodbcsql-11.0.2270.0.tar.gz`.  
  
3. Accédez au répertoire `msodbcsql-11.0.2270.0`, où devrait se trouver un fichier nommé **install.sh**.  
  
4. Pour afficher la liste des options d’installation disponibles, exécutez la commande suivante : **./install.sh**.  
  
5. Effectuez une sauvegarde de **odbcinst.ini**. L’installation du pilote met à jour **odbcinst.ini**. odbcinst.ini contient la liste des pilotes enregistrés auprès du Gestionnaire de pilotes unixODBC. Pour découvrir l’emplacement du fichier odbcinst.ini sur votre ordinateur, exécutez la commande suivante : ```odbc_config --odbcinstini```.  
  
6. Avant d’installer le pilote, exécutez la commande suivante : `./install.sh verify`. La sortie de `./install.sh verify` indique si votre ordinateur dispose des logiciels requis pour prendre en charge le pilote ODBC sur Linux.  
  
7. Quand vous êtes prêt à installer le pilote ODBC sur Linux, exécutez la commande suivante : `./install.sh install`. Si vous devez spécifier une commande d’installation (`bin-dir` ou `lib-dir`), spécifiez-la après l’option **install**.  
  
8. Après avoir examiné le contrat de licence, tapez **YES** pour poursuivre l’installation.  
  
L’installation place le pilote dans `/opt/microsoft/msodbcsql/11.0.2270.0`. Le pilote et ses fichiers associés doivent être dans `/opt/microsoft/msodbcsql/11.0.2270.0`.  
  
Pour vérifier que le pilote Microsoft ODBC sur Linux a été inscrit correctement, exécutez la commande suivante : ```odbcinst -q -d -n "ODBC Driver 11 for SQL Server"```.  
  
### <a name="uninstall"></a>Désinstaller l’interface  
  
Vous pouvez désinstaller le pilote ODBC 11 sur Linux, en exécutant les commandes suivantes :  
  
1. `rm -f /usr/bin/sqlcmd`
  
2. `rm -f /usr/bin/bcp`  
  
3. `rm -rf /opt/microsoft/msodbcsql`  
  
4. `odbcinst -u -d -n "ODBC Driver 11 for SQL Server"`
  
## <a name="driver-files"></a>Fichiers de pilote

Le pilote ODBC sur Linux est constitué des composants suivants :

|Composant|Description|  
|---------------|-----------------|  
|libmsodbcsql-17.X.so.X.X ou libmsodbcsql-13.X.so.X.X|Fichier bibliothèque dynamique (`so`) d’objets partagés contenant l’ensemble des fonctionnalités du pilote. Ce fichier est installé dans `/opt/microsoft/msodbcsql17/lib64/` pour Driver 17 et dans `/opt/microsoft/msodbcsql/lib64/` pour Driver 13.|  
|`msodbcsqlr17.rll` ou `msodbcsqlr13.rll`|Fichier de ressources qui accompagne la bibliothèque du pilote. Ce fichier est installé dans `[driver .so directory]../share/resources/en_US/`| 
|msodbcsql.h|Fichier d’en-tête qui contient toutes les nouvelles définitions nécessaires à l’utilisation du pilote.<br /><br /> **Remarque :**  Vous ne pouvez pas référencer msodbcsql.h et odbcss.h dans le même programme.<br /><br /> msodbcsql.h est installé dans `/opt/microsoft/msodbcsql17/include/` pour Driver 17 et dans `/opt/microsoft/msodbcsql/include/` pour Driver 13. |
|LICENSE.txt|Fichier texte qui contient les termes du contrat de licence utilisateur final. Ce fichier est placé dans `/usr/share/doc/msodbcsql17/` pour Driver 17 et dans `/usr/share/doc/msodbcsql/` pour Driver 13.|
|RELEASE_NOTES|Fichier texte qui contient les notes de publication. Ce fichier est placé dans `/usr/share/doc/msodbcsql17/` pour Driver 17 et dans `/usr/share/doc/msodbcsql/` pour Driver 13.|

## <a name="resource-file-loading"></a>Chargement du fichier de ressources

Le pilote doit charger le fichier de ressources pour pouvoir fonctionner. Ce fichier est appelé `msodbcsqlr17.rll` ou `msodbcsqlr13.rll` selon la version du pilote. L’emplacement du fichier `.rll` est relatif à l’emplacement du pilote lui-même (`so` ou `dylib`), comme indiqué dans le tableau ci-dessus. Depuis la version 17.1, le pilote peut aussi essayer de charger le fichier `.rll` à partir du répertoire par défaut si le chargement échoue à partir du chemin d’accès relatif. Le chemin du fichier de ressources par défaut sur Linux est `/opt/microsoft/msodbcsql17/share/resources/en_US/`.

## <a name="troubleshooting"></a>Dépannage

Si vous ne parvenez pas à établir de connexion à SQL Server à l’aide du pilote ODBC, consultez l’article sur la [résolution des problèmes de connexion](known-issues-in-this-version-of-the-driver.md#connectivity).

## <a name="next-steps"></a>Étapes suivantes

Après avoir installé le pilote, vous pouvez essayer l’[exemple d’application ODBC C++](../../odbc/cpp-code-example-app-connect-access-sql-db.md). Pour plus d’informations sur le développement d’applications ODBC, consultez [Développement d’applications](../../../odbc/reference/develop-app/developing-applications.md).

Pour plus d’informations, consultez les [notes de publication](release-notes-odbc-sql-server-linux-mac.md) et la [configuration système requise](system-requirements.md) du pilote ODBC.
