---
description: HAS_PERMS_BY_NAME (Transact-SQL)
title: HAS_PERMS_BY_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- HAS_PERMS_BY_NAME
- HAS_PERMS_BY_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], verifying
- current permission status
- checking permission status
- verifying permission status
- testing permissions
- HAS_PERMS_BY_NAME function
ms.assetid: eaf8cc82-1047-4144-9e77-0e1095df6143
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c80d6a873f0004344e7407dbf25adee5a02c2cd4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184004"
---
# <a name="has_perms_by_name-transact-sql"></a>HAS_PERMS_BY_NAME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Évalue l'autorisation effective de l'utilisateur actuel sur un élément sécurisable. Une fonction associée est [fn_my_permissions](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
HAS_PERMS_BY_NAME ( securable , securable_class , permission    
    [ , sub-securable ] [ , sub-securable_class ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *securable*  
 Indique le nom de l'élément sécurisable. Si l'élément sécurisable est le serveur lui-même, cette valeur doit être définie avec NULL. *securable* est une expression scalaire de type **sysname**. Il n'y a pas de valeur par défaut.  
  
 *securable_class*  
 Nom de la classe de l'élément sécurisable sur lequel l'autorisation est testée. *securable_class* est une expression scalaire de type **nvarchar(60)**.  
  
 Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], l’argument securable_class doit avoir l’une des valeurs suivantes : **DATABASE**, **OBJECT**, **ROLE**, **SCHEMA** ou **USER**.  
  
 *permission*  
 Expression scalaire non-NULL de type **sysname** qui représente le nom de l’autorisation à vérifier. Il n'y a pas de valeur par défaut. Le nom d'autorisation ANY représente une autorisation générique.  
  
 *sub-securable*  
 Expression scalaire facultative de type **sysname** qui représente le nom de la sous-entité sécurisable sur laquelle l’autorisation est testée. La valeur par défaut est NULL.  
  
> [!NOTE]  
>  Les éléments sécurisables sub ne peuvent pas utiliser de crochets sous la forme **'[** _sub name_ **]'** . Utilisez **'** _sub name_ **'** à la place.  
  
 *sub-securable_class*  
 Expression scalaire facultative de type **nvarchar(60)** qui représente la classe de la sous-entité sécurisable sur laquelle l’autorisation est testée. La valeur par défaut est NULL.  
  
 Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], l’argument sub-securable_class est valide uniquement si l’argument securable_class a la valeur **OBJECT**. Si l’argument securable_class a la valeur **OBJECT**, l’argument sub-securable_class doit être défini sur **COLUMN**.  
  
## <a name="return-types"></a>Types de retour  
 **int**  
  
 Renvoie NULL lorsque la requête échoue.  
  
## <a name="remarks"></a>Remarques  
 Cette fonction intégrée teste si le principal actif a une autorisation effective sur un élément sécurisable spécifié. HAS_PERMS_BY_NAME retourne 1 lorsque l'utilisateur dispose d'autorisation effective sur l'élément sécurisable, 0 lorsque l'utilisateur n'a pas cette autorisation et NULL si la classe sécurisable ou l'autorisation n'est pas valide. Une autorisation effective peut être :  
  
-   une autorisation accordée directement au principal et non refusée ;  
  
-   une autorisation impliquée par une autorisation de niveau supérieur détenue par le principal et non refusée ;  
  
-   une autorisation accordée à un rôle ou à un groupe dont le principal est membre et non refusé ;  
  
-   une autorisation détenue par un rôle ou un groupe dont le principal est membre et non refusé.  
  
 L'évaluation des autorisations a toujours lieu dans le contexte de sécurité de l'appelant. Pour déterminer si un autre utilisateur a une autorisation effective, l'appelant doit avoir l'autorisation IMPERSONATE sur cet utilisateur.  
  
 Pour les entités au niveau schéma, les noms non NULL en une, deux ou trois parties sont acceptés. Pour les entités au niveau base de données, un nom en une partie est accepté ; la valeur NULL indique la « base de données active ». Pour le serveur lui-même, la valeur NULL (« serveur actif ») est exigée. Cette fonction ne peut pas contrôler les autorisations sur un serveur lié ou sur un utilisateur Windows pour lequel aucun principal au niveau serveur n'a été créé.  
  
 La requête suivante renvoie la liste des classes intégrées des éléments sécurisables :  
  
