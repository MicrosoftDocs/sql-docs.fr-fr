---
description: sys.database_connection_stats (Azure SQL Database)
title: sys.database_connection_stats
titleSuffix: Azure SQL Database
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.database_connection_stats
- database_connection_stats
- database_connection_stats_TSQL
- sys.database_connection_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_connection_stats
- database_connection_stats
ms.assetid: 5c8cece0-63b0-4dee-8db7-6b43d94027ec
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current
ms.openlocfilehash: 9aaca2dee46593d637b11cde9b4e9e2a55664f6b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350889"
---
# <a name="sysdatabase_connection_stats-azure-sql-database"></a>sys.database_connection_stats (Azure SQL Database)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Contient des statistiques pour les [!INCLUDE[ssSDS](../../includes/sssds-md.md)] événements de **connectivité** de base de données, fournissant une vue d’ensemble des succès et des échecs de connexion de base de données. Pour plus d’informations sur les événements de connectivité, consultez types d’événements dans [sys.event_log &#40;Azure SQL Database&#41;](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md).  
  
|Statistique|Type|Description|  
|---------------|----------|-----------------|  
|**database_name**|**sysname**|Nom de la base de données.|  
|**heure-début**|**datetime2**|Date et heure UTC indiquant le début de l'intervalle d'agrégation. L'heure est toujours un multiple de 5 minutes. Par exemple :<br /><br /> '2011-09-28 16:00:00'<br />'2011-09-28 16:05:00'<br />'2011-09-28 16:10:00'|  
|**heure-fin**|**datetime2**|Date et heure UTC indiquant la fin de l'intervalle d'agrégation. **End_time** est toujours exactement 5 minutes plus tard que le **start_time** correspondant dans la même ligne.|  
|**success_count**|**int**|Nombre de connexions réussies.|  
|**total_failure_count**|**int**|Nombre total d'échecs de connexion. Il s’agit de la somme des **connection_failure_count**, **terminated_connection_count** et **throttled_connection_count**, et n’inclut pas les événements de blocage.|  
|**connection_failure_count**|**int**|Nombre d'échecs de connexion.|  
|**terminated_connection_count**|**int**|**_Applicable uniquement à [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] v11._**<br /><br /> Nombre de connexions terminées.|  
|**throttled_connection_count**|**int**|**_Applicable uniquement à [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] v11._**<br /><br /> Nombre de connexions limitées.|  
  
## <a name="remarks"></a>Notes  
  
### <a name="event-aggregation"></a>Agrégation d'événements

 Les informations relatives aux événements de cette vue sont collectées et agrégées par intervalles de 5 minutes. Les colonnes de nombre représentent le nombre de fois qu'un événement particulier de connectivité s'est produit pour une base de données spécifique dans un intervalle de temps donné.  
  
 Par exemple, si un utilisateur ne parvient pas à se connecter à la base de données Database1 sept fois entre 11h00 et 11h05 le 5/2/2012 (UTC), ces informations sont disponibles dans une ligne de cette vue :  
  
|**database_name**|**heure-début**|**heure-fin**|**success_count**|**total_failure_count**|**connection_failure_count**|**terminated_connection_count**|**throttled_connection_count**|  
|------------------------|---------------------|-------------------|------------------------|-------------------------------|------------------------------------|---------------------------------------|--------------------------------------|  
|`Database1`|`2012-02-05 11:00:00`|`2012-02-05 11:05:00`|`0`|`7`|`7`|`0`|`0`|  
  
### <a name="interval-start_time-and-end_time"></a>Heure de début (start_time) et heure de fin (end_time) de l'intervalle

 Un événement est inclus dans un intervalle d’agrégation lorsque l’événement se produit *sur* ou _après_**start_time** et _avant_**end_time** pour cet intervalle. Par exemple, un événement se produisant exactement à `2012-10-30 19:25:00.0000000` est inclus uniquement dans le deuxième intervalle indiqué ci-dessous :  
  
```  
  
start_time                    end_time  
2012-10-30 19:20:00.0000000   2012-10-30 19:25:00.0000000  
2012-10-30 19:25:00.0000000   2012-10-30 19:30:00.0000000  
```  
  
### <a name="data-updates"></a>Mises à jour des données

 Les données de cette vue sont cumulées au fil du temps. Généralement, les données sont cumulées pendant une heure à compter du début de l'intervalle d'agrégation, mais cela peut prendre jusqu'à 24 heures avant que toutes les données apparaissent dans la vue. Pendant ce temps, les informations d'une seule ligne peuvent être mises à jour périodiquement.  
  
### <a name="data-retention"></a>Conservation des données

 Les données de cette vue sont conservées pendant un maximum de 30 jours, voire moins en fonction du nombre de bases de données et du nombre d’événements uniques générés par chaque base de données. Pour conserver ces informations plus longtemps, copiez les données dans une base de données distincte. Une fois que vous avez effectué une copie initiale de la vue, les lignes de la vue peuvent être mises à jour au fur et à mesure que les données sont cumulées. Pour tenir à jour votre copie des données, effectuez périodiquement une analyse des lignes de la table pour détecter une augmentation du nombre d'événements dans les lignes existantes et pour identifier les nouvelles lignes (vous pouvez identifier les lignes qui sont uniques à l'aide des heures de début et de fin), puis mettez à jour votre copie des données en fonction de ces modifications.  
  
### <a name="errors-not-included"></a>Erreurs non incluses.

 Cette vue peut ne pas inclure toutes les informations de connexion et d'erreur :  
  
- Cette vue n’inclut pas toutes les [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Erreurs de base de données qui peuvent se produire, mais seulement celles spécifiées dans les types d’événements dans [sys.event_log &#40;Azure SQL Database&#41;](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md).  
  
- En cas de défaillance d’un ordinateur dans le centre de données, il est possible qu’il [!INCLUDE[ssSDS](../../includes/sssds-md.md)] manque une petite quantité de données dans la table d’événements.  
  
- Si une adresse IP a été bloquée par DoSGuard, les événements de tentative de connexion à partir de cette adresse IP ne peuvent pas être collectés et n'apparaitront pas dans cette vue.  
  
## <a name="permissions"></a>Autorisations

 Les utilisateurs autorisés à accéder à la base de données **Master** disposent d’un accès en lecture seule à cette vue.  
  
## <a name="example"></a> Exemple

 L’exemple suivant montre une requête de **sys.database_connection_stats** pour retourner un résumé des connexions de base de données qui se sont produites entre midi sur 9/25/2011 et midi le 9/28/2011 (UTC). Par défaut, les résultats de la requête sont triés par **start_time** (ordre croissant).  
  
```sql
SELECT *  
FROM sys.database_connection_stats
WHERE start_time>='2011-09-25 12:00:00' and end_time<='2011-09-28 12:00:00';  
```  

## <a name="see-also"></a>Voir aussi

 [Résoudre des problèmes de connexion à Azure SQL Database](/azure/sql-database/sql-database-troubleshoot-common-connection-issues)  
  
  
