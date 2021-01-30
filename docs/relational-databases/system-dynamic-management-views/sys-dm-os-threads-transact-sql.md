---
description: sys.dm_os_threads (Transact-SQL)
title: sys.dm_os_threads (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_threads_TSQL
- sys.dm_os_threads
- dm_os_threads
- sys.dm_os_threads_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_threads dynamic management view
ms.assetid: a5052701-edbf-4209-a7cb-afc9e65c41c1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 91c0b28d2b30cc7e7d5f11eed8fe25f4c4907198
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193787"
---
# <a name="sysdm_os_threads-transact-sql"></a>sys.dm_os_threads (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne la liste de tous les threads du système d'exploitation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui s'exécutent sous le processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_os_threads**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|thread_address|**varbinary (8)**|Adresse mémoire (clé primaire) du thread.|  
|started_by_sqlservr|**bit**|Indique l'initiateur du thread.<br /><br /> 1 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a démarré le thread.<br /><br /> 0 = un autre composant a démarré le thread, par exemple une procédure stockée étendue dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|os_thread_id|**int**|ID du thread qui est attribué par le système d'exploitation.|  
|status|**int**|Indicateur d'état interne.|  
|instruction_address|**varbinary (8)**|Adresse de l'instruction qui est en cours d'exécution.|  
|creation_time|**datetime**|Heure de création du thread.|  
|kernel_time|**bigint**|Temps du noyau utilisé par ce thread.|  
|usermode_time|**bigint**|Temps utilisateur utilisé par ce thread.|  
|stack_base_address|**varbinary (8)**|Adresse mémoire haute de la pile de ce thread.|  
|stack_end_address|**varbinary (8)**|Adresse mémoire basse de la pile de ce thread.|  
|stack_bytes_committed|**int**|Nombre d'octets validés dans la pile.|  
|stack_bytes_used|**int**|Nombre d'octets actuellement utilisés activement sur le thread.|  
|affinité|**bigint**|Masque d'UC sur lequel s'exécute ce thread. Cela dépend de la valeur configurée par l’instruction **ALTER Server Configuration Set process Affinity** . Peut être différent du planificateur en cas d'affinité logicielle.|  
|Priorité|**int**|Priorité du thread.|  
|Paramètres régionaux|**int**|Indicateur de paramètres régionaux mis en cache (LCID) pour le thread.|  
|par jeton|**varbinary (8)**|Descripteur du jeton d'emprunt d'identité mis en cache pour le thread.|  
|is_impersonating|**int**|Indique si ce thread utilise l'emprunt d'identité Win32.<br /><br /> 1 = Le thread utilise des informations d'autorisation sécurisées différentes des informations par défaut du processus. Cela indique que le thread emprunte l'identité d'une entité différente de celle qui a créé le processus.|  
|is_waiting_on_loader_lock|**int**|État du système d'exploitation qui indique si le thread attend le verrou du chargeur.|  
|fiber_data|**varbinary (8)**|Fibre Win32 actuelle en cours d'exécution sur le thread. S'applique uniquement lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est configuré pour le regroupement léger.|  
|thread_handle|**varbinary (8)**|À usage interne uniquement|  
|event_handle|**varbinary (8)**|À usage interne uniquement|  
|scheduler_address|**varbinary (8)**|Adresse mémoire du planificateur associé au thread. Pour plus d’informations, consultez [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).|  
|worker_address|**varbinary (8)**|Adresse mémoire du thread de travail associé à ce thread. Pour plus d’informations, consultez [sys.dm_os_workers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).|  
|fiber_context_address|**varbinary (8)**|Adresse interne du contexte de la fibre. S'applique uniquement lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est configuré pour le regroupement léger.|  
|self_address|**varbinary (8)**|Pointeur de cohérence interne.|  
|processor_group|**smallint**|**S’applique à** : [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] et versions ultérieures.<br /><br /> ID de groupe de processeurs.|  
|pdw_node_id|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   

## <a name="notes-on-linux-version"></a>Remarques sur la version de Linux

En raison du fonctionnement du moteur SQL dans Linux, certaines de ces informations ne correspondent pas aux données de diagnostic Linux. Par exemple, `os_thread_id` ne correspond pas au résultat d’outils comme `ps` , `top` ou procfs (/Proc/ `pid` ).  Cela est dû à la couche d’abstraction de la plateforme (SQLPAL), une couche entre les composants SQL Server et le système d’exploitation.

## <a name="examples"></a>Exemples  
 Au démarrage, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] démarre des threads puis leur associe des threads de travail. Cependant, des composants externes (par exemple, une procédure stockée étendue) peuvent démarrer des threads sous le processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne contrôle pas ces threads. sys.dm_os_threads pouvez fournir des informations sur les threads non autorisés qui consomment des ressources dans le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] processus.  
  
 La requête suivante permet de rechercher des threads de travail (ainsi que la durée utilisée pour l'exécution) qui exécutent des threads non démarrés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]
>  Dans un souci de concision, la requête suivante utilise un astérisque (`*`) dans l'instruction `SELECT`. Évitez d'utiliser l'astérique (*), surtout avec des affichages catalogue, des vues de gestion dynamique et des fonctions table système. Les futures mises à niveau et versions de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent ajouter des colonnes et modifier l’ordre des colonnes en fonction de ces vues et fonctions. Ces changements entraveront peut-être le fonctionnement des applications qui attendent un ordre et un nombre de colonnes spécifiques.  
  
```  
SELECT *  
  FROM sys.dm_os_threads  
  WHERE started_by_sqlservr = 0;  
```  
  
## <a name="see-also"></a>Voir aussi  
  [sys.dm_os_workers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md)   
 [SQL Server vues de gestion dynamique liées au système d’exploitation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


