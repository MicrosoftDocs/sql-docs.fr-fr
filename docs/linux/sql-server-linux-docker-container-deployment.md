---
title: Déployer des conteneurs Docker SQL Server et s’y connecter
description: Examiner comment SQL Server peut être déployé sur des conteneurs Docker et découvrir les différents outils permettant de se connecter à SQL Server depuis l’intérieur et l’extérieur du conteneur
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 82737f18-f5d6-4dce-a255-688889fdde69
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: e4524dccd4241a2400e8bf1be76bbc326149a8c6
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878796"
---
# <a name="deploy-and-connect-to-sql-server-docker-containers"></a>Déployer des conteneurs Docker SQL Server et s’y connecter

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article explique comment déployer des conteneurs Docker SQL Server et s’y connecter.

Pour d’autres scénarios de déploiement, consultez :

- [Windows](../database-engine/install-windows/install-sql-server.md)
- [Linux](../linux/sql-server-linux-setup.md)
- [Kubernetes - Clusters Big Data](../big-data-cluster/deploy-get-started.md)

> [!NOTE]
> Cet article se concentre spécifiquement sur l’utilisation de l’image mssql-server-linux. L’image Windows n’est pas traitée, mais vous pouvez obtenir plus d’informations dans la [page du Hub Docker mssql-server-windows](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/).

> [!IMPORTANT]
> Avant de choisir d’exécuter un conteneur SQL Server pour les cas d’utilisation de production, consultez notre [stratégie de support pour les conteneurs SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server) afin de vérifier que vous êtes en cours d’exécution sur une configuration prise en charge.

Cette vidéo de 6 minutes offre une introduction à l’exécution de SQL Server sur les conteneurs :

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2019-in-Containers/player?WT.mc_id=dataexposed-c9-niner]

## <a name="pull-and-run-the-container-image"></a>Extraire et exécuter l’image conteneur

Pour extraire et exécuter les images conteneur Docker pour SQL Server 2017 et SQL Server 2019, suivez les prérequis et les étapes du guide de démarrage rapide suivant :

- [Exécuter l’image conteneur de SQL Server 2017 avec Docker](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)
- [Exécuter l’image conteneur SQL Server 2019 avec Docker](quickstart-install-connect-docker.md?view=sql-server-ver15&preserve-view=true)

Cet article de configuration fournit des scénarios d’utilisation supplémentaires dans les sections suivantes.

## <a name="connect-and-query"></a>Se connecter et interroger

Vous pouvez vous connecter à et interroger SQL Server dans un conteneur à partir de l’extérieur du conteneur ou au sein du conteneur. Les sections suivantes détaillent les deux scénarios.

### <a name="tools-outside-the-container"></a>Outils en dehors du conteneur

Vous pouvez vous connecter à l’instance SQL Server sur votre machine Docker à partir de n’importe quel outil externe Linux, Windows ou macOS qui prend en charge les connexions SQL. Certains outils courants sont les suivants :

- [Azure Data Studio](../azure-data-studio/quickstart-sql-server.md)
- [sqlcmd](sql-server-linux-setup-tools.md)
- [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)
- [SQL Server Management Studio (SSMS) sur Windows](sql-server-linux-manage-ssms.md)

L’exemple suivant utilise **sqlcmd** pour se connecter à SQL Server s’exécutant dans un conteneur Docker. L’adresse IP dans la chaîne de connexion est l’adresse IP de l’ordinateur hôte qui exécute le conteneur.

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4 -U SA -P "<YourPassword>"
```
::: zone-end

Si vous avez mappé un port hôte qui n’est pas la valeur **1433** par défaut, ajoutez ce port à la chaîne de connexion. Par exemple, si vous avez spécifié `-p 1400:1433` dans votre commande `docker run`, connectez-vous en spécifiant explicitement le port 1400.

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4,1400 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4,1400 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4,1400 -U SA -P "<YourPassword>"
```
::: zone-end

### <a name="tools-inside-the-container"></a>Outils au sein du conteneur

À partir de SQL Server 2017, les [outils en ligne de commande SQL Server](sql-server-linux-setup-tools.md) sont inclus dans l’image conteneur. Si vous joignez une invite de commandes interactive à l’image, vous pouvez exécuter les outils localement.

