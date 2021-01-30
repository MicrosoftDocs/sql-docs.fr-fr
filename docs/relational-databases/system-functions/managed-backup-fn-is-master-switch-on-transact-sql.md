---
description: managed_backup.fn_is_master_switch_on managed_backup (Transact-SQL)
title: managed_backup managed_backup.fn_is_master_switch_on (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_is_master_switch_on
- fn_is_master_switch_on_TSQL
- smart_admin.fn_is_master_switch_on
- smart_admin.fn_is_master_switch_on_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_is_master_switch_on
- fn_is_master_switch_on
ms.assetid: e8c2108d-b104-46cb-9645-a15f46112c86
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 32395235a4d644814c7ff090fd74abc9030b2eda
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207377"
---
# <a name="managed_backupfn_is_master_switch_on-transact-sql"></a>managed_backup.fn_is_master_switch_on managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Retourne l'état des opérations de [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] sur l'instance de SQL Server.  
  
 Utilisez cette fonction pour obtenir l'état actuel de la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
managed_backup.fn_is_master_switch_on ()  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Arguments  
 Aucun  
  
## <a name="return-type"></a>Type de retour  
 **BIT**  
  
 1 = la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est active, 0 = la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] est interrompue.  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert des autorisations SELECT sur la fonction.  
  
## <a name="see-also"></a> Voir aussi  
 [Sauvegarde managée de SQL Server vers Microsoft Azure](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)  
  
  
