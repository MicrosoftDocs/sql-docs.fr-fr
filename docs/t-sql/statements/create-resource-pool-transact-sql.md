---
description: CREATE RESOURCE POOL (Transact-SQL)
title: CREATE RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE RESOURCE POOL
- RESOURCE POOL
- CREATE_RESOURCE_POOL_TSQL
- RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE RESOURCE POOL
ms.assetid: 82712505-c6f9-4a65-a469-f029b5a2d6cd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a55efe97292550be8d0150514c56c3eb478f39ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188507"
---
# <a name="create-resource-pool-transact-sql"></a>CREATE RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Crée un pool de ressources du gouverneur de ressources dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un gouverneur de ressources représente un sous-ensemble des ressources physiques (mémoire, UC et E/S) d'une instance du moteur de base de données. Le Gouverneur de ressources permet à un administrateur de base de données de répartir des ressources serveur entre plusieurs pools de ressources (64 pools au maximum). Le gouverneur de ressources n'est pas disponible dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Syntaxe  
```syntaxsql
CREATE RESOURCE POOL pool_name  
[ WITH  
    (  
        [ MIN_CPU_PERCENT = value ]  
        [ [ , ] MAX_CPU_PERCENT = value ]   
        [ [ , ] CAP_CPU_PERCENT = value ]   
        [ [ , ] AFFINITY {SCHEDULER =  
                  AUTO 
                | ( <scheduler_range_spec> )   
                | NUMANODE = ( <NUMA_node_range_spec> )
                } ]   
        [ [ , ] MIN_MEMORY_PERCENT = value ]  
        [ [ , ] MAX_MEMORY_PERCENT = value ]  
        [ [ , ] MIN_IOPS_PER_VOLUME = value ]  
        [ [ , ] MAX_IOPS_PER_VOLUME = value ]  
    )   
]  
[;]  
  
<scheduler_range_spec> ::=  
{ SCHED_ID | SCHED_ID TO SCHED_ID }[,...n]  
  
<NUMA_node_range_spec> ::=  
{ NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID }[,...n]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*pool_name*  
Nom défini par l'utilisateur du pool de ressources. *pool_name* est alphanumérique, peut contenir jusqu’à 128 caractères, doit être unique dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et doit respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
MIN_CPU_PERCENT =*value*  
Spécifie la bande passante de l'UC moyenne garantie pour toutes les demandes dans le pool de ressources en cas de contention de l'UC. *value* est un entier dont la valeur par défaut est 0. La plage autorisée pour *value* est comprise entre 0 et 100.  
  
MAX_CPU_PERCENT =*value*  
Spécifie la bande passante de l’UC moyenne maximale que toutes les demandes du pool de ressources recevront en cas de contention de l’UC. *value* est un entier dont la valeur par défaut est 100. La plage autorisée pour *value* est comprise entre 1 et 100.  
  
CAP_CPU_PERCENT =*value*   
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
Spécifie une limite maximale d'utilisation fixe sur la bande passante de l'UC que toutes les demandes dans le pool de ressources recevront. Limite le niveau de la bande passante maximum de l'UC pour qu'il soit identique à la valeur spécifiée. *value* est un entier dont la valeur par défaut est 100. La plage autorisée pour *value* est comprise entre 1 et 100.  
  
AFFINITY {SCHEDULER = AUTO | ( \<scheduler_range_spec> ) | NUMANODE = (\<NUMA_node_range_spec>)}      
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
Attache le pool de ressources aux planificateurs spécifiques. La valeur par défaut est AUTO.  
  
AFFINITY SCHEDULER = **(** \<scheduler_range_spec> **)** mappe le pool de ressources aux planifications [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identifiées par les ID spécifiés. Ces ID correspondent aux valeurs de la colonne scheduler_id dans [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md). 
  
Quand vous utilisez AFFINITY NUMANODE = **(** \<NUMA_node_range_spec> **)** , le pool de ressources a une affinité avec les planificateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui sont mappés aux processeurs physiques correspondant au nœud NUMA ou à la plage de nœuds donnée. Vous pouvez utiliser la requête [!INCLUDE[tsql](../../includes/tsql-md.md)] ci-après pour découvrir le mappage entre la configuration NUMA physique et les ID du planificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
```sql  
SELECT osn.memory_node_id AS [numa_node_id], sc.cpu_id, sc.scheduler_id  
FROM sys.dm_os_nodes AS osn  
INNER JOIN sys.dm_os_schedulers AS sc   
    ON osn.node_id = sc.parent_node_id   
    AND sc.scheduler_id < 1048576;  
