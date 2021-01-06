---
title: Résolution des problèmes liés aux conteneurs Docker SQL Server
description: Explorer les différentes techniques de résolution des problèmes que vous pouvez utiliser pour corriger les erreurs courantes rencontrées lors de l’utilisation de conteneurs Docker Linux avec des images SQL Server
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017'
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: d94ae00cb8f3581be51f53d999068e60c4d5b7f1
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878787"
---
# <a name="troubleshooting-sql-server-docker-containers"></a>Résolution des problèmes liés aux conteneurs Docker SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article traite des erreurs courantes rencontrées lors du déploiement et de l’utilisation de conteneurs Docker SQL Server, puis indique des techniques de résolution des problèmes afin de les corriger.

## <a name="docker-command-errors"></a>Erreurs de commande Docker

Si vous recevez des erreurs pour des commandes `docker`, assurez-vous que le service Docker est en cours d’exécution et essayez de l’exécuter avec des autorisations élevées.

Par exemple, sur Linux, vous pouvez obtenir l’erreur suivante lors de l’exécution des commandes `docker` :

```output
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```

Si vous recevez cette erreur sur Linux, essayez d’exécuter les mêmes commandes précédées de `sudo`. En cas d’échec, vérifiez que le service Docker est en cours d’exécution et démarrez-le si nécessaire.

```bash
sudo systemctl status docker
sudo systemctl start docker
```

Sur Windows, vérifiez que vous lancez PowerShell ou votre invite de commandes en tant qu’administrateur.

## <a name="sql-server-container-startup-errors"></a>Erreurs de démarrage du conteneur SQL Server

Si le conteneur SQL Server ne parvient pas à s’exécuter, essayez les tests suivants :

- Si vous recevez une erreur comme `failed to create endpoint CONTAINER_NAME on network bridge. Error starting proxy: listen tcp 0.0.0.0:1433 bind: address already in use.`, vous essayez de mapper le port 1433 du conteneur sur un port qui est déjà en cours d’utilisation. Cela peut se produire si vous exécutez SQL Server localement sur l’ordinateur hôte. Cela peut également se produire si vous démarrez deux conteneurs SQL Server et que vous essayez de les mapper tous les deux sur le même port hôte. Dans ce cas, utilisez le paramètre `-p` pour mapper le port de conteneur 1433 sur un port d’hôte différent. Par exemple : 

    <!--SQL Server 2017 on Linux -->
    ::: moniker range="= sql-server-linux-2017 || = sql-server-2017"
    
    ::: zone pivot="cs1-bash"
    ```bash
    docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-powershell"
    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-cmd"
    ```cmd
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: moniker-end
    
    <!--SQL Server 2019 on Linux-->
    ::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15"
    
    ::: zone pivot="cs1-bash"
    ```bash
    docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-powershell"
    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-cmd"
    ```cmd
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: moniker-end

- Si vous recevez une erreur comme `Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.30tdout=1&tail=all: dial unix /var/run/docker.sock: connect: permission denied` quand vous essayez de démarrer un conteneur, ajoutez votre utilisateur au groupe Docker dans Ubuntu. Ensuite, déconnectez-vous et reconnectez-vous, car cette modification affecte les nouvelles sessions. 

   ```bash
    usermod -aG docker $USER
   ```

- Vérifiez si des messages d’erreur s’affichent dans le conteneur.

   ```bash
   docker logs e69e056c702d
   ```

