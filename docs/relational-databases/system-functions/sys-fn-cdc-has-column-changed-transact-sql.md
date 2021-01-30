---
description: sys.fn_cdc_has_column_changed (Transact-SQL)
title: sys.fn_cdc_has_column_changed (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_cdc_has_column_changed_TSQL
- sys.fn_cdc_has_column_changed
- fn_cdc_has_column_changed_TSQL
- fn_cdc_has_column_changed
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_cdc_has_column_changed
- fn_cdc_has_column_changed
ms.assetid: 2b9e6278-050d-4ffc-8d1a-09606180facc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 53539981e3c24fa03cf2636fd32e40d729ded6d0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194425"
---
# <a name="sysfn_cdc_has_column_changed-transact-sql"></a>sys.fn_cdc_has_column_changed (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Identifie si le masque de mise à jour spécifié indique que la colonne spécifiée a été mise à jour dans la ligne de modification associée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.fn_cdc_has_column_changed ( 'capture_instance','column_name' , update_mask )  
```  
  
## <a name="arguments"></a>Arguments  
 **'** *capture_instance* **'**  
 Nom de l’instance de capture. *capture_instance* est de **type sysname**.  
  
 **'** *column_name* **'**  
 Colonne capturée de l'instance de capture spécifiée sur laquelle des rapports doivent être effectués. *column_name* est de **type sysname**.  
  
 *update_mask*  
 Masque qui identifie les colonnes mises à jour dans toute ligne de modification associée. *update_mask* est **de type varbinary (128)**.  
  
## <a name="return-type"></a>Type de retour  
 **bit**  
  
## <a name="remarks"></a>Notes  
 Vous pouvez utiliser cette fonction pour extraire les informations d'un masque de mise à jour retournées dans une requête pour les données de modifications. Elle est particulièrement utile lors du post-traitement du masque de mise à jour lorsque vous devez savoir si une colonne particulière dans la ligne de modification associée a été modifiée. Pour plus d’informations, consultez [À propos de la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md).  
  
 Lorsque ces informations sont renvoyées dans le cadre d’une requête de données modifiées, nous vous recommandons d’utiliser les fonctions [sys.fn_cdc_get_column_ordinal](../../relational-databases/system-functions/sys-fn-cdc-get-column-ordinal-transact-sql.md) et [sys.fn_cdc_is_bit_set](../../relational-databases/system-functions/sys-fn-cdc-is-bit-set-transact-sql.md) au lieu de cette fonction. Utilisez la fonction fn_cdc_get_column_ordinal avant d'interroger la modification de données de sorte que l'ordinal de colonne souhaité ne soit calculé qu'une seule fois. Utilisez fn_cdc_is_bit_set dans la requête afin d'extraire les informations du masque de mise à jour pour chaque ligne retournée.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle serveur fixe sysadmin ou au rôle de base de données fixe db_owner. Pour tous les autres utilisateurs, requiert l'autorisation SELECT sur toutes les colonnes capturées dans la table source et, si un rôle de régulation pour l'instance de capture a été défini, l'appartenance à ce rôle de base de données.  
  
## <a name="see-also"></a>Voir aussi  
 [CDC. &#60;capture_instance&#62;_CT &#40;Transact-SQL&#41;](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md)   
 [cdc.captured_columns &#40;Transact-SQL&#41;](../../relational-databases/system-tables/cdc-captured-columns-transact-sql.md)  
  
  
