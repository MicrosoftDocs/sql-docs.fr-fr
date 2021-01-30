---
description: sys.dm_tran_active_snapshot_database_transactions (Transact-SQL)
title: sys.dm_tran_active_snapshot_database_transactions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_active_snapshot_database_transactions_TSQL
- dm_tran_active_snapshot_database_transactions
- sys.dm_tran_active_snapshot_database_transactions
- dm_tran_active_snapshot_database_transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_active_snapshot_database_transactions dynamic management view
ms.assetid: 55b83f9c-da10-4e65-9846-f4ef3c0c0f36
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7798fa7e5e93b4c0e5b5f227877cec536195513b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99134077"
---
# <a name="sysdm_tran_active_snapshot_database_transactions-transact-sql"></a>sys.dm_tran_active_snapshot_database_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Dans une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette vue de gestion dynamique retourne une table virtuelle pour toutes les transactions actives qui génèrent des versions de ligne ou qui sont susceptibles d'y accéder. Les transactions sont incluses lorsque l'une des conditions suivantes, au minimum, est remplie :  
  
-   Lorsque au moins l'une des deux options de base de données ALLOW_SNAPSHOT_ISOLATION et READ_COMMITTED_SNAPSHOT est définie à ON :  
  
    -   Il existe une ligne pour chaque transaction exécutée avec le niveau d'isolement d'instantané ou le niveau d'isolement de lecture validée avec le contrôle de version de ligne.  
  
    -   Il existe une ligne pour chaque transaction qui entraîne la création d'une version de ligne dans la base de données active. Par exemple, la transaction génère une version de ligne en mettant à jour ou en supprimant une ligne dans la base de données active.  
  
-   Lorsqu'un déclencheur est activé, il existe une ligne pour la transaction sous laquelle le déclencheur est exécuté.  
  
-   Lorsqu'une procédure d'indexation est en cours d'exécution, il existe une ligne pour la transaction qui crée l'index.  
  
-   Lorsqu'une session MARS (Multiple Active Results Sets) est activée, il existe une ligne pour chaque transaction qui accède aux versions de ligne.  
  
 Cette vue de gestion dynamique n'inclut pas les transactions système.  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_tran_active_snapshot_database_transactions**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.dm_tran_active_snapshot_database_transactions  
