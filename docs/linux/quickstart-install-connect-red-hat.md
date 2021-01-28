---
title: 'RHEL : Installer SQL Server sur Linux'
titleSuffix: SQL Server
description: Ce guide de démarrage rapide montre comment installer SQL Server 2017 ou SQL Server 2019 sur RHEL (Red Hat Enterprise Linux), puis comment créer et interroger une base de données avec sqlcmd.
author: VanMSFT
ms.custom: seo-lt-2019
ms.author: vanto
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 92503f59-96dc-4f6a-b1b0-d135c43e935e
ms.openlocfilehash: cb266632fccaceb4639c7fdf0cfbf5368aad2c80
ms.sourcegitcommit: 2bdf1f1ee88f4fe3e872227d025e965e95d1b2b4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98711975"
---
# <a name="quickstart-install-sql-server-and-create-a-database-on-red-hat"></a>Démarrage rapide : Installer SQL Server et créer une base de données sur Red Hat

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Dans ce démarrage rapide, vous installez SQL Server 2017 ou SQL Server 2019 sur Red Hat Enterprise Linux (RHEL). Ensuite, vous vous connectez avec **sqlcmd** pour créer votre première base de données et exécuter des requêtes.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Dans ce guide de démarrage rapide, vous installez SQL Server 2019 sur Red Hat Enterprise Linux (RHEL) 7/8. Ensuite, vous vous connectez avec **sqlcmd** pour créer votre première base de données et exécuter des requêtes.

::: moniker-end

> [!TIP]
> Ce tutoriel nécessite l'intervention de l'utilisateur et une connexion Internet. Si vous êtes intéressé par les procédures d'installation [sans assistance](sql-server-linux-setup.md#unattended) ou [hors ligne](sql-server-linux-setup.md#offline), voir [Conseils d’installation pour SQL Server sur Linux](sql-server-linux-setup.md).

## <a name="prerequisites"></a>Prérequis

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Vous devez disposer d’une machine RHEL 7.3 - 7.8 ou 8.0 - 8.3 avec **au moins 2 Go** de mémoire.

::: moniker-end

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Vous devez disposer d’une machine RHEL 7.3 - 7.8 ou 8.0 - 8.3 avec **au moins 2 Go** de mémoire.

::: moniker-end