```  
  
MIN_MEMORY_PERCENT =*value*    
Spécifie la quantité de mémoire minimale réservée à ce pool de ressources qui ne peut pas être partagée avec d'autres pools de ressources. *value* est un entier dont la valeur par défaut est 0. La plage autorisée pour *value* est comprise entre 0 et 100.  
  
MAX_MEMORY_PERCENT =*value*    
Spécifie la mémoire totale du serveur qui peut être utilisée par les demandes dans ce pool de ressources. *value* est un entier dont la valeur par défaut est 100. La plage autorisée pour *value* est comprise entre 1 et 100.  
  
MIN_IOPS_PER_VOLUME =*value*    
**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.  
  
Spécifie les opérations d'E/S minimales par seconde (IOPS) par volume disque à réserver au pool de ressources. La plage autorisée pour *value* est comprise entre 0 et 2^31-1 (2 147 483 647). Spécifiez 0 pour indiquer l'absence de seuil minimal pour le pool. La valeur par défaut est 0.  
  
MAX_IOPS_PER_VOLUME =*value*    
**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.  
  
Spécifie les opérations d'E/S maximales par seconde (IOPS) par volume disque à autoriser pour le pool de ressources. La plage autorisée pour *value* est comprise entre 0 et 2^31-1 (2 147 483 647). Spécifiez 0 pour définir un seuil illimité pour le pool. La valeur par défaut est 0.  
  
Si la valeur `MAX_IOPS_PER_VOLUME` d’un pool est égale à 0, ce pool n’est pas régi du tout et peut prendre toutes les E/S par seconde du système, même si MIN_IOPS_PER_VOLUME est défini sur d’autres pools. Dans ce cas, nous recommandons de définir la valeur `MAX_IOPS_PER_VOLUME` de ce pool sur un nombre élevé (par exemple, la valeur maximale 2^31-1) pour qu’il soit régi en matière d’E/S.  
  
## <a name="remarks"></a>Notes  
`MIN_IOPS_PER_VOLUME` et `MAX_IOPS_PER_VOLUME` spécifient le nombre minimal et maximal de lectures et d’écritures par seconde. Ces lectures ou écritures peuvent être de toute taille et n'indiquent pas un débit minimal ou maximal.  
  
Les valeurs de `MAX_CPU_PERCENT` et `MAX_MEMORY_PERCENT` doivent être respectivement supérieures ou égales aux valeurs de `MIN_CPU_PERCENT` et `MIN_MEMORY_PERCENT`.  
  
`CAP_CPU_PERCENT` est différent de `MAX_CPU_PERCENT`, dans la mesure où les charges de travail associées au pool peuvent utiliser la capacité du processeur au-delà de la valeur `MAX_CPU_PERCENT` si elle est disponible, mais pas au-delà de la valeur `CAP_CPU_PERCENT`.  
  
Le pourcentage de processeur total de chaque composant d’affinité (planificateur(s) ou nœud(s) NUMA) ne doit pas dépasser 100 %.  
  
## <a name="permissions"></a>Autorisations  
Nécessite l'autorisation `CONTROL SERVER`.  
  
## <a name="examples"></a>Exemples  
### <a name="1-shows-how-to-create-a-resource-pool"></a>1. Créer un pool de ressources

Cet exemple crée un pool de ressources nommé « bigPool ». Ce pool utilise les paramètres par défaut du gouverneur de ressources.  
  
```sql  
CREATE RESOURCE POOL bigPool;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
### <a name="2-set-the-cap_cpu_percent-to-a-hard-cap-and-set-affinity-scheduler"></a>2. Définir CAP_CPU_PERCENT sur une limite inconditionnelle et définir AFFINITY SCHEDULER

Définissez CAP_CPU_PERCENT sur la limite inconditionnelle de 30 % et définissez AFFINITY SCHEDULER sur les plages 0-63 et 128-191. 
  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
```sql  
CREATE RESOURCE POOL PoolAdmin  
WITH (  
     MIN_CPU_PERCENT = 10,  
     MAX_CPU_PERCENT = 20,  
     CAP_CPU_PERCENT = 30,  
     AFFINITY SCHEDULER = (0 TO 63, 128 TO 191),  
     MIN_MEMORY_PERCENT = 5,  
     MAX_MEMORY_PERCENT = 15  
      );  
```  
  
### <a name="3-set-min_iops_per_volume-and-max_iops_per_volume"></a>3. Définir MIN_IOPS_PER_VOLUME et MAX_IOPS_PER_VOLUME   

Définissez MIN_IOPS_PER_VOLUME sur 20 et MAX_IOPS_PER_VOLUME sur 100. Ces valeurs déterminent les opérations de lecture et d'écriture d'E/S physiques disponibles pour le pool de ressources.  
  
**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.  
  
```sql  
CREATE RESOURCE POOL PoolAdmin  
WITH (  
    MIN_IOPS_PER_VOLUME = 20,  
    MAX_IOPS_PER_VOLUME = 100  
      );  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md)     
 [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-resource-pool-transact-sql.md)     
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)     
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)     
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)     
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)     
 [Pool de ressources de Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md)     
 [Créer un pool de ressources](../../relational-databases/resource-governor/create-a-resource-pool.md)    
  