- Assurez-vous que vous respectez les exigences minimales en matière de mémoire et de disque spécifiées dans la section [conditions préalables](quickstart-install-connect-docker.md#requirements) de l’article de démarrage rapide.

- Si vous utilisez un logiciel de gestion de conteneur, assurez-vous qu’il prend en charge les processus de conteneur s’exécutant en tant que root. Le processus sqlservr dans le conteneur s’exécute en tant que root.

- Si votre conteneur Docker SQL Server s’arrête immédiatement après son démarrage, vérifiez vos journaux Docker. Si vous utilisez PowerShell sur Windows avec la commande `docker run`, utilisez des guillemets doubles plutôt que des simples. Avec PowerShell Core, utilisez des guillemets simples.

- Passez en revue [les journaux de configuration et des erreurs de SQL Server](#errorlogs).

## <a name="enable-dump-captures"></a>Activer le vidage de captures

Si le processus de SQL Server échoue à l’intérieur du conteneur, vous devez créer un nouveau conteneur avec **SYS_PTRACE** activé. Cela ajoute la fonctionnalité Linux de suivi de processus, ce qui est nécessaire pour créer un fichier de vidage sur une exception. Le fichier de vidage peut être utilisé par le support pour vous aider à résoudre le problème. La commande Docker suivante active cette fonctionnalité.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -e 'MSSQL_PID=Developer' --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -e 'MSSQL_PID=Developer' --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="sql-server-connection-failures"></a>Échecs de connexion à SQL Server

Si vous ne pouvez pas vous connecter à l’instance SQL Server s’exécutant dans votre conteneur, effectuez les tests suivants :

- Assurez-vous que votre conteneur SQL Server est en cours d'exécution en consultant la colonne **ÉTAT** de la sortie `docker ps -a`. Si ce n’est pas le cas, utilisez `docker start <Container ID>` pour le démarrer.

- Si vous avez mappé sur un autre port hôte que celui par défaut (pas 1433), assurez-vous que vous spécifiez le port dans votre chaîne de connexion. Vous pouvez voir votre mappage de port dans la colonne **PORTS** de la sortie `docker ps -a`. Par exemple, la commande suivante connecte sqlcmd à un conteneur qui écoute sur le port 1401 :

    ::: zone pivot="cs1-bash"
    ```bash
    sqlcmd -S 10.3.2.4,1401 -U SA -P '<YourPassword>'
    ```
    ::: zone-end

    ::: zone pivot="cs1-powershell"
    ```PowerShell
    sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
    ```
    ::: zone-end

    ::: zone pivot="cs1-cmd"
    ```cmd
    sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
    ```
    ::: zone-end

- Si vous avez utilisé `docker run` avec un volume de données ou un conteneur de volume de données mappé existant, SQL Server ignore la valeur de `SA_PASSWORD`. Au lieu de cela, le mot de passe utilisateur préconfiguré est utilisé à partir des données de SQL Server dans le volume de données ou conteneur de volume de données. Vérifiez que vous utilisez le mot de passe SA associé aux données auxquelles pour lesquelles vous effectuez la jointure.

- Passez en revue [les journaux de configuration et des erreurs de SQL Server](#errorlogs).

## <a name="sql-server-availability-groups"></a>Groupes de disponibilité SQL Server

Si vous utilisez Docker avec des groupes de disponibilité SQL Server, il y a deux exigences supplémentaires.

- Mappez le port utilisé pour la communication du réplica (5022 par défaut). Par exemple, spécifiez `-p 5022:5022` dans le cadre de votre commande `docker run`.

- Définissez explicitement le nom d’hôte du conteneur avec le paramètre `-h YOURHOSTNAME` de la commande `docker run`. Ce nom d’hôte est utilisé lorsque vous configurez votre groupe de disponibilité. Si vous ne le spécifiez pas avec `-h`, l’**ID du conteneur** est utilisé par défaut.

## <a name="sql-server-setup-and-error-logs"></a><a id="errorlogs"></a> Journaux de configuration et des erreurs de SQL Server

Vous pouvez consulter les journaux de configuration et des erreurs de SQL Server dans **/var/opt/mssql/log**. Si le conteneur n’est pas en cours d’exécution, démarrez d’abord le conteneur. Utilisez ensuite une invite de commandes interactive pour inspecter les journaux. Vous pouvez obtenir l’ID du conteneur en exécutant la commande `docker ps`.

```bash
docker start <ContainerID>
docker exec -it <ContainerID> "bash"
```

À partir de la session bash à l’intérieur de votre conteneur, exécutez les commandes suivantes :

```bash
cd /var/opt/mssql/log
cat setup*.log
cat errorlog
```

> [!TIP]
> Si vous avez monté un répertoire hôte sur **/var/opt/mssql** lorsque vous avez créé votre conteneur, vous pouvez à la place l’examiner dans le sous-répertoire **log** sur le chemin d’accès mappé sur l’ordinateur hôte.

## <a name="execute-commands-in-a-container"></a>Exécuter des commandes dans un conteneur

Si vous disposez d’un conteneur en cours d’exécution, vous pouvez exécuter des commandes dans le conteneur à partir d’un terminal hôte.

Pour récupérer l’ID de conteneur, exécutez :

```bash
docker ps -a
```

Pour démarrer un terminal bash dans le conteneur, exécutez :

```bash
docker exec -it <Container ID> /bin/bash
```

Vous pouvez maintenant exécuter des commandes comme si vous les exécutiez sur le terminal à l’intérieur du conteneur. Quand vous avez terminé, tapez `exit`. Cela quitte la session de commande interactive, mais votre conteneur continue à s’exécuter.

## <a name="next-steps"></a>Étapes suivantes

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Bien démarrer avec les images de conteneur SQL Server 2017 sur Docker en avec le [démarrage rapide](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true).

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Bien démarrer avec les images de conteneur SQL Server 2019 sur Docker en suivant le [démarrage rapide](quickstart-install-connect-docker.md).

::: moniker-end

- [Déployer des conteneurs Docker SQL Server et s’y connecter](sql-server-linux-docker-container-deployment.md)

- [Référencer une autre configuration et une personnalisation pour des conteneurs Docker](sql-server-linux-docker-container-configure.md)

- [Sécuriser des conteneurs Docker SQL Server](sql-server-linux-docker-container-security.md)
