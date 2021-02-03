---
description: GetAncestor (moteur de base de données)
title: GetAncestor (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GetAncestor_TSQL
- GetAncestor
dev_langs:
- TSQL
helpviewer_keywords:
- GetAncestor [Database Engine]
ms.assetid: b96a986f-d5e4-4034-8013-de7974594ee9
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 0a306814f0146fdb0dc1833d0d6f98d84cae8f1a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172546"
---
# <a name="getancestor-database-engine"></a>GetAncestor (moteur de base de données)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne un **hierarchyid** qui représente le *n* ième ancêtre de *this*.
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Transact-SQL syntax  
child.GetAncestor ( n )   
```  
  
```syntaxsql
-- CLR syntax  
SqlHierarchyId GetAncestor ( int n )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*n*  
**int** représentant le nombre de niveaux à remonter dans la hiérarchie.
  
## <a name="return-types"></a>Types de retour
**Type de retour SQL Server : hierarchyid**
  
**Type de retour CLR : SqlHierarchyId**
  
## <a name="remarks"></a>Remarques  
Utilisé pour tester si chaque nœud de la sortie a pour ancêtre le nœud actuel au niveau spécifié.
  
Si un nombre supérieur à [GetLevel()](../../t-sql/data-types/getlevel-database-engine.md) est passé, la valeur Null est retournée.
  
Si un nombre négatif est passé, une exception est levée.
  
## <a name="examples"></a>Exemples  
  
### <a name="a-finding-the-child-nodes-of-a-parent"></a>R. Recherche des nœuds enfants d'un parent  
`GetAncestor(1)` retourne les employés qui ont `david0` pour ancêtre immédiat (leur parent). L'exemple suivant utilise `GetAncestor(1)`.
  
```sql
DECLARE @CurrentEmployee hierarchyid  
SELECT @CurrentEmployee = OrgNode FROM HumanResources.EmployeeDemo  
WHERE LoginID = 'adventure-works\david0'  
  
SELECT OrgNode.ToString() AS Text_OrgNode, *  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode.GetAncestor(1) = @CurrentEmployee ;  
```  
  
### <a name="b-returning-the-grandchildren-of-a-parent"></a>B. Retour des petits-enfants d'un parent  
`GetAncestor(2)` retourne les employés situés dans la hiérarchie deux niveaux en-dessous du nœud actuel. Ces employés sont les petits-enfants du nœud actuel. L'exemple suivant utilise `GetAncestor(2)`.
  
```sql
DECLARE @CurrentEmployee hierarchyid  
SELECT @CurrentEmployee = OrgNode FROM HumanResources.EmployeeDemo  
WHERE LoginID = 'adventure-works\ken0'  
  
SELECT OrgNode.ToString() AS Text_OrgNode, *  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode.GetAncestor(2) = @CurrentEmployee ;  
```  
  
### <a name="c-returning-the-current-row"></a>C. Retour de la ligne actuelle  
Pour retourner le nœud actuel en utilisant `GetAncestor(0)`, exécutez le code suivant.
  
```sql
DECLARE @CurrentEmployee hierarchyid  
SELECT @CurrentEmployee = OrgNode FROM HumanResources.EmployeeDemo  
WHERE LoginID = 'adventure-works\david0'  
  
SELECT OrgNode.ToString() AS Text_OrgNode, *  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode.GetAncestor(0) = @CurrentEmployee ;  
```  
  
### <a name="d-returning-a-hierarchy-level-if-a-table-isnt-present"></a>D. Retour d’un niveau de la hiérarchie si une table n’est pas présente  
`GetAncestor` retourne le niveau sélectionné de la hiérarchie même si une table n’est pas présente. Par exemple, le code suivant désigne un employé actuel et retourne le `hierarchyid` de l’ancêtre de l’employé actuel sans référence à une table.
  
```sql
DECLARE @CurrentEmployee hierarchyid ;  
DECLARE @TargetEmployee hierarchyid ;  
SELECT @CurrentEmployee = '/2/3/1.2/5/3/' ;  
SELECT @TargetEmployee = @CurrentEmployee.GetAncestor(2) ;  
SELECT @TargetEmployee.ToString(), @TargetEmployee ;  
```  
  
### <a name="e-calling-a-common-language-runtime-method"></a>E. Appel d'une méthode CLR (Common Language Runtime)  
L'extrait de code suivant appelle la méthode `GetAncestor()`.
  
```sql
this.GetAncestor(1)  
```  
  
## <a name="see-also"></a>Voir aussi
[IsDescendantOf &#40;moteur de base de données&#41;](../../t-sql/data-types/isdescendantof-database-engine.md)  
[Référence de méthodes de type de données hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Données hiérarchiques &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