1. Utilisez la commande `docker exec -it` pour démarrer un interpréteur de commandes bash interactif dans votre conteneur en cours d’exécution. Dans l’exemple suivant, `e69e056c702d` est l’ID de conteneur.

    ```bash
    docker exec -it e69e056c702d "bash"
    ```

    > [!TIP]
    > Vous n’avez pas toujours besoin de spécifier l’ID de conteneur entier. Vous ne devez spécifier que suffisamment de caractères pour l’identifier de manière unique. Ainsi, dans cet exemple, il peut suffire d’utiliser `e6` ou `e69` plutôt que l’ID complet. Pour déterminer l’ID du conteneur, exécutez la commande `docker ps -a`.

2. Une fois dans le conteneur, connectez-vous localement avec sqlcmd. Sqlcmd n’est pas dans le chemin par défaut, vous devez spécifier le chemin complet.

    ```bash
    /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourPassword>'
    ```

3. Lorsque vous avez terminé avec sqlcmd, saisissez `exit`.

4. Lorsque vous avez terminé avec l’invite de commandes interactive, saisissez `exit`. Le conteneur continue de s’exécuter une fois que vous avez quitté l’interpréteur de commandes bash interactif.

## <a name="check-the-container-version"></a><a id="version"></a> Vérifier la version du conteneur

Si vous souhaitez connaître la version de SQL Server dans un conteneur Docker en cours d’exécution, exécutez la commande suivante pour l’afficher. Remplacez `<Container ID or name>` par l’ID ou le nom du conteneur cible. Remplacez `<YourStrong!Passw0rd>` par le mot de passe SQL Server pour la connexion SA.

::: zone pivot="cs1-bash"
```bash
sudo docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd \
-S localhost -U SA -P '<YourStrong!Passw0rd>' \
-Q 'SELECT @@VERSION'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd `
-S localhost -U SA -P "<YourStrong!Passw0rd>" `
-Q "SELECT @@VERSION"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd ^
-S localhost -U SA -P "<YourStrong!Passw0rd>" ^
-Q "SELECT @@VERSION"
```
::: zone-end

Vous pouvez également identifier la version SQL Server et le numéro de version d’une image de conteneur Docker cible. La commande suivante affiche la version SQL Server et les informations de version de l’image **mcr.microsoft.com/mssql/server:2017-latest**. Pour ce faire, elle exécute un nouveau conteneur avec une variable d’environnement **PAL_PROGRAM_INFO= 1**. Le conteneur résultant s’arrête instantanément et la commande `docker rm` le supprime.

::: zone pivot="cs1-bash"
```bash
sudo docker run -e PAL_PROGRAM_INFO=1 --name sqlver \
-ti mcr.microsoft.com/mssql/server:2019-latest && \
sudo docker rm sqlver
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e PAL_PROGRAM_INFO=1 --name sqlver `
-ti mcr.microsoft.com/mssql/server:2019-latest; `
docker rm sqlver
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e PAL_PROGRAM_INFO=1 --name sqlver ^
-ti mcr.microsoft.com/mssql/server:2019-latest && ^
docker rm sqlver
```
::: zone-end

Les commandes précédentes affichent des informations de version semblables à la sortie suivante :

```output
sqlservr
  Version 15.0.4063.15
  Build ID 8a3bb4cca325e1d0b3071b3a193f6a1d74b440fbd95d2fb18881651a5b9ec8e8
  Build Type release
  Git Version 0335c462
  Built at Fri Aug 28 04:50:27 GMT 2020

PAL
  Build ID cc5ceea1b3d294f7d0166f99932f98c7eacfaaa81bcd7cf23c6a89f785829b63
  Build Type release
  Git Version ae9d66dff
  Built at Fri Aug 28 04:46:48 GMT 2020

Packages
  system.security                         6.2.9200.10,unset,
  system.certificates                     6.2.9200.10,unset,
  secforwarderxplat                       15.0.4063.15
  sqlservr                                15.0.4063.15
  system.common                           10.0.17134.1246.202005133
  system.netfx                            4.7.2.461814
  system                                  6.2.9200.10,unset,
  sqlagent                                15.0.4063.15
```

## <a name="run-a-specific-sql-server-container-image"></a><a id="tags"></a> Exécuter une image de conteneur SQL Server spécifique

> [!NOTE]
> À compter de SQL Server 2019 CU3, Ubuntu 18.04 est pris en charge. Vous pouvez récupérer la liste de toutes les étiquettes disponibles pour mssql/server sur <https://mcr.microsoft.com/v2/mssql/server/tags/list>.

