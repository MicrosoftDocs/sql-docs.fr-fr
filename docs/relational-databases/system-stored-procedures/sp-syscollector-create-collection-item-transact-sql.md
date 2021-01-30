---
description: sp_syscollector_create_collection_item (Transact-SQL)
title: sp_syscollector_create_collection_item (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_create_collection_item
- sp_syscollector_create_collection_item_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_create_collection_item
- data collector [SQL Server], stored procedures
ms.assetid: 60dacf13-ca12-4844-b417-0bc0a8bf0ddb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e58f8407c152be375099e9eb70e3fe63d0100e67
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207039"
---
# <a name="sp_syscollector_create_collection_item-transact-sql"></a>sp_syscollector_create_collection_item (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée un élément de collecte dans un jeu d'éléments de collecte définis par l'utilisateur. Un élément de collecte définit les données à collecter et la fréquence de la collecte.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syscollector_create_collection_item   
      [ @collection_set_id = ] collection_set_id   
    , [ @collector_type_uid = ] 'collector_type_uid'  
    , [ @name = ] 'name'   
    , [ [ @frequency = ] frequency ]  
    , [ @parameters = ] 'parameters'  
    , [ @collection_item_id = ] collection_item_id OUTPUT  
```  
  
## <a name="arguments"></a>Arguments  
 [ @collection_set_id =] *collection_set_id*  
 Identificateur local unique pour le jeu d'éléments de collecte. *collection_set_id* est de **type int**.  
  
 [ @collector_type_uid =] '*collector_type_uid*'  
 GUID qui identifie le type de collecteur à utiliser pour cet élément *collector_type_uid* est de type **uniqueidentifier** et n’a pas de valeur par défaut. Pour obtenir une liste de types de collecteurs, interrogez la vue système syscollector_collector_types.  
  
 [ @name =] '*nom*'  
 Nom de l'élément de collecte. *Name* est de **type sysname** et ne peut pas être une chaîne vide ou avoir la valeur null.  
  
 le *nom* doit être unique. Pour obtenir une liste de noms d'élément de collecte actuels, interrogez la vue système syscollector_collection_items.  
  
 [ @frequency =] *fréquence*  
 Permet de spécifier (en secondes) la fréquence de collecte des données par cet élément de collecte. *Frequency* est de **type int**, avec 5 comme valeur par défaut. La valeur minimale pouvant être spécifiée est de 5 secondes.  
  
 Si le jeu d'éléments de collecte est défini en mode non mis en cache, la fréquence est ignorée car ce mode provoque l'exécution de la collecte et du téléchargement des données selon la planification spécifiée pour le jeu d'éléments de collecte. Pour afficher le mode de collecte du jeu d’ensembles de collectes, interrogez la vue système [syscollector_collection_sets](../../relational-databases/system-catalog-views/syscollector-collection-sets-transact-sql.md) .  
  
 [ @parameters =] '*paramètres*'  
 Les paramètres d'entrée pour le type de collecteur. *Parameters* est de type **XML** avec NULL comme valeur par défaut. Le schéma des *paramètres* doit correspondre au schéma des paramètres du type de collecteur.  
  
 [ @collection_item_id =] *collection_item_id*  
 Identificateur unique qui identifie l'élément du jeu d'éléments de collecte. *collection_item_id* est de **type int** et a une sortie.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 sp_syscollector_create_collection_item doit être exécuté dans le contexte de la base de données système msdb.  
  
 Le jeu d'éléments de collecte auquel l'élément de collecte est ajouté doit être arrêté avant que ce dernier ne soit créé. Il est impossible d'ajouter des éléments de collecte à des jeux d'éléments de collecte système.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle de base de données fixe dc_admin (avec autorisation EXECUTE) pour exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée un élément de collecte selon le type collecteur `Generic T-SQL Query Collector Type` et l'ajoute au jeu d'éléments de collecte appelé `Simple collection set test 2`. Pour créer le jeu d’collections spécifié, exécutez l’exemple B dans [sp_syscollector_create_collection_set &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md).  
  
```  
USE msdb;  
GO  
DECLARE @collection_item_id int;  
DECLARE @collection_set_id int = (SELECT collection_set_id   
                                  FROM syscollector_collection_sets  
                                  WHERE name = N'Simple collection set test 2');  
DECLARE @collector_type_uid uniqueidentifier =   
    (SELECT collector_type_uid  
     FROM syscollector_collector_types  
     WHERE name = N'Generic T-SQL Query Collector Type');  
DECLARE @params xml =   
    CONVERT(xml, N'\<ns:TSQLQueryCollector xmlns:ns="DataCollectorType">  
            <Query>  
                <Value>SELECT * FROM sys.objects</Value>  
                <OutputTable>MyOutputTable</OutputTable>  
            </Query>  
            <Databases>   
                <Database> UseSystemDatabases = "true"   
                           UseUserDatabases = "true"  
                </Database>  
            </Databases>  
         \</ns:TSQLQueryCollector>');  
  
EXEC sp_syscollector_create_collection_item  
    @collection_set_id = @collection_set_id,  
    @collector_type_uid = @collector_type_uid,  
    @name = 'My custom TSQL query collector item',  
    @frequency = 6000,  
    @parameters = @params,  
    @collection_item_id = @collection_item_id OUTPUT;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)   
 [sp_syscollector_update_collection_item &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-update-collection-item-transact-sql.md)   
 [sp_syscollector_delete_collection_item &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-delete-collection-item-transact-sql.md)   
 [syscollector_collector_types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collector-types-transact-sql.md)   
 [sp_syscollector_create_collection_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md)   
 [syscollector_collection_items &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collection-items-transact-sql.md)  
  
  
