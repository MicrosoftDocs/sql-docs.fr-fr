---
title: Configurer des variables d’environnement pour SQL Server sur Linux
description: Cet article explique comment utiliser des variables d’environnement pour configurer des paramètres SQL Server 2017 spécifiques sur Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: fb9455970c58daa65f8068792a4edc8ae7e20f51
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343176"
---
# <a name="configure-sql-server-settings-with-environment-variables-on-linux"></a>Configurer des paramètres SQL Server à l’aide de variables d’environnement sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Vous pouvez utiliser plusieurs variables d’environnement différentes pour configurer SQL Server 2017 sur Linux. Ces variables sont utilisées dans deux scénarios :

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Vous pouvez utiliser différentes variables d’environnement pour configurer SQL Server 2019 sur Linux. Ces variables sont utilisées dans deux scénarios :

::: moniker-end

- Pour configurer l’installation initiale à l’aide de la commande `mssql-conf setup`.
- Pour configurer un nouveau [conteneur SQL Server dans Docker](quickstart-install-connect-docker.md).

> [!TIP]
> Si vous devez configurer SQL Server après ces scénarios d’installation, consultez [Configurer SQL Server sur Linux avec l’outil mssql-conf](sql-server-linux-configure-mssql-conf.md).

## <a name="environment-variables"></a>Variables d'environnement

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

