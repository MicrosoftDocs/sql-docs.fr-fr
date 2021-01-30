---
description: managed_backup.fn_get_health_status managed_backup (Transact-SQL)
title: managed_backup managed_backup.fn_get_health_status (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_get_health_status_TSQL
- smart_admin.fn_get_health_status_TSQL
- smart_admin.fn_get_health_status
- fn_get_health_status
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_get_health_status
- fn_get_health_status
ms.assetid: b376711d-444a-4b5e-b483-8df323b4e31f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b139114cb7be4094c652f1fbf69313d4f7f1f2db
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207398"
---
# <a name="managed_backupfn_get_health_status-transact-sql"></a>managed_backup.fn_get_health_status managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Retourne une table de 0, d'une ou plusieurs lignes de nombre agrégé des erreurs signalées par des Événements étendus pour une période donnée.  
  
 La fonction est utilisée pour signaler l’état d’intégrité des services sous Smart admin.  Actuellement [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pris en charge dans le cadre de Smart admin. Ainsi, les erreurs retournées sont liées à la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
managed_backup.fn_get_health_status([@begin_time = ] 'time_1' , [ @end_time = ] 'time_2')  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Arguments  
 [@begin_time]  
 Début de la période à partir de laquelle le nombre agrégé des erreurs est calculé.  Le @begin_time paramètre est DateTime. La valeur par défaut est NULL. Lorsque la valeur est NULL, la fonction traite les événements signalés 30 minutes avant l'heure actuelle.  
  
 [ @end_time]  
 Fin de la période à partir de laquelle le nombre agrégé des erreurs est calculé. Le @end_time  paramètre est DateTime avec NULL comme valeur par défaut. Lorsque la valeur est NULL, la fonction traite les événements étendus jusqu'à l'heure actuelle.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|number_of_storage_connectivity_errors|int|Nombre d’erreurs de connexion lorsque le programme se connecte au compte de stockage Azure.|  
|number_of_sql_errors|int|Nombre d'erreurs retourné lorsque le programme se connecte au moteur SQL Server.|  
|number_of_invalid_credential_errors|int|Nombre d'erreurs retourné lorsque le programme tente de s'authentifier en utilisant les informations d'identification SQL.|  
|number_of_other_errors|int|Nombre d'erreurs dans des catégories autres que la connectivité, SQL ou les informations d'identification.|  
|number_of_corrupted_or_deleted_backups|int|Nombre de fichiers de sauvegarde supprimés ou endommagés.|  
|number_of_backup_loops|int|Nombre de fois où l'agent de sauvegarde analyse toutes les bases de données configurées avec la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|number_of_retention_loops|int|Nombre de fois où les bases de données sont analysées pour évaluer la période de rétention définie.|  
  
## <a name="best-practices"></a>Bonnes pratiques  
 Ces nombres agrégés peuvent servir à surveiller l'intégrité du système. Par exemple, si la colonne number_ of_retention_loops indique 0 pour 30 minutes, il est possible que la gestion de la rétention soit trop longue, ou ne fonctionne pas correctement. Les colonnes d’erreur non null peuvent indiquer des problèmes et les journaux des événements étendus doivent être vérifiés pour en savoir plus sur les problèmes éventuels. Vous pouvez également utiliser la procédure stockée **managed_backup. sp_get_backup_diagnostics** pour obtenir la liste des événements étendus afin de trouver les détails de l’erreur.  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert des autorisations **Select** sur la fonction.  
  
## <a name="examples"></a>Exemples  
  
-   L'exemple suivant retourne le nombre agrégé des erreurs au cours des 30 dernières minutes d'exécution.  
  
    ```  
    SELECT *  
    FROM managed_backup.fn_get_health_status(NULL, NULL)  
  
    ```  
  
-   L'exemple suivant retourne le nombre agrégé des erreurs pour la semaine en cours :  
  
    ```  
    Use msdb  
    Go  
    DECLARE @startofweek datetime  
    DECLARE @endofweek datetime  
    SET @startofweek = DATEADD(Day, 1-DATEPART(WEEKDAY, CURRENT_TIMESTAMP), CURRENT_TIMESTAMP)   
    SET @endofweek = DATEADD(Day, 7-DATEPART(WEEKDAY, CURRENT_TIMESTAMP), CURRENT_TIMESTAMP)  
    SELECT *  
    FROM managed_backup.fn_get_health_status(@startofweek, @endofweek)  
  
    ```  
  
  
