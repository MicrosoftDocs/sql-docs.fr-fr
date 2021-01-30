---
description: Geo-Replication actif-sp_wait_for_database_copy_sync
title: sp_wait_for_database_copy_sync
titleSuffix: Azure SQL Database
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sp_wait_for_database_copy_sync_TSQL
- sp_wait_for_database_copy_sync
dev_langs:
- TSQL
helpviewer_keywords:
- sp_wait_for_database_copy_sync
ms.assetid: 7068da7f-cb74-47f2-b064-eb076a0d3885
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 3651d23116aa12e25e7f6f1441a432fdeaafe4fb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180244"
---
# <a name="active-geo-replication---sp_wait_for_database_copy_sync"></a>Geo-Replication actif-sp_wait_for_database_copy_sync
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Cette procédure aborde la relation [!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)] entre une base de données primaire et une base de données secondaire. L’appel de la **sp_wait_for_database_copy_sync** entraîne l’attente de l’application jusqu’à ce que toutes les transactions validées soient répliquées et acceptées par la base de données secondaire active. Exécutez **sp_wait_for_database_copy_sync** uniquement sur la base de données primaire.  
  
||  
|-|  
|**S'applique à**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_wait_for_database_copy_sync [ @target_server = ] 'server_name'   
     , [ @target_database = ] 'database_name'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @target_server =] 'SERVER_NAME'  
 Nom du serveur SQL Database qui héberge la base de données secondaire active. server_name est de type sysname, sans valeur par défaut.  
  
 [ @target_database = ] 'database_name'  
 Nom de la base de données secondaire active. database_name est de type sysname, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 Retourne 0 en cas de réussite ou un numéro d'erreur en cas d'échec.  
  
 Les conditions d'erreur les plus probables sont les suivantes :  
  
-   Le nom du serveur ou le nom de la base de données est manquant.  
  
-   Le lien est introuvable sur le nom du serveur ou la base de données spécifié.  
  
-   La connectivité de l'interlien est perdue. **sp_wait_for_database_copy_sync** retournera après le délai de connexion.  
  
## <a name="permissions"></a>Autorisations  
 Tout utilisateur dans la base de données primaire peut appeler cette procédure stockée système. La connexion doit être un utilisateur dans les bases de données primaire et secondaire active.  
  
## <a name="remarks"></a>Notes  
 Toutes les transactions validées avant un appel de **sp_wait_for_database_copy_sync** sont envoyées à la base de données secondaire active.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant appelle **sp_wait_for_database_copy_sync** pour s’assurer que toutes les transactions sont validées dans la base de données primaire, DB0, sont envoyées à sa base de données secondaire active sur le serveur cible ubfyu5ssyt.  
  
```  
USE db0;  
GO  
EXEC sys.sp_wait_for_database_copy_sync @target_server = N'ubfyu5ssyt1', @target_database = N'db0';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_continuous_copy_status &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-continuous-copy-status-azure-sql-database.md)   
 [Vues de gestion dynamique (DMV) de géo-réplication et fonctions &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/geo-replication-dynamic-management-views-and-functions-azure-sql-database.md)   
 [sys.dm_geo_replication_link_status](../system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md)
  
  
