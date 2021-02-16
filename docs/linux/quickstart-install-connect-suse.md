---
title: 'SUSE : Installer SQL Server sur Linux'
description: Ce démarrage rapide montre comment installer SQL Server 2017 ou SQL Server 2019 sur SUSE Linux Enterprise Server, puis comment créer et interroger une base de données avec sqlcmd.
author: VanMSFT
ms.author: vanto
ms.date: 04/10/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 31ddfb80-f75c-4f51-8540-de6213cb68b8
ms.openlocfilehash: 7aadd757e9e825ace17b69d28f2ab8ecc78123da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340398"
---
# <a name="quickstart-install-sql-server-and-create-a-database-on-suse-linux-enterprise-server"></a>Démarrage rapide : Installer SQL Server et créer une base de données sur SUSE Linux Enterprise Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Dans ce guide de démarrage rapide, vous installez SQL Server 2017 ou SQL Server 2019 sur SUSE Linux Enterprise Server (SLES) v12 SP2. Ensuite, vous vous connectez avec **sqlcmd** pour créer votre première base de données et exécuter des requêtes.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Dans ce guide de démarrage rapide, vous installez SQL Server 2019 sur SUSE Linux Enterprise Server (SLES) v12. Ensuite, vous vous connectez avec **sqlcmd** pour créer votre première base de données et exécuter des requêtes.

> [!IMPORTANT]
> SQL Server 2019 est pris en charge sur SUSE Enterprise Linux Server v12 SP2, SP3, SP4 ou SP5.

::: moniker-end

> [!TIP]
> Ce tutoriel nécessite l'intervention de l'utilisateur et une connexion Internet. Si vous êtes intéressé par les procédures d'installation [sans assistance](sql-server-linux-setup.md#unattended) ou [hors ligne](sql-server-linux-setup.md#offline), voir [Conseils d’installation pour SQL Server sur Linux](sql-server-linux-setup.md).

## <a name="prerequisites"></a>Prérequis

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Vous devez disposer d’une machine SLES version 12 SP2 avec **au moins 2 Go** de mémoire. Le système de fichiers doit être **XFS** ou **EXT4**. Les autres systèmes de fichiers, tels que **BTRFS**, ne sont pas pris en charge.

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Vous devez disposer d’une machine SLES v12 SP2, SP3,SP4 ou SP5 avec **au moins 2 Go** de mémoire. Le système de fichiers doit être **XFS** ou **EXT4**. Les autres systèmes de fichiers, tels que **BTRFS**, ne sont pas pris en charge.

::: moniker-end

