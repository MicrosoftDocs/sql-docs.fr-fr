---
description: sys.dm_clr_appdomains (Transact-SQL)
title: sys.dm_clr_appdomains (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_clr_appdomains
- sys.dm_clr_appdomains
- dm_clr_appdomains_TSQL
- sys.dm_clr_appdomains_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_appdomains dynamic management dynamic management view
ms.assetid: 9fe0d4fd-950a-4274-a493-85e776278045
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7579a0648bbc5eee90512bb70dfd667728b56834
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208103"
---
# <a name="sysdm_clr_appdomains-transact-sql"></a>sys.dm_clr_appdomains (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie une ligne pour chaque domaine d'application du serveur. Le domaine d’application (**AppDomain**) est une construction dans le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] Common Language Runtime (CLR) qui est l’unité d’isolation pour une application. Vous pouvez utiliser cette vue pour comprendre et dépanner les objets d’intégration du CLR qui s’exécutent dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Il existe plusieurs types d'objets de base de données managés d'intégration CLR. Pour obtenir des informations générales sur ces objets, consultez [génération d’objets de base de données avec intégration du Common Language Runtime (CLR)](../../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md). Chaque fois que ces objets sont exécutés, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée un **AppDomain** sous lequel il peut charger et exécuter le code requis. Le niveau d’isolation d’un **AppDomain** est un **AppDomain** par base de données par propriétaire. Autrement dit, tous les objets CLR appartenant à un utilisateur sont toujours exécutés dans le même **AppDomain** par base de données (si un utilisateur inscrit des objets de base de données CLR dans différentes bases de données, les objets de base de données CLR s’exécuteront dans différents domaines d’application). Un **AppDomain** n’est pas détruit une fois l’exécution du code terminée. Il est mis en cache dans la mémoire pour les prochaines exécutions. Les performances en sont alors améliorées.  
  
 Pour plus d’informations, consultez [domaines d’application](../../sql-server/what-s-new-in-sql-server-2016.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**appdomain_address**|**varbinary (8)**|Adresse de l' **AppDomain**. Tous les objets de base de données gérés appartenant à un utilisateur sont toujours chargés dans le même **AppDomain**. Vous pouvez utiliser cette colonne pour rechercher tous les assemblys actuellement chargés dans cet **AppDomain** dans **sys.dm_clr_loaded_assemblies**.|  
|**appdomain_id**|**int**|ID de l' **AppDomain**. Chaque **AppDomain** a un ID unique.|  
|**appdomain_name**|**varchar (386)**|Nom de l' **AppDomain** comme assigné par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**creation_time**|**datetime**|Heure de création de l' **AppDomain** . Étant donné que les **AppDomains** sont mis en cache et réutilisés pour de meilleures performances, **creation_time** n’est pas nécessairement l’heure à laquelle le code a été exécuté.|  
|**db_id**|**int**|ID de la base de données dans laquelle cet **AppDomain** a été créé. Le code stocké dans deux bases de données différentes ne peut pas partager un **AppDomain**.|  
|**user_id**|**int**|ID de l’utilisateur dont les objets peuvent s’exécuter dans cet **AppDomain**.|  
|**state**|**nvarchar(128)**|Descripteur pour l’état actuel de l' **AppDomain**. Un domaine d'application AppDomain peut être dans différents états de sa création à sa suppression. Pour plus d'informations, consultez la section « Remarques » de cette rubrique.|  
|**strong_refcount**|**int**|Nombre de références fortes à ce **AppDomain**. Cela reflète le nombre de lots en cours d’exécution qui utilisent cet **AppDomain**. Notez que l’exécution de cette vue créera un **refcount fort**; même si n’est pas un code en cours d’exécution, **strong_refcount** aura la valeur 1.|  
|**weak_refcount**|**int**|Nombre de références faibles à cet **AppDomain**. Cela indique combien d’objets à l’intérieur de l' **AppDomain** sont mis en cache. Lorsque vous exécutez un objet de base de données managé, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le met en cache à l’intérieur de l' **AppDomain** pour une réutilisation ultérieure. Les performances en sont alors améliorées.|  
|**cost**|**int**|Coût de l' **AppDomain**. Plus le coût est élevé, plus il est probable que ce **AppDomain** soit déchargé sous sollicitation de la mémoire. Le coût dépend généralement de la quantité de mémoire requise pour recréer cet **AppDomain**.|  
|**value**|**int**|Valeur de l' **AppDomain**. Plus la valeur est faible, plus il est probable que ce **AppDomain** soit déchargé sous sollicitation de la mémoire. La valeur dépend généralement du nombre de connexions ou de lots qui utilisent cet **AppDomain**.|  
|**total_processor_time_ms**|**bigint**|Temps processeur total, en millisecondes, utilisé par tous les threads lors de l'exécution dans le domaine d'application actuel depuis le démarrage du processus. Cela équivaut à **System. AppDomain. MonitoringTotalProcessorTime**.|  
|**total_allocated_memory_kb**|**bigint**|Taille totale, en kilo-octets, de toutes les allocations mémoire faites par le domaine d'application depuis sa création, sans soustraction de la mémoire recueillie. Cela équivaut à **System. AppDomain. MonitoringTotalAllocatedMemorySize**.|  
|**survived_memory_kb**|**bigint**|Nombre de kilo-octets qui ont survécu à la dernière collection bloquante complète et connus pour être référencés par le domaine d'application actuel. Cela équivaut à **System. AppDomain. MonitoringSurvivedMemorySize**.|  
  
## <a name="remarks"></a>Notes  
 Il existe une relation un-à-peut entre **dm_clr_appdomains. appdomain_address** et **dm_clr_loaded_assemblies. appdomain_address**.  
  
 Les tableaux suivants répertorient les valeurs d' **État** possibles, leurs descriptions et le moment où elles se produisent dans le cycle de vie d' **AppDomain** . Vous pouvez utiliser ces informations pour suivre le vie d’un **AppDomain** et pour surveiller le déchargement des instances **AppDomain** suspectes ou répétitives, sans avoir à analyser le journal des événements Windows.  
  
## <a name="appdomain-initialization"></a>Initialisation du domaine d'application AppDomain  
  
|State|Description|  
|-----------|-----------------|  
|E_APPDOMAIN_CREATING|**AppDomain** est en cours de création.|  
  
## <a name="appdomain-usage"></a>Utilisation du domaine d'application AppDomain  
  
|State|Description|  
|-----------|-----------------|  
|E_APPDOMAIN_SHARED|L' **AppDomain** du runtime est prêt à être utilisé par plusieurs utilisateurs.|  
|E_APPDOMAIN_SINGLEUSER|L' **AppDomain** est prêt à être utilisé dans les opérations DDL. Ceux-ci diffèrent d'E_APPDOMAIN_SHARED en ceci que les domaines d'application AppDomains partagés sont utilisés pour les exécutions d'intégration du CLR par opposition aux opérations DDL. Ces domaines d'application AppDomains sont isolés d'autres opérations simultanées.|  
|E_APPDOMAIN_DOOMED|L' **AppDomain** est planifié pour être déchargé, mais les threads sont en cours d’exécution.|  
  
## <a name="appdomain-cleanup"></a>Suppression du domaine d'application AppDomain  
  
|State|Description|  
|-----------|-----------------|  
|E_APPDOMAIN_UNLOADING|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a demandé que le CLR décharge le **domaine** d’application AppDomain, généralement parce que l’assembly qui contient les objets de base de données managés a été modifié ou supprimé.|  
|E_APPDOMAIN_UNLOADED|Le CLR a déchargé l' **AppDomain**. C’est généralement le résultat d’une procédure de remontée en raison de **ThreadAbort**, **OutOfMemory** ou d’une exception non gérée dans le code utilisateur.|  
|E_APPDOMAIN_ENQUEUE_DESTROY|**AppDomain** a été déchargé dans CLR et a été défini pour être détruit par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|E_APPDOMAIN_DESTROY|Le **domaine** d’application AppDomain est en cours de destruction par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|E_APPDOMAIN_ZOMBIE|L' **AppDomain** a été détruit par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; Toutefois, toutes les références à **AppDomain** n’ont pas été nettoyées.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW SERVER STATE sur la base de données.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant montre comment afficher les détails d’un **AppDomain** pour un assembly donné :  
  
```  
select appdomain_id, creation_time, db_id, user_id, state  
from sys.dm_clr_appdomains a  
where appdomain_address =   
(select appdomain_address   
 from sys.dm_clr_loaded_assemblies  
   where assembly_id = 500);  
```  
  
 L’exemple suivant montre comment afficher tous les assemblys d’un **AppDomain** donné :  
  
```  
select a.name, a.assembly_id, a.permission_set_desc, a.is_visible, a.create_date, l.load_time   
from sys.dm_clr_loaded_assemblies as l   
inner join sys.assemblies as a  
on l.assembly_id = a.assembly_id  
where l.appdomain_address =   
(select appdomain_address   
from sys.dm_clr_appdomains  
where appdomain_id = 15);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_clr_loaded_assemblies &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-clr-loaded-assemblies-transact-sql.md)   
 [Vues de gestion dynamique liées au Common Language Runtime &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
