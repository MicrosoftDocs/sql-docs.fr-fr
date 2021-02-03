---
description: ALTER RESOURCE POOL (Transact-SQL)
title: ALTER RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_RESOURCE_POOL_TSQL
- ALTER RESOURCE POOL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER RESOURCE POOL
ms.assetid: 9c1c4cfb-0e3b-4f01-bf57-3fce94c7d1d4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0f33cb191bee4b8856983f946a768c5abdd38726
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204048"
---
# <a name="alter-resource-pool-transact-sql"></a>ALTER RESOURCE POOL (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie une configuration de pool de ressources du gouverneur de ressources existante dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER RESOURCE POOL { pool_name | "default" }  
[WITH  
    ( [ MIN_CPU_PERCENT = value ]  
    [ [ , ] MAX_CPU_PERCENT = value ]   
    [ [ , ] CAP_CPU_PERCENT = value ]   
    [ [ , ] AFFINITY {
                        SCHEDULER = AUTO 
                      | ( <scheduler_range_spec> ) 
                      | NUMANODE = ( <NUMA_node_range_spec> )
                      }]   
    [ [ , ] MIN_MEMORY_PERCENT = value ]  
    [ [ , ] MAX_MEMORY_PERCENT = value ]   
    [ [ , ] MIN_IOPS_PER_VOLUME = value ]  
    [ [ , ] MAX_IOPS_PER_VOLUME = value ]  
)]   
[;]  
  
<scheduler_range_spec> ::=  
{SCHED_ID | SCHED_ID TO SCHED_ID}[,...n]  
  
<NUMA_node_range_spec> ::=  
{NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID}[,...n]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 { *pool_name* |  **"default"** }  
 Nom d'un pool de ressources défini par l'utilisateur existant ou du pool de ressources par défaut créé lors de l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Le paramètre default doit être placé entre des guillemets doubles ("") ou des crochets ([]) lorsqu'il est utilisé avec l'instruction ALTER RESOURCE POOL pour éviter tout conflit avec DEFAULT, qui est un mot réservé au système. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
> [!NOTE]  
>  Les groupes de charges de travail et les pools de ressources prédéfinis utilisent tous des noms en minuscule, tels que « default ». Ce facteur doit être pris en considération pour les serveurs qui utilisent un classement qui respecte la casse. Les serveurs avec un classement qui ne respecte pas la casse, tel que SQL_Latin1_General_CP1_CI_AS, traitent "default" et "Default" comme identiques.  
  
 MIN_CPU_PERCENT =*value*  
 Spécifie la bande passante de l'UC moyenne garantie pour toutes les demandes dans le pool de ressources en cas de contention de l'UC. *value* est un entier dont la valeur par défaut est 0. La plage autorisée pour *value* est comprise entre 0 et 100.  
  
 MAX_CPU_PERCENT =*value*  
 Spécifie la bande passante de l'UC moyenne maximale que toutes les demandes du pool de ressources recevront en cas de contention de l'UC. *value* est un entier dont la valeur par défaut est 100. La plage autorisée pour *value* est comprise entre 1 et 100.  
  
 CAP_CPU_PERCENT =*value*  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 Spécifie la capacité maximale cible de l’UC pour les requêtes dans le pool de ressources. *value* est un entier dont la valeur par défaut est 100. La plage autorisée pour *value* est comprise entre 1 et 100.  
  
