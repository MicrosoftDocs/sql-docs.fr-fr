---
description: sp_column_privileges_ex (Transact-SQL)
title: sp_column_privileges_ex (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_column_privileges_ex
- sp_column_privileges_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_column_privileges_ex
ms.assetid: 98cb6e58-4007-40fc-b048-449fb2e7e6be
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c4ce393362851e17471829272544df86840153fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174512"
---
# <a name="sp_column_privileges_ex-transact-sql"></a>sp_column_privileges_ex (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne les privilèges de colonne de la table spécifiée sur le serveur lié spécifié.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_column_privileges_ex [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_catalog' ]   
     [ , [ @column_name = ] 'column_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @table_server = ] 'table_server'` Nom du serveur lié pour lequel des informations doivent être retournées. *table_server* est de **type sysname**, sans valeur par défaut.  
  
`[ @table_name = ] 'table_name'` Nom de la table qui contient la colonne spécifiée. *table_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @table_schema = ] 'table_schema'` Est le schéma de la table. *TABLE_SCHEMA* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @table_catalog = ] 'table_catalog'` Nom de la base de données dans laquelle réside le *table_name* spécifié. *TABLE_CATALOG* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @column_name = ] 'column_name'` Nom de la colonne pour laquelle des informations de privilège doivent être fournies. *column_name* est de **type sysname**, avec NULL comme valeur par défaut (tous communs).  
  
## <a name="result-sets"></a>Jeux de résultats  
 Le tableau suivant présente les colonnes du jeu de résultats. Les résultats retournés sont triés par **TABLE_QUALIFIER**, **TABLE_OWNER**, **table_name**, **column_name** et **privilège**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|Nom du qualificateur de la table. Divers produits SGBD prennent en charge les noms de tables en trois parties (_qualificateur_**.** _propriétaire_**.** _nom_). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette colonne représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table. Ce champ peut contenir la valeur NULL.|  
|**TABLE_SCHEM**|**sysname**|Nom du propriétaire de la table. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de l'utilisateur de la base de données qui a créé la table. Ce champ retourne toujours une valeur.|  
|**TABLE_NAME**|**sysname**|Nom de la table. Ce champ retourne toujours une valeur.|  
|**COLUMN_NAME**|**sysname**|Nom de colonne, pour chaque colonne de la **table_name** retournée. Ce champ retourne toujours une valeur.|  
|**GRANTOR**|**sysname**|Nom d’utilisateur de base de données qui a accordé des autorisations sur cet **column_name** au **bénéficiaire** mentionné. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette colonne est toujours identique à la **TABLE_OWNER**. Ce champ retourne toujours une valeur.<br /><br /> La colonne **GRANTOR** peut être soit le propriétaire de la base de données (**TABLE_OWNER**), soit une personne à laquelle le propriétaire de la base de données a accordé des autorisations à l’aide de la clause with Grant option de l’instruction GRANT.|  
|**GRANTEE**|**sysname**|Nom d’utilisateur de base de données auquel des autorisations ont été accordées sur cette **column_name** par le fournisseur de **la liste.** Ce champ retourne toujours une valeur.|  
|**LIMITÉS**|**varchar (** 32 **)**|L'une des autorisations sur les colonnes disponibles. Les autorisations relatives aux colonnes peuvent prendre l'une des valeurs suivantes (ou d'autres valeurs prises en charge par la source des données si leur implémentation est définie) :<br /><br /> SELECT = **GRANTEE** peut récupérer des données pour les colonnes.<br /><br /> INSERT = **GRANTEE** peut fournir des données pour cette colonne lorsque de nouvelles lignes sont insérées (par le **bénéficiaire**) dans la table.<br /><br /> UPDATE = **GRANTEE** peut modifier des données existantes dans la colonne.<br /><br /> REFERENCEs = **GRANTEE** peut faire référence à une colonne d’une table étrangère dans une relation clé primaire/clé étrangère. Les relations clé primaire/clé étrangère sont définies grâce à des contraintes portant sur les tables.|  
|**IS_GRANTABLE**|**varchar (** 3 **)**|Indique si le **bénéficiaire** est autorisé à accorder des autorisations à d’autres utilisateurs (souvent appelés autorisations « accorder avec Grant »). Les valeurs possibles sont YES, NO ou NULL. Une valeur inconnue ou NULL renvoie à une source de données où le « droit d'accorder » ne s'applique pas.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne les informations de privilèges de colonne relatives à la table `HumanResources.Department` de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] située sur le serveur lié `Seattle1`.  
  
```  
EXEC sp_column_privileges_ex @table_server = 'Seattle1',   
   @table_name = 'Department',   
   @table_schema = 'HumanResources',  
   @table_catalog ='AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_table_privileges_ex &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-table-privileges-ex-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
