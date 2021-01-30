---
title: sys.sp_rda_reconcile_batch (Transact-SQL) | Microsoft Docs
description: Découvrez comment utiliser sys.sp_rda_reconcile_batch pour rapprocher l’ID de lot dans la table de SQL Server compatible Stretch avec l’ID de lot stocké dans la table Azure distante.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_reconcile_batch
- sys.sp_rda_reconcile_batch_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_reconcile_batch stored procedure
ms.assetid: 6d21eac3-7b6c-4fe0-8bc4-bf503f3948a6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 858da83b7529198da29cc5665219e9b83dfe3a41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211783"
---
# <a name="syssp_rda_reconcile_batch-transact-sql"></a>sys.sp_rda_reconcile_batch (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Rapproche l’ID de lot stocké dans la table de SQL Server avec Stretch avec l’ID de lot stocké dans la table Azure distante.  
  
 En règle générale, vous devez uniquement exécuter **sp_rda_reconcile_batch** si vous avez supprimé manuellement les données migrées les plus récentes de la table distante. Lorsque vous supprimez manuellement les données distantes qui incluent le lot le plus récent, les ID de lot ne sont pas synchronisés et la migration s’arrête.  
 
 Pour supprimer des données qui ont déjà été migrées vers Azure, consultez les notes sur cette page.
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
   
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_rda_reconcile_batch @objname = '@objname'  
  
```  
  
## <a name="arguments"></a>Arguments  
 \@objname = '*\@ objname*'  
 Nom de la table de SQL Server prenant en charge Stretch.  
  
## <a name="permissions"></a>Autorisations  
 Requiert les autorisations db_owner.  
  
## <a name="remarks"></a>Notes  
 Si vous souhaitez supprimer des données qui ont déjà été migrées vers Azure, effectuez les opérations suivantes.  
  
1.  Suspendre la migration des données. Pour plus d’informations, consultez [Suspension et reprise de la migration de données &#40;Stretch Database&#41;](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).  
  
2.  Supprimez les données de la table de mise en lots SQL Server en exécutant une commande DELETE avec l’indicateur STAGE_ONLY. Pour plus d’informations, consultez [effectuer des mises à jour et des suppressions d’administration](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md#adminHints).
  
3.  Supprimez les mêmes données de la table Azure distante en exécutant une commande DELETE avec l’indicateur REMOTE_ONLY.  
  
4.  Exécutez **sp_rda_reconcile_batch**.  
  
5.  Reprendre la migration des données. Pour plus d’informations, consultez [Suspension et reprise de la migration de données &#40;Stretch Database&#41;](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).  
  
## <a name="example"></a>Exemple  
 Pour rapprocher les ID de lot, exécutez l’instruction suivante.  
  
```sql  
EXEC sp_rda_reconcile_batch @objname = N'StretchEnabledTableName';  
```  
  
  