Pour installer Red Hat Enterprise Linux sur votre propre machine, voir [https://access.redhat.com/products/red-hat-enterprise-linux/evaluation](https://access.redhat.com/products/red-hat-enterprise-linux/evaluation). Vous pouvez également créer des machines virtuelles RHEL dans Azure. Consultez [Créer et gérer des machines virtuelles Linux avec l’interface Azure CLI](/azure/virtual-machines/linux/tutorial-manage-vm), puis utilisez `--image RHEL` dans l’appel à `az vm create`.

Si vous avez déjà installé une version CTP ou RC de SQL Server, vous devez d’abord supprimer l’ancien référentiel avant de suivre ces étapes. Pour plus d'informations, voir [Configurer des référentiels Linux pour SQL Server 2017 et 2019](sql-server-linux-change-repo.md).

Pour les autres configurations système requises, voir [Configuration système requise pour SQL Server sur Linux](sql-server-linux-setup.md#system).

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="install-sql-server"></a><a id="install"></a>Installer SQL Server

> [!NOTE]
> RHEL 8 est pris en charge pour SQL Server 2017 à partir de CU20. Les commandes suivantes pour SQL Server 2017 pointent vers le référentiel RHEL 8. RHEL 8 n’est pas préinstallé avec python2, ce qui est requis par SQL Server. Avant de commencer les étapes d’installation de SQL Server, exécutez la commande et vérifiez que python2 est sélectionné comme interpréteur :
>
> ```
> sudo alternatives --config python
> # If not configured, install python2 and openssl10 using the following commands: 
> sudo yum install python2
> sudo yum install compat-openssl10
> # Configure python2 as the default interpreter using this command: 
> sudo alternatives --config python
> ```
> Pour plus d’informations, consultez le blog suivant sur l’installation de python2 et sa configuration en tant qu’interpréteur par défaut : https://www.redhat.com/en/blog/installing-microsoft-sql-server-red-hat-enterprise-linux-8-beta.
>

Pour configurer SQL Server sur RHEL, exécutez les commandes suivantes dans un terminal afin d’installer le package **mssql-server** :

1. Téléchargez le fichier config du référentiel Microsoft SQL Server 2017 Red Hat :

   Pour RHEL7 :
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo
   ```
   
   Pour RHEL8 :
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2017.repo
   ```

   > [!TIP]
   > Si vous voulez tester SQL Server 2019, vous devez inscrire à la place le référentiel SQL Server 2019. Utilisez la commande suivante pour les installations SQL Server 2019 :
   >
   > Pour RHEL7 :
   >
   > ```bash
   > sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo
   > ```
   >
   > Pour RHEL8 :
   >
   > ```bash
   > sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019.repo
   > ```

2. Exécutez les commandes suivantes pour installer SQL Server :

   ```bash
   sudo yum install -y mssql-server
   ```

3. Une fois l'installation du package terminée, lancez **mssql-conf setup** et suivez les invites pour définir le mot de passe AS et choisir votre édition.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!TIP]
   > Les éditions suivantes de SQL Server 2017 sont sous licence libre : Evaluation, Developer et Express.

   > [!NOTE]
   > Assurez-vous de spécifier un mot de passe fort pour le compte AS (longueur minimale de 8 caractères, lettres majuscules et minuscules comprises, chiffres de la base 10 et/ou symboles non alphanumériques).

4. Une fois la configuration terminée, vérifiez que le service est en cours d'exécution :

   ```bash
   systemctl status mssql-server
   ```

5. Pour autoriser les connexions à distance, ouvrez le port SQL Server sur le pare-feu de RHEL. Le port par défaut du serveur SQL est TCP 1433. Si vous utilisez **FirewallD** pour votre pare-feu, vous pouvez utiliser les commandes suivantes :

   ```bash
   sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
   sudo firewall-cmd --reload
   ```

À ce stade, SQL Server fonctionne sur votre machine RHEL et est prêt à l'emploi !

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="install-sql-server"></a><a id="install"></a>Installer SQL Server

> [!NOTE]
> Les commandes suivantes pour SQL Server 2019 pointent vers le référentiel RHEL 8. RHEL 8 n’est pas préinstallé avec python2, ce qui est requis par SQL Server. Avant de commencer les étapes d’installation de SQL Server, exécutez la commande et vérifiez que python2 est sélectionné comme interpréteur : 
>
> ```
> sudo alternatives --config python
> # If not configured, install python2 and openssl10 using the following commands: 
> sudo yum install python2
> sudo yum install compat-openssl10
> # Configure python2 as the default interpreter using this command: 
> sudo alternatives --config python
> ``` 
> 
> Pour plus d’informations sur ces étapes, consultez le blog suivant qui traite de l’installation de python2 et de sa configuration comme interpréteur par défaut : https://www.redhat.com/en/blog/installing-microsoft-sql-server-red-hat-enterprise-linux-8-beta.
> 

Pour configurer SQL Server sur RHEL, exécutez les commandes suivantes dans un terminal afin d’installer le package **mssql-server** :

1. Téléchargez le fichier config du référentiel Microsoft SQL Server 2019 Red Hat :
   
   Pour RHEL7 :
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo
   ```
   
   Pour RHEL8 :
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019.repo
   ```

2. Exécutez les commandes suivantes pour installer SQL Server :

   ```bash
   sudo yum install -y mssql-server
   ```

3. Une fois l'installation du package terminée, lancez **mssql-conf setup** et suivez les invites pour définir le mot de passe AS et choisir votre édition.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!NOTE]
   > Assurez-vous de spécifier un mot de passe fort pour le compte AS (longueur minimale de 8 caractères, lettres majuscules et minuscules comprises, chiffres de la base 10 et/ou symboles non alphanumériques).

4. Une fois la configuration terminée, vérifiez que le service est en cours d'exécution :

   ```bash
   systemctl status mssql-server
   ```

5. Pour autoriser les connexions à distance, ouvrez le port SQL Server sur le pare-feu de RHEL. Le port par défaut du serveur SQL est TCP 1433. Si vous utilisez **FirewallD** pour votre pare-feu, vous pouvez utiliser les commandes suivantes :

   ```bash
   sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
   sudo firewall-cmd --reload
   ```

À ce stade, SQL Server 2019 fonctionne sur votre machine RHEL et est prêt à l’emploi !

::: moniker-end

## <a name="install-the-sql-server-command-line-tools"></a><a id="tools"></a>Installer les outils en ligne de commande SQL Server

Pour créer une base de données, vous devez vous connecter à un outil capable d’exécuter des instructions Transact-SQL sur SQL Server. Les étapes suivantes installent les outils en ligne de commande SQL Server : [sqlcmd](../tools/sqlcmd-utility.md) et [bcp](../tools/bcp-utility.md).

1. Téléchargez le fichier config du référentiel Microsoft Red Hat.

   Pour RHEL7 :
   
   ```bash
   sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/7/prod.repo
   ```
   
   Pour RHEL8 :
   
   ```bash
   sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/8/prod.repo
   ```

1. Si vous aviez une version précédente de **mssql-tools** installée, supprimez tous les anciens packages unixODBC.

   ```bash
   sudo yum remove unixODBC-utf16 unixODBC-utf16-devel
   ```

1. Exécutez les commandes suivantes pour installer **mssql-tools** avec le package pour développeur unixODBC. Pour plus d’informations, consultez [Installer le pilote Microsoft ODBC pour SQL Server (Linux)](../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

   ```bash
   sudo yum install -y mssql-tools unixODBC-devel
   ```

1. Par commodité, ajoutez `/opt/mssql-tools/bin/` à votre variable d'environnement **PATH**. Vous pourrez ainsi exécuter les outils sans spécifier le chemin complet. Exécutez les commandes suivantes afin de modifier la variable **PATH** pour les sessions de connexion et les sessions interactives/sans connexion :

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

[!INCLUDE [Connect, create, and query data](../includes/sql-linux-quickstart-connect-query.md)]
