---
title: sys. sp_rda_get_rpo_duration (Transact-SQL) | Microsoft Docs
description: Utilisez sys. sp_rda_get_rpo_duration pour obtenir le nombre d’heures de données migrées qui SQL Server conservées dans une table de mise en lots pour restaurer entièrement la base de données Azure distante.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: language-reference
f1_keywords:
- sys.sp_rda_get_rpo_duration
- sys.sp_rda_get_rpo_duration_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_get_rpo_duration stored procedure
ms.assetid: 35882067-3072-47ff-9024-ca453c0f49a7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b3b52298861da031dc5eab6b3a32135eec9d26cb
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89538459"
---
# <a name="syssp_rda_get_rpo_duration-transact-sql"></a>sys. sp_rda_get_rpo_duration (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Obtient le nombre d’heures de données migrées que SQL Server conserve dans une table de mise en lots pour garantir une restauration complète de la base de données Azure distante, si une limite de restauration dans le temps est nécessaire. 
  
  Pour plus d’informations, consultez [Stretch Database réduit le risque de perte de données pour vos données Azure en conservant temporairement les lignes migrées](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md#stretchRPO).  
    
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
    
## <a name="syntax"></a>Syntaxe    
    
```    
    
sp_rda_get_rpo_duration @durationinhours output    
    
```    
    
## <a name="output-parameter"></a>Paramètre de sortie    
 *\@durationinhours*    
  Nombre d’heures (valeur entière non null) des données migrées que SQL Server conserve pour la base de données Stretch actuelle.    
    
## <a name="permissions"></a>Autorisations    
 Requiert db_owner autorisations.    
    
## <a name="remarks"></a>Notes    
 Modifiez la valeur en exécutant [sys. sp_rda_set_rpo_duration &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-set-rpo-duration-transact-sql.md).    
    
## <a name="see-also"></a>Voir aussi    
 [sys. sp_rda_set_rpo_duration &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-set-rpo-duration-transact-sql.md)     
 [Restaurer des bases de données Stretch (Stretch Database)](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)    
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)    
    
  
