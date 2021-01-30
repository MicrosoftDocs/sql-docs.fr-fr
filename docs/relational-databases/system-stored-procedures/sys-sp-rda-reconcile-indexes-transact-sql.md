---
title: sys.sp_rda_reconcile_indexes (Transact-SQL) | Microsoft Docs
description: En savoir plus sur sys.sp_rda_reconcile_indexes. Consultez Comment utiliser cette procédure stockée Transact-SQL pour effectuer la mise en file d’attente d’une tâche de schéma pour rapprocher des index sur une table distante.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sp_rda_reconcile_indexes
- sp_rda_reconcile_indexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_reconcile_indexes stored procedure
ms.assetid: 96b31ab9-bf84-46d6-9990-81f5c51f885a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 28152ad8d8ed10eff6f0576a6fd9f508f0e8fc98
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211738"
---
# <a name="syssp_rda_reconcile_indexes-transact-sql"></a>sys.sp_rda_reconcile_indexes (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Met en file d’attente une tâche de schéma pour rapprocher des index sur la table distante. Une fois cette tâche terminée, la table distante a les mêmes index qui existent sur la table locale avec Stretch.  
  
 Si une autre tâche est mise en file d’attente pour rapprocher les index lorsque vous appelez **sp_rda_reconcile_indexes**, cette procédure stockée ne met pas en file d’attente une tâche dupliquée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_rda_reconcile_indexes [@objname = ] 'objname'  
  
```  
  
## <a name="arguments"></a>Arguments  
 [ @objname =] *'objname'*  
 Nom qualifié ou non qualifié de la table prenant en charge Stretch pour laquelle vous souhaitez rapprocher des index. Les guillemets sont requis uniquement si vous spécifiez un objet qualifié.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou >0 (échec)  
  
## <a name="see-also"></a>Voir aussi  
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
