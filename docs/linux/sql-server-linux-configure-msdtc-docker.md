---
title: Transactions distribuées (MSDTC) avec SQL Server sur Docker
description: Apprenez à utiliser MSDTC (Microsoft Distributed Transaction Coordinator) pour les transactions distribuées dans un conteneur SQL Server sur Docker.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: f6cc1ad3c66ae7f4ec1817af3bcaa3934a1858ef
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622890"
---
# <a name="how-to-use-distributed-transactions-with-sql-server-on-docker"></a>Guide pratique pour utiliser des transactions distribuées avec SQL Server sur Docker

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article explique comment configurer des conteneurs SQL Server Linux sur Docker pour les transactions distribuées.

Les images de conteneur SQL Server prennent en charge Microsoft Distributed Transaction Coordinator (MSDTC), qui est nécessaire pour les transactions distribuées. Pour comprendre les exigences de communication pour MSDTC, consultez [Comment configurer Microsoft Distributed Transaction Coordinator (MSDTC) sur Linux](sql-server-linux-configure-msdtc.md). Cet article explique les conditions requises et les scénarios spécifiques aux conteurs Docker pour SQL Server.

## <a name="configuration"></a>Configuration

Pour activer la transaction MSDTC dans des conteneurs pour Docker, vous devez définir deux nouvelles variables d’environnement :

- **MSSQL_RPC_PORT** : le port TCP auquel le service mappeur de point de terminaison RPC est lié et sur lequel il écoute.  
- **MSSQL_DTC_TCP_PORT** : le port sur lequel le service MSDTC est configuré pour l’écoute.

### <a name="pull-and-run"></a>Extraire et exécuter

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

L’exemple suivant montre comment utiliser ces variables d’environnement pour tirer (pull) et exécuter un seul conteneur SQL Server 2017 configuré pour MSDTC. Cela lui permet de communiquer avec n’importe quelle application sur tous les ordinateurs hôtes.

```bash
docker run \
   -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' \
   -e 'MSSQL_RPC_PORT=135' -e 'MSSQL_DTC_TCP_PORT=51000' \
   -p 51433:1433 -p 135:135 -p 51000:51000  \
   -d mcr.microsoft.com/mssql/server:2017-latest
```

```PowerShell
docker run `
   -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
   -e "MSSQL_RPC_PORT=135" -e "MSSQL_DTC_TCP_PORT=51000" `
   -p 51433:1433 -p 135:135 -p 51000:51000  `
   -d mcr.microsoft.com/mssql/server:2017-latest
```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

L’exemple suivant montre comment utiliser ces variables d’environnement pour tirer (pull) et exécuter un seul conteneur SQL Server 2019 configuré pour MSDTC. Cela lui permet de communiquer avec n’importe quelle application sur tous les ordinateurs hôtes.

```bash
docker run \
   -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' \
   -e 'MSSQL_RPC_PORT=135' -e 'MSSQL_DTC_TCP_PORT=51000' \
   -p 51433:1433 -p 135:135 -p 51000:51000  \
   -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

```PowerShell
docker run `
   -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
   -e "MSSQL_RPC_PORT=135" -e "MSSQL_DTC_TCP_PORT=51000" `
   -p 51433:1433 -p 135:135 -p 51000:51000  `
   -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

::: moniker-end

> [!IMPORTANT]
> La commande précédente ne fonctionne que pour Docker sur Linux. Pour Docker sur Windows, l’hôte Windows écoute déjà sur le port 135. Vous pouvez supprimer le paramètre `-p 135:135` pour Docker sur Windows, mais cela présente quelques limitations. Le conteneur résultant ne peut alors pas être utilisé pour les transactions distribuées qui impliquent l’hôte ; il peut participer uniquement aux transactions distribuées entre les conteneurs Docker sur l’hôte.

Dans cette commande, le service **Mappeur de point de terminaison RPC** a été lié au port 135 et le service **MSDTC** a été lié au port 51000 au sein du réseau virtuel Docker. La communication TDS de SQL Server se produit sur le port 1433 au sein du réseau virtuel de Docker. Ces ports ont été exposés en externe auprès de l’hôte sous la forme du port TDS 51433, du port 135 du mappeur de point de terminaison RPC et du port MSDTC 51000.

> [!NOTE]
> Le mappeur de point de terminaison RPC et le port MSDTC n’ont pas besoin d’être identiques sur l’hôte et le conteneur. Ainsi, alors que le port du mappeur de point de terminaison RPC a été configuré pour être 135 sur le conteneur, il peut potentiellement être mappé au port 13501 ou à tout autre port disponible sur le serveur hôte.

## <a name="configure-the-firewall"></a>Configurer le pare-feu

Pour pouvoir communiquer avec et via l’hôte, vous devez également configurer le pare-feu sur le serveur hôte pour les conteneurs. Ouvrez le pare-feu pour tous les ports que le conteneur Docker expose pour la communication externe. Dans l’exemple précédent, il s’agirait des ports 135, 51433 et 51000. Il s’agit des ports sur l’hôte lui-même, et non des ports auxquels ils sont mappés dans le conteneur. Ainsi, si le port de mappeur de point de terminaison RPC 51000 du conteneur a été mappé au port hôte 51001, le port 51001 (et non 51000) doit être ouvert dans le pare-feu pour la communication avec l’ordinateur hôte.  

L’exemple suivant montre comment créer ces règles sur Ubuntu.

```bash
sudo ufw allow from any to any port 51433 proto tcp
sudo ufw allow from any to any port 51000 proto tcp
sudo ufw allow from any to any port 135 proto tcp
```

L’exemple suivant montre comment effectuer cette opération sur Red Hat Enterprise Linux (RHEL) :

```bash
sudo firewall-cmd --zone=public --add-port=51433/tcp --permanent
sudo firewall-cmd --zone=public --add-port=51000/tcp --permanent
sudo firewall-cmd --zone=public --add-port=135/tcp --permanent
sudo firewall-cmd --reload
```

## <a name="configure-port-routing-on-the-host"></a>Configurer le routage de ports sur l’hôte

Dans l’exemple précédent, étant donné qu’un conteneur Docker unique mappe le port RPC 135 au port 135 sur l’hôte, les transactions distribuées avec l’hôte devraient maintenant fonctionner sans configuration supplémentaire. Notez qu’il est possible d’utiliser directement le port 135 dans les conteneurs s’exécutant en tant que root, car SQL Server s’exécute avec des privilèges élevés dans ces conteneurs. Pour SQL Server en dehors d’un conteneur ou pour des conteneurs non root, vous devez utiliser un port éphémère différent dans le conteneur, par exemple 13500, puis router le trafic sur le port 135 vers ce port. Vous devez également configurer des règles de routage de port dans le conteneur à partir du port 135 du conteneur vers le port éphémère.

Par ailleurs, si vous décidez de mapper le port 135 du conteneur à un port différent sur l’hôte, par exemple 13500, vous devez configurer le routage de port sur l’hôte. Cela permet au conteneur Docker de participer aux transactions distribuées avec l’hôte et les autres serveurs externes.

Pour plus d’informations, consultez [Configurer le routage de ports](sql-server-linux-configure-msdtc.md#configure-port-routing).

> [!NOTE]
> Les conteneurs SQL Server 2017 s’exécutent dans des conteneurs root par défaut, tandis que les conteneurs SQL Server 2019 s’exécutent en tant qu’utilisateur non-root.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur MSDTC sur Linux, consultez [Comment configurer Microsoft Distributed Transaction Coordinator (MSDTC) sur Linux](sql-server-linux-configure-msdtc.md).
