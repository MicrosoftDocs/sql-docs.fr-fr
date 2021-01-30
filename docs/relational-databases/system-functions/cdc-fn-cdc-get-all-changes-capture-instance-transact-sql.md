---
description: cdc.fn_cdc_get_all_changes_&lt;capture_instance&gt;  (Transact-SQL)
title: cdc.fn_cdc_get_all_changes_ &lt; capture_instance &gt;  (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_all_changes_<capture_instance>
- change data capture [SQL Server], querying metadata
- cdc.fn_cdc_get_all_changes_<capture_instance>
ms.assetid: c6bad147-1449-4e20-a42e-b51aed76963c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5250335e5266212c49acc2b789479129786dccd0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196217"
---
# <a name="cdcfn_cdc_get_all_changes_ltcapture_instancegt--transact-sql"></a>cdc.fn_cdc_get_all_changes_&lt;capture_instance&gt;  (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque modification appliquée à la table source dans la plage spécifiée de numéros séquentiels dans le journal. Si une ligne source est modifiée à plusieurs reprises pendant l'intervalle, chaque modification est représentée dans le jeu de résultats retourné. En plus de retourner les données de modification, quatre colonnes de métadonnées fournissent les informations nécessaires pour appliquer les modifications à une autre source de données. Les options de filtrage de lignes régissent le contenu des colonnes de métadonnées aussi bien que les lignes retournées dans le jeu de résultats. Lorsque l'option de filtrage de lignes 'all' est spécifiée, chaque modification a exactement une ligne pour identifier la modification. Lorsque l'option 'all update old' est spécifiée, les opérations de mise à jour sont représentées sous la forme de deux lignes : une qui contient les valeurs des colonnes capturées avant la mise à jour et une autre qui contient les valeurs des colonnes capturées après la mise à jour.  
  
 Cette fonction d'énumération est créée lorsqu'une table source est activée pour la capture des données modifiées. Le nom de fonction est dérivé et utilise le format **cdc.fn_cdc_get_all_changes_**_capture_instance_ où *capture_instance* est la valeur spécifiée pour l’instance de capture lorsque la table source est activée pour la capture de données modifiées.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
cdc.fn_cdc_get_all_changes_capture_instance ( from_lsn , to_lsn , '<row_filter_option>' )  
  
<row_filter_option> ::=  
{ all  
 | all update old  
}  
```  
  
## <a name="arguments"></a>Arguments  
 *from_lsn*  
 Valeur LSN qui représente le point de terminaison inférieur de la plage de numéros séquentiels dans le journal à inclure dans le jeu de résultats. *from_lsn* est **de type binaire (10)**.  
  
 Seules les lignes du [CDC. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) table de modifications avec une valeur dans **_ _ $ start_lsn** supérieure ou égale à *from_lsn* sont incluses dans le jeu de résultats.  
  
 *to_lsn*  
 Valeur LSN qui représente le point de terminaison supérieur de la plage de numéros séquentiels dans le journal à inclure dans le jeu de résultats. *to_lsn* est **de type binaire (10)**.  
  
 Seules les lignes du [CDC. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) table de modifications avec une valeur dans **_ _ $ start_lsn** supérieure ou égale à *from_lsn* et inférieure ou égale à *to_lsn* sont incluses dans le jeu de résultats.  
  
 <row_filter_option> :: = {All | All Update Old}  
 Option qui régit le contenu des colonnes de métadonnées aussi bien que les lignes retournées dans le jeu de résultats.  
  
 Il peut s'agir de l'une des options suivantes :  
  
 all  
 Retourne toutes les modifications dans la plage spécifiée de numéro séquentiel dans le journal. Pour les modifications dues à une opération de mise à jour, cette option retourne seulement la ligne qui contient les nouvelles valeurs après que la mise à jour a été appliquée.  
  
 all update old  
 Retourne toutes les modifications dans la plage spécifiée de numéro séquentiel dans le journal. Pour les modifications dues à une opération de mise à jour, cette option retourne à la fois la ligne qui contient les valeurs de colonnes avant la mise à jour et celle qui contient les valeurs de colonnes après la mise à jour.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**__$start_lsn**|**binary(10)**|Numéro séquentiel dans le journal de validation associé à la modification qui préserve l'ordre de validation de la modification. Les modifications validées dans la même transaction partagent la même valeur LSN de validation.|  
|**__$seqval**|**binary(10)**|Valeur de classement utilisée pour classer les modifications d'une ligne dans une transaction.|  
|**__$operation**|**int**|Identifie l'opération du langage de manipulation de données permettant d'appliquer la ligne de données de modification à la source de données cible. Il peut s'agir d'une des méthodes suivantes :<br /><br /> 1 = suppression<br /><br /> 2 = insertion<br /><br /> 3 = mise à jour (les valeurs de colonne capturées sont celles avant l'opération de mise à jour). Cette valeur s'applique uniquement lorsque l'option de filtre de lignes 'all update old' est spécifiée.<br /><br /> 4 = mise à jour (les valeurs de colonne capturées sont celles après l'opération de mise à jour)|  
|**__$update_mask**|**varbinary(128)**|Masque de bits avec un bit correspondant à chaque colonne capturée identifiée pour l'instance de capture. Tous les bits définis de cette valeur sont définis sur 1 lorsque **_ _ $ Operation** = 1 ou 2. Lorsque **_ _ $ Operation** = 3 ou 4, seuls les bits correspondant aux colonnes qui ont changé ont la valeur 1.|  
|**\<captured source table columns>**|varie|Les colonnes restantes retournées par la fonction sont les colonnes capturées identifiées lorsque l'instance de capture a été créée. Si aucune colonne n'a été spécifiée dans la liste des colonnes capturées, toutes les colonnes de la table source sont retournées.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle serveur fixe **sysadmin** ou **db_owner** rôle de base de données fixe. Pour tous les autres utilisateurs, requiert l'autorisation SELECT sur toutes les colonnes capturées dans la table source et, si un rôle de régulation pour l'instance de capture a été défini, l'appartenance à ce rôle de base de données. Lorsque l’appelant n’a pas l’autorisation d’afficher les données sources, la fonction retourne l’erreur 229 (« l’autorisation SELECT a été refusée sur l’objet «fn_cdc_get_all_changes_... », la base de données « \<DatabaseName> », le schéma « cdc ».»).  
  
## <a name="remarks"></a>Notes  
 Si la plage de numéros séquentiels dans le journal spécifiée n'est pas située dans la chronologie de suivi des modifications pour l'instance de capture, la fonction retourne l'erreur 208 (« Un nombre insuffisant d'arguments a été fourni pour la procédure ou fonction cdc.fn_cdc_get_all_changes. »).  
  
 Les colonnes de type de données **image**, **Text** et **ntext** reçoivent toujours une valeur NULL lorsque **_ _ $ Operation** = 1 ou **_ _ $ Operation** = 3. Une valeur NULL est assignée aux colonnes de type de données **varbinary (max)**, **varchar (max)** ou **nvarchar (max)** quand **_ _ $ Operation** = 3, sauf si la colonne a été modifiée au cours de la mise à jour. Lorsque **_ _ $ Operation** = 1, ces colonnes sont affectées de leur valeur au moment de la suppression. Les colonnes calculées incluses dans une instance de capture ont toujours une valeur NULL.  
  
## <a name="examples"></a>Exemples  
 Plusieurs [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] modèles qui montrent comment utiliser les fonctions de requête de capture de données modifiées sont disponibles. Ces modèles sont disponibles dans le menu **affichage** de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] . Pour plus d’informations, consultez [Explorateur de modèles](../../ssms/template/template-explorer.md).  
  
 Cet exemple illustre le `Enumerate All Changes for Valid Range Template`. Elle utilise la fonction `cdc.fn_cdc_get_all_changes_HR_Department` pour signaler toutes les modifications actuellement disponibles pour l’instance de capture `HR_Department` , qui est définie pour la table source HumanResources. Department dans la [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] base de données.  
  
```  
-- ========  
-- Enumerate All Changes for Valid Range Template  
-- ========  
USE AdventureWorks2012;  
GO  
  
DECLARE @from_lsn binary(10), @to_lsn binary(10);  
SET @from_lsn = sys.fn_cdc_get_min_lsn('HR_Department');  
SET @to_lsn   = sys.fn_cdc_get_max_lsn();  
SELECT * FROM cdc.fn_cdc_get_all_changes_HR_Department  
  (@from_lsn, @to_lsn, N'all');  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [sys.fn_cdc_map_time_to_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md)   
 [sys.sp_cdc_get_ddl_history &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md)   
 [sys.sp_cdc_get_captured_columns &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-captured-columns-transact-sql.md)   
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [À propos de la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
