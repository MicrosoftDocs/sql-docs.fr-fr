---
title: 'Docker : Installer des conteneurs pour SQL Server sur Linux'
description: Ce démarrage rapide montre comment utiliser Docker pour exécuter l’image conteneur de SQL Server 2017 et 2019. Ensuite, vous créez et interrogez une base de données avec sqlcmd.
ms.custom: seo-lt-2019, contperf-fy21q1
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.date: 09/07/2020
ms.topic: quickstart
ms.prod: sql
ms.technology: linux
ms.prod_service: linux
ms.assetid: 82737f18-f5d6-4dce-a255-688889fdde69
monikerRange: '>= sql-server-linux-2017 || >= sql-server-2017'
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: f3bf3cff9f7497c5f880cbd75aef26b387c7415e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093282"
---
# <a name="quickstart-run-sql-server-container-images-with-docker"></a>Démarrage rapide : Exécuter des images de conteneur SQL Server avec Docker
[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

> [!NOTE]
> Les exemples ci-dessous utilisent le fichier docker.exe, mais la plupart de ces commandes fonctionnent également avec Podman. Il fournit l’interface CLI semblable au moteur de conteneurs Docker. Vous pouvez trouver plus d’informations sur Podman [ici](http://docs.podman.io/en/latest).

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Dans ce démarrage rapide, vous utilisez Docker pour extraire et exécuter l’image conteneur de SQL Server 2017, [mssql-server-linux](https://hub.docker.com/_/microsoft-mssql-server). Ensuite, vous vous connectez avec **sqlcmd** pour créer votre première base de données et exécuter des requêtes.

> [!TIP]
> Si vous souhaitez exécuter des conteneurs SQL Server 2019, consultez la [version SQL Server 2019 de cet article](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true).

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

> [!NOTE]
> À compter de SQL Server 2019 CU3, Ubuntu 18.04 est pris en charge.

Dans ce guide de démarrage rapide, vous utilisez Docker pour tirer (pull) et exécuter l’image conteneur SQL Server 2019, [mssql-server](https://hub.docker.com/r/microsoft/mssql-server). Ensuite, vous vous connectez avec **sqlcmd** pour créer votre première base de données et exécuter des requêtes.

> [!TIP]
> Ce guide de démarrage rapide crée des conteneurs SQL Server 2019. Si vous préférez créer des conteneurs SQL Server 2017, consultez la [version SQL Server 2017 de cet article](quickstart-install-connect-docker.md?view=sql-server-linux-2017&preserve-view=true).

::: moniker-end

Cette image est composée de SQL Server s’exécutant sur Linux basé sur Ubuntu 18.04. Elle peut être utilisée avec Docker Engine 1.8+ sur Linux ou sur Docker pour Mac/Windows. Ce guide de démarrage rapide se concentre spécifiquement sur l’utilisation de SQL Server sur l’image **Linux**. L’image Windows n’est pas traitée, mais vous pouvez obtenir plus d’informations dans la [page du Hub Docker mssql-server-windows-developer](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/).

## <a name="prerequisites"></a><a id="requirements"></a> Conditions préalables

- Docker Engine 1.8+ sur n’importe quelle distribution Linux prise en charge ou Docker pour Mac/Windows. Pour plus d’informations, consultez [Installer Docker](https://docs.docker.com/engine/installation/).
- Pilote de stockage **overlay2** de Docker. Il s’agit de la valeur par défaut pour la plupart des utilisateurs. Si vous n’utilisez pas ce fournisseur de stockage et que vous devez en changer, consultez les instructions et les avertissements dans la [documentation de Docker pour la configuration d’overlay2](https://docs.docker.com/storage/storagedriver/overlayfs-driver/#configure-docker-with-the-overlay-or-overlay2-storage-driver).
- Au moins 2 Go d’espace disque.
- Au moins 2 Go de RAM.
- [Configuration système requise pour SQL Server sur Linux](sql-server-linux-setup.md#system).

<!--The following H2 is versioned for 2017 and 2019. Much of the content is duplicated, so
any changes to one section should be duplicated in the other-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="pull-and-run-the-2017-container-image"></a><a id="pullandrun2017"></a> Extraire et exécuter l’image conteneur 2017

Avant de commencer les étapes suivantes, assurez-vous que vous avez sélectionné votre interpréteur de commandes préféré (bash, PowerShell ou CMD) en haut de cet article.

1. Extrayez l’image conteneur Linux de SQL Server 2017 à partir du registre de conteneurs Microsoft.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   > [!TIP]
   > Si vous souhaitez exécuter des conteneurs SQL Server 2019, consultez la [version SQL Server 2019 de cet article](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true#pullandrun2019).

   La commande précédente extrait la dernière image conteneur de SQL Server 2017. Si vous voulez extraire une image spécifique, ajoutez un signe deux-points et le nom de la balise (par exemple, `mcr.microsoft.com/mssql/server:2017-GA-ubuntu`). Pour voir toutes les images disponibles, consultez [la page du hub Docker mssql-server](https://hub.docker.com/r/microsoft/mssql-server).

   ::: zone pivot="cs1-bash"
   Pour les commandes bash de cet article, `sudo` est utilisé. Sur macOS, `sudo` peut ne pas être nécessaire. Sur Linux, si vous ne souhaitez pas utiliser `sudo` pour exécuter Docker, vous pouvez configurer un groupe **docker** et ajouter des utilisateurs à ce groupe. Pour plus d’informations, consultez [Étapes consécutives à l’installation pour Linux](https://docs.docker.com/install/linux/linux-postinstall/).

   ::: zone-end

2. Pour exécuter l’image conteneur avec Docker, vous pouvez utiliser la commande suivante à partir d’un interpréteur de commandes bash (Linux/macOS) ou d’une invite de commandes PowerShell avec élévation de privilèges.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" \
      -p 1433:1433 --name sql1 -h sql1 \
      -d \
      mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   
   > [!NOTE]
   > Si vous utilisez PowerShell Core, remplacez les guillemets doubles par des guillemets simples.
   
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d `
      mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d `
      mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   > [!NOTE]
   > Le mot de passe doit suivre la stratégie de mot de passe SQL Server par défaut, sinon le conteneur ne peut pas configurer SQL Server et s’arrête de fonctionner. Par défaut, le mot de passe doit comporter au moins 8 caractères et doit contenir des caractères appartenant à trois des quatre catégories suivantes : Lettres majuscules, lettres minuscules, chiffres en base 10 et symboles. Vous pouvez examiner le journal des erreurs en exécutant la commande [docker logs](https://docs.docker.com/engine/reference/commandline/logs/).
   >
   > Par défaut, cette commande crée un conteneur avec l’édition Développeur de SQL Server 2017. Le processus d’exécution des éditions de production dans des conteneurs est légèrement différent. Pour plus d’informations, consultez [Exécuter des images conteneur de production](./sql-server-linux-docker-container-deployment.md#production).

   Le tableau suivant décrit les paramètres de l’exemple `docker run` précédent :

   | Paramètre | Description |
   |-----|-----|
   | **-e "ACCEPT_EULA=Y"** |  Définissez la variable **ACCEPT_EULA** sur n’importe quelle valeur pour confirmer que vous acceptez le [Contrat de licence utilisateur final](https://go.microsoft.com/fwlink/?linkid=857698). Paramètre obligatoire pour l’image de SQL Server. |
   | **-e "SA_PASSWORD=\<YourStrong@Passw0rd\>"** | Spécifiez votre propre mot de passe fort, qui doit avoir au moins 8 caractères et respecter les [exigences de mot de passe SQL Server](../relational-databases/security/password-policy.md). Paramètre obligatoire pour l’image de SQL Server. |
   | **-p 1433:1433** | Mappez un port TCP sur l’environnement hôte (première valeur) à un port TCP dans le conteneur (deuxième valeur). Dans cet exemple, SQL Server écoute sur TCP 1433 dans le conteneur, mappé au port 1433 sur l’hôte. |
   | **--name sql1** | Spécifiez un nom personnalisé pour le conteneur plutôt qu’un nom généré de manière aléatoire. Si vous exécutez plusieurs conteneurs, vous ne pouvez pas réutiliser le même nom. |
   | **-h sql1** | Utilisé pour définir explicitement le nom d’hôte du conteneur, si vous ne le spécifiez pas, il prend par défaut l’ID du conteneur qui est un GUID système généré de manière aléatoire. |
   | **-d** | Exécutez le conteneur en arrière-plan (démon) |
   | **mcr.microsoft.com/mssql/server:2017-latest** | Image conteneur Linux de SQL Server 2017. |

3. Pour afficher vos conteneurs Docker, utilisez la commande `docker ps`.


   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker ps -a
   ```
   ::: zone-end

   Vous devez obtenir une sortie similaire à la capture d’écran suivante :

   ![Sortie de la commande Docker ps](./media/sql-server-linux-setup-docker/docker-ps-command.png)

4. Si la colonne **ÉTAT** affiche **En cours d’exécution**, SQL Server est en cours d’exécution dans le conteneur et écoute sur le port spécifié dans la colonne **PORTS**. Si la colonne **ÉTAT** pour votre conteneur SQL Server affiche **Quitté**, consultez la [section Résolution des problèmes dans le guide de configuration](./sql-server-linux-docker-container-troubleshooting.md).

Le paramètre `-h` (nom d’hôte), comme indiqué ci-dessus, remplace le nom interne du conteneur par une valeur personnalisée. Il s’agit du nom retourné dans la requête Transact-SQL suivante :

```sql
SELECT @@SERVERNAME,
    SERVERPROPERTY('ComputerNamePhysicalNetBIOS'),
    SERVERPROPERTY('MachineName'),
    SERVERPROPERTY('ServerName')
```

Pour identifier facilement le conteneur cible, définissez `-h` et `--name` sur la même valeur.

5. Pour finir, modifiez votre mot de passe AS car le `SA_PASSWORD` est visible dans la sortie`ps -eax` et stocké dans la variable d’environnement du même nom. Consultez les étapes ci-dessous.

::: moniker-end
<!--End of 2017 "Pull and run" section-->

<!--This is the 2019 version of the "Pull and run" section-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="pull-and-run-the-2019-container-image"></a><a id="pullandrun2019"></a> Extraire et exécuter l’image conteneur 2019

Avant de commencer les étapes suivantes, assurez-vous que vous avez sélectionné votre interpréteur de commandes préféré (bash, PowerShell ou CMD) en haut de cet article.

1. Extrayez l’image conteneur Linux de SQL Server 2019 à partir de Microsoft Azure Container Registry.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   
   > [!NOTE]
   > Si vous utilisez PowerShell Core, remplacez les guillemets doubles par des guillemets simples.
   
   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker pull mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   > [!TIP]
   > Ce guide de démarrage rapide utilise l’image Docker SQL Server 2019. Si vous souhaitez exécuter l’image SQL Server 2017, consultez la [version SQL Server 2017 de cet article](quickstart-install-connect-docker.md?view=sql-server-linux-2017&preserve-view=true#pullandrun2017).

   La commande précédente tire (pull) l’image conteneur SQL Server 2019 basée sur Ubuntu. Pour utiliser à la place des images de conteneur basées sur RedHat, consultez [Exécuter des images de conteneur RHEL](./sql-server-linux-docker-container-deployment.md#rhel). Pour voir toutes les images disponibles, consultez [la page du hub Docker mssql-server-linux](https://hub.docker.com/_/microsoft-mssql-server).

   ::: zone pivot="cs1-bash"
   Pour les commandes bash de cet article, `sudo` est utilisé. Sur macOS, `sudo` peut ne pas être nécessaire. Sur Linux, si vous ne souhaitez pas utiliser `sudo` pour exécuter Docker, vous pouvez configurer un groupe **docker** et ajouter des utilisateurs à ce groupe. Pour plus d’informations, consultez [Étapes consécutives à l’installation pour Linux](https://docs.docker.com/install/linux/linux-postinstall/).
   ::: zone-end

2. Pour exécuter l’image conteneur avec Docker, vous pouvez utiliser la commande suivante à partir d’un interpréteur de commandes bash (Linux/macOS) ou d’une invite de commandes PowerShell avec élévation de privilèges.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" \
      -p 1433:1433 --name sql1 -h sql1 \
      -d mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   > [!NOTE]
   > Le mot de passe doit suivre la stratégie de mot de passe SQL Server par défaut, sinon le conteneur ne peut pas configurer SQL Server et s’arrête de fonctionner. Par défaut, le mot de passe doit comporter au moins 8 caractères et doit contenir des caractères appartenant à trois des quatre catégories suivantes : Lettres majuscules, lettres minuscules, chiffres en base 10 et symboles. Vous pouvez examiner le journal des erreurs en exécutant la commande [docker logs](https://docs.docker.com/engine/reference/commandline/logs/).
   >
   > Par défaut, cette commande crée un conteneur avec l’édition Développeur de SQL Server 2019.

   Le tableau suivant décrit les paramètres de l’exemple `docker run` précédent :

   | Paramètre | Description |
   |-----|-----|
   | **-e "ACCEPT_EULA=Y"** |  Définissez la variable **ACCEPT_EULA** sur n’importe quelle valeur pour confirmer que vous acceptez le [Contrat de licence utilisateur final](https://go.microsoft.com/fwlink/?LinkId=746388). Paramètre obligatoire pour l’image de SQL Server. |
   | **-e "SA_PASSWORD=\<YourStrong@Passw0rd\>"** | Spécifiez votre propre mot de passe fort, qui doit avoir au moins 8 caractères et respecter les [exigences de mot de passe SQL Server](../relational-databases/security/password-policy.md). Paramètre obligatoire pour l’image de SQL Server. |
   | **-p 1433:1433** | Mappez un port TCP sur l’environnement hôte (première valeur) à un port TCP dans le conteneur (deuxième valeur). Dans cet exemple, SQL Server écoute sur TCP 1433 dans le conteneur, mappé au port 1433 sur l’hôte. |
   | **--name sql1** | Spécifiez un nom personnalisé pour le conteneur plutôt qu’un nom généré de manière aléatoire. Si vous exécutez plusieurs conteneurs, vous ne pouvez pas réutiliser le même nom. |
   | **-h sql1** | Utilisé pour définir explicitement le nom d’hôte du conteneur, si vous ne le spécifiez pas, il prend par défaut l’ID du conteneur qui est un GUID système généré de manière aléatoire. |
   | **mcr.microsoft.com/mssql/server:2019-latest** | Image conteneur SQL Server 2019 Ubuntu Linux. |

3. Pour afficher vos conteneurs Docker, utilisez la commande `docker ps`.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker ps -a
   ```
   ::: zone-end

   Vous devez obtenir une sortie similaire à la capture d’écran suivante :

   ![Sortie de la commande Docker ps](./media/sql-server-linux-setup-docker/docker-ps-command.png)

4. Si la colonne **ÉTAT** affiche **En cours d’exécution**, SQL Server est en cours d’exécution dans le conteneur et écoute sur le port spécifié dans la colonne **PORTS**. Si la colonne **ÉTAT** de votre conteneur SQL Server indique **Terminé**, consultez [Résolution des problèmes liés aux conteneurs Docker SQL Server](sql-server-linux-docker-container-troubleshooting.md).

Le paramètre `-h` (nom d’hôte), comme indiqué ci-dessus, remplace le nom interne du conteneur par une valeur personnalisée. Il permet de changer le nom interne du conteneur en une valeur personnalisée. Il s’agit du nom retourné dans la requête Transact-SQL suivante :

```sql
SELECT @@SERVERNAME,
    SERVERPROPERTY('ComputerNamePhysicalNetBIOS'),
    SERVERPROPERTY('MachineName'),
    SERVERPROPERTY('ServerName')
```

Pour identifier facilement le conteneur cible, définissez `-h` et `--name` sur la même valeur.


5. Pour finir, modifiez votre mot de passe AS car le `SA_PASSWORD` est visible dans la sortie`ps -eax` et stocké dans la variable d’environnement du même nom. Consultez les étapes ci-dessous.


::: moniker-end
<!--End of 2019 "Pull and run" section-->



## <a name="change-the-sa-password"></a><a id="sapassword"></a> Changer le mot de passe AS

<!-- This section was pasted in from includes/sql-server-linux-change-docker-password.md, to better support zone pivots. 2019/02/11 -->

Le compte **SA** est un administrateur système sur l’instance SQL Server qui est créée lors de l’installation. Une fois le conteneur SQL Server créé, la variable d’environnement `SA_PASSWORD` que vous avez spécifiée peut être découverte en exécutant `echo $SA_PASSWORD` dans le conteneur. Pour des raisons de sécurité, changez le mot de passe pour SA.

1. Choisissez un mot de passe fort à utiliser pour l’utilisateur SA.

1. Utilisez `docker exec` pour exécuter **sqlcmd** pour changer le mot de passe avec Transact-SQL. Dans l’exemple suivant, remplacez l’ancien mot de passe, `<YourStrong!Passw0rd>`, et le nouveau mot de passe, `<YourNewStrong!Passw0rd>`, par vos propres valeurs de mot de passe.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P "<YourStrong@Passw0rd>" \
      -Q 'ALTER LOGIN SA WITH PASSWORD="<YourNewStrong@Passw0rd>"'
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourStrong@Passw0rd>" `
      -Q "ALTER LOGIN SA WITH PASSWORD='<YourNewStrong@Passw0rd>'"
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourStrong!Passw0rd>" `
      -Q "ALTER LOGIN SA WITH PASSWORD='<YourNewStrong@Passw0rd>'"
   ```
   ::: zone-end

## <a name="connect-to-sql-server"></a>Se connecter à SQL Server

La procédure suivante utilise l’outil en ligne de commande SQL Server, [**sqlcmd**](../tools/sqlcmd-utility.md), dans le conteneur pour se connecter à SQL Server.

1. Utilisez la commande `docker exec -it` pour démarrer un interpréteur de commandes bash interactif dans votre conteneur en cours d’exécution. Dans l’exemple suivant, `sql1` est le nom spécifié par le paramètre `--name` quand vous avez créé le conteneur.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker exec -it sql1 "bash"
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker exec -it sql1 "bash"
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker exec -it sql1 "bash"
   ```
   ::: zone-end

2. Une fois dans le conteneur, connectez-vous localement avec sqlcmd. Sqlcmd n’est pas dans le chemin par défaut, vous devez spécifier le chemin complet.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "<YourNewStrong@Passw0rd>"
   ```

   > [!TIP]
   > Vous pouvez omettre le mot de passe sur la ligne de commande pour être invité à l’entrer.

3. Si l’opération réussit, vous devez accéder à une invite de commandes **sqlcmd** : `1>`.

## <a name="create-and-query-data"></a>Créer et interroger des données

Les sections suivantes vous guident lors de l’utilisation de **sqlcmd** et Transact-SQL pour créer une base de données, ajouter des données et exécuter une requête.

### <a name="create-a-new-database"></a>Créer une base de données

La procédure suivante crée une base de données nommée `TestDB`.

1. À partir de l’invite de commandes **sqlcmd**, collez la commande Transact-SQL suivante pour créer une base de données de test :

   ```sql
   CREATE DATABASE TestDB
   ```

2. Sur la ligne suivante, écrivez une requête pour retourner le nom de toutes les bases de données sur votre serveur :

   ```sql
   SELECT Name from sys.Databases
   ```

3. Les deux commandes précédentes n’ont pas été exécutées immédiatement. Tapez `GO` sur une nouvelle ligne pour exécuter les commandes précédentes :

   ```sql
   GO
   ```

### <a name="insert-data"></a>Insertion des données

Créez ensuite une table, `Inventory`, et insérez deux nouvelles lignes.

1. À partir de l’invite de commandes **sqlcmd**, basculez le contexte vers la nouvelle base de données `TestDB` :

   ```sql
   USE TestDB
   ```

2. Créez une table nommée `Inventory` :

   ```sql
   CREATE TABLE Inventory (id INT, name NVARCHAR(50), quantity INT)
   ```

3. Insérez des données dans la nouvelle table :

   ```sql
   INSERT INTO Inventory VALUES (1, 'banana', 150); INSERT INTO Inventory VALUES (2, 'orange', 154);
   ```

4. Tapez `GO` pour exécuter les commandes précédentes :

   ```sql
   GO
   ```

### <a name="select-data"></a>Sélectionner les données

Exécutez maintenant une requête pour retourner des données de la table `Inventory`.

1. Dans l’invite de commandes **sqlcmd**, entrez une requête qui retourne les lignes de la table `Inventory` dont la quantité est supérieure à 152 :

   ```sql
   SELECT * FROM Inventory WHERE quantity > 152;
   ```

2. Exécutez la commande :

   ```sql
   GO
   ```

### <a name="exit-the-sqlcmd-command-prompt"></a>Quitter l’invite de commandes sqlcmd

1. Pour mettre fin à votre session **sqlcmd**, tapez `QUIT` :

   ```sql
   QUIT
   ```

2. Pour quitter l’invite de commandes interactive dans votre conteneur, tapez `exit`. Le conteneur continue de s’exécuter une fois que vous avez quitté l’interpréteur de commandes bash interactif.

## <a name="connect-from-outside-the-container"></a><a id="connectexternal"></a> Se connecter en dehors du conteneur

Vous pouvez aussi vous connecter à l’instance SQL Server sur votre machine Docker à partir de n’importe quel outil externe Linux, Windows ou macOS qui prend en charge les connexions SQL.

Les étapes suivantes utilisent **sqlcmd** en dehors de votre conteneur pour se connecter à SQL Server en cours d’exécution dans le conteneur. Ces étapes supposent que vous avez déjà installé les outils en ligne de commande SQL Server en dehors de votre conteneur. Les mêmes principes s’appliquent quand vous utilisez d’autres outils, mais le processus de connexion est propre à chaque outil.

1. Recherchez l’adresse IP de la machine qui héberge votre conteneur. Sur Linux, utilisez **ifconfig** ou **ip addr**. Sur Windows, utilisez **ipconfig**.

1. Pour cet exemple, installez l'outil **sqlcmd** sur votre ordinateur client. Pour plus d’informations, consultez [Installer sqlcmd sur Windows](../tools/sqlcmd-utility.md) ou [Installer sqlcmd sur Linux](sql-server-linux-setup-tools.md).

1. Exécutez sqlcmd en spécifiant l’adresse IP et le port mappé au port 1433 dans votre conteneur. Dans cet exemple, il s’agit du même port, 1433, sur l’hôte. Si vous avez spécifié un autre port mappé sur l’ordinateur hôte, vous pouvez l’utiliser ici. Vous avez également besoin d’ouvrir le port d’entrée approprié sur votre pare-feu pour autoriser la connexion.

   ::: zone pivot="cs1-bash"
   ```bash
   sqlcmd -S <ip_address>,1433 -U SA -P "<YourNewStrong@Passw0rd>"
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   sqlcmd -S <ip_address>,1433 -U SA -P "<YourNewStrong@Passw0rd>"
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   sqlcmd -S <ip_address>,1433 -U SA -P "<YourNewStrong@Passw0rd>"
   ```
   ::: zone-end

1. Exécutez les commandes Transact-SQL. Quand vous avez terminé, tapez `QUIT`.

Voici d’autres outils courants pour vous connecter à SQL Server :

- [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)
- [SQL Server Management Studio (SSMS) sur Windows](sql-server-linux-manage-ssms.md)
- [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md)
- [mssql-cli (préversion)](https://github.com/dbcli/mssql-cli/blob/master/doc/usage_guide.md)
- [PowerShell Core](sql-server-linux-manage-powershell-core.md)

## <a name="remove-your-container"></a>Supprimer votre conteneur

Si vous voulez supprimer le conteneur SQL Server utilisé dans ce didacticiel, exécutez les commandes suivantes :

::: zone pivot="cs1-bash"
```bash
sudo docker stop sql1
sudo docker rm sql1
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker stop sql1
docker rm sql1
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker stop sql1
docker rm sql1
```
::: zone-end

> [!WARNING]
> L’arrêt et la suppression d’un conteneur supprime définitivement toutes les données SQL Server dans le conteneur. Si vous avez besoin de conserver vos données, [créez et copiez un fichier de sauvegarde en dehors du conteneur](tutorial-restore-backup-in-sql-server-container.md) ou utilisez une [technique de persistance de données de conteneur](sql-server-linux-docker-container-configure.md#persist).

## <a name="docker-demo"></a>Démonstration Docker

Une fois que vous avez essayé d’utiliser l’image conteneur de SQL Server pour Docker, vous pouvez être intéressé par des informations sur l’utilisation de Docker pour améliorer le développement et les tests. La vidéo suivante illustre l’utilisation de Docker dans une intégration continue et un scénario de déploiement.

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T152/player]

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir un didacticiel sur la restauration de fichiers de sauvegarde de base de données dans un conteneur, consultez [Restaurer une base de données SQL Server dans un conteneur Linux Docker](tutorial-restore-backup-in-sql-server-container.md). Explorez d’autres scénarios, comme l’exécution de [plusieurs conteneurs](sql-server-linux-docker-container-deployment.md#multiple), la [persistance des données](sql-server-linux-docker-container-configure.md#persist) et la [résolution des problèmes](sql-server-linux-docker-container-troubleshooting.md).

Consultez également le [dépôt GitHub mssql-docker](https://github.com/Microsoft/mssql-docker) pour obtenir des ressources, des commentaires et les problèmes connus.
