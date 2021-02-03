---
description: GetDescendant (moteur de base de données)
title: GetDescendant (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GetDescendant_TSQL
- GetDescendant
dev_langs:
- TSQL
helpviewer_keywords:
- GetDescendant [Database Engine]
ms.assetid: f5f39596-033e-4243-acbc-caa188b45b03
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 21c00b305f4bc31782307f7abb779c46ffe97e6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165178"
---
# <a name="getdescendant-database-engine"></a>GetDescendant (moteur de base de données)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne un nœud enfant du parent.
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Transact-SQL syntax  
parent.GetDescendant ( child1 , child2 )   
```  
  
```syntaxsql
-- CLR syntax  
SqlHierarchyId GetDescendant ( SqlHierarchyId child1 , SqlHierarchyId child2 )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*child1*  
Null ou **hierarchyid** d’un enfant du nœud actuel.
  
*child2*  
Null ou **hierarchyid** d’un enfant du nœud actuel.
  
## <a name="return-types"></a>Types de retour  
**Type de retour SQL Server : hierarchyid**
  
**Type de retour CLR : SqlHierarchyId**
  
## <a name="remarks"></a>Remarques  
Retourne un nœud enfant qui est un descendant du parent.
-   Si le parent est NULL, retourne NULL.  
-   Si parent n'est pas NULL et que enfant1 et enfant2 sont NULL, retourne un enfant du parent.  
-   Si parent et enfant1 ne sont pas NULL et que enfant2 est NULL, retourne un enfant de parent supérieur à enfant1.  
-   Si parent et enfant2 ne sont pas NULL et que enfant1 est NULL, retourne un enfant de parent inférieur à enfant2.  
-   Si parent, enfant1 et enfant2 ne sont pas tous NULL, retourne un enfant de parent supérieur à enfant1 et inférieur à enfant2.  
-   Si enfant1 n'est pas NULL et n'est pas un enfant du parent, une exception est levée.  
-   Si enfant2 n'est pas NULL et n'est pas un enfant du parent, une exception est levée.  
-   Si enfant1 >= enfant2, une exception est levée.  
  
GetDescendant est déterministe. Par conséquent, si GetDescendant est appelée avec les mêmes entrées, elle produit toujours la même sortie. Toutefois, l'identité exacte de l'enfant produite peut varier en fonction de sa relation avec les autres nœuds, comme cela est indiqué dans l'exemple C.
  
## <a name="examples"></a>Exemples  
  
### <a name="a-inserting-a-row-as-the-least-descendant-node"></a>R. Insertion d'une ligne comme nœud le moins descendant  
Un nouvel employé est embauché, dont le responsable est un employé existant au nœud `/3/1/`. Exécutez le code suivant pour insérer la nouvelle ligne en utilisant la méthode GetDescendant sans arguments pour spécifier le nouveau nœud de lignes comme `/3/1/1/` :
  
```sql
DECLARE @Manager hierarchyid;   
SET @Manager = CAST('/3/1/' AS hierarchyid);  
  
INSERT HumanResources.EmployeeDemo (OrgNode, LoginID, Title, HireDate)  
VALUES  
(@Manager.GetDescendant(NULL, NULL),  
'adventure-works\FirstNewEmployee', 'Application Intern', '3/11/07') ;  
```  
  
### <a name="b-inserting-a-row-as-a-greater-descendant-node"></a>B. Insertion d'une ligne comme nœud descendant supérieur  
Un autre nouvel employé est embauché, avec le même responsable que dans l’exemple A. Exécutez le code suivant pour insérer la nouvelle ligne en utilisant la méthode GetDescendant à l’aide de l’argument child1 afin de spécifier que le nœud de la nouvelle ligne suit le nœud dans l’exemple A et devient `/3/1/2/` :
  
```sql
DECLARE @Manager hierarchyid, @Child1 hierarchyid;  
  
SET @Manager = CAST('/3/1/' AS hierarchyid);  
SET @Child1 = CAST('/3/1/1/' AS hierarchyid);  
  
INSERT HumanResources.EmployeeDemo (OrgNode, LoginID, Title, HireDate)  
VALUES  
(@Manager.GetDescendant(@Child1, NULL),  
'adventure-works\SecondNewEmployee', 'Application Intern', '3/11/07') ;  
```  
  
### <a name="c-inserting-a-row-between-two-existing-nodes"></a>C. Insertion d'une ligne entre deux nœuds existants  
Un troisième employé est embauché, avec le même responsable que dans l’exemple A. Cet exemple insère la nouvelle ligne à un nœud supérieur au `FirstNewEmployee` de l’exemple A, et inférieur au `SecondNewEmployee` de l’exemple B. Exécutez le code suivant en utilisant la méthode GetDescendant. Utilisez l'argument child1 et l'argument child2 pour spécifier que le nœud de la nouvelle ligne devient le nœud `/3/1/1.1/` :
  
```sql
DECLARE @Manager hierarchyid, @Child1 hierarchyid, @Child2 hierarchyid;  
  
SET @Manager = CAST('/3/1/' AS hierarchyid);  
SET @Child1 = CAST('/3/1/1/' AS hierarchyid);  
SET @Child2 = CAST('/3/1/2/' AS hierarchyid);  
  
INSERT HumanResources.EmployeeDemo (OrgNode, LoginID, Title, HireDate)  
VALUES  
(@Manager.GetDescendant(@Child1, @Child2),  
'adventure-works\ThirdNewEmployee', 'Application Intern', '3/11/07') ;  
  
```  
  
Après l’exécution des exemples A, B, et C, les nœuds ajoutés à la table seront des homologues avec les valeurs **hierarchyid** suivantes :
  
`/3/1/1/`
  
`/3/1/1.1/`
  
`/3/1/2/`
  
Le nœud `/3/1/1.1/` est supérieur à au nœud `/3/1/1/` mais au même niveau dans la hiérarchie.
  
### <a name="d-scalar-examples"></a>D. Exemples scalaires  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge les insertions et suppressions arbitraires de tous les nœuds **hierarchyid**. En utilisant GetDescendant(), il est toujours possible de générer un nœud entre deux nœuds **hierarchyid** quelconques. Exécutez le code suivant pour générer des exemples de nœuds à l'aide de `GetDescendant` :
  
```sql
DECLARE @h hierarchyid = hierarchyid::GetRoot();  
DECLARE @c hierarchyid = @h.GetDescendant(NULL, NULL);  
SELECT @c.ToString();  
DECLARE @c2 hierarchyid = @h.GetDescendant(@c, NULL);  
SELECT @c2.ToString();  
SET @c2 = @h.GetDescendant(@c, @c2);  
SELECT @c2.ToString();  
SET @c = @h.GetDescendant(@c, @c2);  
SELECT @c.ToString();  
SET @c2 = @h.GetDescendant(@c, @c2);  
SELECT @c2.ToString();  
```  
  
### <a name="e-clr-example"></a>E. Exemple CLR  
L'extrait de code suivant appelle la méthode `GetDescendant()` :
  
```sql
SqlHierarchyId parent, child1, child2;  
parent = SqlHierarchyId.GetRoot();  
child1 = parent.GetDescendant(SqlHierarchyId.Null, SqlHierarchyId.Null);  
child2 = parent.GetDescendant(child1, SqlHierarchyId.Null);  
Console.Write(parent.GetDescendant(child1, child2).ToString());  
```  
  
## <a name="see-also"></a>Voir aussi
[Référence de méthodes de type de données hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Données hiérarchiques &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