Pour installer SUSE Linux Enterprise Server sur votre propre machine, accédez à [https://www.suse.com/products/server](https://www.suse.com/products/server). Vous pouvez également créer des machines virtuelles SLES dans Azure. Consultez [Créer et gérer des machines virtuelles Linux avec l’interface Azure CLI](/azure/virtual-machines/linux/tutorial-manage-vm), puis utilisez `--image SLES` dans l’appel à `az vm create`.

Si vous avez déjà installé une version CTP ou RC de SQL Server, vous devez d’abord supprimer l’ancien référentiel avant de suivre ces étapes. Pour plus d'informations, voir [Configurer des référentiels Linux pour SQL Server 2017 et 2019](sql-server-linux-change-repo.md).

> [!NOTE]
> Pour le moment, le [sous-système Windows pour Linux](/windows/wsl/about) pour Windows 10 n'est pas pris en charge comme cible d'installation.

Pour les autres configurations système requises, voir [Configuration système requise pour SQL Server sur Linux](sql-server-linux-setup.md#system).

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="install-sql-server-2017"></a><a id="install"></a>Installer SQL Server 2017

Pour configurer SQL Server 2017 sur SLES, exécutez les commandes suivantes dans un terminal afin d’installer le package **mssql-server** :

1. Téléchargez le fichier config du référentiel Microsoft SQL Server 2017 SLES :

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo
   ```

   > [!TIP]
   > Si vous voulez tester SQL Server 2019, vous devez inscrire à la place le référentiel SQL Server 2019. Utilisez la commande suivante pour les installations SQL Server 2019 :
   >
   > ```bash
   > sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019.repo
   > ```

2. Actualisez vos référentiels.

   ```bash
   sudo zypper --gpg-auto-import-keys refresh 
   ```
   
   Pour vous assurer que la clé de signature de package Microsoft est installée sur votre système, importez-la à l’aide de la commande ci-dessous : 
   
   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```
   
3. Exécutez les commandes suivantes pour installer SQL Server :

   ```bash
   sudo zypper install -y mssql-server
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
   systemctl status mssql-server
   ```

6. Si vous prévoyez de vous connecter à distance, vous devrez peut-être aussi ouvrir le port TCP de SQL Server (par défaut 1433) sur votre pare-feu. Si vous utilisez le pare-feu SuSE, vous devez modifier le fichier config **/etc/sysconfig/SuSEfirewall2**. Modifiez l'entrée **FW_SERVICES_EXT_TCP** pour inclure le numéro de port SQL Server.

   ```
   FW_SERVICES_EXT_TCP="1433"
   ```

À ce stade, SQL Server fonctionne sur votre machine SLES et est prêt à l'emploi !

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="install-sql-server-2019"></a><a id="install"></a>Installer SQL Server 2019

Pour configurer SQL Server 2019 sur SLES, exécutez les commandes suivantes dans un terminal afin d’installer le package **mssql-server** :

1. Téléchargez le fichier config du référentiel Microsoft SQL Server 2019 SLES :

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019.repo
   ```

2. Actualisez vos référentiels.

   ```bash
   sudo zypper --gpg-auto-import-keys refresh 
   ```

   Pour vous assurer que la clé de signature du package Microsoft est installée sur votre système, utilisez la commande suivante pour importer la clé : 

   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```
   
3. Exécutez les commandes suivantes pour installer SQL Server :

   ```bash
   sudo zypper install -y mssql-server
   ```

4. Une fois l'installation du package terminée, lancez **mssql-conf setup** et suivez les invites pour définir le mot de passe AS et choisir votre édition.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!NOTE]
   > Assurez-vous de spécifier un mot de passe fort pour le compte AS (longueur minimale de 8 caractères, lettres majuscules et minuscules comprises, chiffres de la base 10 et/ou symboles non alphanumériques).

5. Une fois la configuration terminée, vérifiez que le service est en cours d'exécution :

   ```bash
   systemctl status mssql-server
   ```

6. Si vous prévoyez de vous connecter à distance, vous devrez peut-être aussi ouvrir le port TCP de SQL Server (par défaut 1433) sur votre pare-feu. Si vous utilisez le pare-feu SuSE, vous devez modifier le fichier config **/etc/sysconfig/SuSEfirewall2**. Modifiez l'entrée **FW_SERVICES_EXT_TCP** pour inclure le numéro de port SQL Server.

   ```
   FW_SERVICES_EXT_TCP="1433"
   ```

À ce stade, SQL Server 2019 fonctionne sur votre machine SLES et est prêt à l’emploi !

::: moniker-end


## <a name="install-the-sql-server-command-line-tools"></a><a id="tools"></a>Installer les outils en ligne de commande SQL Server

Pour créer une base de données, vous devez vous connecter à un outil capable d’exécuter des instructions Transact-SQL sur SQL Server. Les étapes suivantes installent les outils en ligne de commande SQL Server : [sqlcmd](../tools/sqlcmd-utility.md) et [bcp](../tools/bcp-utility.md).

1. Ajoutez le référentiel Microsoft SQL Server à Zypper.

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo 
   sudo zypper --gpg-auto-import-keys refresh
   ```

1. Installez **mssql-tools** avec le package pour développeur unixODBC. Pour plus d’informations, consultez [Installer le pilote Microsoft ODBC pour SQL Server (Linux)](../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

   ```bash
   sudo zypper install -y mssql-tools unixODBC-devel
   ```

1. Par commodité, ajoutez `/opt/mssql-tools/bin/` à votre variable d'environnement **PATH**. Vous pourrez ainsi exécuter les outils sans spécifier le chemin complet. Exécutez les commandes suivantes afin de modifier la variable **PATH** pour les sessions de connexion et les sessions interactives/sans connexion :

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

[!INCLUDE [Connect, create, and query data](../includes/sql-linux-quickstart-connect-query.md)]