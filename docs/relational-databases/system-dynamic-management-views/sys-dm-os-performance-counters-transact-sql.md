---
description: sys.dm_os_performance_counters (Transact-SQL)
title: sys.dm_os_performance_counters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_performance_counters
- sys.dm_os_performance_counters_TSQL
- dm_os_performance_counters_TSQL
- sys.dm_os_performance_counters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_performance_counters dynamic management view
ms.assetid: a1c3e892-cd48-40d4-b6be-2a9246e8fbff
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f58af388d1ee1913a15c1e39f0a5ddb3aebfaeb2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184789"
---
# <a name="sysdm_os_performance_counters-transact-sql"></a>sys.dm_os_performance_counters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie une ligne par compteur de performance maintenu par le serveur. Pour plus d’informations sur chaque compteur de performance, consultez [utiliser des objets SQL Server](../../relational-databases/performance-monitor/use-sql-server-objects.md).  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_os_performance_counters**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_name**|**nchar(128)**|Catégorie à laquelle ce compteur appartient.|  
|**counter_name**|**nchar(128)**|Nom du compteur. Pour obtenir plus d’informations sur un compteur, il s’agit du nom de la rubrique à sélectionner dans la liste des compteurs [utilisés SQL Server objets](../../relational-databases/performance-monitor/use-sql-server-objects.md). |  
|**instance_name**|**nchar(128)**|Nom d'une instance particulière du compteur. Contient souvent le nom de la base de données.|  
|**cntr_value**|**bigint**|Valeur actuelle du compteur.<br /><br /> **Remarque :** Pour les compteurs par seconde, cette valeur est cumulative. La valeur de la fréquence doit se calculer en échantillonnant la valeur à des intervalles de temps discrets. La différence entre deux valeurs prélevées successives est égale à la fréquence de l'intervalle de temps utilisé.|  
|**cntr_type**|**int**|Type de compteur défini par l'architecture de performances Windows. Pour plus d’informations sur les types de compteurs de performance, consultez [types de compteurs de performance WMI](/windows/desktop/WmiSdk/wmi-performance-counter-types) sur docs ou la documentation de Windows Server.|  
|**pdw_node_id**|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="remarks"></a>Notes  
 Si l'instance d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'affiche pas les compteurs de performance du système d'exploitation Windows, utilisez la requête [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante pour vérifier si les compteurs de performance ont été désactivés.  
  
```sql  
SELECT COUNT(*) FROM sys.dm_os_performance_counters;  
```  
  
Si la valeur de retour est 0 ligne, cela signifie que les compteurs de performance ont été désactivés. Vous devez ensuite consulter le journal d’installation et Rechercher l’erreur 3409, `Reinstall sqlctr.ini for this instance, and ensure that the instance login account has correct registry permissions.` ce qui indique que les compteurs de performance n’ont pas été activés. Les erreurs qui précèdent immédiatement l'erreur 3409 doivent indiquer la cause première de l'échec d'activation des compteurs de performance. Pour plus d’informations sur les fichiers journaux d’installation, consultez [afficher et lire SQL Server fichiers journaux d’installation](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  

Les compteurs de performances où la valeur de la `cntr_type` colonne est 65792, 272696320 et 537003264 affichent une valeur de compteur instantané instantané.

Les compteurs de performances où la valeur de la `cntr_type` colonne est 272696576, 1073874176 et 1073939712 affichent des valeurs de compteur cumulées au lieu d’un instantané instantané. Par conséquent, pour obtenir une lecture de type instantané, vous devez comparer le delta entre deux points de collection.

## <a name="permission"></a>Autorisation

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
 
## <a name="examples"></a>Exemples  
 L’exemple suivant retourne tous les compteurs de performances qui affichent des valeurs de compteur d’instantanés.  
  
```sql  
SELECT object_name, counter_name, instance_name, cntr_value, cntr_type  
FROM sys.dm_os_performance_counters
WHERE cntr_type = 65792 OR cntr_type = 272696320 OR cntr_type = 537003264;  
```  
  
## <a name="see-also"></a>Voir aussi  
  [SQL Server vues de gestion dynamique liées au système d’exploitation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.sysperfinfo &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysperfinfo-transact-sql.md)  
  
