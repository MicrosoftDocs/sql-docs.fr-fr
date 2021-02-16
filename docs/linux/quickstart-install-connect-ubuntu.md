---
title: 'Ubuntu : Installer SQL Server sur Linux'
description: Ce démarrage rapide montre comment installer SQL Server 2017 ou SQL Server 2019 sur Ubuntu, puis comment créer et interroger une base de données avec sqlcmd.
author: VanMSFT
ms.author: vanto
ms.date: 07/15/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: seo-lt-2019
ms.technology: linux
ms.assetid: 31c8c92e-12fe-4728-9b95-4bc028250d85
ms.openlocfilehash: 11adcb6d14675952e19fcc37994614348fdae70c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339177"
---
# <a name="quickstart-install-sql-server-and-create-a-database-on-ubuntu"></a>Démarrage rapide : Installer SQL Server et créer une base de données sur Ubuntu
[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]


<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Dans ce guide de démarrage rapide, vous allez installer SQL Server 2017 sur Ubuntu 16.04/18.04. Ensuite, vous vous connectez avec **sqlcmd** pour créer votre première base de données et exécuter des requêtes.

> [!TIP]
> Ce tutoriel nécessite l'intervention de l'utilisateur et une connexion Internet. Si vous êtes intéressé par les procédures d'installation sans assistance ou hors ligne, voir [Conseils d’installation pour SQL Server sur Linux](sql-server-linux-setup.md). Pour obtenir la liste des plateformes prises en charge, consultez nos [notes de publication](sql-server-linux-release-notes.md).

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Dans ce guide de démarrage rapide, vous allez installer SQL Server 2019 sur Ubuntu 16.04/18.04. Ensuite, vous vous connectez avec **sqlcmd** pour créer votre première base de données et exécuter des requêtes.

> [!TIP]
> Ce tutoriel nécessite l'intervention de l'utilisateur et une connexion Internet. Si vous êtes intéressé par les procédures d'installation sans assistance ou hors ligne, voir [Conseils d’installation pour SQL Server sur Linux](sql-server-linux-setup.md). Pour obtenir la liste des plateformes prises en charge, consultez nos [notes de publication](sql-server-linux-release-notes-2019.md).

::: moniker-end

## <a name="prerequisites"></a>Prérequis

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Vous devez disposer d’une machine Ubuntu 16.04 ou 18.04 avec **au moins 2 Go** de mémoire.

Pour installer Ubuntu 18.04 sur votre propre machine, accédez à <http://releases.ubuntu.com/bionic/>. Vous pouvez également créer des machines virtuelles Ubuntu dans Azure. Voir [Créer et gérer des machines virtuelles Linux avec l’interface Azure CLI](/azure/virtual-machines/linux/tutorial-manage-vm).

> [!NOTE]
> Pour le moment, le [sous-système Windows pour Linux](/windows/wsl/about) pour Windows 10 n'est pas pris en charge comme cible d'installation.

Pour les autres configurations système requises, voir [Configuration système requise pour SQL Server sur Linux](sql-server-linux-setup.md#system).

> [!NOTE]
> Ubuntu 18.04 est pris en charge à compter de SQL Server 2017 CU20. Si vous souhaitez utiliser les instructions de cet article avec Ubuntu 18.04, veillez à utiliser le bon [chemin d’accès au référentiel](sql-server-linux-change-repo.md) correct, `18.04` au lieu de `16.04`.
>
> Si vous exécutez SQL Server sur une version antérieure, la configuration est possible avec des [modifications](/archive/blogs/sql_server_team/installing-sql-server-2017-for-linux-on-ubuntu-18-04-lts).

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Vous devez disposer d’une machine Ubuntu 16.04 ou 18.04 avec **au moins 2 Go** de mémoire.

Pour installer Ubuntu 18.04 sur votre propre machine, accédez à <http://releases.ubuntu.com/bionic/>. Vous pouvez également créer des machines virtuelles Ubuntu dans Azure. Voir [Créer et gérer des machines virtuelles Linux avec l’interface Azure CLI](/azure/virtual-machines/linux/tutorial-manage-vm).

> [!NOTE]
> Pour le moment, le [sous-système Windows pour Linux](/windows/wsl/about) pour Windows 10 n'est pas pris en charge comme cible d'installation.

Pour les autres configurations système requises, voir [Configuration système requise pour SQL Server sur Linux](sql-server-linux-setup.md#system).

::: moniker-end

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="install-sql-server"></a><a id="install"></a>Installer SQL Server

> [!NOTE]
> Les commandes suivantes pour SQL Server 2017 pointent vers le référentiel Ubuntu 18.04. Si vous utilisez Ubuntu 16.04, remplacez le chemin ci-dessous par `/ubuntu/16.04/` au lieu de `/ubuntu/18.04/`.

Pour configurer SQL Server sur Ubuntu, exécutez les commandes suivantes dans un terminal afin d’installer le package **mssql-server**.

1. Importez les clés GPG de dépôt public :

   ```bash
   wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. Enregistrez le référentiel Microsoft SQL Server Ubuntu :
   
   Pour Ubuntu 16.04 :
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"
   ```
   
   Pour Ubuntu 18.04 :
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2017.list)"
   ```

   > [!TIP]
   > Si vous voulez tester SQL Server 2019, vous devez inscrire à la place le référentiel SQL Server 2019. Utilisez la commande suivante pour les installations SQL Server 2019 :
   >
   > Pour Ubuntu 16.04 :
   >
   > ```bash
   > sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2019.list)"
   > ```
   >
   > Pour Ubuntu 18.04 :
   >
   > ```bash
   > sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019.list)"
   > ```

3. Exécutez les commandes suivantes pour installer SQL Server :

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server
   ```

