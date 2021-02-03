---
description: IsDescendantOf (moteur de base de données)
title: IsDescendantOf (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IsDescendant_TSQL
- IsDescendant
dev_langs:
- TSQL
helpviewer_keywords:
- IsDescendantOf [Database Engine]
ms.assetid: edc80444-b697-410f-9419-0f63c9b5618d
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a3ffb6820f43553a6adcc2f613d93657b23b79f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165093"
---
# <a name="isdescendantof-database-engine"></a>IsDescendantOf (moteur de base de données)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne la valeur True si *this* est un descendant de parent.
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Transact-SQL syntax  
child. IsDescendantOf ( parent )  
```  
  
```syntaxsql
-- CLR syntax  
SqlHierarchyId IsDescendantOf (SqlHierarchyId parent )  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*parent*  
Nœud **hierarchyid** pour lequel le test IsDescendantOf doit être effectué.
  
## <a name="return-types"></a>Types de retour  
**Type de retour SQL Server : bit**
  
**Type de retour CLR : SqlBoolean**
  
## <a name="remarks"></a>Remarques  
Retourne la valeur true pour tous les nœuds de la sous-arborescence dont la racine est le parent, et false pour tous les autres nœuds.
  
Le parent est considéré comme étant son propre descendant.
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-isdescendantof-in-a-where-clause"></a>R. Utilisation d'IsDescendantOf dans une clause WHERE  
L'exemple suivant retourne un responsable et les employés qui travaillent sous ses ordres :
  
```sql
DECLARE @Manager hierarchyid  
SELECT @Manager = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\dylan0'  
  
SELECT * FROM HumanResources.EmployeeDemo  
WHERE OrgNode.IsDescendantOf(@Manager) = 1  
```  
  
### <a name="b-using-isdescendantof-to-evaluate-a-relationship"></a>B. Utilisation d'IsDescendantOf pour évaluer une relation  
Le code suivant déclare et remplit trois variables. Il évalue ensuite la relation hiérarchique et retourne l'un de deux résultats imprimés selon la comparaison :
  
```sql
DECLARE @Manager hierarchyid, @Employee hierarchyid, @LoginID nvarchar(256)  
SELECT @Manager = OrgNode FROM HumanResources.EmployeeDemo  
WHERE LoginID = 'adventure-works\terri0' ;  
  
SELECT @Employee = OrgNode, @LoginID = LoginID FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\rob0'  
  
IF @Employee.IsDescendantOf(@Manager) = 1  
   BEGIN  
    PRINT 'LoginID ' + @LoginID + ' is a subordinate of the selected Manager.'  
   END  
ELSE  
   BEGIN  
    PRINT 'LoginID ' + @LoginID + ' is not a subordinate of the selected Manager.' ;  
   END  
```  
  
### <a name="c-calling-a-common-language-runtime-method"></a>C. Appel d'une méthode CLR (Common Language Runtime)  
L'extrait de code suivant appelle la méthode `IsDescendantOf()`.
  
```sql
this.IsDescendantOf(Parent)  
```  
  
## <a name="see-also"></a>Voir aussi
[Référence de méthodes de type de données hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Données hiérarchiques &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
