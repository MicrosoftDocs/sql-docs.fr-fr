---
description: cdc.fn_cdc_get_net_changes_&lt;capture_instance&gt; (Transact-SQL)
title: cdc.fn_cdc_get_net_changes_ &lt; capture_instance &gt; (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_net_changes_<capture_instance>
- change data capture [SQL Server], querying metadata
- cdc.fn_cdc_get_net_changes_<capture_instance>
ms.assetid: 43ab0d1b-ead4-471c-85f3-f6c4b9372aab
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1b16d1da947970652be4879880ce1cfccf7ca796
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196211"
---
# <a name="cdcfn_cdc_get_net_changes_ltcapture_instancegt-transact-sql"></a>cdc.fn_cdc_get_net_changes_&lt;capture_instance&gt; (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne de modification nette pour chaque ligne source modifiée dans la plage de numéros séquentiels dans le journal (LSN) spécifiée.  
  
 **Wait, qu’est-ce qu’un LSN ?** Chaque enregistrement dans le [Journal des transactions SQL Server](../logs/the-transaction-log-sql-server.md) est identifié de manière unique par un numéro séquentiel dans le journal (LSN). Les LSN sont ordonnés de telle sorte que si LSN2 est supérieur à LSN1, la modification décrite par l’enregistrement de journal référencé par LSN2 s’est produite **après** la modification décrite par le LSN d’enregistrement de journal.  
  
 Le LSN d’un enregistrement de journal dans lequel un événement significatif s’est produit peut être utile pour construire des séquences de restauration correctes. Étant donné que les LSN sont ordonnés, vous pouvez les comparer à l’égalité et à l’inégalité (autrement dit, \<, > , =, \<=, > =). Ces comparaisons sont utiles pour créer des séquences de restauration.  
  
 Lorsqu’une ligne source a plusieurs modifications au cours de la plage du LSN, une seule ligne qui reflète le contenu final de la ligne est retournée par la fonction d’énumération décrite ci-dessous. Par exemple, si une transaction insère une ligne dans la table source et qu’une transaction ultérieure dans la plage LSN met à jour une ou plusieurs colonnes de cette ligne, la fonction ne retourne qu' **une** seule ligne, qui comprend les valeurs de colonne mises à jour.  
  
 Cette fonction d'énumération est créée lorsqu'une table source est activée pour la capture des données modifiées et que le suivi net est spécifié. Pour activer le suivi net, la table source doit avoir une clé primaire ou un index unique. Le nom de fonction est dérivé et utilise le format cdc.fn_cdc_get_net_changes_ *capture_instance*, où *capture_instance* est la valeur spécifiée pour l’instance de capture lorsque la table source a été activée pour la capture de données modifiées. Pour plus d’informations, consultez [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
cdc.fn_cdc_get_net_changes_capture_instance ( from_lsn , to_lsn , '<row_filter_option>' )  
  
<row_filter_option> ::=  
{ all  
 | all with mask  
 | all with merge  
}  
```  
  
## <a name="arguments"></a>Arguments  
 *from_lsn*  
 Numéro séquentiel dans le journal qui représente le point de terminaison inférieur de la plage de numéros séquentiels dans le journal à inclure dans le jeu de résultats. *from_lsn* est **de type binaire (10)**.  
  
 Seules les lignes du [CDC. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) table de modifications avec une valeur dans _ _ $ start_lsn supérieure ou égale à *from_lsn* sont incluses dans le jeu de résultats.  
  
 *to_lsn*  
 Numéro séquentiel dans le journal qui représente le point de terminaison supérieur de la plage de numéros séquentiels dans le journal à inclure dans le jeu de résultats. *to_lsn* est **de type binaire (10)**.  
  
 Seules les lignes du [CDC. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) table de modifications dont la valeur est comprise dans _ _ $ start_lsn inférieure ou égale à *from_lsn* ou égale à *to_lsn* sont incluses dans le jeu de résultats.  
  
 *<row_filter_option>* :: = {All | all with mask | all with Merge}  
 Option qui régit le contenu des colonnes de métadonnées aussi bien que les lignes retournées dans le jeu de résultats. Il peut s'agir de l'une des options suivantes :  
  
 all  
 Retourne le LSN de la dernière modification apportée à la ligne et l’opération nécessaire pour appliquer la ligne dans les colonnes de métadonnées _ _ start_lsn et \_ \_ $operation. La colonne \_ \_ $Update _MASK a toujours la valeur null.  
  
 all with mask  
 Retourne le LSN de la dernière modification apportée à la ligne et l’opération nécessaire pour appliquer la ligne dans les colonnes de métadonnées _ _ start_lsn et \_ \_ $operation. En outre, lorsqu’une opération de mise à jour retourne ( \_ \_ $operation = 4), les colonnes capturées modifiées dans la mise à jour sont marquées dans la valeur retournée dans \_ \_ $Update _MASK.  
  
 all with merge  
 Retourne le numéro séquentiel dans le journal de la modification finale apportée à la ligne dans les colonnes de métadonnées __$start_lsn. La colonne \_ \_ $operation aura l’une des deux valeurs suivantes : 1 pour suppression et 5 pour indiquer que l’opération nécessaire pour appliquer la modification est une insertion ou une mise à jour. La colonne \_ \_ $Update _MASK a toujours la valeur null.  
  
 Dans la mesure où la logique permettant de déterminer l'opération précise pour une modification donnée augmente la complexité de la requête, cette option est conçue pour améliorer le performances des requêtes lorsqu'il suffit d'indiquer que l'opération nécessaire pour appliquer la donnée modifiée est une insertion ou une mise à jour, mais il n'est pas utile de différencier explicitement les deux. Cette option est très intéressante dans les environnements cibles où une opération de fusion est disponible directement, tel qu'un environnement [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|__$start_lsn|**binary(10)**|Numéro séquentiel dans le journal associé à la transaction de validation de la modification.<br /><br /> Toutes les modifications validées dans la même transaction partagent le même numéro séquentiel dans le journal de validation. Par exemple, si une opération de mise à jour sur la table source modifie deux colonnes sur deux lignes, la table de modifications contiendra quatre lignes, chacune avec le même _ _ $ start_lsnvalue.|  
|__$operation|**int**|Identifie l'opération du langage de manipulation de données permettant d'appliquer la ligne de données de modification à la source de données cible.<br /><br /> Si la valeur du paramètre row_filter_option est tout ou tout avec le masque, la valeur dans cette colonne peut être l'une des valeurs suivantes :<br /><br /> 1 = suppression<br /><br /> 2 = insertion<br /><br /> 4 = mise à jour<br /><br /> Si la valeur du paramètre row_filter_option est tout ou tout avec fusion, la valeur dans cette colonne peut être l'une des suivantes :<br /><br /> 1 = suppression|  
|__$update_mask|**varbinary(128)**|Masque de bits avec un bit correspondant à chaque colonne capturée identifiée pour l'instance de capture. Tous les bits définis de cette valeur ont la valeur 1 lorsque __$operation = 1 ou 2. Lorsque \_ \_ $operation = 3 ou 4, seuls les bits correspondant aux colonnes qui ont changé ont la valeur 1.|  
|*\<captured source table columns>*|varie|Les colonnes restantes retournées par la fonction sont les colonnes de la table source qui ont été identifiées comme colonnes capturées lorsque l'instance de capture a été créée. Si aucune colonne n'a été spécifiée dans la liste des colonnes capturées, toutes les colonnes de la table source sont retournées.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle serveur fixe sysadmin ou au rôle de base de données fixe db_owner. Pour tous les autres utilisateurs, requiert l'autorisation SELECT sur toutes les colonnes capturées dans la table source et, si un rôle de régulation pour l'instance de capture a été défini, l'appartenance à ce rôle de base de données. Lorsque l'appelant n'a pas l'autorisation de consulter les données sources, la fonction retourne l'erreur 208 (nom d'objet non valide).  
  
## <a name="remarks"></a>Notes  
 Si la plage spécifiée de numéro séquentiel dans le journal ne se situe pas dans la chronologie de suivi des modifications pour l'instance de capture, la fonction retourne l'erreur 208 (nom d'objet non valide).

 En cas de modification de l’identificateur unique d’une ligne, fn_cdc_get_net_changes affiche la commande de mise à jour initiale avec une commande DELETE, puis INSERT à la place.  Ce comportement est nécessaire pour effectuer le suivi de la clé à la fois avant et après la modification.
  
## <a name="examples"></a>Exemples  
 L’exemple suivant utilise la fonction `cdc.fn_cdc_get_net_changes_HR_Department` pour signaler les modifications nettes apportées à la table source `HumanResources.Department` pendant un intervalle de temps spécifique.  
  
 En premier lieu, la fonction `GETDATE` est utilisée pour marquer le début de l'intervalle de temps. Après avoir appliqué à la table source plusieurs instructions DML, la fonction `GETDATE` est rappelée pour identifier la fin de l'intervalle de temps. La fonction [sys.fn_cdc_map_time_to_lsn](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md) est ensuite utilisée pour mapper l’intervalle de temps à une plage de requêtes de capture de données modifiées délimitée par des valeurs LSN. Enfin, la fonction `cdc.fn_cdc_get_net_changes_HR_Department` est interrogée pour obtenir les modifications nettes apportées à la table source pendant l'intervalle de temps. Remarquez que la ligne qui est insérée, puis supprimée n'apparaît pas dans le jeu de résultats retourné par la fonction. En effet, une ligne qui est d'abord ajoutée, puis supprimée dans une fenêtre de requête ne produit aucune modification nette dans la table source pendant l'intervalle. Avant d’exécuter cet exemple, vous devez d’abord exécuter l’exemple B dans [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @begin_time datetime, @end_time datetime, @from_lsn binary(10), @to_lsn binary(10);  
-- Obtain the beginning of the time interval.  
SET @begin_time = GETDATE() -1;  
-- DML statements to produce changes in the HumanResources.Department table.  
INSERT INTO HumanResources.Department (Name, GroupName)  
VALUES (N'MyDept', N'MyNewGroup');  
  
UPDATE HumanResources.Department  
SET GroupName = N'Resource Control'  
WHERE GroupName = N'Inventory Management';  
  
DELETE FROM HumanResources.Department  
WHERE Name = N'MyDept';  
  
-- Obtain the end of the time interval.  
SET @end_time = GETDATE();  
-- Map the time interval to a change data capture query range.  
SET @from_lsn = sys.fn_cdc_map_time_to_lsn('smallest greater than or equal', @begin_time);  
SET @to_lsn = sys.fn_cdc_map_time_to_lsn('largest less than or equal', @end_time);  
  
-- Return the net changes occurring within the query window.  
SELECT * FROM cdc.fn_cdc_get_net_changes_HR_Department(@from_lsn, @to_lsn, 'all');  
```  
  
## <a name="see-also"></a>Voir aussi  
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [sys.fn_cdc_map_time_to_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md)   
 [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [À propos de la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
