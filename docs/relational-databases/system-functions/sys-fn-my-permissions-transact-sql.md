---
description: sys.fn_my_permissions (Transact-SQL)
title: sys.fn_my_permissions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_my_permissions_TSQL
- fn_my_permissions_TSQL
- sys.fn_my_permissions
- fn_my_permissions
dev_langs:
- TSQL
helpviewer_keywords:
- fn_my_permissions function
- sys.fn_my_permissions function
ms.assetid: 30f97f00-03d8-443a-9de9-9ec420b7699b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5e67f2be709f91ee3214f490cfbcc2b932ad7dc5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101323"
---
# <a name="sysfn_my_permissions-transact-sql"></a>sys.fn_my_permissions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie la liste des autorisations accordées effectivement au principal sur un élément sécurisable. Une fonction associée est [HAS_PERMS_BY_NAME](../../t-sql/functions/has-perms-by-name-transact-sql.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn_my_permissions ( securable , 'securable_class' )  
```  
  
## <a name="arguments"></a>Arguments  
 *securable*  
 Indique le nom de l'élément sécurisable. Si l'élément sécurisable est le serveur ou une base de données, cette valeur doit être définie sur NULL. *securable* est une expression scalaire de type **sysname**. l’élément *sécurisable* peut être un nom en plusieurs parties.  
  
 '*securable_class*'  
 Nom de la classe de l'élément sécurisable pour lequel les autorisations sont affichées. *securable_class* est de **type sysname**. *securable_class* doit correspondre à l’un des éléments suivants : rôle d’application, assembly, clé asymétrique, certificat, contrat, base de données, point de terminaison, catalogue de texte intégral, connexion, type de message, objet, liaison de service distant, rôle, itinéraire, schéma, serveur, service, clé symétrique, type, utilisateur, collection de schémas XML.  
  
## <a name="columns-returned"></a>Colonnes retournées  
 Le tableau suivant répertorie les colonnes retournées par **fn_my_permissions** . Chaque ligne renvoyée décrit une autorisation détenue par le contexte de sécurité actuel sur l'élément sécurisable. Renvoie NULL si la requête échoue.  
  
|Nom de la colonne|Type|Description|  
|-----------------|----------|-----------------|  
|entity_name|**sysname**|Nom de l'élément sécurisable pour lequel les autorisations affichées sont effectivement accordées.|  
|subentity_name|**sysname**|Nom de colonne si l'élément sécurisable contient des colonnes, sinon NULL.|  
|permission_name|**nvarchar**|Nom de l’autorisation.|  
  
## <a name="remarks"></a>Notes  
 Cette fonction table renvoie la liste des autorisations effectives détenues par le principal appelant sur un élément sécurisable spécifié. Une autorisation effective est l’un des éléments suivants :  
  
-   une autorisation accordée directement au principal et non refusée ;  
  
-   une autorisation impliquée par une autorisation de niveau supérieur détenue par le principal et non refusée ;  
  
-   une autorisation accordée à un rôle ou à un groupe dont le principal est membre et non refusé ;  
  
-   une autorisation détenue par un rôle ou un groupe dont le principal est membre et non refusé.  
  
 L'évaluation des autorisations a toujours lieu dans le contexte de sécurité de l'appelant. Pour déterminer si un autre principal dispose d'une autorisation effective, l'appelant doit disposer de l'autorisation IMPERSONATE sur ce principal.  
  
 Pour les entités au niveau schéma, les noms non NULL en une, deux ou trois parties sont acceptés. Pour les entités au niveau de la base de données, un nom en une partie est accepté, avec une valeur null signifiant «*base de données active*». Pour le serveur lui-même, la valeur NULL (« serveur actif ») est exigée. **fn_my_permissions** ne pouvez pas vérifier les autorisations sur un serveur lié.  
  
 La requête suivante renvoie la liste des classes intégrées des éléments sécurisables :  
  
```  
SELECT DISTINCT class_desc FROM fn_builtin_permissions(default)  
    ORDER BY class_desc;  
