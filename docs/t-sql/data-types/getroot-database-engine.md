---
description: GetRoot (moteur de base de données)
title: GetRoot (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GetRoot
- GetRoot_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GetRoot [Database Engine]
ms.assetid: 240b70f1-eeda-44ab-b4bb-9e4af80fa7c0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 6a61597b95b51e731b1a27f02d23b2f7765476cd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211743"
---
# <a name="getroot-database-engine"></a>GetRoot (moteur de base de données)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne la racine de la structure hiérarchique. GetRoot() est une méthode statique.
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Transact-SQL syntax  
hierarchyid::GetRoot ( )   
```  
  
```syntaxsql
-- CLR syntax  
static SqlHierarchyId GetRoot ( )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour  
**Type de retour SQL Server : hierarchyid**
  
**Type de retour CLR : SqlHierarchyId**
  
## <a name="remarks"></a>Remarques  
Sert à déterminer le nœud racine dans une arborescence hiérarchique.
  
## <a name="examples"></a>Exemples  
  
### <a name="a-transact-sql-example"></a>R. Exemple Transact-SQL  
L'exemple suivant retourne la racine de l'arborescence hiérarchique :
  
```sql
SELECT OrgNode.ToString() AS Text_OrgNode, *  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode = hierarchyid::GetRoot()  
```  
  
### <a name="b-clr-example"></a>B. Exemple CLR  
L’extrait de code suivant appelle la méthode GetRoot() :
  
```sql
SqlHierarchyId.GetRoot()  
```  
  
## <a name="see-also"></a>Voir aussi
[Référence de méthodes de type de données hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Données hiérarchiques &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