4. Une fois l'installation du package terminée, lancez **mssql-conf setup** et suivez les invites pour définir le mot de passe AS et choisir votre édition.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!TIP]
   > Les éditions suivantes de SQL Server 2017 sont sous licence libre : Evaluation, Developer et Express.

   > [!NOTE]
   > Assurez-vous de spécifier un mot de passe fort pour le compte AS (longueur minimale de 8 caractères, lettres majuscules et minuscules comprises, chiffres de la base 10 et/ou symboles non alphanumériques).

5. Une fois la configuration terminée, vérifiez que le service est en cours d'exécution :

   ```bash
   systemctl status mssql-server --no-pager
   ```

6. Si vous prévoyez de vous connecter à distance, vous devrez peut-être aussi ouvrir le port TCP de SQL Server (par défaut 1433) sur votre pare-feu.

À ce stade, SQL Server fonctionne sur votre machine Ubuntu et est prêt à l'emploi !

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="install-sql-server"></a><a id="install"></a>Installer SQL Server

> [!NOTE]
> Les commandes suivantes pour SQL Server 2019 pointent vers le référentiel Ubuntu 18.04. Si vous utilisez Ubuntu 16.04, remplacez le chemin ci-dessous par `/ubuntu/16.04/` au lieu de `/ubuntu/18.04/`.

Pour configurer SQL Server sur Ubuntu, exécutez les commandes suivantes dans un terminal afin d’installer le package **mssql-server**.

1. Importez les clés GPG de dépôt public :

   ```bash
   wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. Inscrivez le référentiel Microsoft SQL Server Ubuntu pour SQL Server 2019 :
   
   Pour Ubuntu 16.04 :
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2019.list)"
   ```
   
   Pour Ubuntu 18.04 :
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019.list)"
   ```

3. Exécutez les commandes suivantes pour installer SQL Server :

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server
   ```

4. Une fois l'installation du package terminée, lancez **mssql-conf setup** et suivez les invites pour définir le mot de passe AS et choisir votre édition.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!NOTE]
   > Assurez-vous de spécifier un mot de passe fort pour le compte AS (longueur minimale de 8 caractères, lettres majuscules et minuscules comprises, chiffres de la base 10 et/ou symboles non alphanumériques).

5. Une fois la configuration terminée, vérifiez que le service est en cours d'exécution :

   ```bash
   systemctl status mssql-server --no-pager
   ```

6. Si vous prévoyez de vous connecter à distance, vous devrez peut-être aussi ouvrir le port TCP de SQL Server (par défaut 1433) sur votre pare-feu.

À ce stade, SQL Server 2019 fonctionne sur votre machine Ubuntu et est prêt à l’emploi !

::: moniker-end

## <a name="install-the-sql-server-command-line-tools"></a><a id="tools"></a>Installer les outils en ligne de commande SQL Server

Pour créer une base de données, vous devez vous connecter à un outil capable d’exécuter des instructions Transact-SQL sur SQL Server. Les étapes suivantes installent les outils en ligne de commande SQL Server : [sqlcmd](../tools/sqlcmd-utility.md) et [bcp](../tools/bcp-utility.md).

Suivez les étapes suivantes pour installer **mssql-tools** sur Ubuntu. 

1. Importez les clés GPG de référentiel public.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Enregistrez le référentiel Microsoft Ubuntu.
   
   Pour Ubuntu 16.04 :
   
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

   Pour Ubuntu 18.04 :
   
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

1. Mettez à jour la liste des sources et exécutez la commande d'installation avec le package pour développeur unixODBC. Pour plus d’informations, consultez [Installer le pilote Microsoft ODBC pour SQL Server (Linux)](../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

   ```bash
   sudo apt-get update 
   sudo apt-get install mssql-tools unixodbc-dev
   ```

   > [!Note] 
   > Pour mettre à jour vers la dernière version de **mssql-tools**, exécutez les commandes suivantes :
   >
   > ```bash
   > sudo apt-get update 
   > sudo apt-get install mssql-tools 
   > ```

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

[!INCLUDE [Connect, create, and query data](../includes/sql-linux-quickstart-connect-query.md)]
