---
title: Installer sur Linux
titleSuffix: SQL Server Machine Learning Services
description: 'Découvrez comment installer SQL Server Machine Learning Services (Python et R) sur Linux : Red Hat, Ubuntu et SUSE.'
author: dphansen
ms.author: davidph
manager: cgronlun
ms.date: 11/24/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning-services
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 462857bd0a9aac7b63659bad809dcdc9a577f3b0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031491"
---
# <a name="install-sql-server-machine-learning-services-python-and-r-on-linux"></a>Installer SQL Server Machine Learning Services (Python et R) sur Linux

[!INCLUDE [SQL Server 2019 - Linux](../includes/applies-to-version/sqlserver2019-linux.md)]

Cet article vous guide lors de l’installation de [SQL Server Machine Learning Services](../machine-learning//sql-server-machine-learning-services.md) sur Linux. Les scripts Python et R peuvent être exécutés dans la base de données à l’aide de Machine Learning Services.

Vous pouvez installer Machine Learning Services sur Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) et Ubuntu. Pour plus d’informations, consultez la [section Plateformes prises en charge dans Conseils d’installation pour SQL Server sur Linux](sql-server-linux-setup.md#supportedplatforms).

> [!NOTE]
> Machine Learning Services est installé par défaut sur des Clusters Big Data SQL Server. Pour plus d’informations, consultez [Utiliser Machine Learning Services (Python et R) sur des clusters Big Data](../big-data-cluster/machine-learning-services.md).

<a name="mro"></a>

## <a name="pre-install-checklist"></a>Liste de contrôle avant l’installation

* [Installez SQL Server sur Linux](sql-server-linux-setup.md) et vérifiez l’installation.

* Vérifiez la présence des extensions Python et R dans les référentiels Linux SQL Server. 
  Si vous avez déjà configuré des référentiels source pour l’installation du moteur de base de données, vous pouvez exécuter les commandes d’installation du package **mssql-mlservices** à l’aide de la même inscription de référentiel.

* (R uniquement) Microsoft R Open (MRO) fournit la distribution R de base pour la fonctionnalité R dans SQL Server et est une condition préalable à l’utilisation de RevoScaleR, de MicrosoftML et d’autres packages R installés avec Machine Learning Services.
    * La version requise est MRO 3.5.2.
    * Choisissez une des deux approches suivantes pour installer MRO :
        * Téléchargez le tarball MRO à partir de MRAN, décompressez-le, puis exécutez son script install.sh. Vous pouvez suivre les [instructions d’installation sur MRAN](https://mran.microsoft.com/releases/3.5.2) si vous souhaitez adopter cette approche.
        * Inscrivez le référentiel **packages.microsoft.com** comme décrit ci-dessous pour installer la distribution MRO : microsoft-r-open-mro et microsoft-r-open-mkl. 
    * Consultez les sections d’installation ci-dessous pour savoir comment installer MRO.

* Vous devez disposer d’un outil pour l’exécution des commandes T-SQL. 

  * Vous pouvez utiliser [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md), qui est un outil de base de données gratuit exécutable sur Linux, Windows et macOS.

## <a name="package-list"></a>Liste des packages

Sur un appareil connecté à Internet, les packages sont téléchargés et installés indépendamment du moteur de base de données à l’aide du programme d’installation de package pour chaque système d’exploitation. Le tableau suivant décrit tous les packages disponibles, mais pour R et Python, vous spécifiez des packages qui fournissent soit l’installation complète des fonctionnalités soit l’installation des fonctionnalités minimales.

Packages d’installation disponibles :

| Nom du package | S’applique à | Description |
|--------------|----------|-------------|
|mssql-server-extensibility  | Tous | Framework d’extensibilité utilisé pour exécuter Python et R. |
| microsoft-openmpi  | Python, R | Interface de passage de messages utilisée par les bibliothèques Revo* pour la parallélisation sur Linux. |
| mssql-mlservices-python | Python | Distribution open source d’Anaconda et de Python. |
|mssql-mlservices-mlm-py  | Python | *Installation complète*. Fournit des modèles préformés, revoscalepy et microsoftml pour la caractérisation d’images et l’analyse textuelle des sentiments.| 
|mssql-mlservices-packages-py  | Python | *Installation minimale*. Fournit revoscalepy et microsoftml. <br/>Exclut les modèles préformés. | 
| [microsoft-r-open*](#mro) | R | Distribution open source de R, composée de trois packages. |
|mssql-mlservices-mlm-r  | R | *Installation complète*. Fournit : RevoScaleR, MicrosoftML, sqlRUtils, olapR des modèles préentraînés pour la caractérisation d’images et l’analyse des sentiments dans un texte.| 
|mssql-mlservices-packages-r  | R | *Installation minimale*. Fournit RevoScaleR, sqlRUtils, MicrosoftML et olapr. <br/>Exclut les modèles préformés. |

<a name="RHEL"></a>

## <a name="install-on-rhel"></a>Installer sur RHEL

Effectuez les étapes ci-dessous pour installer SQL Server Machine Learning Services sur Red Hat Enterprise Linux (RHEL).

### <a name="install-mro-on-rhel"></a>Installer MRO sur RHEL

Les commandes suivantes inscrivent le référentiel fournissant MRO. Après l’inscription, les commandes permettant d’installer d’autres packages R, telles que mssql-mlservices-mml-r, incluent automatiquement MRO en tant que dépendance de package.
```bash
# Import the Microsoft repository key

sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

# Set the location of the package repo at the "prod" directory
# The following command is for version 7.x
# For 6.x, replace 7 with 6 to get that version
rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm

# Update packages on your system (optional)
yum update
```

Options d’installation pour Python et R :

*  Installez la prise en charge linguistique en fonction de vos besoins (une ou plusieurs langues).
*  L’*installation complète* fournit toutes les fonctionnalités disponibles, y compris les modèles de Machine Learning préentraînés.
*  L’*installation minimale* exclut les modèles mais offre toutes les fonctionnalités.

> [!Tip]
> Si possible, exécutez `yum clean all` pour actualiser les packages sur le système avant l’installation.

### <a name="full-installation"></a>Installation complète

Inclut :
*  Python open source
*  R open source
*  Framework d’extensibilité
*  Microsoft-openmpi
*  Extensions (Python, R)
*  Bibliothèques de Machine Learning
*  Modèles préentraînés pour Python et R

```bash
# Install as root or sudo
# Add everything (all R, Python)
# Be sure to include -9.4.7* in mlsservices package names
sudo yum install mssql-mlservices-mlm-py-9.4.7*
sudo yum install mssql-mlservices-mlm-r-9.4.7*
```

### <a name="minimum-installation"></a>Installation minimale

Inclut :
*  Python open source
*  R open source
*  Framework d’extensibilité
*  Microsoft-openmpi
*  Bibliothèques Core Revo*
*  Bibliothèques de Machine Learning

```bash
# Install as root or sudo
# Minimum install of R, Python extensions
# Be sure to include -9.4.6* in mlsservices package names
sudo yum install mssql-mlservices-packages-py-9.4.7*
sudo yum install mssql-mlservices-packages-r-9.4.7*
```
<a name="ubuntu"></a>

## <a name="install-on-ubuntu"></a>Installer sur Ubuntu

Effectuez les étapes ci-dessous pour installer SQL Server Machine Learning Services sur Ubuntu.

### <a name="install-mro-on-ubuntu"></a>Installer MRO sur Ubuntu

Les commandes suivantes inscrivent le référentiel fournissant MRO. Après l’inscription, les commandes permettant d’installer d’autres packages R, telles que mssql-mlservices-mml-r, incluent automatiquement MRO en tant que dépendance de package.

```bash
# Install as root
sudo su

# Optionally, if your system does not have the https apt transport option
apt-get install apt-transport-https

# Set the location of the package repo the "prod" directory containing the distribution.
# This example specifies 16.04. Replace with 14.04 if you want that version
wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb

# Register the repo
dpkg -i packages-microsoft-prod.deb

# Update packages on your system (required), including MRO installation
sudo apt-get update
```

Options d’installation pour Python et R :

*  Installez la prise en charge linguistique en fonction de vos besoins (une ou plusieurs langues).
*  L’*installation complète* fournit toutes les fonctionnalités disponibles, y compris les modèles de Machine Learning préentraînés.
*  L’*installation minimale* exclut les modèles mais offre toutes les fonctionnalités.

> [!Tip]
> Si possible, exécutez `apt-get update` pour actualiser les packages sur le système avant l’installation. 

### <a name="full-installation"></a>Installation complète 

Inclut :
*  Python open source
*  R open source
*  Framework d’extensibilité
*  Microsoft-openmpi
*  Extensions Python
*  Extensions R
*  Bibliothèques de Machine Learning
*  Modèles préentraînés pour Python et R

```bash
# Install as root or sudo
# Add everything (all R, Python)
# There is no asterisk in this full install
sudo apt-get install mssql-mlservices-mlm-py 
sudo apt-get install mssql-mlservices-mlm-r 
```

### <a name="minimum-installation"></a>Installation minimale 

Inclut :
*  Python open source
*  R open source
*  Framework d’extensibilité
*  Microsoft-openmpi
*  Bibliothèques Core Revo*
*  Bibliothèques de Machine Learning

```bash
# Install as root or sudo
# Minimum install of R, Python
# No asterisk
sudo apt-get install mssql-mlservices-packages-py
sudo apt-get install mssql-mlservices-packages-r
```

<a name="SLES"></a>

## <a name="install-on-sles"></a>Installer sur SLES

Effectuez les étapes ci-dessous pour installer SQL Server Machine Learning Services sur SUSE Linux Enterprise Server (SLES).

### <a name="install-mro-on-sles"></a>Installer MRO sur SLES

Les commandes suivantes inscrivent le référentiel fournissant MRO. Après l’inscription, les commandes permettant d’installer d’autres packages R, telles que mssql-mlservices-mml-r, incluent automatiquement MRO en tant que dépendance de package.

```bash
# Install as root
sudo su

# Set the location of the package repo at the "prod" directory containing the distribution
# This example is for SLES12, the only supported version of SLES in Machine Learning Server
zypper ar -f https://packages.microsoft.com/sles/12/prod packages-microsoft-com

# Update packages on your system (optional)
zypper update
```

Options d’installation pour Python et R :

*  Installez la prise en charge linguistique en fonction de vos besoins (une ou plusieurs langues).
*  L’*installation complète* fournit toutes les fonctionnalités disponibles, y compris les modèles de Machine Learning préentraînés.
*  L’*installation minimale* exclut les modèles mais offre toutes les fonctionnalités.

### <a name="full-installation"></a>Installation complète 

Inclut :
*  Python open source
*  R open source
*  Framework d’extensibilité
*  Microsoft-openmpi
*  Extensions pour Python et R
*  Bibliothèques de Machine Learning
*  Modèles préentraînés pour Python et R

```bash
# Install as root or sudo
# Add everything (all R, Python)
sudo zypper install mssql-mlservices-mlm-py
sudo zypper install mssql-mlservices-mlm-r
```

### <a name="minimum-installation"></a>Installation minimale 

Inclut :
*  Python open source
*  R open source
*  Framework d’extensibilité
*  Microsoft-openmpi
*  Bibliothèques Core Revo*
*  Bibliothèques de Machine Learning 

```bash
# Install as root or sudo
# Minimum install of R, Python extensions
sudo zypper install mssql-mlservices-packages-py
sudo zypper install mssql-mlservices-packages-r
```

## <a name="post-install-config-required"></a>Configuration après installation (obligatoire)

La configuration supplémentaire s’effectue principalement avec [l’outil mssql-conf](sql-server-linux-configure-mssql-conf.md).

1. Une fois l’installation du package terminée, lancez mssql-conf setup et suivez les invites pour définir le mot de passe AS et choisir votre édition. Effectuez cette étape uniquement si vous n’avez pas encore configuré SQL Server sur Linux. 

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

2. Acceptez les contrats de licence pour les extensions Python et R open source. Utilisez la commande suivante :

   ```bash
   # Run as SUDO or root
   # Use set + EULA 
   sudo /opt/mssql/bin/mssql-conf set EULA accepteulaml Y
   ```
   Le programme d’installation détecte les packages mssql-mlservices et demande l’acceptation du CLUF (s’il n’a pas encore été accepté) lors de l’exécution de `mssql-conf setup`. Pour plus d’informations sur les paramètres de CLUF, consultez [Configurer SQL Server avec l’outil mssql-conf](sql-server-linux-configure-mssql-conf.md#mlservices-eula).

3. Activez l’accès réseau sortant. L’accès réseau sortant est désactivé par défaut. Pour activer les requêtes sortantes, définissez la propriété booléenne « outboundnetworkaccess » à l’aide de l’outil mssql-conf. Pour plus d’informations, consultez [Configurer SQL Server sur Linux avec mssql-conf](sql-server-linux-configure-mssql-conf.md#mlservices-outbound-access).

   ```bash
   # Run as SUDO or root
   # Enable outbound requests over the network
   sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 1
   ```

4. Pour l’intégration de fonctionnalités R uniquement, définissez la variable d' environnement **MKL_CBWR** pour [garantir la cohérence de la sortie](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) des calculs d’Intel Math Kernel Library (MKL).

   + Modifiez ou créez un fichier `.bash_profile` dans le répertoire racine de votre utilisateur en ajoutant la ligne `export MKL_CBWR="AUTO"` au fichier.

   + Exécutez ce fichier en saisissant `source .bash_profile` à l’invite de commandes bash.

5. Redémarrez le service SQL Server Launchpad et l’instance du moteur de base de données pour lire les valeurs mises à jour à partir du fichier INI. Un message de notification s’affiche quand un paramètre relatif à l’extensibilité est modifié.  

   ```bash
   systemctl restart mssql-launchpadd

   systemctl restart mssql-server.service
   ```

6. Activez l’exécution du script externe à l’aide d’Azure Data Studio ou d’un autre outil comme SQL Server Management Studio (Windows uniquement) qui exécute Transact-SQL.

   ```sql
   EXEC sp_configure 'external scripts enabled', 1 
   RECONFIGURE WITH OVERRIDE 
   ```

7. Redémarrez le service Launchpad.

## <a name="verify-installation"></a>Vérifier l'installation

Les bibliothèques R (MicrosoftML, RevoScaleR et autres) sont disponibles sur `/opt/mssql/mlservices/libraries/RServer`.

Les bibliothèques Python (microsoftml et revoscalepy) sont disponibles sur `/opt/mssql/mlservices/libraries/PythonServer`.

Pour valider l’installation

* Exécutez un script T-SQL qui exécute une procédure stockée système appelant Python ou R à l’aide d’un outil de requête. 

* Exécutez la commande SQL suivante pour tester l’exécution de R dans SQL Server. Des erreurs ? Essayez un redémarrage du service, `sudo systemctl restart mssql-server.service`.
  ```sql
  EXEC sp_execute_external_script   
  @language =N'R', 
  @script=N' 
  OutputDataSet <- InputDataSet', 
  @input_data_1 =N'SELECT 1 AS hello' 
  WITH RESULT SETS (([hello] int not null)); 
  GO 
  ```
 
* Exécutez la commande SQL suivante pour tester l’exécution de Python dans SQL Server. 
 
  ```sql
  EXEC sp_execute_external_script  
  @language =N'Python', 
  @script=N' 
  OutputDataSet = InputDataSet; 
  ', 
  @input_data_1 =N'SELECT 1 AS hello' 
  WITH RESULT SETS (([hello] int not null)); 
  GO 
  ```

<a name="install-all"></a>

## <a name="unattended-installation"></a>Installation sans assistance

À l'aide de [l’installation sans assistance](sql-server-linux-setup.md#unattended) du moteur de base de données, ajoutez les packages pour mssql-mlservices et les CLUF.

 Utilisez l’un des paramètres de CLUF propres à mlservices pour les distributions R et Python open source :

```bash
sudo /opt/mssql/bin/mssql-conf setup accept-eula-ml
```

Le CLUF complet est documenté dans [Configurer SQL Server sur Linux avec l’outil mssql-conf](sql-server-linux-configure-mssql-conf.md#mlservices-eula).

## <a name="offline-installation"></a>Installation hors connexion

Suivez les instructions [d'installation hors connexion](sql-server-linux-setup.md#offline) pour connaître les étapes d’installation des packages. Recherchez votre site de téléchargement, puis téléchargez des packages spécifiques à l’aide de la liste de packages ci-dessous.

> [!Tip]
> Plusieurs outils de gestion des packages fournissent des commandes qui peuvent vous aider à déterminer les dépendances du package. Pour yum, utilisez `sudo yum deplist [package]`. Pour Ubuntu, utilisez `sudo apt-get install --reinstall --download-only [package name]`, suivi de `dpkg -I [package name].deb`.

 
### <a name="download-site"></a>Site de téléchargement

Téléchargez des packages à partir de [https://packages.microsoft.com/](https://packages.microsoft.com/). Tous les packages mlservices pour Python et R sont colocalisés avec le package du moteur de base de données. La version de base des packages mlservices est 9.4.6 Souvenez-vous que les packages microsoft-r-open se trouvent dans un [autre référentiel](#mro).

### <a name="rhel7-paths"></a>Chemins d’accès RHEL/7

|Package|Emplacement de téléchargement|
|--|----|
| Packages mssql/mlservices | [https://packages.microsoft.com/rhel/7/mssql-server-2019/](https://packages.microsoft.com/rhel/7/mssql-server-2019/) |
| Packages microsoft-r-open | [https://packages.microsoft.com/rhel/7/prod/](https://packages.microsoft.com/rhel/7/prod/) | 

### <a name="ubuntu1604-paths"></a>Chemins d’accès Ubuntu/16.04

|Package|Emplacement de téléchargement|
|--|----|
| Packages mssql/mlservices | [https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/) |
| Packages microsoft-r-open | [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/) | 

### <a name="sles12-paths"></a>Chemins d’accès SLES/12

|Package|Emplacement de téléchargement|
|--|----|
| Packages mssql/mlservices | [https://packages.microsoft.com/sles/12/mssql-server-2019/](https://packages.microsoft.com/sles/12/mssql-server-2019/) |
| Packages microsoft-r-open | [https://packages.microsoft.com/sles/12/prod/](https://packages.microsoft.com/sles/12/prod/) | 

Sélectionnez les extensions que vous souhaitez utiliser et téléchargez les packages nécessaires pour un langage spécifique. Les noms de fichiers incluent des informations sur la plateforme dans le suffixe.

### <a name="package-list"></a>Liste des packages

Selon les extensions que vous souhaitez utiliser, téléchargez les packages nécessaires pour un langage spécifique. Les noms de fichiers exacts incluent des informations sur la plateforme dans le suffixe, mais les noms de fichier ci-dessous devraient être suffisamment proches pour vous permettre de déterminer les fichiers à obtenir.

```
# Core packages 
mssql-server-15.0.1000
mssql-server-extensibility-15.0.1000

# R
microsoft-openmpi-3.0.0
microsoft-r-open-mkl-3.5.2
microsoft-r-open-mro-3.5.2
mssql-mlservices-packages-r-9.4.7.64
mssql-mlservices-mlm-r-9.4.7.64


# Python
microsoft-openmpi-3.0.0
mssql-mlservices-python-9.4.7.64
mssql-mlservices-packages-py-9.4.7.64
mssql-mlservices-mlm-py-9.4.7.64
```

## <a name="next-steps"></a>Étapes suivantes

Les développeurs Python peuvent apprendre à utiliser Python avec SQL Server en effectuant les didacticiels suivants :

+ [Tutoriel Python : Prédire la location de ski avec régression linéaire dans SQL Server Machine Learning Services](../machine-learning/tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Tutoriel Python : Catégoriser des clients à l’aide de k-moyennes avec SQL Server Machine Learning Services](../machine-learning/tutorials/python-clustering-model.md)

Les développeurs peuvent démarrer avec quelques exemples simples et découvrir les principes de base du fonctionnement de R avec SQL Server. Pour l’étape suivante, consultez les liens suivants :

+ [Démarrage rapide : Exécuter R dans T-SQL](../machine-learning/tutorials/quickstart-r-create-script.md)
+ [Tutoriel : Analytique dans la base de données pour les développeurs R](../machine-learning/tutorials/r-taxi-classification-introduction.md)
