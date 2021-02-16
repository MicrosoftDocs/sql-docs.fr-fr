---
title: Installer les outils en ligne de commande SQL Server sur Linux
titleSuffix: SQL Server
description: Découvrez comment installer les outils en ligne de commande SQL Server, les pilotes Microsoft ODBC et leurs dépendances sur Linux.
author: VanMSFT
ms.author: vanto
ms.date: 06/30/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: sqlfreshmay19
ms.technology: linux
ms.assetid: eff8e226-185f-46d4-a3e3-e18b7a439e63
ms.openlocfilehash: 36508869b30688e7a87035b50a7a06ca1e2a083a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346353"
---
# <a name="install-sqlcmd-and-bcp-the-sql-server-command-line-tools-on-linux"></a>Installer sqlcmd et bcp, les outils en ligne de commande SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Les étapes suivantes installent les outils en ligne de commande, les pilotes Microsoft ODBC et leurs dépendances. Le package **mssql-tools** contient les éléments suivants :

- **sqlcmd** : Utilitaire de requête de ligne de commande.
- **bcp** : Utilitaire d’importation et d’exportation en bloc.

Installez les outils pour votre plateforme :

- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)
- [macOS](#macos)
- [Docker](#docker)

Cet article décrit comment installer les outils en ligne de commande. Si vous recherchez des exemples d’utilisation de **sqlcmd** ou de **bcp**, consultez les [liens](#next-steps) à la fin de cette rubrique.

## <a name="a-idrhelinstall-tools-on-rhel-8"></a><a id="RHEL"><a/>Installer les outils sur RHEL 8

Utilisez les étapes suivantes pour installer **mssql-tools** sur Red Hat Enterprise Linux. 

1. Entrez en mode superutilisateur.

   ```bash
   sudo su
   ```

1. Téléchargez le fichier config du référentiel Microsoft Red Hat.

   ```bash
   curl https://packages.microsoft.com/config/rhel/8/prod.repo > /etc/yum.repos.d/msprod.repo
   ```

1. Quittez le mode superutilisateur.

   ```bash
   exit
   ```

1. Si vous aviez une version précédente de **mssql-tools** installée, supprimez tous les anciens packages unixODBC.

   ```bash
   sudo yum remove mssql-tools unixODBC-utf16-devel
   ```

1. Exécutez les commandes suivantes pour installer **mssql-tools** avec le package pour développeur unixODBC.

   ```bash
   sudo yum install mssql-tools unixODBC-devel
   ```

   > [!Note] 
   > Pour mettre à jour vers la dernière version de **mssql-tools**, exécutez les commandes suivantes :
   >    ```bash
   >   sudo yum check-update
   >   sudo yum update mssql-tools
   >   ```

1. **Facultatif** : Ajoutez `/opt/mssql-tools/bin/` à votre variable d'environnement **PATH** dans un interpréteur de commandes Bash.

   Afin de rendre **sqlcmd/bcp** accessible depuis l’interpréteur de commandes Bash pour les sessions de connexion, modifiez votre **PATH** dans le fichier **~/.bash_profile** à l’aide de la commande suivante :

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   Afin de rendre **sqlcmd/bcp** accessible depuis l’interpréteur de commandes Bash pour les sessions interactives/sans connexion, modifiez le **PATH** dans le fichier **~/.bashrc** à l’aide de la commande suivante :

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="install-tools-on-ubuntu-1604"></a><a id="ubuntu"></a>Installer les outils sur Ubuntu 16.04

Suivez les étapes suivantes pour installer **mssql-tools** sur Ubuntu.

> [!NOTE]
> Ubuntu 18.04 est pris en charge à compter de SQL Server 2019 CU3. Si vous utilisez Ubuntu 18.04, remplacez le chemin du référentiel `/ubuntu/16.04` par `/ubuntu/18.04`.

1. Importez les clés GPG de référentiel public.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Enregistrez le référentiel Microsoft Ubuntu.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

1. Mettez à jour la liste des sources et exécutez la commande d'installation avec le package pour développeur unixODBC.

   ```bash
   sudo apt-get update 
   sudo apt-get install mssql-tools unixodbc-dev
   ```

   > [!Note] 
   > Pour mettre à jour vers la dernière version de **mssql-tools**, exécutez les commandes suivantes :
   >    ```bash
   >   sudo apt-get update 
   >   sudo apt-get install mssql-tools 
   >   ```

1. **Facultatif** : Ajoutez `/opt/mssql-tools/bin/` à votre variable d'environnement **PATH** dans un interpréteur de commandes Bash.

   Afin de rendre **sqlcmd/bcp** accessible depuis l’interpréteur de commandes Bash pour les sessions de connexion, modifiez votre **PATH** dans le fichier **~/.bash_profile** à l’aide de la commande suivante :

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   Afin de rendre **sqlcmd/bcp** accessible depuis l’interpréteur de commandes Bash pour les sessions interactives/sans connexion, modifiez le **PATH** dans le fichier **~/.bashrc** à l’aide de la commande suivante :

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="install-tools-on-sles-12"></a><a id="SLES"></a>Installer les outils sur SLES 12

Utilisez les étapes suivantes pour installer **mssql-tools** sur SUSE Linux Enterprise Server. 

1. Ajoutez le référentiel Microsoft SQL Server à Zypper.

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo 
   sudo zypper --gpg-auto-import-keys refresh
   ```

1. Installez **mssql-tools** avec le package pour développeur unixODBC.

   ```bash
   sudo zypper install mssql-tools unixODBC-devel
   ```

   > [!Note] 
   > Pour mettre à jour vers la dernière version de **mssql-tools**, exécutez les commandes suivantes :
   >    ```bash
   >   sudo zypper refresh
   >   sudo zypper update mssql-tools
   >   ```

1. **Facultatif** : Ajoutez `/opt/mssql-tools/bin/` à votre variable d'environnement **PATH** dans un interpréteur de commandes Bash.

   Afin de rendre **sqlcmd/bcp** accessible depuis l’interpréteur de commandes Bash pour les sessions de connexion, modifiez votre **PATH** dans le fichier **~/.bash_profile** à l’aide de la commande suivante :

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   Afin de rendre **sqlcmd/bcp** accessible depuis l’interpréteur de commandes Bash pour les sessions interactives/sans connexion, modifiez le **PATH** dans le fichier **~/.bashrc** à l’aide de la commande suivante :

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="install-tools-on-macos"></a><a id="macos"></a> Installer les outils sur macOS

Une préversion de **sqlcmd** et de **bcp** est désormais disponible sur macOS. Pour plus d'informations, consultez [Annonce](https://blogs.technet.microsoft.com/dataplatforminsider/2017/05/16/sql-server-command-line-tools-for-macos-released/).

*Installez [Homebrew](https://brew.sh) si vous ne l’avez pas déjà :*

- `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

Pour installer les outils pour Mac El Capitan et Sierra, utilisez les commandes suivantes :

```bash
# brew untap microsoft/mssql-preview if you installed the preview version 
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
brew install mssql-tools
#for silent install: 
#HOMEBREW_NO_ENV_FILTERING=1 ACCEPT_EULA=y brew install mssql-tools
```

## <a name="docker"></a><a id="docker"></a> Docker

Si vous [exécutez SQL Server dans un conteneur Docker](quickstart-install-connect-docker.md), les outils en ligne de commande SQL Server sont déjà inclus dans l’image conteneur Linux SQL Server. Si vous joignez un conteneur en cours d’exécution avec un interpréteur de commandes Bash interactif, vous pouvez exécuter les outils localement.

## <a name="offline-installation"></a>Installation hors connexion

[!INCLUDE[SQL Server Linux offline package installation](../includes/sql-server-linux-offline-package-install-intro.md)]

1. Tout d’abord, localisez et copiez le package **mssql-tools** pour votre distribution Linux :

   | Distribution Linux | Emplacement du package **mssql-tools** |
   |---|---|
   | Red Hat | [https://packages.microsoft.com/rhel/7.3/prod](https://packages.microsoft.com/rhel/7.3/prod) |
   | SLES | [https://packages.microsoft.com/sles/12/prod](https://packages.microsoft.com/sles/12/prod)|
   | Ubuntu 16.04 | [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/mssql-tools](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/mssql-tools) |

1. Localisez et copiez également le package **msodbcsql**, qui est une dépendance. Le package **msodbcsql** a également une dépendance sur **unixODBC-devel** (Red Hat et SLES) ou **unixodbc-dev** (Ubuntu). L’emplacement des packages **msodbcsql** est répertorié dans la table suivante :

   | Distribution Linux | Emplacement des packages ODBC |
   |---|---|
   | Red Hat | [https://packages.microsoft.com/rhel/8/prod](https://packages.microsoft.com/rhel/8/prod) |
   | SLES | [https://packages.microsoft.com/sles/12/prod](https://packages.microsoft.com/sles/12/prod)|
   | Ubuntu 16.04 | [**msodbcsql**](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql)<br/>[**unixodbc-dev**](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/u/unixodbc/) |

1. **Déplacez les packages téléchargés sur votre machine Linux**. Si vous avez utilisé une autre machine pour télécharger les packages, vous pouvez déplacer les packages vers votre machine Linux à l’aide de la commande **scp**.

1. **Installer les packages** : Installez les packages **mssql-tools** et **msodbc**. Si vous recevez des erreurs de dépendance, ignorez-les jusqu’à l’étape suivante.

    | Plateforme | Commandes d’installation de packages |
    |-----|-----|
    | Red Hat | `sudo yum localinstall msodbcsql-<version>.rpm`<br/>`sudo yum localinstall mssql-tools-<version>.rpm` |
    | SLES | `sudo zypper install msodbcsql-<version>.rpm`<br/>`sudo zypper install mssql-tools-<version>.rpm` |
    | Ubuntu | `sudo dpkg -i msodbcsql_<version>.deb`<br/>`sudo dpkg -i mssql-tools_<version>.deb` |

1. **Résoudre des dépendances manquantes** : Vous avez peut-être des dépendances manquantes à ce stade. Si ce n’est pas le cas, vous pouvez ignorer cette étape. Dans certains cas, vous devez localiser et installer manuellement ces dépendances.

    Pour les packages RPM, vous pouvez inspecter les dépendances requises avec les commandes suivantes :

    ```bash
    rpm -qpR msodbcsql-<version>.rpm
    rpm -qpR mssql-tools-<version>.rpm
    ```

    Pour les packages Debian, si vous avez accès à des référentiels approuvés contenant ces dépendances, la solution la plus simple consiste à utiliser la commande **apt-get** :

    ```bash
    sudo apt-get -f install
    ```

    > [!NOTE]
    > Cette commande termine également l’installation des packages SQL Server.

    Si cela ne fonctionne pas pour votre package Debian, vous pouvez inspecter les dépendances requises avec les commandes suivantes :

    ```bash
    dpkg -I msodbcsql_<version>_amd64.deb | grep "Depends:"
    dpkg -I mssql-tools_<version>_amd64.deb | grep "Depends:"
    ```

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir un exemple d’utilisation de **sqlcmd** pour se connecter à SQL Server et créer une base de données, consultez l’un des guides de démarrage rapide suivants :

- [Installer sur Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Installer sur SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Installer sur Ubuntu](quickstart-install-connect-ubuntu.md)
- [Exécuter sur Docker](quickstart-install-connect-ubuntu.md)

Pour obtenir un exemple d’utilisation de **bcp** pour importer et exporter des données en bloc, consultez [Copie en bloc de données vers SQL Server sur Linux](sql-server-linux-migrate-bcp.md).