GO  
```  
  
 Si la valeur par défaut est fournie comme élément *sécurisable* ou *securable_class*, la valeur sera interprétée comme null.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-effective-permissions-on-the-server"></a>R. Affichage des autorisations effectives sur le serveur  
 L'exemple suivant renvoie la liste des autorisations effectives détenues par l'appelant sur le serveur.  
  
```  
SELECT * FROM fn_my_permissions(NULL, 'SERVER');  
GO  
```  
  
### <a name="b-listing-effective-permissions-on-the-database"></a>B. Affichage des autorisations effectives sur la base de données  
 L'exemple suivant renvoie la liste des autorisations effectives détenues par l'appelant sur la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE AdventureWorks2012;  
SELECT * FROM fn_my_permissions (NULL, 'DATABASE');  
GO  
```  
  
### <a name="c-listing-effective-permissions-on-a-view"></a>C. Affichage des autorisations effectives sur une vue  
 L'exemple suivant renvoie la liste des autorisations effectives détenues par l'appelant sur la vue `vIndividualCustomer` du schéma `Sales` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE AdventureWorks2012;  
SELECT * FROM fn_my_permissions('Sales.vIndividualCustomer', 'OBJECT')   
    ORDER BY subentity_name, permission_name ;   
GO   
```  
  
### <a name="d-listing-effective-permissions-of-another-user"></a>D. Affichage des autorisations effectives d'un autre utilisateur  
 L'exemple suivant renvoie la liste des autorisations effectives détenues par l'utilisateur de base de données `Wanida` sur la table `Employee` du schéma `HumanResources` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. L'appelant doit disposer de l'autorisation IMPERSONATE sur l'utilisateur `Wanida`.  
  
```  
EXECUTE AS USER = 'Wanida';  
SELECT * FROM fn_my_permissions('HumanResources.Employee', 'OBJECT')   
    ORDER BY subentity_name, permission_name ;    
REVERT;  
GO  
```  
  
### <a name="e-listing-effective-permissions-on-a-certificate"></a>E. Affichage des autorisations effectives sur un certificat  
 L'exemple suivant renvoie la liste des autorisations effectives détenues par l'appelant sur un certificat nommé `Shipping47` dans la base de données active.  
  
```  
SELECT * FROM fn_my_permissions('Shipping47', 'CERTIFICATE');  
GO  
```  
  
### <a name="f-listing-effective-permissions-on-an-xml-schema-collection"></a>F. Affichage des autorisations effectives sur une collection de schémas XML  
 L’exemple suivant retourne une liste des autorisations effectives de l’appelant sur une collection de schémas XML nommée `ProductDescriptionSchemaCollection` dans la [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] base de données.  
  
```  
USE AdventureWorks2012;  
SELECT * FROM fn_my_permissions('ProductDescriptionSchemaCollection',  
    'XML SCHEMA COLLECTION');  
GO  
```  
  
### <a name="g-listing-effective-permissions-on-a-database-user"></a>G. Affichage des autorisations effectives sur un utilisateur de base de données  
 L'exemple suivant renvoie la liste des autorisations effectives détenues par l'appelant sur un utilisateur nommé `MalikAr` dans la base de données active.  
  
```  
SELECT * FROM fn_my_permissions('MalikAr', 'USER');  
GO  
```  
  
### <a name="h-listing-effective-permissions-of-another-login"></a>H. Affichage des autorisations effectives d'un autre nom de connexion  
 L'exemple suivant renvoie la liste des autorisations effectives détenues par le nom d'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`WanidaBenshoof` sur la table `Employee` du schéma `HumanResources` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. L'appelant doit disposer de l'autorisation IMPERSONATE sur le nom d'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`WanidaBenshoof`.  
  
```  
EXECUTE AS LOGIN = 'WanidaBenshoof';  
SELECT * FROM fn_my_permissions('AdventureWorks2012.HumanResources.Employee', 'OBJECT')   
    ORDER BY subentity_name, permission_name ;    
REVERT;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de sécurité &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [Hiérarchie des autorisations &#40;Moteur de base de données&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)  
  
  
