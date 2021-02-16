---
title: Configurer un dossier de captures instantanées de réplication (ports autres que ceux par défaut)
titleSuffix: SQL Server on Linux
description: Apprenez à configurer des partages de dossier de captures instantanées avec des ports autres que ceux par défaut pour la réplication SQL Server sur Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 09/24/2018
ms.topic: article
ms.prod: sql
ms.technology: linux
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: f51dd4ebdb3a797a54eb488e8d99c02017dc70e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346478"
---
# <a name="configure-replication-with-non-default-ports-sql-server-linux"></a>Configurer la réplication avec des ports autres que ceux par défaut (SQL Server sur Linux)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Vous pouvez configurer la réplication avec des instances de SQL Server sur Linux qui écoutent sur n’importe quel port configuré avec le paramètre network.tcpport mssql-conf. Le port doit être ajouté au nom du serveur lors de la configuration si les conditions suivantes sont remplies :

1. La configuration de la réplication implique une instance de SQL Server sur Linux
2. Toute instance (Windows ou Linux) est à l’écoute sur un port autre que celui par défaut. 

Le nom de serveur d’une instance peut être trouvé en exécutant @@servername sur l’instance. N’utilisez pas l’adresse IP à la place du nom du serveur. L’utilisation de l’adresse IP pour le serveur de publication, le serveur de distribution ou l’abonné peut entraîner une erreur.

> [!NOTE]
> La création d’une réplication de SQL Server sur Linux avec un port autre que celui par défaut ne fonctionnera qu’avec SQL Server 2019 et versions ultérieures.

## <a name="examples"></a>Exemples

« Serveur1 » écoute sur le port 1500 sur Linux. Pour configurer « Serveur1 » pour la distribution, exécutez `sp_adddistributor` avec `@distributor`. Par exemple : 

```sql
exec sp_adddistributor @distributor = 'Server1,1500'
```

« Serveur1 » écoute sur le port 1500 sur Linux. Pour configurer un serveur de publication, exécutez `sp_adddistpublisher` avec `@publisher`. Par exemple :

```sql
exec sp_adddistpublisher @publisher = 'Server1,1500' ,  ,  
```

« Serveur2 » écoute sur le port 6549 sur Linux. Pour configurer « Serveur2 » en tant qu’abonné, exécutez `sp_addsubscription` avec `@subscriber`. Par exemple :

```sql
exec sp_addsubscription @subscriber = 'Server2,6549' ,  ,  
```

« Serveur3 » écoute le port 6549 sur Windows avec le nom de serveur Serveur3 et le nom d’instance MSSQL2017. Pour configurer « Serveur3 » en tant qu’abonné, exécutez `sp_addsubscription` avec `@subscriber`. Par exemple :

```sql
exec sp_addsubscription @subscriber = 'Server3/MSSQL2017,6549',  ,  
```

## <a name="next-steps"></a>Étapes suivantes

[Concepts : Réplication SQL Server sur Linux](sql-server-linux-replication.md)

[Procédures stockées de réplication](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md).

