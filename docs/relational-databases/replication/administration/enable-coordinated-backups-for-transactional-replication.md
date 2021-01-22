---
title: Activer les sauvegardes coordonnées (Transactionnel)
description: Découvrez comment activer les sauvegardes coordonnées sur la base de données de distribution afin que le journal des transactions de la base de données de publication Réplication transactionnelle ne soit pas tronqué tant que les transactions qui ont été propagées sur le serveur de distribution n’ont pas été sauvegardées.
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- transactional replication, backup and restore
- sp_replicationdboption
- sync with backup [SQL Server replication]
- coordinated backups [SQL Server replication]
- backups [SQL Server replication], transactional replication
ms.assetid: 73a914ba-8b2d-4f4d-ac1b-db9bac676a30
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 34911188162ac5b63f5a43d510d10d503820d200
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170471"
---
# <a name="enable-coordinated-backups-for-transactional-replication"></a>Activer les sauvegardes coordonnées pour la réplication transactionnelle
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Lorsque vous activez une base de données pour la réplication transactionnelle, vous pouvez spécifier que toutes les transactions doivent être sauvegardées avant d'être remises à la base de données de distribution. Vous pouvez activer la sauvegarde coordonnée également sur la base de données de distribution afin que le journal des transactions de la base de données de publication ne soit pas tronqué tant que les transactions qui ont été propagées sur le serveur de distribution n'ont pas été sauvegardées. Pour plus d’informations, voir [Stratégies de sauvegarde et de restauration de la réplication transactionnelle et d'instantané](../../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md).  
  
### <a name="to-enable-coordinated-backups-for-a-database-published-with-transactional-replication"></a>Pour activer les sauvegardes coordonnées d'une base de données publiée avec la réplication transactionnelle  
  
1.  Sur le serveur de publication, utilisez la fonction `SELECT DATABASEPROPERTYEX(DB_NAME(),'IsSyncWithBackup')` [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../../t-sql/functions/databasepropertyex-transact-sql.md) pour retourner la propriété **IsSyncWithBackup** de la base de données de publication. Si la fonction retourne **1**, les sauvegardes coordonnées sont déjà activées pour la base de données publiée.  
  
2.  Si la fonction de l’étape 1 retourne **0**, exécutez [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) sur le serveur de publication de la base de données de publication. Spécifiez la valeur **sync with backup** pour **\@optname** et **true** pour **\@value**.  
  
    > [!NOTE]  
    >  Si vous modifiez l'option **sync with backup** en **false**, le point de troncation de la base de données de publication sera mis à jour après que l'Agent de lecture du journal se soit exécuté ou après un intervalle si l'Agent de lecture du journal s'exécute continuellement. L'intervalle maximal est contrôlé par le paramètre d'agent **-MessageInterval** (lequel a une valeur par défaut de 30 secondes).  
  
### <a name="to-enable-coordinated-backups-for-a-distribution-database"></a>Pour activer des sauvegardes coordonnées pour une base de données de distribution  
  
1.  Sur le serveur de distribution, utilisez la fonction [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../../t-sql/functions/databasepropertyex-transact-sql.md) pour retourner la propriété **IsSyncWithBackup** de la base de données de distribution. Si la fonction retourne **1**, les sauvegardes coordonnées sont déjà activées pour la base de données de distribution.  
  
2.  Si la fonction de l’étape 1 retourne **0**, exécutez [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) sur le serveur de distribution de la base de données de distribution. Spécifiez la valeur **sync with backup** pour **\@optname** et **true** pour **\@value**.  
  
### <a name="to-disable-coordinated-backups"></a>Pour désactiver les sauvegardes coordonnées  
  
1.  Sur le serveur de publication de la base de données de publication ou sur le serveur de distribution de la base de données de distribution, exécutez [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md). Spécifiez la valeur **sync with backup** pour **\@optname** et **false** pour **\@value**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-retrieve-the-issyncwithbackup-property-for-the-current-database"></a>R. Récupérer la propriété `IsSyncWithBackup` de la base de données active

Cet exemple retourne la propriété `IsSyncWithBackup` de la base de données active :
  
```sql
SELECT DATABASEPROPERTYEX(DB_NAME(),'IsSyncWithBackup')`
```

### <a name="b-retrieve-the-issyncwithbackup-property-for-a-specific-database"></a>B. Récupérer la propriété `IsSyncWithBackup` d’une base de données spécifique

Cet exemple retourne la propriété `IsSyncWithBackup` de la base de données `NameOfDatabaseToCheck` :
  
```sql
SELECT DATABASEPROPERTYEX('NameOfDatabaseToCheck','IsSyncWithBackup')`
```
