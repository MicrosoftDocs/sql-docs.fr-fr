---
description: ALTER EXTERNAL RESOURCE POOL (Transact-SQL)
title: ALTER EXTERNAL RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: reference
f1_keywords:
- ALTER_EXTERNAL_RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER EXTERNAL RESOURCE POOL statement
ms.assetid: 634c327d-971b-49ba-b8a2-e243a04040db
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 13cceefecf9d5c67cfde6cd10ec1b86fc564ebf6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204222"
---
# <a name="alter-external-resource-pool-transact-sql"></a>ALTER EXTERNAL RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Change un pool externe Resource Governor qui spécifie quelles ressources peuvent être utilisées par les processus externes. 

::: moniker range="=sql-server-2016"
Pour [!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)] dans [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], le pool externe gouverne `rterm.exe`, `BxlServer.exe` et les autres processus qu’ils engendrent.
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
Pour [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)], le pool externe gouverne `rterm.exe`, `python.exe`, `BxlServer.exe` et les autres processus qu’ils engendrent.
::: moniker-end

![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

## <a name="syntax"></a>Syntaxe
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
```syntaxsql
ALTER EXTERNAL RESOURCE POOL { pool_name | "default" }
[ WITH (
    [ MAX_CPU_PERCENT = value ]
    [ [ , ] MAX_MEMORY_PERCENT = value ]
    [ [ , ] MAX_PROCESSES = value ]
    )
]
[ ; ]
  
<CPU_range_spec> ::=
{ CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]
```  
::: moniker-end
::: moniker range="=sql-server-2016||=sql-server-2017"
 ```syntaxsql

ALTER EXTERNAL RESOURCE POOL { pool_name | "default" }
[ WITH (
    [ MAX_CPU_PERCENT = value ]
    [ [ , ] AFFINITY CPU =
            {
                AUTO
              | ( <cpu_range_spec> )
              | NUMANODE = (( <NUMA_node_id> )
            } ]   
    [ [ , ] MAX_MEMORY_PERCENT = value ]
    [ [ , ] MAX_PROCESSES = value ]
    )
]
[ ; ]
  
<CPU_range_spec> ::=
{ CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]
```  
::: moniker-end 

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

{ *pool_name* | "default" }  
Nom d’un pool de ressources externe existant défini par l'utilisateur ou du pool de ressources externe par défaut créé au moment de l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
Quand il est utilisé avec l’instruction `ALTER EXTERNAL RESOURCE POOL`, le paramètre default doit être placé entre des guillemets doubles ("") ou des crochets ([]) pour éviter tout conflit avec `DEFAULT`, qui est un mot réservé au système.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
MAX_CPU_PERCENT =*value*  
Spécifie la bande passante processeur moyenne maximale que toutes les demandes du pool de ressources externes peuvent recevoir en cas de contention du processeur. *value* est un entier. La plage autorisée pour *value* est comprise entre 1 et 100.

MAX_MEMORY_PERCENT =*value*  
Spécifie la mémoire totale du serveur qui peut être utilisée par les demandes dans ce pool de ressources externes. *value* est un entier. La plage autorisée pour *value* est comprise entre 1 et 100.

MAX_PROCESSES =*value*  
Spécifie le nombre maximal de processus autorisés pour le pool de ressources externes. Spécifiez 0 pour définir un seuil illimité pour le pool, qui est alors limité uniquement par les ressources de l’ordinateur.
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
MAX_CPU_PERCENT =*value*  
Spécifie la bande passante processeur moyenne maximale que toutes les demandes du pool de ressources externes peuvent recevoir en cas de contention du processeur. *value* est un entier. La plage autorisée pour *value* est comprise entre 1 et 100.

AFFINITY {CPU = AUTO | ( \<CPU_range_spec> ) | NUMANODE = (\<NUMA_node_range_spec>)}  
Joint le pool de ressources externe aux UC spécifiées.

AFFINITY CPU = **(** \<CPU_range_spec> **)** mappe le pool de ressources externes aux processeurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identifiés par les valeurs CPU_ID données. Quand vous utilisez AFFINITY NUMANODE =  **(** \<NUMA_node_range_spec> **)** , le pool de ressources externes est associé par affinité aux processeurs physiques [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correspondant au nœud NUMA ou à la plage de nœuds donnée.

MAX_MEMORY_PERCENT =*value*  
Spécifie la mémoire totale du serveur qui peut être utilisée par les demandes dans ce pool de ressources externes. *value* est un entier. La plage autorisée pour *value* est comprise entre 1 et 100.

MAX_PROCESSES =*value*  
Spécifie le nombre maximal de processus autorisés pour le pool de ressources externes. Spécifiez 0 pour définir un seuil illimité pour le pool, qui est alors limité uniquement par les ressources de l’ordinateur.
::: moniker-end
## <a name="remarks"></a>Notes

[!INCLUDE[ssDE](../../includes/ssde-md.md)] implémente le pool de ressources quand vous exécutez l’instruction [ALTER RESOURCE GOVERNOR RECONFIGURE](../../t-sql/statements/alter-resource-governor-transact-sql.md).

Pour obtenir des informations générales sur les pools de ressources, consultez [Pool de ressources du gouverneur de ressources](../../relational-databases/resource-governor/resource-governor-resource-pool.md), [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md) et [sys.dm_resource_governor_external_resource_pool_affinity &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md).  

Pour plus d’informations sur l’utilisation de pools de ressources externes pour gouverner les travaux de Machine Learning, consultez [Gouvernance des ressources de Machine Learning dans SQL Server](../../machine-learning/administration/resource-governor.md).
## <a name="permissions"></a>Autorisations

Nécessite l'autorisation `CONTROL SERVER`.

## <a name="examples"></a>Exemples

L’instruction suivante change un pool externe, en limitant l’utilisation de l’UC à 50 % et la mémoire maximale à 25 % de la mémoire disponible sur l’ordinateur.
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
```sql
ALTER EXTERNAL RESOURCE POOL ep_1
WITH (
    MAX_CPU_PERCENT = 50
    , MAX_MEMORY_PERCENT = 25
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

::: moniker range="=sql-server-2016||=sql-server-2017"
```sql
ALTER EXTERNAL RESOURCE POOL ep_1
WITH (
    MAX_CPU_PERCENT = 50
    , AFFINITY CPU = AUTO
    , MAX_MEMORY_PERCENT = 25
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
::: moniker-end

## <a name="see-also"></a>Voir aussi

+ [Gouvernance des ressources de Machine Learning dans SQL Server](../../machine-learning/administration/resource-governor.md)
+ [external scripts enabled (option de configuration de serveur)](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)
+ [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md)
+ [DROP EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)
+ [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md)
+ [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)
+ [Pool de ressources de Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md)
+ [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md) 
