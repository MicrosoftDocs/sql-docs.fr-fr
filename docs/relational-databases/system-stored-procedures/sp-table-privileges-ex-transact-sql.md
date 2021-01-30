---
description: sp_table_privileges_ex (Transact-SQL)
title: sp_table_privileges_ex (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_table_privileges_ex
- sp_table_privileges_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_table_privileges_ex
ms.assetid: b58d4a07-5c40-4f17-b66e-6d6b17188dda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 41c9548f0c4841b6a9c1845a22c7883cd42ad88b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203675"
---
# <a name="sp_table_privileges_ex-transact-sql"></a>sp_table_privileges_ex (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations sur les privilèges relatifs à la table spécifiée provenant du serveur lié indiqué.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_table_privileges_ex [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_catalog' ]  
     [ , [@fUsePattern =] 'fUsePattern']  
```  
  
## <a name="arguments"></a>Arguments  
`[ @table_server = ] 'table_server'` Nom du serveur lié pour lequel des informations doivent être retournées. *table_server* est de **type sysname**, sans valeur par défaut.  
  
`[ @table_name = ] 'table_name']` Nom de la table pour laquelle les informations de privilège de table sont fournies. *table_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @table_schema = ] 'table_schema'` Est le schéma de la table. Propriétaire de la table dans certains environnements SGBD. *TABLE_SCHEMA* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @table_catalog = ] 'table_catalog'` Nom de la base de données dans laquelle réside le *table_name* spécifié. *TABLE_CATALOG* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @fUsePattern = ] 'fUsePattern'` Détermine si les caractères « _ », « % », « [ » et « ] » sont interprétés comme des caractères génériques. Les valeurs valides sont 0 (critères spéciaux désactivés) et 1 (critères spéciaux activés). *fUsePattern* est de **bits**, avec 1 comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|Nom du qualificateur de la table. Divers produits SGBD prennent en charge les noms de tables en trois parties (_qualificateur_**.** _propriétaire_**.** _nom_). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette colonne représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table. Ce champ peut contenir la valeur NULL.|  
|**TABLE_SCHEM**|**sysname**|Nom du propriétaire de la table. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de l'utilisateur de la base de données qui a créé la table. Ce champ retourne toujours une valeur.|  
|**TABLE_NAME**|**sysname**|Nom de la table. Ce champ retourne toujours une valeur.|  
|**GRANTOR**|**sysname**|Nom d’utilisateur de base de données qui a accordé des autorisations sur cet **table_name** au **bénéficiaire** indiqué. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette colonne est toujours identique à la **TABLE_OWNER**. Ce champ retourne toujours une valeur. En outre, la colonne GRANTOR peut être soit le propriétaire de la base de données (**TABLE_OWNER**), soit un utilisateur auquel le propriétaire de la base de données a accordé l’autorisation à l’aide de la clause with Grant option de l’instruction GRANT.|  
|**GRANTEE**|**sysname**|Nom d’utilisateur de base de données auquel des autorisations ont été accordées sur ce **table_name** par le fournisseur de **la liste.** Ce champ retourne toujours une valeur.|  
|**LIMITÉS**|**varchar (** 32 **)**|L'une des autorisations disponibles sur la table. Les autorisations sur les tables peuvent prendre l'une des valeurs suivantes (ou d'autres valeurs prises en charge par la source des données si leur implémentation est définie) :<br /><br /> SELECT = **GRANTEE** peut récupérer des données pour une ou plusieurs colonnes.<br /><br /> INSERT = **GRANTEE** peut fournir des données pour les nouvelles lignes d’une ou plusieurs colonnes.<br /><br /> UPDATE = **GRANTEE** peut modifier des données existantes pour une ou plusieurs colonnes.<br /><br /> DELETE = **GRANTEE** peut supprimer des lignes de la table.<br /><br /> REFERENCEs = **GRANTEE** peut faire référence à une colonne d’une table étrangère dans une relation clé primaire/clé étrangère. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les relations clé primaire/clé étrangère sont définies grâce à des contraintes de table.<br /><br /> La portée de l’action donnée au **bénéficiaire** par un privilège de table spécifique dépend de la source de données. Par exemple, l’autorisation mettre à jour peut permettre au **bénéficiaire** de mettre à jour toutes les colonnes d’une table sur une source de données et **uniquement les colonnes** pour lesquelles le fournisseur d’autorisations a une autorisation de mise à jour sur une autre source de données.|  
|**IS_GRANTABLE**|**varchar (** 3 **)**|Indique si le **bénéficiaire** est autorisé à accorder des autorisations à d’autres utilisateurs. On appelle habituellement ce mécanisme « transmission des droits ». Les valeurs possibles sont YES, NO ou NULL. Une valeur inconnue (ou NULL) fait référence à une source de données où la « transmission des droits » ne s'applique pas.|  
  
## <a name="remarks"></a>Notes  
 Les résultats retournés sont triés par **TABLE_QUALIFIER**, **TABLE_OWNER**, **table_name** et **privilège**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne les informations relatives aux privilèges des tables dont le nom commence par `Product` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] résidant sur le serveur lié `Seattle1` ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est considéré comme le serveur lié).  
  
```  
EXEC sp_table_privileges_ex @table_server = 'Seattle1',   
   @table_name = 'Product%',   
   @table_schema = 'Production',  
   @table_catalog ='AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_column_privileges_ex &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-column-privileges-ex-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées de requêtes distribuées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)  
  
  