```  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**transaction_id**|**bigint**|Numéro d'identification unique assigné pour la transaction. L'ID de transaction permet principalement d'identifier la transaction dans les opérations de verrouillage.|  
|**transaction_sequence_num**|**bigint**|Numéro de séquence de la transaction. Il s'agit d'un numéro de séquence unique qui est attribué à une transaction lorsqu'elle démarre. Les transactions qui ne produisent pas d'enregistrements de version et n'utilisent pas d'analyses d'instantané ne recevront pas de numéro de séquence.|  
|**commit_sequence_num**|**bigint**|Numéro de séquence qui indique quand la transaction se termine (validée ou arrêtée). Pour les transactions actives, la valeur est NULL.|  
|**is_snapshot**|**int**|0 = n'est pas une transaction d'isolement d'instantané.<br /><br /> 1 = est une transaction d'isolement d'instantané.|  
|**session_id**|**int**|ID de la session qui a démarré la transaction.|  
|**first_snapshot_sequence_num**|**bigint**|Il s'agit du plus petit numéro de séquence des transactions qui étaient actives lors de la création d'un instantané. Lors de l'exécution, une transaction d'instantané prend un instantané de toutes les transactions actives présentes. Pour les transactions non liées à des instantanés, la valeur 0 est affichée dans cette colonne.|  
|**max_version_chain_traversed**|**int**|Longueur maximale de la chaîne de versions traversée pour trouver la version cohérente d'un point de vue transactionnel.|  
|**average_version_chain_traversed**|**real**|Nombre moyen de versions de ligne dans les chaînes de versions traversées.|  
|**elapsed_time_seconds**|**bigint**|Temps écoulé depuis que la transaction a obtenu son numéro de séquence.|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   

## <a name="remarks"></a>Notes  
 **sys.dm_tran_active_snapshot_database_transactions** signale les transactions auxquelles est affecté un numéro de séquence de transaction (xsn). Ce numéro de séquence est attribué lorsque la transaction accède pour la première fois à la banque des versions. Dans une base de données qui est activée pour l'isolement d'instantané ou l'isolement de lecture validée avec le contrôle de version de ligne, les exemples indiquent à quel moment un numéro de séquence est attribué à une transaction :  
  
-   Si une transaction est exécutée avec le niveau d'isolement sérialisable, un numéro de séquence est attribué lorsque la transaction exécute pour la première fois une instruction (par exemple, une opération UPDATE) qui entraîne la création d'une version de ligne.  
  
-   Si une transaction est exécutée avec le niveau d'isolement d'instantané, un numéro de séquence est attribué lorsqu'une instruction DML (Data Manipulation Language), y compris une opération SELECT, est exécutée.  
  
 Les numéros de séquence des transactions augmentent d'une unité à chaque transaction démarrée dans une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre un scénario de test dans lequel quatre transactions simultanées, chacune étant identifiée par un numéro de séquence de transaction, sont exécutées dans une base de données où les options ALLOW_SNAPSHOT_ISOLATION et READ_COMMITTED_SNAPSHOT sont définies à ON. Les transactions suivantes sont exécutées :  
  
-   XSN-57 est une opération Update exécutée avec le niveau d'isolement sérialisable.  
  
-   XSN-58 est identique à XSN-57.  
  
-   XSN-59 est une opération Select exécutée avec le niveau d'isolement d'instantané.  
  
-   XSN-60 est identique à XSN-59.  
  
 La requête suivante est exécutée :  
  
```  
SELECT   
    transaction_id,  
    transaction_sequence_num,  
    commit_sequence_num,  
    is_snapshot session_id,  
    first_snapshot_sequence_num,  
    max_version_chain_traversed,  
    average_version_chain_traversed,  
    elapsed_time_seconds  
  FROM sys.dm_tran_active_snapshot_database_transactions;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
transaction_id  transaction_sequence_num  commit_sequence_num  
--------------  ------------------------  -------------------  
9295            57                        NULL  
9324            58                        NULL  
9387            59                        NULL  
9400            60                        NULL  
  
is_snapshot  session_id   first_snapshot_sequence_num  
-----------  -----------  ---------------------------  
0            54           0  
0            53           0  
1            52           57  
1            51           57  
  
max_version_chain_traversed  average_version_chain_traversed  
---------------------------  -------------------------------  
0                            0  
0                            0  
1                            1  
1                            1  
  
elapsed_time_seconds  
--------------------  
419  
397  
359  
333  
```  
  
 Les informations suivantes évaluent les résultats de **sys.dm_tran_active_snapshot_database_transactions**:  
  
-   XSN-57 : étant donné que cette transaction n’est pas exécutée avec l’isolement d’instantané, la `is_snapshot` valeur et `first_snapshot_sequence_num` est `0` . `transaction_sequence_num` indique qu'un numéro de séquence de transaction a été attribué à cette transaction, car au moins l'une des options de base de données ALLOW_SNAPSHOT_ISOLATION et READ_COMMITTED_SNAPSHOT est activée (ON).  
  
-   XSN-58 : Cette transaction n'est pas exécutée avec le niveau d'isolement d'instantané, et les informations fournies pour XSN-57 s'appliquent.  
  
-   XSN-59 : Il s'agit de la première transaction active exécutée avec le niveau d'isolement d'instantané. Cette transaction lit les données qui sont validées avant XSN-57, comme l'indique l'argument `first_snapshot_sequence_num`. Le résultat de cette transaction indique également que le nombre maximal de chaîne de versions traversées pour une ligne est `1`, avec une moyenne de `1` version traversée pour chaque ligne utilisée. Ceci signifie que les transactions XSN-57, XSN-58 et XSN-60 n'ont pas modifié les lignes et les ont validées.  
  
-   XSN-60 : Il s'agit de la seconde transaction exécutée avec le niveau d'isolement d'instantané. Le résultat affiche les mêmes informations que pour la transaction XSN-59.  
  
## <a name="see-also"></a>Voir aussi  
 [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


