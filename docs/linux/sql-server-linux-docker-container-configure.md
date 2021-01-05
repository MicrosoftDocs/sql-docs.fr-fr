---
title: Configurer et personnaliser des conteneurs Docker SQL Server
description: Comprendre les différentes façons de personnaliser des conteneurs Docker SQL Server et de les configurer en fonction de vos besoins
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: 3fa589fdb522014994923edfa1a7d7858cfa7b72
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878808"
---
# <a name="configure-and-customize-sql-server-docker-containers"></a>Configurer et personnaliser des conteneurs Docker SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article explique comment vous pouvez configurer et personnaliser des conteneurs Docker SQL Server : persistance de vos données, déplacement de vos fichiers entre des conteneurs et modification des paramètres par défaut.

## <a name="create-a-customized-container"></a><a id="customcontainer"></a> Créer un conteneur personnalisé

Il est possible de créer votre propre [Dockerfile](https://docs.docker.com/engine/reference/builder/#usage) pour créer un conteneur SQL Server personnalisé. Pour plus d’informations, consultez [une démonstration qui associe SQL Server et une application Node](https://github.com/twright-msft/mssql-node-docker-demo-app). Si vous créez votre propre Dockerfile, tenez compte du processus de premier plan, car ce processus contrôle la durée de vie du conteneur. S’il se termine, le conteneur s’arrête. Par exemple, si vous souhaitez exécuter un script et démarrer SQL Server, assurez-vous que le processus SQL Server est la commande la plus à droite. Toutes les autres commandes sont exécutées en arrière-plan. La commande suivante illustre cela à l’intérieur d’un fichier Dockerfile :

```bash
/usr/src/app/do-my-sql-commands.sh & /opt/mssql/bin/sqlservr
```

Si vous avez inversé les commandes dans l’exemple précédent, le conteneur s’arrêtera à la fin du script do-my-sql-commands.sh.

## <a name="persist-your-data"></a><a id="persist"></a> Rendre vos données persistantes

Vos modifications de configuration et fichiers de base de données SQL Server sont conservés dans le conteneur même si vous redémarrez le conteneur avec `docker stop` et `docker start`. Toutefois, si vous supprimez le conteneur avec `docker rm`, tout ce qui se trouve dans le conteneur est supprimé, y compris SQL Server et vos bases de données. La section suivante explique comment utiliser des **volumes de données** pour conserver les fichiers de votre base de données même si les conteneurs associés sont supprimés.

> [!IMPORTANT]
> Par SQL Server, il est essentiel que vous compreniez la persistance des données dans Docker. En plus de la discussion de cette section, consultez la documentation de Docker pour savoir [comment gérer les données dans les conteneurs Docker](https://docs.docker.com/engine/tutorials/dockervolumes/).

### <a name="mount-a-host-directory-as-data-volume"></a>Monter un répertoire hôte en tant que volume de données

La première option consiste à monter un répertoire sur votre hôte en tant que volume de données dans votre conteneur. Pour ce faire, utilisez la commande `docker run` avec l'indicateur `-v <host directory>:/var/opt/mssql`. Cela permet de restaurer les données entre les exécutions de conteneur.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

Cette technique vous permet également de partager et d’afficher les fichiers sur l’ordinateur hôte en dehors de Docker.

> [!IMPORTANT]
> Actuellement, le mappage du volume hôte pour **Docker sur Windows** ne prend pas en charge le mappage de l’intégralité du répertoire `/var/opt/mssql`. Toutefois, vous pouvez mapper un sous-répertoire comme `/var/opt/mssql/data` à votre ordinateur hôte.
>
> Le mappage du volume hôte pour **Docker sur Mac** avec l’image SQL Server sur Linux n’est pas pris en charge pour l’instant. Utilisez des conteneurs de volume de données à la place. Cette restriction est spécifique au répertoire `/var/opt/mssql`. La lecture à partir d’un répertoire monté fonctionne bien. Par exemple, vous pouvez monter un répertoire hôte à l’aide de -v sur Mac et restaurer une sauvegarde à partir d’un fichier .bak résidant sur l’hôte.

### <a name="use-data-volume-containers"></a>Utiliser des conteneurs de volume de données

La deuxième option consiste à utiliser un conteneur de volume de données. Vous pouvez créer un conteneur de volume de données en spécifiant un nom de volume à la place d’un répertoire hôte avec le paramètre `-v`. L’exemple suivant crée un volume de données partagé nommé **sqlvolume**.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

> [!NOTE]
> Cette technique pour la création implicite d’un volume de données dans la commande d’exécution ne fonctionne pas avec les versions antérieures de Docker. Dans ce cas, utilisez les étapes explicites décrites dans la documentation de Docker, [Création et montage d’un conteneur de volume de données](https://docs.docker.com/engine/tutorials/dockervolumes/#creating-and-mounting-a-data-volume-container).

Même si vous arrêtez et supprimez ce conteneur, le volume de données persiste. Vous pouvez l’afficher avec la commande `docker volume ls`.

```command
docker volume ls
```

Si vous créez ensuite un autre conteneur avec le même nom de volume, le nouveau conteneur utilise les mêmes données SQL Server contenues dans le volume.

Pour supprimer un conteneur de volume de données, utilisez la commande `docker volume rm`.

> [!WARNING]
> Si vous supprimez le conteneur de volume de données, toutes les données de *SQL Server* dans le conteneur sont définitivement supprimées.

### <a name="backup-and-restore"></a>Sauvegarde et restauration

Outre ces techniques de conteneur, vous pouvez également utiliser des techniques de sauvegarde et de restauration SQL Server standard. Vous pouvez utiliser des fichiers de sauvegarde pour protéger vos données ou déplacer les données vers une autre instance de SQL Server. Pour plus d’informations, consultez [Sauvegarder et restaurer des bases de données SQL Server sur Linux](sql-server-linux-backup-and-restore-database.md).

> [!WARNING]
> Si vous créez des sauvegardes, assurez-vous de créer ou de copier les fichiers de sauvegarde en dehors du conteneur. Dans le cas contraire, si le conteneur est supprimé, les fichiers de sauvegarde sont également supprimés.

## <a name="copy-files-from-a-container"></a>Copier des fichiers à partir d’un conteneur

Pour copier un fichier hors du conteneur, utilisez la commande suivante :

```command
docker cp <Container ID>:<Container path> <host path>
```

Vous pouvez obtenir l’ID du conteneur en exécutant la commande `docker ps -a`.

**Exemple :**

::: zone pivot="cs1-bash"
```bash
docker cp d6b75213ef80:/var/opt/mssql/log/errorlog /tmp/errorlog
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker cp d6b75213ef80:/var/opt/mssql/log/errorlog C:\Temp\errorlog
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker cp d6b75213ef80:/var/opt/mssql/log/errorlog C:\Temp\errorlog
```
::: zone-end

## <a name="copy-files-into-a-container"></a>Copier des fichiers dans un conteneur

Pour copier un fichier dans le conteneur, utilisez la commande suivante :

```command
docker cp <Host path> <Container ID>:<Container path>
```

**Exemple :**

::: zone pivot="cs1-bash"
```bash
docker cp /tmp/mydb.mdf d6b75213ef80:/var/opt/mssql/data
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker cp C:\Temp\mydb.mdf d6b75213ef80:/var/opt/mssql/data
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker cp C:\Temp\mydb.mdf d6b75213ef80:/var/opt/mssql/data
```
::: zone-end

## <a name="configure-the-timezone"></a><a id="tz"></a> Configurer le fuseau horaire

Pour exécuter SQL Server dans un conteneur Linux avec un fuseau horaire spécifique, configurez la variable d’environnement `TZ`. Pour trouver la valeur de fuseau horaire appropriée, exécutez la commande `tzselect` à partir d’une invite Bash Linux :

```command
tzselect
```

Après avoir sélectionné le fuseau horaire, `tzselect` affiche une sortie similaire à ce qui suit :

```output
The following information has been given:

        United States
        Pacific

Therefore TZ='America/Los_Angeles' will be used.
```

Vous pouvez utiliser ces informations pour définir la même variable d’environnement dans votre conteneur Linux. L’exemple suivant montre comment exécuter SQL Server dans un conteneur dans le fuseau horaire `Americas/Los_Angeles` :

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
sudo docker run -e 'ACCEPT_EULA=Y' -e 'A_PASSWORD=<YourStrong!Passw0rd>' \
-p 1433:1433 --name sql1 \
-e 'TZ=America/Los_Angeles'\
-d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sudo docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-p 1433:1433 --name sql1 `
-e "TZ=America/Los_Angeles" `
-d mcr.microsoft.com/mssql/server:2017-latest 
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sudo docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-p 1433:1433 --name sql1 ^
-e "TZ=America/Los_Angeles" ^
-d mcr.microsoft.com/mssql/server:2017-latest 
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
   -p 1433:1433 --name sql1 \
   -e 'TZ=America/Los_Angeles'\
   -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sudo docker run -e 'ACCEPT_EULA=Y' -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
   -p 1433:1433 --name sql1 `
   -e "TZ=America/Los_Angeles" `
   -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sudo docker run -e 'ACCEPT_EULA=Y' -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
   -p 1433:1433 --name sql1 `
   -e "TZ=America/Los_Angeles" `
   -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="change-the-default-file-location"></a><a id="changefilelocation"></a> Modifier l’emplacement des fichiers par défaut

Ajoutez la variable `MSSQL_DATA_DIR` pour modifier votre répertoire de données dans votre commande `docker run`, puis montez un volume à cet emplacement auquel l’utilisateur de votre conteneur a accès.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=MyStrongPassword' -e 'MSSQL_DATA_DIR=/my/file/path' -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=MyStrongPassword' -e 'MSSQL_DATA_DIR=/my/file/path' -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="next-steps"></a>Étapes suivantes

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Bien démarrer avec les images de conteneur SQL Server 2017 sur Docker en suivant le [démarrage rapide](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Bien démarrer avec les images de conteneur SQL Server 2019 sur Docker en suivant le [démarrage rapide](quickstart-install-connect-docker.md)

::: moniker-end

- [Déployer des conteneurs Docker SQL Server et s’y connecter](sql-server-linux-docker-container-deployment.md)

- [Résolution des problèmes liés aux conteneurs Docker SQL Server](sql-server-linux-docker-container-troubleshooting.md)

- [Sécuriser des conteneurs Docker SQL Server](sql-server-linux-docker-container-security.md)
