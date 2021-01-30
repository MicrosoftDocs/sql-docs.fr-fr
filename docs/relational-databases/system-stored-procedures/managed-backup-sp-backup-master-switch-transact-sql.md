---
description: managed_backup.sp_backup_master_switch managed_backup (Transact-SQL)
title: managed_backup managed_backup.sp_backup_master_switch (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_ backup_master_switch
- smart_admin.sp_backup_master_switch
- sp_ backup_master_switch_TSQL
- smart_admin.sp_backup_master_switch_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_ backup_master_switch
- smart_admin.sp_backup_master_switch
ms.assetid: 1ed2b2b2-c897-41cc-bed5-1c6bc47b9dd2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 97202d989a21002d7f24c886378fd3f6c6b35ebd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205309"
---
# <a name="managed_backupsp_backup_master_switch-transact-sql"></a>managed_backup.sp_backup_master_switch managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Suspend ou reprend [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] .  
  
 Utilisez cette procédure stockée pour interrompre temporairement et reprendre la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. De cette façon, tous les paramètres de configuration sont conservés et pourront être appliqués à la reprise des opérations. Lorsque la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est interrompue, la période de rétention n'est pas appliquée. Cela signifie qu'il n'y a pas de vérification pour déterminer si les fichiers doivent être supprimés du stockage ou s'il existe des fichiers de sauvegarde corrompus ou une rupture dans la séquence de journaux de transactions consécutifs.  
  

  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
EXEC managed_backup.sp_backup_master_switch   
                     [@new_state = ] { 0 | 1}  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Arguments  
 @state  
 Définissez l'état de la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Le @state paramètre est de **bits**. Si la valeur 0 est définie, les opérations sont interrompues, et si la valeur est 1, l'opération reprend.  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="security"></a>Sécurité  
 Décrit les problèmes de sécurité relatifs à l'instruction qui inclut les autorisations sous la forme d'une sous-section (en-tête H3). Envisagez d'inclure d'autres sous-sections pour le chaînage des propriétés et l'audit, le cas échéant.  
  
### <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données **db_backupoperator** , avec les autorisations **ALTER ANY CREDENTIAL** et **Execute** sur **sp_delete_backuphistory** procédure stockée.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant peut être utilisé pour interrompre le service de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] sur l'instance sur laquelle il est exécuté :  
  
```  
Use msdb;  
GO  
EXEC managed_backup.sp_backup_master_switch @new_state=0;  
Go  
  
```  
  
 L’exemple suivant peut être utilisé pour reprendre [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] .  
  
```  
Use msdb;  
GO  
EXEC managed_backup.sp_backup_master_switch @new_state=1;  
Go  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Sauvegarde managée de SQL Server vers Microsoft Azure](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)  
  
  
