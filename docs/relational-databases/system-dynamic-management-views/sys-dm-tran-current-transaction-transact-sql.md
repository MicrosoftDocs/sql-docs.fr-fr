---
description: sys.dm_tran_current_transaction (Transact-SQL)
title: sys.dm_tran_current_transaction (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_current_transaction
- sys.dm_tran_current_transaction_TSQL
- dm_tran_current_transaction_TSQL
- dm_tran_current_transaction
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_current_transaction dynamic management view
ms.assetid: 75d5697d-b390-4963-99b8-fa0b4244a40c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ba8f53f07f9a6ab74f4224b9e1846825e12ac7e7
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839066"
---
# <a name="sysdm_tran_current_transaction-transact-sql"></a>sys.dm_tran_current_transaction (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une seule ligne qui affiche des informations sur l'état de la transaction dans la session active.  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_tran_current_transaction**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.dm_tran_current_transaction  
```  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**transaction_id**|**bigint**|ID de transaction de l'instantané actif.|  
|**transaction_sequence_num**|**bigint**|Numéro de séquence de la transaction qui produit la version de l'enregistrement.|  
|**transaction_is_snapshot**|**bit**|État d'isolement de l'instantané. Cette valeur est 1 si la transaction a démarré sous l'isolement d'instantané. Dans le cas contraire, la valeur est 0.|  
|**first_snapshot_sequence_num**|**bigint**|Il s'agit du plus petit numéro de séquence des transactions qui étaient actives lors de la création d'un instantané. Lors de l'exécution, une transaction d'instantané prend un instantané de toutes les transactions actives présentes. Pour les transactions non liées à des instantanés, la valeur 0 est affichée dans cette colonne.|  
|**last_transaction_sequence_num**|**bigint**|Numéro de séquence global. Cette valeur représente le dernier numéro de séquence de transaction généré par le système.|  
|**first_useful_sequence_num**|**bigint**|Numéro de séquence global. Cette valeur représente le plus ancien numéro de séquence de la transaction qui possède des versions de ligne devant être conservées dans la banque des versions. Les versions de ligne qui ont été créées par des transactions antérieures peuvent être supprimées.|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre un scénario de test dans lequel quatre transactions simultanées, chacune étant identifiée par un numéro de séquence de transaction, sont exécutées dans une base de données où les options ALLOW_SNAPSHOT_ISOLATION et READ_COMMITTED_SNAPSHOT sont définies à ON. Les transactions suivantes sont exécutées :  
  
-   XSN-57 est une opération Update exécutée avec le niveau d'isolement sérialisable.  
  
-   XSN-58 est identique à XSN-57.  
  
-   XSN-59 est une opération Select exécutée avec le niveau d'isolement d'instantané.  
  
-   XSN-60 est identique à XSN-59.  
  
 La requête suivante est exécutée dans l'étendue de chaque transaction.  
  
```  
SELECT   
    transaction_id  
   ,transaction_sequence_num  
   ,transaction_is_snapshot  
   ,first_snapshot_sequence_num  
   ,last_transaction_sequence_num  
   ,first_useful_sequence_num  
  FROM sys.dm_tran_current_transaction;  
```  
  
 Voici le résultat de XSN-59.  
  
```  
transaction_id       transaction_sequence_num transaction_is_snapshot  
-------------------- ------------------------ -----------------------  
9387                 59                       1                         
  
first_snapshot_sequence_num last_transaction_sequence_num  
--------------------------- -----------------------------  
57                               61                        
  
first_useful_sequence_num  
-------------------------  
57  
```  
  
 La sortie indique que XSN-59 est une transaction d'instantané qui utilise XSN-57 comme première transaction active lorsque XSN-59 a démarré. Ceci signifie que XSN-59 lit les données validées par les transactions possédant un numéro de séquence de transaction inférieur à XSN-57.  
  
 Voici le résultat de XSN-57.  
  
```  
transaction_id       transaction_sequence_num transaction_is_snapshot  
-------------------- ------------------------ -----------------------  
9295                 57                       0  
  
first_snapshot_sequence_num last_transaction_sequence_num  
--------------------------- -----------------------------  
NULL                        61  
  
first_useful_sequence_num  
-------------------------  
57  
```  
  
 Étant donné que XSN-57 n'est pas une transaction d'instantané, `first_snapshot_sequence_num` est `NULL`.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
