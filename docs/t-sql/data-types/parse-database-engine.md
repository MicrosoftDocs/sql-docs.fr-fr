---
description: Parse (moteur de base de données)
title: Parse (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Parse
- Parse_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Parse [Database Engine]
ms.assetid: b37e28b6-6e2e-470a-945b-ce5252da743a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 7b74df820ef7cd0bebacf77d5c67344bedac2b5e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187855"
---
# <a name="parse-database-engine"></a>Parse (moteur de base de données)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Parse convertit la représentation de chaîne canonique d’un **hierarchyid** en valeur **hierarchyid**. Parse est appelée implicitement quand une conversion d’un type chaîne en **hierarchyid** se produit. Agit comme l’opposé de [ToString](../../t-sql/data-types/tostring-database-engine.md). Parse() est une méthode statique.
  
## <a name="syntax"></a>Syntaxe  
  
```sql
-- Transact-SQL syntax  
hierarchyid::Parse ( input )  
-- This is functionally equivalent to the following syntax   
-- which implicitly calls Parse():  
CAST ( input AS hierarchyid )  
```  
  
```csharp
-- CLR syntax  
static SqlHierarchyId Parse ( SqlString input )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*input*  
[!INCLUDE[tsql](../../includes/tsql-md.md)] : valeur du type de données character converti.
  
CLR : valeur de chaîne évaluée.
  
## <a name="return-types"></a>Types de retour  
**Type de retour SQL Server : hierarchyid**
  
**Type de retour CLR : SqlHierarchyId**
  
## <a name="remarks"></a>Remarques  
Si Parse reçoit une valeur qui n’est pas une représentation de chaîne valide d’un **hierarchyid**, une exception est levée. Par exemple, si les types de données **char** contiennent des espaces de fin, une exception est levée.
  
## <a name="examples"></a>Exemples  
  
### <a name="a-converting-transact-sql-values-without-a-table"></a>R. Conversion de valeurs Transact-SQL sans table  
L’exemple de code suivant utilise `ToString` pour convertir une valeur **hierarchyid** en une chaîne et `Parse` pour convertir une valeur de chaîne en **hierarchyid**.
  
```sql
DECLARE @StringValue AS NVARCHAR(4000), @hierarchyidValue AS hierarchyid  
SET @StringValue = '/1/1/3/'  
SET @hierarchyidValue = 0x5ADE  
  
SELECT hierarchyid::Parse(@StringValue) AS hierarchyidRepresentation,  
@hierarchyidValue.ToString() AS StringRepresentation ;
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
hierarchyidRepresentation    StringRepresentation
-------------------------    -----------------------
0x5ADE                       /1/1/3/
```
  
### <a name="b-clr-example"></a>B. Exemple CLR  
L’extrait de code suivant appelle la méthode Parse() :
  
```csharp
string input = "/1/2/";  
SqlHierarchyId.Parse(input);  
```  
  
## <a name="see-also"></a>Voir aussi
[Référence de méthodes de type de données hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Données hiérarchiques &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
