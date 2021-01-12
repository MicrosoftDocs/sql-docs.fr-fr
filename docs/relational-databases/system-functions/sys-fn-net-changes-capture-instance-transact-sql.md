---
description: sys.fn_net_changes_&lt;capture_instance&gt; (Transact-SQL)
title: sys.fn_net_changes_ &lt; capture_instance &gt; (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_net_changes_TSQL
- fn_net_changes_TSQL
- fn_net_changes
- sys.fn_net_changes
dev_langs:
- TSQL
helpviewer_keywords:
- fn_net_changes_<capture_instance>
- sys.fn_net_changes_<capture_instance>
ms.assetid: 342fa030-9fd9-4b74-ae4d-49f6038a5073
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: eb0c58b3544afd5fa529db0c95af7c2f6ba3e6d3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096343"
---
# <a name="sysfn_net_changes_ltcapture_instancegt-transact-sql"></a>sys.fn_net_changes_&lt;capture_instance&gt; (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Les wrappers pour les fonctions de requête des **modifications nettes** . Les scripts requis pour créer ces fonctions sont générés par la procédure stockée sys.sp_cdc_generate_wrapper_function.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn_net_changes_<capture_instance> ('start_time', 'end_time', '<row_filter_option>' )  
  
<capture_instance> ::= The name of the capture instance.  
<row_filter_option> ::=  
{ all  
  | all with mask  
  | all with merge  
}  
```  
  
## <a name="arguments"></a>Arguments  
 *heure-début*  
 Valeur **DateTime** qui représente le point de terminaison inférieur de la plage des entrées de table de modifications à inclure dans le jeu de résultats.  
  
 Seules les lignes du CDC. <capture_instance>_CT table de modifications qui ont une heure de validation associée strictement supérieure à *start_time* sont incluses dans le jeu de résultats.  
  
 Si une valeur NULL est fournie pour cet argument, le point de terminaison inférieur de la plage de requêtes correspond au point de terminaison inférieur de la plage valide pour l'instance de capture.  
  
 *heure-fin*  
 Valeur **DateTime** qui représente le point de terminaison supérieur de la plage des entrées de table de modifications à inclure dans le jeu de résultats.  
  
 Ce paramètre peut prendre l’une des deux significations, selon la valeur choisie pour @closed_high_end_point lorsque sys.sp_cdc_generate_wrapper_function est appelé pour générer le script pour créer la fonction wrapper :  
  
-   @closed_high_end_point = 1  
  
     Seules les lignes du CDC. <capture_instance>_CT table de modifications qui ont une valeur dans \_ \_ $Start _lsn et une heure de validation correspondante inférieure ou égale à **start_time** sont incluses dans le jeu de résultats.  
  
-   @closed_high_end_point = 0  
  
     Seules les lignes du CDC. <capture_instance>_CT table de modifications qui ont une valeur dans \_ \_ $Start _lsn et une heure de validation correspondante strictement inférieure à **start_time** sont incluses dans le jeu de résultats.  
  
 Si une valeur NULL est fournie pour cet argument, le point de terminaison supérieur de la plage de requêtes correspond au point de terminaison supérieur de la plage valide pour l'instance de capture.  
  
 *<row_filter_option>* :: = {All | all with mask | all with Merge}  
 Option qui régit le contenu des colonnes de métadonnées aussi bien que les lignes retournées dans le jeu de résultats. Il peut s'agir de l'une des options suivantes :  
  
 all  
 Retourne le contenu final d'une ligne modifiée dans les colonnes de contenu ainsi que l'opération requise pour appliquer la ligne dans la colonne de métadonnées __CDC_OPERATION.  
  
 all with mask  
 Retourne le contenu final de toutes les lignes modifiées dans les colonnes de contenu ainsi que l'opération nécessaire pour appliquer chaque ligne dans la colonne de métadonnées __CDC_OPERATION. Si une liste d'indicateurs de mise à jour a été spécifiée quand vous avez généré le script pour créer la fonction wrapper, cette option est requise pour remplir le masque de mise à jour.  
  
 all with merge  
 Retourne le contenu final de toutes les lignes modifiées dans les colonnes de contenu.  
  
 La colonne __CDC_OPERATION aura l'une des deux valeurs suivantes :  
  
-   D si la ligne doit être supprimée.  
  
-   M si la ligne doit être insérée ou mise à jour.  
  
 La logique permettant de déterminer si une insertion ou une mise à jour est nécessaire pour appliquer une modification à la cible ajoute une dose de complexité à la requête. Utilisez cette option pour améliorer les performances lorsqu'il n'est pas nécessaire d'effectuer la distinction entre les opérations d'insertion et de mise à jour. Cette approche est la mieux adaptée dans les environnements cibles où une opération de fusion est disponible directement, tel qu'un environnement [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de colonne|Description|  
|-----------------|-----------------|-----------------|  
|\<columns from @column_list>|**diffère**|Colonnes identifiées dans l’argument **column_list** de la sp_cdc_generate_wrapper_function quand elle est appelée pour générer le script pour créer le wrapper. Si *column_list* a la valeur null, toutes les colonnes sources suivies s’affichent dans le jeu de résultats.|  
|__CDC_OPERATION|**nvarchar (2)**|Code d'opération qui indique quelle opération est requise pour appliquer la ligne à l'environnement cible. L’opération varie en fonction de la valeur de l’argument *row_filter_option* fourni dans l’appel suivant :<br /><br /> *row_filter_option* = « All », « all with mask »<br /><br /> 'D' - opération de suppression<br /><br /> 'I' - opération d'insertion<br /><br /> 'UN' - opération de mise à jour<br /><br /> *row_filter_option* = 'all with Merge'<br /><br /> 'D' - opération de suppression<br /><br /> 'M' – opération d'insertion ou de mise à jour|  
|\<columns from @update_flag_list>|**bit**|Indicateur de bit nommé en ajoutant _uflag au nom de colonne. L’indicateur prend une valeur non NULL uniquement lorsque *row_filter_option* **= « all with mask »** et \_ _CDC_OPERATION **= « un »**. Il est défini à 1 si la colonne correspondante a été modifiée dans la fenêtre de requête. Sinon, il prend la valeur 0.|  
  
## <a name="remarks"></a>Notes  
 La fonction fn_net_changes_<capture_instance> sert de wrapper pour la fonction de requête cdc.fn_cdc_get_net_changes_<capture_instance>. La procédure stockée sys.sp_cdc_generate_wrapper est utilisée pour créer le script du wrapper.  
  
 Les fonctions wrapper ne sont pas créées automatiquement. Vous devez effectuer deux opérations pour créer des fonctions wrapper :  
  
1.  Exécuter la procédure stockée pour générer le script pour créer le wrapper.  
  
2.  Exécuter le script pour créer la fonction wrapper.  
  
 Les fonctions wrapper permettent aux utilisateurs de rechercher systématiquement les modifications qui se sont produites dans un intervalle délimité par des valeurs **DateTime** plutôt que par des valeurs LSN. Les fonctions wrapper effectuent toutes les conversions requises entre les valeurs **DateTime** fournies et les valeurs LSN nécessaires en interne en tant qu’arguments des fonctions de requête. Lorsque les fonctions wrapper sont utilisées en série pour traiter un flux de données modifiées, elles garantissent qu’aucune donnée n’est perdue ou répétée, à condition que la Convention suivante soit suivie : la @end_time valeur de l’intervalle associé à un appel est fournie en tant que @start_time valeur de l’intervalle associé à l’appel suivant.  
  
 L'utilisation du paramètre @closed_high_end_point lors de la création du script vous permet de générer des wrappers destinés à prendre en charge une limite supérieure fermée ou une limite supérieure ouverte sur la fenêtre de requête spécifiée. Autrement dit, vous pouvez décider si les entrées qui ont une heure de validation égale à la limite supérieure de l'intervalle d'extraction doivent être incluses dans l'intervalle. Par défaut, la limite supérieure est incluse.  
  
 Le jeu de résultats retourné par la fonction wrapper des **modifications nettes** retourne uniquement les colonnes suivies qui étaient dans le @column_list lorsque le wrapper a été généré. Si @column_list a la valeur NULL, toutes les colonnes sources suivies sont retournées. Les colonnes sources sont suivies d'une colonne d'opération, __CDC_OPERATION, qui est une colonne à un ou deux caractères qui identifie l'opération.  
  
 Les indicateurs binaires sont ensuite ajoutés au jeu de résultats pour chaque colonne identifiée dans le paramètre @update_flag_list . Pour le wrapper des **modifications nettes** , les indicateurs de bits sont toujours null si le @row_filter_option utilisé dans l’appel à la fonction wrapper est’all’ou’all with Merge'. Si @row_filter_option a la valeur « all with mask » et que __CDC_OPERATION a la valeur « d » ou « I », la valeur de l’indicateur est également null. Si \_ _CDC_OPERATION est’un', l’indicateur est défini sur 1 ou 0, selon que l’opération de mise à jour **nette** a entraîné ou non une modification de la colonne.  
  
 Le modèle de configuration de capture de données modifiées « instanciation CDC Wrapper TVF pour le schéma » montre comment utiliser la procédure stockée sp_cdc_generate_wrapper_function pour obtenir des scripts de création pour toutes les fonctions wrapper pour les fonctions de requête définies d’un schéma. Le modèle crée ensuite ces scripts. Pour plus d’informations sur les modèles, consultez [Explorateur](../../ssms/template/template-explorer.md)de modèles.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.sp_cdc_generate_wrapper_function &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-generate-wrapper-function-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)  
  
  
