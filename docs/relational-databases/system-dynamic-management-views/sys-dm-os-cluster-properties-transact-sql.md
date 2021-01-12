---
description: sys.dm_os_cluster_properties (Transact-SQL)
title: sys.dm_os_cluster_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_cluster_properties_TSQL
- sys.dm_os_cluster_properties
- dm_os_cluster_properties_TSQL
- dm_os_cluster_properties
dev_langs:
- TSQL
helpviewer_keywords:
- dm_os_cluster_properties
- sys.dm_os_cluster_properties
ms.assetid: 6d82e770-fba7-49e0-9a0c-3b34b393e4a7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 98e5e9b90449836a4370771c35e12ea82007aed6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099749"
---
# <a name="sysdm_os_cluster_properties-transact-sql"></a>sys.dm_os_cluster_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne avec les paramètres actuels des propriétés de ressource de cluster [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identifiées dans cette rubrique. Aucune donnée n'est retournée si cette vue est exécutée sur une instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Ces propriétés sont utilisées pour définir les valeurs qui ont une incidence sur la détection des pannes, le temps de réponse en cas d'échec et la journalisation de la surveillance de l'état d'intégrité de l'instance du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  

|Nom de la colonne|Propriété|Description|  
|-----------------|--------------|-----------------|  
|VerboseLogging|bigint|Niveau de journalisation pour le cluster de basculement SQL Server. La journalisation détaillée peut être activée pour fournir des détails supplémentaires dans les journaux des erreurs à des fins de dépannage. Une des valeurs suivantes :<br /><br /> 0 - La journalisation est désactivée (valeur par défaut)<br /><br /> 1 - Erreurs uniquement<br /><br /> 2 - Erreurs et avertissements<br /><br /> Pour plus d’informations, consultez [ALTER Server CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md).|  
|SqlDumperDumpFlags|bigint|Les indicateurs de vidage SQLDumper déterminent le type de fichiers dump généré par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le paramètre par défaut est 0.|  
|SqlDumperDumpPath|nvarchar(260)|Emplacement où l'utilitaire SQLDumper génère les fichiers dump.|  
|SqlDumperDumpTimeOut|bigint|Valeur du délai d'attente, en millisecondes, nécessaire à l'utilitaire SQLDumper pour générer un vidage en cas d'échec de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La valeur par défaut est 0.|  
|FailureConditionLevel|bigint|Définit les conditions dans lesquelles le cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit échouer ou redémarrer. La valeur par défaut est 3. Pour obtenir une explication détaillée ou pour modifier les paramètres de propriété, consultez [configurer les paramètres de propriété FailureConditionLevel](../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md).|  
|HealthCheckTimeout|bigint|Valeur du délai d'attente qui définit la durée pendant laquelle la DLL de ressource du moteur de base de données SQL Server doit attendre les informations d'intégrité du serveur avant de considérer que l'instance de SQL Server ne répond pas. Valeur de délai d'attente, exprimée en millisecondes. La valeur par défaut est 60000. Pour plus d’informations ou pour modifier ce paramètre de propriété, consultez [configurer les paramètres de propriété HealthCheckTimeout](../../sql-server/failover-clusters/windows/configure-healthchecktimeout-property-settings.md).|  
  
## <a name="permissions"></a>Autorisations  
 Requiert les autorisations VIEW SERVER STATE sur l'instance de cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise sys.dm_os_cluster_properties pour renvoyer les paramètres de propriétés pour la ressource du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
SELECT VerboseLogging, SqlDumperDumpFlags, SqlDumperDumpPath, SqlDumperDumpTimeOut, FailureConditionLevel, HealthCheckTimeout  
FROM sys.dm_os_cluster_properties;  
```  
  
 Un exemple d'ensemble de résultats est présenté ci-dessous.  
  
|VerboseLogging|SqlDumperDumpFlags|SqlDumperDumpPath|SqlDumperDumpTimeOut|FailureConditionLevel|HealthCheckTimeout|  
|--------------------|------------------------|-----------------------|--------------------------|---------------------------|------------------------|  
|0|0|NULL|0|3|60000|  
  
  
