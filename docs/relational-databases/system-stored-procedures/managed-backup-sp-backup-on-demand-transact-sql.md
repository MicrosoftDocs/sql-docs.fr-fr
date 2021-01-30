---
description: managed_backup.sp_backup_on_demand managed_backup (Transact-SQL)
title: managed_backup managed_backup.sp_backup_on_demand (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- smart_admin.sp_backup_on_demand
- smart_admin.sp_backup_on_demand_TSQL
- sp_backup_on_demand_TSQL
- sp_backup_on_demand
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.sp_backup_on_demand
- sp_backup_on_demand
ms.assetid: 638f809f-27fa-4c44-a549-9cf37ecc920c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 480338a6194b07f15d8c139a6675649e5c4f8e7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205293"
---
# <a name="managed_backupsp_backup_on_demand-transact-sql"></a>managed_backup.sp_backup_on_demand managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Demande la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pour effectuer une sauvegarde de la base de données spécifiée.  
  
 Utilisez cette procédure stockée pour effectuer des sauvegardes ad hoc d'une base de données configurée avec la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Cela empêche toute interruption dans la chaîne de sauvegarde et [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] les processus sont pris en charge et la sauvegarde est stockée dans le même conteneur de stockage d’objets BLOB Azure.  
  
 À la fin de la sauvegarde, le chemin de fichier de sauvegarde complet est retourné. Il comprend le nom et l'emplacement du nouveau fichier de sauvegarde résultant de l'opération de sauvegarde.  
  
 Une erreur est retournée si la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est sur le point d'exécuter une sauvegarde d'un type donné pour une base de données spécifiée. Dans ce cas, le message d'erreur retourné comprend le chemin de fichier de sauvegarde complet où la sauvegarde est actuellement téléchargée.  
   
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
EXEC managed_backup.sp_backup_on_demand   
[@database_name  =]  'database name',[@type = ] { 'Database' | 'Log' }  
  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Arguments  
 @database_name  
 Nom de la base de données sur laquelle la sauvegarde doit être effectuée. @database_nameEst de **type sysname**.  
  
 @type  
 Type de sauvegarde à effectuer : base de données ou journal. Le @type paramètre est de type **nvarchar (32)**.  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données **db_backupoperator** , avec les autorisations **ALTER ANY CREDENTIAL** et **Execute** sur **sp_delete_backuphistory** procédure stockée.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant effectue une demande de sauvegarde de base de données pour la base de données « TestDB ». La [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est activée pour cette base de données.  
  
```  
Use MSDB  
Go  
EXEC managed_backup.sp_backup_on_demand  
 @database_name = 'TestDB'  
,@type = 'Database'  
  
```  
  
 Pour chaque extrait de code, sélectionnez « tsql » dans le champ d'attribut de langage.  
  
  
