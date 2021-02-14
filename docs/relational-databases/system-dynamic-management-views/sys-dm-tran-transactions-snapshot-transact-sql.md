---
description: sys.dm_tran_transactions_snapshot (Transact-SQL)
title: sys.dm_tran_transactions_snapshot (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_transactions_snapshot
- dm_tran_transactions_snapshot
- sys.dm_tran_transactions_snapshot_TSQL
- dm_tran_transactions_snapshot_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_transactions_snapshot dynamic management view
ms.assetid: 03f64883-07ad-4092-8be0-31973348c647
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 236a53de756755b16c47b36e316ffc873f4fb2e7
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100347644"
---
# <a name="sysdm_tran_transactions_snapshot-transact-sql"></a>sys.dm_tran_transactions_snapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne une table virtuelle pour la **sequence_number** des transactions qui sont actives lorsque chaque transaction d’instantané démarre. Les informations retournées par cette vue peuvent vous être utiles pour les opérations suivantes :  
  
-   Rechercher le nombre de transactions d'instantané actives.  
  
-   Identifier les modifications de données qui sont ignorées par une transaction d'instantané spécifique. Pour une transaction qui est active lorsqu'une transaction d'instantané démarre, toutes les modifications de données effectuées par cette transaction (même après sa validation) sont ignorées par la transaction d'instantané.  
  
 Par exemple, considérez la sortie suivante de **sys.dm_tran_transactions_snapshot**:  
  
```  
transaction_sequence_num snapshot_id snapshot_sequence_num  
------------------------ ----------- ---------------------  
59                       0           57  
59                       0           58  
60                       0           57  
60                       0           58  
60                       0           59  
60                       3           57  
60                       3           58  
60                       3           59  
60                       3           60  
```  
  
 La colonne `transaction_sequence_num` identifie le numéro de séquence de transaction (XSN) des transactions d'instantané actives. Le résultat indique deux valeurs : `59` et `60`. La colonne `snapshot_sequence_num` identifie le numéro de séquence des transactions qui sont actives au moment où chaque transaction d'instantané démarre.  
  
 Le résultat indique que la transaction d'instantané XSN-59 démarre alors que deux transactions actives, XSN-57 et XSN-58, sont en cours d'exécution. Si XSN-57 ou XSN-58 effectue des modifications de données, XSN-59 ignore les changements et utilise le contrôle de version de ligne pour assurer une vue cohérente, d'un point de vue transactionnel, de la base de données.  
  
 La transaction d'instantané XSN-60 ignore les modifications de données apportées par XSN-57, XSN-58 et également XSN 59.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**transaction_sequence_num**|**bigint**|Numéro de séquence (XSN) d'une transaction d'instantané.|  
|**snapshot_id**|**int**|Identificateur d'instantané pour chaque instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] lancée en mode de lecture validée à l'aide du contrôle de version de ligne. Cette valeur permet de générer une vue cohérente, d'un point de vue transactionnel, de la base de données prenant en charge chaque requête qui est exécutée en mode de lecture validée à l'aide du contrôle de version de ligne.|  
|**snapshot_sequence_num**|**bigint**|Numéro de séquence d'une transaction qui était active lorsque la transaction d'instantané a commencé.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
  
## <a name="remarks"></a>Notes  
 Lorsqu'une transaction d'instantané démarre, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] enregistre toutes les transactions qui sont actives à ce moment précis. **sys.dm_tran_transactions_snapshot** signale ces informations pour toutes les transactions d’instantanés actives.  
  
 Chaque transaction est identifiée par un numéro de séquence qui lui est affecté au moment où elle commence. Les transactions commencent au moment où une instruction BEGIN TRANSACTION ou BEGIN WORK est exécutée. Toutefois, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] n'attribue le numéro de séquence de la transaction que lors de l'exécution de la première instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui accède aux données suite à l'instruction BEGIN TRANSACTION ou BEGIN WORK. Les numéros de séquence des transactions sont incrémentés d'une unité à la fois.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  

