---
title: sys. sp_rda_set_rpo_duration (Transact-SQL) | Microsoft Docs
description: En savoir plus sur sys. sp_rda_set_rpo_duration. Utilisez cette procédure stockée pour définir le nombre d’heures de données migrées que SQL Server conserve dans une table de mise en lots.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: language-reference
f1_keywords:
- sys.sp_rda_set_rpo_duration
- sys.sp_rda_set_rpo_duration_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_set_rpo_duration stored procedure
ms.assetid: 95c80c5b-9252-4612-9ea7-544c48834fd2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d37717f2b2c8a3dad804538a9268e64023776422
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89540443"
---
# <a name="syssp_rda_set_rpo_duration-transact-sql"></a>sys. sp_rda_set_rpo_duration (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Définit le nombre d’heures de données migrées que SQL Server conserve dans une table de mise en lots pour garantir une restauration complète de la base de données Azure distante, si une limite de restauration dans le temps est nécessaire.    
    
 Pour plus d’informations, consultez [Stretch Database réduit le risque de perte de données pour vos données Azure en conservant temporairement les lignes migrées](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md#stretchRPO).  
   
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
     
## <a name="syntax"></a>Syntaxe    
    
```    
    
sp_rda_set_rpo_duration [ @duration_hrs = ] duration_hrs    
    
```    
    
## <a name="arguments"></a>Arguments    
 [ @duration_hrs =] *duration_hrs*    
 Nombre d’heures (valeur entière non null) des données migrées que vous souhaitez SQL Server conserver pour la base de données Stretch actuelle. La valeur par défaut et la valeur minimale est de 8 heures.    
 
 > [!NOTE]
 > Des valeurs plus élevées requièrent plus d’espace de stockage sur SQL Server.
    
## <a name="permissions"></a>Autorisations    
 Requiert db_owner autorisations.    
    
## <a name="remarks"></a>Notes    
 Récupérez la valeur actuelle en exécutant [sys. sp_rda_get_rpo_duration &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-get-rpo-duration-transact-sql.md).    
    
## <a name="see-also"></a>Voir aussi    
 [sys. sp_rda_get_rpo_duration &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-get-rpo-duration-transact-sql.md)     
 [Restaurer des bases de données Stretch (Stretch Database)](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)     
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)    
    
  
