---
description: sys. server_resource_stats (Azure SQL Database)
title: sys. server_resource_stats (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 06/28/2018
ms.service: sql-database
ms.reviewer: carlrab, edmaca
ms.topic: language-reference
f1_keywords:
- resource_stats
- sys.resource_stats
- sys.resource_stats_TSQL
- resource_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_stats
- resource_stats
ms.assetid: ''
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||=sqlallproducts-allversions
ms.openlocfilehash: ef3f27b814405cf6ca56a47ffcac8dd467f939f7
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88376705"
---
# <a name="sysserver_resource_stats-azure-sql-database"></a>sys. server_resource_stats (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Retourne les données d’utilisation de l’UC, d’e/s et de stockage pour Azure SQL Managed Instance. Les données sont collectées et agrégées dans des intervalles de cinq minutes. Une ligne est créée pour chaque rapport de 15 secondes. Les données retournées incluent l’utilisation du processeur, la taille de stockage, l’utilisation des e/s et la référence SKU. Les données historiques sont conservées pendant environ 14 jours.

La vue **sys. server_resource_stats** a des définitions différentes selon la version du Managed instance Azure SQL à laquelle la base de données est associée. Tenez compte de ces différences et des modifications requises par votre application lors de la mise à niveau vers une nouvelle version de serveur.
 
  
 Le tableau suivant décrit les colonnes disponibles dans un serveur v12 :  
  
|Colonnes|Type de données|Description|  
|----------------------------|---------------|-----------------|  
|start_time|**datetime2**|Heure UTC indiquant le début de l’intervalle de création de rapports de quinze secondes|  
|end_time|**datetime**|Heure UTC indiquant la fin de l’intervalle de création de rapports de 15 secondes|
|resource_type|Nvarchar(128)|Type de la ressource pour laquelle des métriques sont fournies|
|resource_name|nvarchar(128)|Nom de la ressource.|
|sku|nvarchar(128)|Managed Instance niveau de service de l’instance. Les valeurs possibles sont les suivantes : <br><ul><li>Usage général</li></ul><ul><li>Critique pour l’entreprise</li></ul>|
|hardware_generation|nvarchar(128)|Identificateur de génération de matériel : par exemple, Gen 4 ou Gen 5|
|virtual_core_count|int|Représente le nombre de cœurs virtuels par instance (8, 16 ou 24 en préversion publique)|
|avg_cpu_percent|décimal (5, 2)|Utilisation moyenne du calcul en pourcentage de la limite du niveau de service Managed Instance utilisé par l’instance. Elle est calculée en fonction de la somme du temps processeur de tous les pools de ressources pour toutes les bases de données de l’instance et divisée par le temps processeur disponible pour ce niveau dans l’intervalle donné.|
|reserved_storage_mb|bigint|Stockage réservé par instance (quantité d’espace de stockage que le client a achetée pour l’instance gérée)|
|storage_space_used_mb|décimal (18, 2)|Stockage utilisé par tous les fichiers de base de données dans une instance gérée (y compris les bases de données utilisateur et système)|
|io_request|bigint|Nombre total d’opérations physiques d’e/s au cours de l’intervalle|
|io_bytes_read|bigint|Nombre d’octets physiques lus dans l’intervalle|
|io_bytes_written|bigint|Nombre d’octets physiques écrits dans l’intervalle|

 
> [!TIP]  
>  Pour plus d’informations sur ces limites et les niveaux de service, consultez les rubriques [Managed instance niveaux de service](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance#managed-instance-service-tiers).  
    
## <a name="permissions"></a>Autorisations  
 Cette vue est disponible pour tous les rôles d’utilisateur disposant d’autorisations pour se connecter à la base de données **Master** .  
  
## <a name="remarks"></a>Notes  
 Les données retournées par **sys. server_resource_stats** sont exprimées sous la forme du total utilisé en octets ou en mégaoctets (exprimés dans les noms de colonnes) autres que avg_cpu, exprimé sous la forme d’un pourcentage des limites maximales autorisées pour le niveau de service/niveau de performances que vous exécutez.  
 
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne toutes les bases de données dont la moyenne s'établit à au moins 80 % de l'utilisation du calcul au cours de la dernière semaine.  
  
```sql  
DECLARE @s datetime;  
DECLARE @e datetime;  
SET @s= DateAdd(d,-7,GetUTCDate());  
SET @e= GETUTCDATE();  
SELECT resource_name, AVG(avg_cpu_percent) AS Average_Compute_Utilization   
FROM sys.server_resource_stats   
WHERE start_time BETWEEN @s AND @e  
GROUP BY resource_name  
HAVING AVG(avg_cpu_percent) >= 80  
```  
    
## <a name="see-also"></a>Voir aussi  
 [Niveaux de service Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance#managed-instance-service-tiers)
