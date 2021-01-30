---
description: sp_syscollector_update_collection_item (Transact-SQL)
title: sp_syscollector_update_collection_item (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_update_collection_item
- sp_syscollector_update_collection_item_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data collector [SQL Server], stored procedures
- sp_syscollector_update_collection_item
ms.assetid: 7a0d36c8-c6e9-431d-a5a4-6c1802bce846
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f21ca6f68eed789756a7dbf57f79afe194d40dab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161918"
---
# <a name="sp_syscollector_update_collection_item-transact-sql"></a>sp_syscollector_update_collection_item (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet de modifier les propriétés d'un élément de collecte défini par l'utilisateur ou de le renommer.  
  
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syscollector_update_collection_item   
      [ [ @collection_item_id = ] collection_item_id ]  
    , [ [ @name = ] 'name' ]  
    , [ [ @new_name = ] 'new_name' ]  
    , [ [ @frequency = ] frequency ]  
    , [ [ @parameters = ] 'parameters' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @collection_item_id =] *collection_item_id*  
 Identificateur unique qui identifie l'élément de collecte. *collection_item_id* est de **type int** avec NULL comme valeur par défaut. *collection_item_id* doit avoir une valeur si le *nom* est null.  
  
 [ @name =] '*nom*'  
 Nom de l'élément de collecte. *Name* est de **type sysname** avec NULL comme valeur par défaut. le *nom* doit avoir une valeur si *collection_item_id* a la valeur null.  
  
 [ @new_name =] '*new_name*'  
 Nouveau nom pour l'élément de collecte. *new_name* est de **type sysname** et, s’il est utilisé, ne peut pas être une chaîne vide.  
  
 *new_name* doit être unique. Pour obtenir une liste de noms d'élément de collecte actuels, interrogez la vue système syscollector_collection_items.  
  
 [ @frequency =] *fréquence*  
 Fréquence (en secondes) de la collecte de données par cet élément de collecte. *Frequency* est de **type int**, avec 5 comme valeur par défaut.  
  
 [ @parameters =] '*paramètres*'  
 Paramètres d'entrée pour l'élément de collecte. *Parameters* est de type **XML** avec NULL comme valeur par défaut. Le schéma des *paramètres* doit correspondre au schéma des paramètres du type de collecteur.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Si le jeu d'éléments de collecte est défini en mode non mis en cache, la modification de la fréquence est ignorée car ce mode provoque l'exécution de la collecte et du téléchargement des données selon la planification spécifiée pour le jeu d'éléments de collecte. Pour afficher l'état du jeu d'éléments de collecte, exécutez la requête suivante. Remplacez `<collection_item_id>` par l'ID de l'élément de collecte à mettre à jour.  
  
```  
USE msdb;  
GO  
SELECT cs.collection_set_id, collection_set_uid, cs.name   
    ,'is running' = CASE WHEN is_running =  0 THEN 'No' ELSE 'Yes' END  
    ,'cache mode' = CASE WHEN collection_mode = 0 THEN 'Cached mode' ELSE 'Non-cached mode' END  
FROM syscollector_collection_sets AS cs  
JOIN syscollector_collection_items AS ci   
ON ci.collection_set_id = cs.collection_set_id  
WHERE collection_item_id = <collection_item_id>;  
```  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle de base de données fixe dc_admin ou dc_operator (avec autorisation EXECUTE) pour exécuter cette procédure. Même si dc_operator peut exécuter cette procédure stockée, les membres de ce rôle sont limités en ce qui concerne les propriétés qu'ils peuvent modifier. Les propriétés suivantes peuvent être modifiées uniquement par dc_admin :  
  
-   @new_name  
  
-   @parameters  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants sont basés sur l’élément de collecte créé dans l’exemple défini dans [sp_syscollector_create_collection_item &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-item-transact-sql.md).  
  
### <a name="a-changing-the-collection-frequency"></a>R. Modification de la fréquence de collecte  
 L'exemple suivant modifie la fréquence de collecte pour l'élément de collecte spécifié.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_update_collection_item   
@name = N'My custom TSQL query collector item',  
@frequency = 3000;  
GO  
```  
  
### <a name="b-renaming-a-collection-item"></a>B. Modification de nom d'un élément de collecte  
 L'exemple suivant renomme un élément de collecte.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_update_collection_item   
@name = N'My custom TSQL query collector item',  
@new_name = N'My modified TSQL item';  
GO  
```  
  
### <a name="c-changing-the-parameters-of-a-collection-item"></a>C. Modification des paramètres d'un élément de collecte  
 L'exemple suivant modifie les paramètres associés à l'élément de collecte. L'instruction définie dans l'attribut `<Value>` est modifiée et l'attribut `UseSystemDatabases` est défini sur False. Afin de consulter les paramètres actuels pour cet élément, interrogez la colonne de paramètres dans la vue système syscollector_collection_items. Il se peut que vous deviez modifier la valeur pour `@collection_item_id`.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_update_collection_item   
@collection_item_id = 9,   
@parameters = '  
    \<ns:TSQLQueryCollector xmlns:ns="DataCollectorType">  
        <Query>  
            <Value>SELECT * FROM sys.dm_db_index_usage_stats</Value>  
            <OutputTable>MyOutputTable</OutputTable>  
        </Query>  
        <Databases>  
            <Database> UseSystemDatabases = "false"   
                       UseUserDatabases = "true"</Database>  
        </Databases>  
    \</ns:TSQLQueryCollector>';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)   
 [sp_syscollector_create_collection_item &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-item-transact-sql.md)   
 [syscollector_collection_items &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collection-items-transact-sql.md)  
  
  