```  
SELECT class_desc FROM sys.fn_builtin_permissions(default);  
```  
  
 Les classements suivants sont utilisés :  
  
-   Classement de la base de données active : éléments sécurisables au niveau base de données qui comprennent des éléments sécurisables non contenus dans un schéma ; éléments sécurisables sur l'étendue d'un schéma en une ou deux parties ; base de données cible lors de l'utilisation d'un nom en trois parties.  
  
-   Classement de la base de données master : éléments sécurisables au niveau serveur.  
  
-   « ANY » n'est pas pris en charge pour les contrôles au niveau colonne. Vous devez spécifier l'autorisation appropriée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-do-i-have-the-server-level-view-server-state-permission"></a>R. Ai-je l'autorisation VIEW SERVER STATE au niveau serveur ?  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieur
  
```sql  
SELECT HAS_PERMS_BY_NAME(null, null, 'VIEW SERVER STATE');  
```  
  
### <a name="b-am-i-able-to-impersonate-server-principal-ps"></a>B. Puis-je emprunter l'identité du principal du serveur Ps ?  
  
**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieur
  
```sql  
SELECT HAS_PERMS_BY_NAME('Ps', 'LOGIN', 'IMPERSONATE');  
```  
  
### <a name="c-do-i-have-any-permissions-in-the-current-database"></a>C. Ai-je des autorisations dans la base de données active ?  
  
```sql  
SELECT HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'ANY');  
```  
  
### <a name="d-does-database-principal-pd-have-any-permission-in-the-current-database"></a>D. Le principal de la base de données Pd a-t-il une autorisation dans la base de données active ?  
 Supposons que l'appelant a une autorisation IMPERSONATE sur le principal `Pd`.  
  
```sql  
EXECUTE AS user = 'Pd'  
GO  
SELECT HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'ANY');  
GO  
REVERT;  
GO  
```  
  
### <a name="e-can-i-create-procedures-and-tables-in-schema-s"></a>E. Puis-je créer des procédures et des tables dans le schéma S ?  
 L'exécution du code exemple suivant nécessite l'autorisation `ALTER` dans `S` et l'autorisation `CREATE PROCEDURE` dans la base de données, de même pour des tables.  
  
```sql  
SELECT HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'CREATE PROCEDURE')  
    & HAS_PERMS_BY_NAME('S', 'SCHEMA', 'ALTER') AS _can_create_procs,  
    HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'CREATE TABLE') &  
    HAS_PERMS_BY_NAME('S', 'SCHEMA', 'ALTER') AS _can_create_tables;  
```  
  
### <a name="f-which-tables-do-i-have-select-permission-on"></a>F. Sur quelles tables dois-je sélectionner l'autorisation ?  
  
```sql  
SELECT HAS_PERMS_BY_NAME  
(QUOTENAME(SCHEMA_NAME(schema_id)) + '.' + QUOTENAME(name),   
    'OBJECT', 'SELECT') AS have_select, * FROM sys.tables  
```  
  
### <a name="g-do-i-have-insert-permission-on-the-salesperson-table-in-adventureworks2012"></a>G. Ai-je l'autorisation INSERT sur la table SalesPerson dans AdventureWorks2012 ?  
 Le code exemple suivant suppose que `AdventureWorks2012` est le contexte actuel de ma base de données et utilise un nom en deux parties.  
  
```sql  
SELECT HAS_PERMS_BY_NAME('Sales.SalesPerson', 'OBJECT', 'INSERT');  
```  
  
 Le code exemple suivant ne fait aucune hypothèse sur le contexte actuel de ma base de données et utilise un nom en trois parties.  
  
```sql  
SELECT HAS_PERMS_BY_NAME('AdventureWorks2012.Sales.SalesPerson',   
    'OBJECT', 'INSERT');  
```  
  
### <a name="h-which-columns-of-table-t-do-i-have-select-permission-on"></a>H. Pour quelles colonnes de la table T ai-je l'autorisation SELECT ?  
  
```sql  
SELECT name AS column_name,   
    HAS_PERMS_BY_NAME('T', 'OBJECT', 'SELECT', name, 'COLUMN')   
    AS can_select   
    FROM sys.columns AS c   
    WHERE c.object_id=object_id('T');  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [Hiérarchie des autorisations &#40;Moteur de base de données&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)  
  
  
