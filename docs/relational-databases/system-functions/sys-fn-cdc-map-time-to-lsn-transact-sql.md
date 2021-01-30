---
description: sys.fn_cdc_map_time_to_lsn (Transact-SQL)
title: sys.fn_cdc_map_time_to_lsn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_cdc_map_time_to_lsn
- fn_cdc_map_time_to_lsn_TSQL
- sys.fn_cdc_map_time_to_lsn_TSQL
- fn_cdc_map_time_to_lsn
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_map_time_to_lsn
- sys.fn_cdc_map_time_to_lsn
ms.assetid: 6feb051d-77ae-4c93-818a-849fe518d1d4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6db62a3241b86bdac8dcc955aef865ef8ad99a21
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198745"
---
# <a name="sysfn_cdc_map_time_to_lsn-transact-sql"></a>sys.fn_cdc_map_time_to_lsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne la valeur du numéro séquentiel dans le journal (LSN) à partir de la colonne **start_lsn** de la table système [CDC.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) pour l’heure spécifiée. Vous pouvez utiliser cette fonction pour mapper systématiquement les plages DateTime dans la plage basée sur LSN requise par les fonctions d’énumération de capture de données modifiées [cdc.fn_cdc_get_all_changes_<capture_instance>](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md) et [CDC.fn_cdc_get_net_changes_ ](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)<capture_instance>pour retourner des modifications de données dans cette plage.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.fn_cdc_map_time_to_lsn ( '<relational_operator>', tracking_time )  
  
<relational_operator> ::=  
{  largest less than  
 | largest less than or equal  
 | smallest greater than  
 | smallest greater than or equal  
}  
```  
  
## <a name="arguments"></a>Arguments  
 **'**<relational_operator>**'** {la plus grande inférieure à la plus grande inférieure ou égale à la plus petite plus grande que la plus \| petite supérieure \| \| ou égale}  
 Est utilisé pour identifier une valeur LSN distincte dans la table **CDC.lsn_time_mapping** avec un **tran_end_time** associé qui satisfait la relation par rapport à la valeur de *tracking_time* .  
  
 *relational_operator* est **de type nvarchar (30)**.  
  
 *tracking_time*  
 Valeur datetime à mettre en correspondance. *tracking_time* est de **type DateTime**.  
  
## <a name="return-type"></a>Type de retour  
 **binary(10)**  
  
## <a name="remarks"></a>Notes  
 Pour comprendre comment l' **sys.fn_cdc_map_time_lsn** peut être utilisée pour mapper des plages DateTime à des plages LSN, examinez le scénario suivant. Supposez qu'un utilisateur souhaite extraire des données de modifications de façon quotidienne. Autrement dit, il ne souhaite extraire que les modifications pour un jour donné jusqu'à minuit compris. La limite inférieure de la plage temporelle se situerait à minuit, sans l'inclure, le jour précédent. La limite supérieure se situerait à minuit (compris) le jour donné. L’exemple suivant montre comment la fonction **sys.fn_cdc_map_time_to_lsn** peut être utilisée pour mapper systématiquement cette plage temporelle à la plage basée sur LSN requise par les fonctions d’énumération de capture de données modifiées pour retourner toutes les modifications au sein de cette plage.  
  
 `DECLARE @begin_time datetime, @end_time datetime, @begin_lsn binary(10), @end_lsn binary(10);`  
  
 `SET @begin_time = '2007-01-01 12:00:00.000';`  
  
 `SET @end_time = '2007-01-02 12:00:00.000';`  
  
 `SELECT @begin_lsn = sys.fn_cdc_map_time_to_lsn('smallest greater than', @begin_time);`  
  
 `SELECT @end_lsn = sys.fn_cdc_map_time_to_lsn('largest less than or equal', @end_time);`  
  
 `SELECT * FROM cdc.fn_cdc_get_net_changes_HR_Department(@begin_lsn, @end_lsn, 'all` `');`  
  
 L'opérateur relationnel '`smallest greater than`' est utilisé pour limiter les modifications à celles qui ont été effectuées après minuit le jour précédent. Si plusieurs entrées avec des valeurs LSN différentes partagent la valeur **tran_end_time** identifiée comme limite inférieure dans la table [CDC.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) , la fonction retourne le LSN le plus petit, garantissant ainsi que toutes les entrées sont incluses. Pour la limite supérieure, l’opérateur relationnel « `largest less than or equal to` » est utilisé pour s’assurer que la plage inclut toutes les entrées du jour, y compris celles dont la valeur **tran_end_time** est minuit. Si plusieurs entrées avec des valeurs LSN différentes partagent la valeur **tran_end_time** identifiée comme limite supérieure, la fonction retourne le LSN le plus grand, garantissant ainsi que toutes les entrées sont incluses.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant utilise la `sys.fn_cdc_map_time_lsn` fonction pour déterminer s’il existe des lignes dans la table [CDC.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) avec une valeur **tran_end_time** supérieure ou égale à minuit. Cette requête peut être utilisée pour déterminer, par exemple, si le processus de capture a déjà traité les modifications validées jusqu'à minuit le jour précédent, afin que l'extraction des données de modifications pour ce jour puisse continuer.  
  
```  
DECLARE @extraction_time datetime, @lsn binary(10);  
SET @extraction_time = '2007-01-01 12:00:00.000';  
SELECT @lsn = sys.fn_cdc_map_time_to_lsn ('smallest greater than or equal', @extraction_time);  
IF @lsn IS NOT NULL  
BEGIN  
<some action>  
END  
```  
  
## <a name="see-also"></a>Voir aussi  
 [cdc.lsn_time_mapping &#40;Transact-SQL&#41;](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md)   
 [sys.fn_cdc_map_lsn_to_time &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-map-lsn-to-time-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)  
  
  
