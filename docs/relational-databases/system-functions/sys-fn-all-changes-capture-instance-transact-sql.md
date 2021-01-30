---
description: '&lt;capture_instance sys.fn_all_changes_ &gt; (Transact-SQL)'
title: sys.fn_all_changes_ &lt; capture_instance &gt; (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_all_changes
- sys.fn_all_changes
- fn_all_changes_TSQL
- sys.fn_all_changes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_all_changes_<capture_instance>
- sys.fn_all_changes_<capture_instance>
ms.assetid: 564fae96-b88c-4f22-9338-26ec168ba6f5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a4736042dc8770660f076d3865df4033ef3233d5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194488"
---
# <a name="sysfn_all_changes_ltcapture_instancegt-transact-sql"></a>&lt;capture_instance sys.fn_all_changes_ &gt; (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Wrappers pour les fonctions de requête de **toutes les modifications** . Les scripts requis pour créer ces fonctions sont générés par la procédure stockée sys.sp_cdc_generate_wrapper_function.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn_all_changes_<capture_instance> ('start_time' ,'end_time', '<row_filter_option>' )  
  
<capture_instance> ::= The name of the capture instance.  
<row_filter_option> ::=  
{ all  
  | all update old  
}  
```  
  
## <a name="arguments"></a>Arguments  
 *heure-début*  
 Valeur **DateTime** qui représente le point de terminaison inférieur de la plage des entrées de table de modifications à inclure dans le jeu de résultats.  
  
 Seules les lignes du CDC. <capture_instance>_CT table de modifications qui ont une heure de validation associée supérieure à *start_time* sont incluses dans le jeu de résultats.  
  
 Si une valeur NULL est fournie pour cet argument, le point de terminaison inférieur de la plage de requêtes correspond au point de terminaison inférieur de la plage valide pour l'instance de capture.  
  
 *heure-fin*  
 Valeur **DateTime** qui représente le point de terminaison supérieur de la plage des entrées de table de modifications à inclure dans le jeu de résultats.  
  
 Ce paramètre peut prendre l’une des deux significations possibles, selon la valeur choisie pour @closed_high_end_point lorsque sys.sp_cdc_generate_wrapper_function est appelé pour générer le script de création pour la fonction wrapper :  
  
-   @closed_high_end_point = 1  
  
     Seules les lignes du CDC. <capture_instance>_CT table de modifications qui ont une heure de validation associée inférieure ou égale à end_time sont incluses dans le jeu de résultats.  
  
-   @closed_high_end_point = 0  
  
     Seules les lignes de la table de modifications cdc.capture_instance_CT qui ont une heure de validation associée strictement inférieure à end_time sont incluses dans le jeu de résultats.  
  
 Si une valeur NULL est fournie pour cet argument, le point de terminaison supérieur de la plage de requêtes correspond au point de terminaison supérieur de la plage valide pour l'instance de capture.  
  
 <row_filter_option> :: = {All | All Update Old}  
 Option qui régit le contenu des colonnes de métadonnées ainsi que les lignes retournées dans le jeu de résultats.  
  
 Il peut s'agir de l'une des options suivantes :  
  
 all  
 Retourne toutes les modifications dans la plage spécifiée de numéro séquentiel dans le journal. Pour les modifications résultantes d'une opération de mise à jour, cette option retourne seulement la ligne qui contient les nouvelles valeurs après l'application de la mise à jour.  
  
 all update old  
 Retourne toutes les modifications dans la plage spécifiée de numéro séquentiel dans le journal. Pour les modifications résultantes d'une opération de mise à jour, cette option retourne seulement les deux lignes qui contiennent les valeurs de colonnes avant et après la mise à jour.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de colonne|Description|  
|-----------------|-----------------|-----------------|  
|__CDC_STARTLSN|**binary(10)**|Numéro LSN de validation de la transaction associée à la modification. Toutes les modifications validées dans la même transaction partagent le même numéro LSN de validation.|  
|__CDC_SEQVAL|**binary(10)**|Valeur de classement utilisée pour classer les modifications de ligne dans une transaction.|  
|\<columns from @column_list>|**diffère**|Colonnes identifiées dans l’argument *column_list* pour sp_cdc_generate_wrapper_function quand elle est appelée pour générer le script qui crée la fonction wrapper.|  
|__CDC_OPERATION|**nvarchar (2)**|Code d'opération qui indique l'opération requise pour appliquer la ligne à l'environnement cible. Elle varie en fonction de la valeur de l’argument *row_filter_option* fourni dans l’appel :<br /><br /> *row_filter_option* = 'all'<br /><br /> 'D' - opération de suppression<br /><br /> 'I' - opération d'insertion<br /><br /> 'UN' – opération de mise à jour (nouvelles valeurs)<br /><br /> *row_filter_option* = 'All Update Old'<br /><br /> 'D' - opération de suppression<br /><br /> 'I' - opération d'insertion<br /><br /> 'UN' – opération de mise à jour (nouvelles valeurs)<br /><br /> 'UO' – opération de mise à jour (anciennes valeurs)|  
|\<columns from @update_flag_list>|**bit**|Un indicateur binaire est nommé en ajoutant _uflag au nom de la colonne. L’indicateur a toujours la valeur NULL lorsque \_ _CDC_OPERATION est’d', 'I', de’UO'. Lorsque \_ _CDC_OPERATION est’un', il prend la valeur 1 si la mise à jour a produit une modification apportée à la colonne correspondante. Sinon, il prend la valeur 0.|  
  
## <a name="remarks"></a>Notes  
 La fonction fn_all_changes_<capture_instance> sert de wrapper pour la fonction de requête cdc.fn_cdc_get_all_changes_<capture_instance>. La procédure stockée sys.sp_cdc_generate_wrapper sert à générer le script de création du wrapper.  
  
 Les fonctions wrapper ne sont pas créées automatiquement. Vous devez effectuer deux opérations pour créer des fonctions wrapper :  
  
1.  Exécuter la procédure stockée pour générer le script pour créer le wrapper.  
  
2.  Exécuter le script pour créer la fonction wrapper.  

 Les fonctions wrapper permettent aux utilisateurs de rechercher systématiquement les modifications qui se sont produites dans un intervalle délimité par des valeurs **DateTime** plutôt que par des valeurs LSN. Les fonctions wrapper effectuent toutes les conversions requises entre les valeurs **DateTime** fournies et les valeurs LSN nécessaires en interne en tant qu’arguments des fonctions de requête. Lorsque les fonctions wrapper sont utilisées en série pour traiter un flux de données modifiées, elles garantissent qu’aucune donnée n’est perdue ou répétée, à condition que la Convention suivante soit suivie : la @end_time valeur de l’intervalle associé à un appel est fournie en tant que @start_time valeur de l’intervalle associé à l’appel suivant.  
  
 L'utilisation du paramètre @closed_high_end_point lors de la création du script vous permet de générer des wrappers destinés à prendre en charge une limite supérieure fermée ou une limite supérieure ouverte sur la fenêtre de requête spécifiée. Autrement dit, vous pouvez décider si les entrées qui ont une heure de validation égale à la limite supérieure de l'intervalle d'extraction doivent être incluses dans l'intervalle. Par défaut, la limite supérieure est incluse.  
  
 Le jeu de résultats retourné par la fonction wrapper **All changes** retourne les colonnes _ _ $ start_lsn et \_ \_ $seqval de la table de modifications en tant que colonnes \_ _CDC_STARTLSN et \_ _CDC_SEQVAL, respectivement. Il les suit uniquement avec les colonnes suivies qui sont apparues dans le paramètre *\@ column_list* lors de la génération du wrapper. Si *\@ column_list* a la valeur null, toutes les colonnes sources suivies sont retournées. Les colonnes sources sont suivies d’une colonne d’opération, \_ _CDC_OPERATION, qui est une colonne à un ou deux caractères qui identifie l’opération.  
  
 Les indicateurs de bits sont ensuite rajoutés au jeu de résultats pour chaque colonne identifiée dans le paramètre @update_flag_list. Pour le wrapper de **tous les changements** , les bits indicateurs sont toujours NULL si __CDC_OPERATION est’d', 'I’ou’UO'. Si \_ _CDC_OPERATION est’un', l’indicateur est défini sur 1 ou 0, selon que l’opération de mise à jour a entraîné ou non une modification de la colonne.  
  
 Le modèle de configuration de capture de données modifiées « instanciation CDC Wrapper TVF pour le schéma » montre comment utiliser la procédure stockée sp_cdc_generate_wrapper_function pour obtenir des scripts de création pour toutes les fonctions wrapper pour les fonctions de requête définies d’un schéma. Le modèle crée ensuite ces scripts. Pour plus d’informations sur les modèles, consultez [Explorateur](../../ssms/template/template-explorer.md)de modèles.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.sp_cdc_generate_wrapper_function &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-generate-wrapper-function-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)  
  
  