| Variable d’environnement | Description |
|-----|-----|
| **ACCEPT_EULA** | Définissez la variable **ACCEPT_EULA** sur n’importe quelle valeur pour confirmer que vous acceptez le [Contrat de licence utilisateur final](https://go.microsoft.com/fwlink/?LinkId=746388). Paramètre obligatoire pour l’image de SQL Server. |
| **MSSQL_SA_PASSWORD** | Configurez le mot de passe de l’utilisateur AS. |
| **MSSQL_PID** | Définissez l’édition SQL Server ou la clé de produit. Les valeurs possibles incluent : </br></br>**Evaluation**</br>**Développeur**</br>**Express**</br>**Web**</br>**Standard**</br>**Enterprise**</br>**Clé de produit**</br></br>Si vous spécifiez une clé de produit, celle-ci doit être au format #####-#####-#####-#####-#####, où « # » est un chiffre ou une lettre.|
| **MSSQL_LCID** | Définit l’ID de langue à utiliser pour SQL Server. Par exemple 1036 correspond au français. |
| **MSSQL_COLLATION** | Définit le classement par défaut pour SQL Server. Cela remplace le mappage par défaut de l’ID de langue (LCID) par le classement. |
| **MSSQL_MEMORY_LIMIT_MB** | Définit la quantité maximale de mémoire (en Mo) que SQL Server peut utiliser. Par défaut, il s’agit de 80 % de la mémoire physique totale. |
| **MSSQL_TCP_PORT** | Configurez le port TCP sur lequel SQL Server écoute (par défaut 1433). |
| **MSSQL_IP_ADDRESS** | Définissez l’adresse IP. Actuellement, l’adresse IP doit être de type IPv4 (0.0.0.0). |
| **MSSQL_BACKUP_DIR** | Définissez l’emplacement du répertoire de sauvegarde par défaut. |
| **MSSQL_DATA_DIR** | Modifiez le répertoire dans lequel les nouveaux fichiers de données de la base de données SQL Server (.mdf) sont créés. |
| **MSSQL_LOG_DIR** | Modifiez le répertoire dans lequel les nouveaux fichiers journaux de base de données SQL Server (.ldf) sont créés. |
| **MSSQL_DUMP_DIR** | Modifiez le répertoire dans lequel SQL Server dépose les images mémoire et d’autres fichiers de dépannage par défaut. |
| **MSSQL_ENABLE_HADR** | Activez le groupe de disponibilité. Par exemple, « 1 » est activé et « 0 » est désactivé. |
| **MSSQL_AGENT_ENABLED** | Activer l’agent SQL Server. Par exemple, « true » est activé et « false » est désactivé. Par défaut, l’agent est désactivé.  |
| **MSSQL_MASTER_DATA_FILE** | Définit l’emplacement du fichier de données de la base de données Master. Doit être nommé **master.mdf** jusqu’à la première exécution de SQL Server. |
| **MSSQL_MASTER_LOG_FILE** | Définit l’emplacement du fichier journal de la base de données Master. Doit être nommé **mastlog.ldf** jusqu’à la première exécution de SQL Server. |
| **MSSQL_ERROR_LOG_FILE** | Définit l’emplacement des fichiers de journal d’erreurs. |

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

| Variable d’environnement | Description |
|-----|-----|
| **ACCEPT_EULA** | Définissez la variable **ACCEPT_EULA** sur n’importe quelle valeur pour confirmer que vous acceptez le [Contrat de licence utilisateur final](https://go.microsoft.com/fwlink/?LinkId=746388). Paramètre obligatoire pour l’image de SQL Server. |
| **MSSQL_SA_PASSWORD** | Configurez le mot de passe de l’utilisateur AS. |
| **MSSQL_PID** | Définissez l’édition SQL Server ou la clé de produit. Les valeurs possibles incluent : </br></br>**Evaluation**</br>**Développeur**</br>**Express**</br>**Web**</br>**Standard**</br>**Enterprise**</br>**Clé de produit**</br></br>Si vous spécifiez une clé de produit, celle-ci doit être au format #####-#####-#####-#####-#####, où « # » est un chiffre ou une lettre.|
| **MSSQL_LCID** | Définit l’ID de langue à utiliser pour SQL Server. Par exemple 1036 correspond au français. |
| **MSSQL_COLLATION** | Définit le classement par défaut pour SQL Server. Cela remplace le mappage par défaut de l’ID de langue (LCID) par le classement. |
| **MSSQL_MEMORY_LIMIT_MB** | Définit la quantité maximale de mémoire (en Mo) que SQL Server peut utiliser. Par défaut, il s’agit de 80 % de la mémoire physique totale. |
| **MSSQL_TCP_PORT** | Configurez le port TCP sur lequel SQL Server écoute (par défaut 1433). |
| **MSSQL_IP_ADDRESS** | Définissez l’adresse IP. Actuellement, l’adresse IP doit être de type IPv4 (0.0.0.0). |
| **MSSQL_BACKUP_DIR** | Définissez l’emplacement du répertoire de sauvegarde par défaut. |
| **MSSQL_DATA_DIR** | Modifiez le répertoire dans lequel les nouveaux fichiers de données de la base de données SQL Server (.mdf) sont créés. |
| **MSSQL_LOG_DIR** | Modifiez le répertoire dans lequel les nouveaux fichiers journaux de base de données SQL Server (.ldf) sont créés. |
| **MSSQL_DUMP_DIR** | Modifiez le répertoire dans lequel SQL Server dépose les images mémoire et d’autres fichiers de dépannage par défaut. |
| **MSSQL_ENABLE_HADR** | Activez le groupe de disponibilité. Par exemple, « 1 » est activé et « 0 » est désactivé. |
| **MSSQL_AGENT_ENABLED** | Activer l’agent SQL Server. Par exemple, « true » est activé et « false » est désactivé. Par défaut, l’agent est désactivé.  |
| **MSSQL_MASTER_DATA_FILE** | Définit l’emplacement du fichier de données de la base de données Master. Doit être nommé **master.mdf** jusqu’à la première exécution de SQL Server. |
| **MSSQL_MASTER_LOG_FILE** | Définit l’emplacement du fichier journal de la base de données Master. Doit être nommé **mastlog.ldf** jusqu’à la première exécution de SQL Server. |
| **MSSQL_ERROR_LOG_FILE** | Définit l’emplacement des fichiers de journal d’erreurs. |

::: moniker-end

## <a name="use-with-initial-setup"></a>Utiliser avec l’installation initiale

Cet exemple exécute `mssql-conf setup` avec des variables d’environnement configurées. Les variables d’environnement suivantes sont spécifiées :

- **ACCEPT_EULA** accepte le contrat de licence utilisateur final.
- **MSSQL_PID** spécifie l’édition Développeur sous licence gratuite de SQL Server pour une utilisation hors production.
- **MSSQL_SA_PASSWORD** définit un mot de passe fort.
- **MSSQL_TCP_PORT** définit le port TCP que SQL Server écoute sur 1234.

```bash
sudo ACCEPT_EULA='Y' MSSQL_PID='Developer' MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' MSSQL_TCP_PORT=1234 /opt/mssql/bin/mssql-conf setup
```

## <a name="use-with-docker"></a>Utiliser avec Docker

Cet exemple de commande Docker utilise les variables d’environnement suivantes pour créer un conteneur SQL Server :

- **ACCEPT_EULA** accepte le contrat de licence utilisateur final.
- **MSSQL_PID** spécifie l’édition Développeur sous licence gratuite de SQL Server pour une utilisation hors production.
- **MSSQL_SA_PASSWORD** définit un mot de passe fort.
- **MSSQL_TCP_PORT** définit le port TCP que SQL Server écoute sur 1234. Cela signifie qu’au lieu de mapper le port 1433 (par défaut) à un port hôte, le port TCP personnalisé doit être mappé avec la commande `-p 1234:1234` dans cet exemple.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Si vous exécutez Docker sur Linux/macOS, utilisez la syntaxe suivante avec des guillemets simples :

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID='Developer' -e MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2017-latest
```

Si vous exécutez Docker sur Windows, utilisez la syntaxe suivante avec des guillemets doubles :

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID="Developer" -e MSSQL_SA_PASSWORD="<YourStrong!Passw0rd>" -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2017-latest
```

> [!NOTE]
> Le processus d’exécution des éditions de production dans des conteneurs est légèrement différent. Pour plus d’informations, consultez [Exécuter des images conteneur de production](./sql-server-linux-docker-container-deployment.md#production).

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Si vous exécutez Docker sur Linux/macOS, utilisez la syntaxe suivante avec des guillemets simples :

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID='Developer' -e MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

Si vous exécutez Docker sur Windows, utilisez la syntaxe suivante avec des guillemets doubles :

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID="Developer" -e MSSQL_SA_PASSWORD="<YourStrong!Passw0rd>" -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

::: moniker-end

## <a name="next-steps"></a>Étapes suivantes

Pour d’autres paramètres SQL Server qui ne sont pas répertoriés ici, consultez [Configurer SQL Server sur Linux avec l’outil mssql-conf](sql-server-linux-configure-mssql-conf.md).

Pour plus d’informations sur l’installation et l’exécution de SQL Server sur Linux, consultez [Installer SQL Server sur Linux](sql-server-linux-setup.md).