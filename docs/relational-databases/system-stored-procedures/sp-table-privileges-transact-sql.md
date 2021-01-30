---
description: sp_table_privileges (Transact-SQL)
title: sp_table_privileges (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_table_privileges
- sp_table_privileges_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_table_privileges
ms.assetid: 0512e688-4fc0-4557-8dc8-016672c1e3fe
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3d0fc1dafc04241f5dd884216efbeb3fb5382f7b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183584"
---
# <a name="sp_table_privileges-transact-sql"></a>sp_table_privileges (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne la liste des autorisations de table (telles que INSERT, DELETE, UPDATE, SELECT, REFERENCES) pour la ou les tables spécifiées.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_table_privileges [ @table_name = ] 'table_name'     
     [ , [ @table_owner = ] 'table_owner' ]   
     [ , [ @table_qualifier = ] 'table_qualifier' ]   
     [ , [ @fUsePattern = ] 'fUsePattern' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @table_name =] '*table_name*'  
 Table utilisée pour retourner les informations de catalogue. *table_name* est de type **nvarchar (** 384 **)**, sans valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge.  
  
 [ @table_owner =] '*TABLE_OWNER*'  
 Propriétaire de la table utilisée pour retourner les informations de catalogue. *TABLE_OWNER* est de type **nvarchar (** 384 **)**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge. Si le propriétaire n'est pas précisé, les règles par défaut définissant la visibilité des tables du SGBD sous-jacent sont utilisées.  
  
 Si l'utilisateur actuel est propriétaire d'une table portant le nom spécifié, les colonnes de cette table sont renvoyées. Si *owner* n’est pas spécifié et que l’utilisateur actuel ne possède pas de table portant le *nom* spécifié, cette procédure recherche une table avec la *table_name* spécifiée détenue par le propriétaire de la base de données. Si la recherche de la table aboutit, ce sont les colonnes de cette dernière qui sont retournées.  
  
 [ @table_qualifier =] '*TABLE_QUALIFIER*'  
 Nom du qualificateur de la table. *TABLE_QUALIFIER* est de **type sysname**, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge les noms de table en trois parties (*qualifier.Owner.Name*). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette colonne représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table.  
  
 [ @fUsePattern =] '*fUsePattern*'  
 Détermine si les caractères de soulignement (_), de pourcentage (%) et de crochets ([ou]) sont interprétés comme des caractères génériques. Les valeurs valides sont 0 (critères spéciaux désactivés) et 1 (critères spéciaux activés). *fUsePattern* est de **bits**, avec 1 comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|TABLE_QUALIFIER|**sysname**|Nom du qualificateur de la table. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette colonne représente le nom de la base de données. Ce champ peut contenir la valeur NULL.|  
|TABLE_OWNER|**sysname**|Nom du propriétaire de la table. Ce champ retourne toujours une valeur.|  
|TABLE_NAME|**sysname**|Nom de la table. Ce champ retourne toujours une valeur.|  
|GRANTOR|**sysname**|Nom de l'utilisateur de la base de données qui a accordé des autorisations sur la table TABLE_NAME au GRANTEE répertorié. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne doit toujours être identique à TABLE_OWNER. Ce champ retourne toujours une valeur. D'autre part, la colonne GRANTOR représente soit le propriétaire de la table TABLE_OWNER, soit un utilisateur auquel le propriétaire de la base de données a accordé des autorisations à l'aide de la clause WITH GRANT OPTION de l'instruction GRANT.|  
|GRANTEE|**sysname**|Nom de l'utilisateur de la base de données qui a reçu des autorisations sur la table TABLE_NAME de l'utilisateur GRANTEE répertorié. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cette colonne contient toujours un utilisateur de base de données à partir de la vue sys.database_principalssystem. Ce champ retourne toujours une valeur.|  
|PRIVILEGE|**sysname**|L'une des autorisations disponibles sur la table. Les autorisations sur les tables peuvent prendre l'une des valeurs suivantes (ou d'autres valeurs prises en charge par la source de données si leur mise en œuvre est définie) :<br /><br /> SELECT = GRANTEE peut extraire des données pour une ou plusieurs colonnes.<br /><br /> INSERT = GRANTEE peut fournir des données à une ou plusieurs colonnes en rajoutant de nouvelles lignes.<br /><br /> UPDATE = GRANTEE peut modifier des données existantes dans une ou plusieurs colonnes.<br /><br /> DELETE = GRANTEE peut supprimer des lignes de la table.<br /><br /> REFERENCES = GRANTEE peut faire référence à une colonne d'une table étrangère dans une relation clé primaire/clé étrangère. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les relations clé primaire/clé étrangère sont définies grâce à des contraintes portant sur les tables.<br /><br /> Le rayon d'action qu'un privilège de table particulier accorde à l'utilisateur GRANTEE dépend de la source de données. Par exemple, le privilège UPDATE peut autoriser le GRANTEE à mettre à jour toutes les colonnes d'une table pour une source de données, et uniquement les colonnes pour lesquelles le GRANTOR possède le privilège UPDATE pour une autre source de données.|  
|IS_GRANTABLE|**sysname**|Indique si le GRANTEE est autorisé ou non à accorder des autorisations à d'autres utilisateurs. Il y est souvent fait référence sous le terme « transmission des droits ». Les valeurs possibles sont YES, NO ou NULL. Une valeur inconnue (ou NULL) fait référence à une source de données à laquelle la « transmission des droits » ne s'applique pas.|  
  
## <a name="remarks"></a>Notes  
 La procédure stockée sp_table_privileges est équivalente à SQLTablePrivileges dans ODBC. Les résultats retournés sont classés par TABLE_QUALIFIER, TABLE_OWNER, TABLE_NAME et PRIVILEGE.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
 Cet exemple retourne des informations de privilège sur toutes les tables dont le nom commence par le mot `Contact`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_table_privileges   
   @table_name = 'Contact%';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de catalogue &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