Il existe des scénarios dans lesquels il est possible que vous ne souhaitiez pas utiliser la dernière image de conteneur SQL Server. Pour exécuter une image de conteneur SQL Server spécifique, procédez comme suit :

1. Identifiez la **balise** Docker pour la version que vous souhaitez utiliser. Pour voir toutes les balises disponibles, consultez [la page du hub Docker mssql-server-linux](https://hub.docker.com/_/microsoft-mssql-server).

2. Extrayez l’image de conteneur SQL Server avec la balise. Par exemple, pour extraire l’image 2019-CU7-ubuntu-18.04, remplacez `<image_tag>` dans la commande suivante par `2019-CU7-ubuntu-18.04`.

   ```bash
   docker pull mcr.microsoft.com/mssql/server:<image_tag>
   ```

3. Pour exécuter un nouveau conteneur avec cette image, spécifiez le nom de la balise dans la commande `docker run`. Dans la commande suivante, remplacez `<image_tag>` par la version que vous souhaitez exécuter.

   ::: zone pivot="cs1-bash"
   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

Vous pouvez également utiliser ces étapes pour rétrograder un conteneur existant. Par exemple, vous souhaiterez peut-être restaurer ou rétrograder un conteneur en cours d’exécution à des fins de dépannage ou de test. Pour rétrograder un conteneur en cours d’exécution, vous devez utiliser une technique de persistance pour le dossier de données. Suivez les mêmes étapes que celles décrites dans la [section de mise à niveau](#upgrade), mais spécifiez le nom de balise de l’ancienne version lorsque vous exécutez le nouveau conteneur.

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="run-rhel-based-container-images"></a><a id="rhel"></a> Exécuter des images conteneurs basées sur RHEL

La documentation pour les images conteneur SQL Server Linux pointe vers des conteneurs basés sur Ubuntu. À partir de SQL Server 2019, vous pouvez utiliser des conteneurs basés sur Red Hat Enterprise Linux (RHEL). Un exemple d’image pour RHEL va ressembler à **mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8**.

Par exemple, la commande suivante tire (pull) le conteneur CU 1 pour SQL Server 2019 qui utilise RHEL 8 :

::: zone pivot="cs1-bash"
```bash
sudo docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: moniker-end

## <a name="run-production-container-images"></a><a id="production"></a> Exécuter des images de conteneur de production

Le guide de [démarrage rapide](quickstart-install-connect-docker.md) de la section précédente exécute la version gratuite de l’édition Developer de SQL Server à partir du Docker Hub. La plupart des informations s’appliquent toujours si vous souhaitez exécuter des images de conteneur de production, comme les éditions Enterprise, Standard ou Web. Toutefois, il existe quelques différences qui sont décrites ici.

- Vous ne pouvez utiliser SQL Server dans un environnement de production que si vous disposez d’une licence valide. Vous pouvez vous procurer gratuitement une licence de production SQL Server Express [ici](https://go.microsoft.com/fwlink/?linkid=857693). Les licences SQL Server Standard et Enterprise Edition sont disponibles via le [programme de licence en volume Microsoft](https://www.microsoft.com/licensing/default.aspx).

- L’image de conteneur du développeur peut également être configurée pour exécuter les éditions de production. Procédez comme suit pour exécuter les éditions de production :

Passez en revue les conditions requises et exécutez les procédures dans le [démarrage rapide](quickstart-install-connect-docker.md). Vous devez spécifier votre édition de production avec la variable d’environnement **MSSQL_PID**. L’exemple suivant montre comment exécuter la dernière image de conteneur SQL Server 2017 pour l’édition Enterprise :

::: zone pivot="cs1-bash"
```bash
docker run --name sqlenterprise \
-e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' \
-e 'MSSQL_PID=Enterprise' -p 1433:1433 \
-d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run --name sqlenterprise `
-e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-e "MSSQL_PID=Enterprise" -p 1433:1433 `
-d "mcr.microsoft.com/mssql/server:2019-latest"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run --name sqlenterprise `
-e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" ^
-e "MSSQL_PID=Enterprise" -p 1433:1433 ^
-d "mcr.microsoft.com/mssql/server:2019-latest"
```
::: zone-end

> [!IMPORTANT]
> En passant la valeur **Y** à la variable d’environnement **ACCEPT_EULA** et une valeur d'édition à **MSSQL_PID**, vous indiquez que vous disposez d’une licence valide et existante pour l’édition et la version de SQL Server que vous avez l’intention d’utiliser. Vous acceptez également que votre utilisation du logiciel SQL Server exécuté dans une image de conteneur Docker sera régie par les conditions de votre licence SQL Server.

> [!NOTE]
> Pour obtenir la liste complète des valeurs possibles pour **MSSQL_PID**, consultez [Configurer les paramètres de SQL Server avec des variables d’environnement sur Linux](sql-server-linux-configure-environment-variables.md).

## <a name="run-multiple-sql-server-containers"></a><a id="multiple"></a>Exécuter plusieurs conteneurs SQL Server

Docker offre un moyen d’exécuter plusieurs conteneurs SQL Server sur le même ordinateur hôte. Utilisez cette approche pour les scénarios qui requièrent plusieurs instances de SQL Server sur le même hôte. Chaque conteneur doit s’exposer lui-même sur un port différent.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

L’exemple suivant crée deux conteneurs SQL Server 2017 et les mappe aux ports **1401** et **1402** sur l’ordinateur hôte.

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

L’exemple suivant crée deux conteneurs SQL Server 2019 et les mappe aux ports **1401** et **1402** sur l’ordinateur hôte.

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

Il existe maintenant deux instances de SQL Server s’exécutant dans des conteneurs distincts. Les clients peuvent se connecter à chaque instance de SQL Server à l’aide de l’adresse IP de l’ordinateur hôte Docker et du numéro de port du conteneur.

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4,1401 -U SA -P '<YourPassword>'
sqlcmd -S 10.3.2.4,1402 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
sqlcmd -S 10.3.2.4,1402 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
sqlcmd -S 10.3.2.4,1402 -U SA -P "<YourPassword>"
```
::: zone-end

## <a name="upgrade-sql-server-in-containers"></a><a id="upgrade"></a> Mettre à niveau SQL Server dans les conteneurs

Pour mettre à niveau l’image de conteneur avec Docker, commencez par identifier la balise pour la version de votre mise à niveau. Extrayez cette version à partir du registre à l’aide de la commande `docker pull` :

```command
docker pull mcr.microsoft.com/mssql/server:<image_tag>
```

Cela met à jour l’image de SQL Server pour les nouveaux conteneurs que vous créez, mais ne met pas à jour SQL Server dans les conteneurs en cours d’exécution. Pour ce faire, vous devez créer un nouveau conteneur avec la dernière image de conteneur SQL Server et migrer vos données vers ce nouveau conteneur.

1. Assurez-vous que vous utilisez une des [techniques de persistance des données](sql-server-linux-docker-container-configure.md#persist) pour votre conteneur SQL Server existant. Cela vous permet de démarrer un nouveau conteneur avec les mêmes données.

1. Arrêtez le conteneur SQL Server à l'aide de la commande `docker stop`.

1. Créez un conteneur SQL Server avec `docker run` et spécifiez un répertoire hôte mappé ou un conteneur de volume de données. Veillez à utiliser la balise spécifique pour votre mise à niveau SQL Server. Le nouveau conteneur utilise désormais une nouvelle version de SQL Server avec vos données SQL Server existantes.

   > [!IMPORTANT]
   > La mise à niveau est uniquement prise en charge entre RC1, RC2 et GA pour l’instant.

1. Vérifiez vos bases de données et vos données dans le nouveau conteneur.

1. Si vous le souhaitez, supprimez l'ancien conteneur avec `docker rm`.

## <a name="next-steps"></a>Étapes suivantes

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Bien démarrer avec les images de conteneur SQL Server 2017 sur Docker en suivant le [démarrage rapide](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Bien démarrer avec les images de conteneur SQL Server 2019 sur Docker en suivant le [démarrage rapide](quickstart-install-connect-docker.md)

::: moniker-end

- [Référencer une autre configuration et une personnalisation pour des conteneurs Docker](sql-server-linux-docker-container-configure.md)

- Consulter le [dépôt GitHub mssql-docker](https://github.com/Microsoft/mssql-docker) pour obtenir des ressources, des commentaires et les problèmes connus

- [Résolution des problèmes liés aux conteneurs Docker SQL Server](sql-server-linux-docker-container-troubleshooting.md)

- [Explorer la haute disponibilité des conteneurs SQL Server](sql-server-linux-container-ha-overview.md)

- [Sécuriser des conteneurs Docker SQL Server](sql-server-linux-docker-container-security.md)
