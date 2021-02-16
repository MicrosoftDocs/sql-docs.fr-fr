---
description: Ajouter une mémoire tampon de journal persistante à une base de données
title: Ajouter une mémoire tampon de journal persistante à une base de données
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- PMEM
- persistent memory
- persisted log buffer
- add log file
- create log buffer
- remove log buffer
ms.assetid: 8ead516a-1334-4f40-84b2-509d0a8ffa45
author: briancarrig
ms.author: brcarrig
manager: amitban
ms.openlocfilehash: ab2554561abb1590db33249a0ade4e8458b0fd54
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272370"
---
# <a name="add-persisted-log-buffer-to-a-database"></a>Ajouter une mémoire tampon de journal persistante à une base de données
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette rubrique explique comment ajouter une mémoire tampon de journal persistante à une base de données dans [!INCLUDE[sqlv15](../../includes/sssql19-md.md)] en utilisant [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="permissions"></a>Autorisations

Nécessite l'autorisation ALTER sur la base de données.  

## <a name="configure-persistent-memory-device-linux"></a>Configurer un module de mémoire persistante (Linux)

Pour configurer un module de mémoire persistante dans [Linux](../../linux/sql-server-linux-configure-pmem.md).

## <a name="configure-persistent-memory-device-windows"></a>Configurer un module de mémoire persistante (Windows)

Pour configurer un module de mémoire persistante dans [Windows](/windows-server/storage/storage-spaces/deploy-pmem/).
  
## <a name="add-a-persisted-log-buffer-to-a-database"></a>Ajouter une mémoire tampon de journal persistante à une base de données  

Les exemples suivants ajoutent une mémoire tampon de journal persistante.

```sql
ALTER DATABASE <MyDB> 
  ADD LOG FILE 
  (
    NAME = <DAXlog>, 
    FILENAME = '<Filepath to DAX Log File>', 
    SIZE = 20MB
  );
```

Le volume ou le montage sur lequel le nouveau fichier journal est placé doit être formaté avec DAX (NTFS) ou monté avec l’option DAX (XFS/EXT4).

## <a name="remove-a-persisted-log-buffer"></a>Supprimer une mémoire tampon de journal persistante

Pour supprimer de manière sécurisée une mémoire tampon de journal persistante, la base de données doit être placée en mode mono-utilisateur afin de vider la mémoire tampon de journal persistante.

L’exemple suivant supprime une mémoire tampon de journal persistante.

```sql
ALTER DATABASE <MyDB> SET SINGLE_USER;
ALTER DATABASE <MyDB> REMOVE FILE <DAXlog>;
ALTER DATABASE <MyDB> SET MULTI_USER;
```

## <a name="limitations"></a>Limites

[Transparent Data Encryption (TDE)](../security/encryption/transparent-data-encryption.md) n’est pas compatible avec la mémoire tampon de journal persistante.

Les [groupes de disponibilité](../../t-sql/statements/create-availability-group-transact-sql.md) ne peuvent utiliser cette fonctionnalité que sur des réplicas secondaires en raison du besoin d’une sémantique d’écriture de journal normale sur le réplica principal. Cependant, le petit fichier journal doit être créé sur tous les nœuds (idéalement sur des volumes ou des montages DAX).

## <a name="backup-and-restore-operations"></a>Opérations de sauvegarde et de restauration

Les conditions de restauration normales s’appliquent. Si la mémoire tampon de journal persistante est restaurée sur un volume ou un montage DAX, elle continue de fonctionner. Sinon, elle peut être supprimée de manière sécurisée.
  
## <a name="next-steps"></a>Étapes suivantes

- [How It Works (It Just Runs Faster): Non-Volatile Memory SQL Server Tail Of Log Caching on NVDIMM](/archive/blogs/bobsql/how-it-works-it-just-runs-faster-non-volatile-memory-sql-server-tail-of-log-caching-on-nvdimm)
- [Data exposed: Latency and Durability with SQL Server 2016](https://channel9.msdn.com/Shows/Data-Exposed/Latency-and-Durability-with-SQL-Server-2016)
- [Accélération de la latence de validation des transactions en utilisant la mémoire de classe stockage dans Windows Server 2016/SQL Server 2016 SP1](/archive/blogs/sqlserverstorageengine/transaction-commit-latency-acceleration-using-storage-class-memory-in-windows-server-2016sql-server-2016-sp1)