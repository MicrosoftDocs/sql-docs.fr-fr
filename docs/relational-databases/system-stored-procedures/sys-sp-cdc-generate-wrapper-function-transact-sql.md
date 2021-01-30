---
description: sys.sp_cdc_generate_wrapper_function (Transact-SQL)
title: sys.sp_cdc_generate_wrapper_function (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_generate_wrapper_function_TSQL
- sp_cdc_generate_wrapper_function
- sys.sp_cdc_generate_wrapper_function_TSQL
- sys.sp_cdc_generate_wrapper_function
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_cdc_generate_wrapper_function
- sp_cdc_generate_wrapper_function
ms.assetid: 85bc086d-8a4e-4949-a23b-bf53044b925c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9f6dc9038b4f23b7e38e69555ceed06fab90ba92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205973"
---
# <a name="syssp_cdc_generate_wrapper_function-transact-sql"></a>sys.sp_cdc_generate_wrapper_function (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Génère des scripts qui permettent de créer des fonctions wrapper pour les fonctions de requête de capture de données modifiées disponibles dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'API prise en charge dans les wrappers générés permet de spécifier l'intervalle de requête en tant qu'intervalle datetime. La fonction peut ainsi être utilisée dans de nombreuses applications d'entreposage, y compris celles que développent les concepteurs de packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui utilisent la technologie de capture de données modifiées pour déterminer la charge incrémentielle.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_cdc_generate_wrapper_function  
    [ [ @capture_instance sysname = ] 'capture_instance'  
    [ , [ @closed_high_end_point = ] closed_high_end_pt  
    [ , [ @column_list = ] 'column_list'  
```  
  
```  
  
[ , [ @update_flag_list = ] 'update_flag_list'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @capture_instance =] '*capture_instance*'  
 Instance de capture pour laquelle les scripts seront générés. *capture_instance* est de **type sysname** et sa valeur par défaut est null. Si une valeur est omise ou définie explicitement sur NULL, des scripts wrapper sont générés pour toutes les instances de capture.  
  
 [ @closed_high_end_point =] *high_end_pt_flag*  
 Bit indicateur ; indique si les modifications dont l'heure de validation est égale au point de terminaison supérieur doivent être incluses dans l'intervalle d'extraction par la procédure générée. *high_end_pt_flag* est de **bits** et a une valeur par défaut de 1, qui indique que le point de terminaison doit être inclus. La valeur 0 indique que toutes les heures de validation seront strictement inférieures au point de terminaison supérieur.  
  
 [ @column_list =] '*column_list*'  
 Liste de colonnes capturées à inclure dans le jeu de résultats retourné par la fonction wrapper. *column_list* est de type **nvarchar (max)** et sa valeur par défaut est null. Lorsque la valeur NULL est spécifiée, toutes les colonnes capturées sont incluses.  
  
 [ @update_flag_list =] '*update_flag_list*'  
 Liste de colonnes incluses pour lesquelles un indicateur de mise à jour est inclus dans le jeu de résultats retourné par la fonction wrapper. *update_flag_list* est de type **nvarchar (max)** et sa valeur par défaut est null. Lorsque la valeur NULL est spécifiée, aucun indicateur de mise à jour n'est inclus.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de colonne|Description|  
|-----------------|-----------------|-----------------|  
|**function_name**|**nvarchar (145)**|Nom de la fonction générée.|  
|**create_script**|**nvarchar(max)**|Script qui crée la fonction wrapper de l'instance de capture.|  
  
## <a name="remarks"></a>Notes  
 Le script qui crée la fonction permettant d'encapsuler la requête all-changes pour une instance de capture est toujours généré. Si l'instance de capture prend en charge des requêtes net-changes, le script permettant de générer un wrapper pour cette requête est également généré.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre comment utiliser `sys.sp_cdc_generate_wrapper_function` pour créer des wrappers pour toutes les fonctions de capture de données modifiées.  
  
```  
DECLARE @wrapper_functions TABLE (  
    function_name sysname,  
    create_script nvarchar(max));  
  
INSERT INTO @wrapper_functions  
EXEC sys.sp_cdc_generate_wrapper_function;  
  
DECLARE @create_script nvarchar(max);  
DECLARE #hfunctions CURSOR LOCAL fast_forward  
FOR   
    SELECT create_script FROM @wrapper_functions;  
  
OPEN #hfunctions;  
FETCH #hfunctions INTO @create_script;  
WHILE (@@fetch_status <> -1)  
BEGIN  
    EXEC sp_executesql @create_script  
    FETCH #hfunctions INTO @create_script  
END;  
  
CLOSE #hfunctions;  
DEALLOCATE #hfunctions;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de capture des données modifiées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/change-data-capture-stored-procedures-transact-sql.md)   
 [Capture de données modifiées &#40;&#41;SSIS ](../../integration-services/change-data-capture/change-data-capture-ssis.md)  
  
  