> [!NOTE]  
>  En raison de la nature statistique de la gouvernance de l’UC, vous pouvez remarquer des pics occasionnels supérieurs à la valeur spécifiée dans CAP_CPU_PERCENT.  
  
 AFFINITY {SCHEDULER = AUTO | (Scheduler_range_spec) | NUMANODE = (NUMA_node_range_spec)}  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 Attache le pool de ressources aux planificateurs spécifiques. La valeur par défaut est AUTO.  
  
 AFFINITY SCHEDULER = (Scheduler_range_spec) mappe le pool de ressources aux planifications [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identifiées par les ID donnés. Ces ID correspondent aux valeurs de la colonne scheduler_id dans [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).  
  
 Lorsque vous utilisez AFFINITY NAMANODE = (NUMA_node_range_spec), le pool de ressources possède une affinité avec les planificateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui mappent aux UC physiques correspondant au nœud NUMA ou à la plage de nœuds donnée. Vous pouvez utiliser la requête Transact-SQL ci-après pour découvrir le mappage entre la configuration NUMA physique et les ID du planificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
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
  
 Spécifie les opérations d'E/S minimales par seconde (IOPS) par volume disque à réserver au pool de ressources. La plage autorisée pour *value* est comprise entre 0 et 2^31-1 (2 147 483 647). Spécifiez 0 pour indiquer l'absence de seuil minimal pour le pool.  
  
 MAX_IOPS_PER_VOLUME =*value*  
 **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.  
  
 Spécifie les opérations d'E/S maximales par seconde (IOPS) par volume disque à autoriser pour le pool de ressources. La plage autorisée pour *value* est comprise entre 0 et 2^31-1 (2 147 483 647). Spécifiez 0 pour définir un seuil illimité pour le pool. La valeur par défaut est 0.  
  
 Si le MAX_IOPS_PER_VOLUME pour un pool a la valeur 0, le pool n'est pas régi du tout et peut prendre toutes les E/S par seconde dans le système même si MIN_IOPS_PER_VOLUME est défini pour d'autres pools. Dans ce cas, nous vous recommandons de définir la valeur MAX_IOPS_PER_VOLUME de ce pool sur un nombre élevé (par exemple, la valeur maximale 2^31-1) si vous voulez que ce pool soit régi pour les E/S.  
  
## <a name="remarks"></a>Notes  
 MAX_CPU_PERCENT et MAX_MEMORY_PERCENT doivent respectivement être supérieurs ou égaux à MIN_CPU_PERCENT et MIN_MEMORY_PERCENT.  
  
 MAX_CPU_PERCENT peut utiliser une capacité d’UC supérieure à la valeur de MAX_CPU_PERCENT si elle est disponible. Bien qu’il puisse y avoir des pics périodiques au-dessus de CAP_CPU_PERCENT, les charges de travail ne doivent pas dépasser CAP_CPU_PERCENT pendant de longues périodes, même quand une capacité d’UC supplémentaire est disponible.  
  
 Le pourcentage de l'UC total pour chaque composant d'affinité (planificateur(s) ou nœud(s) NUMA) ne doit pas dépasser 100 %.  
  
 Lorsque vous exécutez des instructions DDL, nous vous recommandons de connaître les états du gouverneur de ressources. Pour plus d’informations, consultez [Resource Governor](../../relational-databases/resource-governor/resource-governor.md).  
  
 Quand vous modifiez un paramètre qui affecte le plan, le nouveau paramètre prend effet uniquement dans les plans précédemment mis en cache après l’exécution de DBCC FREEPROCCACHE (*pool_name*), où *pool_name* est le nom d’un pool de ressources Resource Governor.  
  
-   Si vous changez AFFINITY de plusieurs planificateurs à un planificateur unique, l’exécution de DBCC FREEPROCCACHE n’est pas obligatoire, car des plans parallèles peuvent s’exécuter en mode série. Toutefois, cela risque de ne pas être aussi efficace qu’un plan compilé en tant que plan en série.  
  
-   Si vous changez AFFINITY d’un planificateur unique à plusieurs planificateurs, l’exécution de DBCC FREEPROCCACHE n’est pas obligatoire. Toutefois, les plans en série ne pouvant pas s’exécuter en parallèle, l’effacement du cache respectif permettra aux nouveaux plans d’être compilés à l’aide du parallélisme.  
  
> [!CAUTION]  
>  L’effacement des plans mis en cache à partir d’un pool de ressources associé à plusieurs groupes de charges de travail affecte tous les groupes de charges de travail contenant le pool de ressources défini par l’utilisateur identifié par *pool_name*.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL SERVER.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant conserve tous les paramètres du pool de ressources par défaut sur le pool `default` à l'exception de `MAX_CPU_PERCENT` qui est remplacé par `25`.  
  
```sql  
ALTER RESOURCE POOL "default"  
WITH  
     ( MAX_CPU_PERCENT = 25);  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
 Dans l'exemple suivant, `CAP_CPU_PERCENT` définit la limite maximale d'utilisation fixe à 80 % et `AFFINITY SCHEDULER` a une valeur individuelle de 8 et une plage de 12 à 16.  
  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
```sql  
ALTER RESOURCE POOL Pool25  
WITH(   
     MIN_CPU_PERCENT = 5,  
     MAX_CPU_PERCENT = 10,       
     CAP_CPU_PERCENT = 80,  
     AFFINITY SCHEDULER = (8, 12 TO 16),   
     MIN_MEMORY_PERCENT = 5,  
     MAX_MEMORY_PERCENT = 15  
);  
  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
