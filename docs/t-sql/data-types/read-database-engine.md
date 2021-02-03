---
description: Read (moteur de base de données) à l’aide de CSharp
title: Read (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Read_TSQL
- Read
dev_langs:
- TSQL
helpviewer_keywords:
- Read [Database Engine]
ms.assetid: f2b8207c-b69f-4327-a874-100b3a1f27d8
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4acfebf93f27a64fd0902a3bf9a2bad4e6d5ffe3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187848"
---
# <a name="read-database-engine-by-using-csharp"></a>Read (moteur de base de données) à l’aide de CSharp
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Read lit la représentation binaire de **SqlHierarchyId** à partir du **BinaryReader** passé et définit l’objet **SqlHierarchyId** sur cette valeur. Read ne peut pas être appelée au moyen de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Utilisez plutôt CAST ou CONVERT.
  
## <a name="syntax"></a>Syntaxe  

<!--
This is not T-SQL, despite the ```sql colorizer specified.
Neither should this be ```syntaxsql.
Rather, this is C# (or C# syntax).  Same for the later code blocks.
I am making this fix now, from ```sql to ```cs, on 2020/04/16.  GeneMi.
-->

```csharp
void Read( BinaryReader r )   
```  

## <a name="arguments"></a>Arguments
*r*  
 Objet **BinaryReader** qui produit un flux binaire correspondant à une représentation binaire d’un nœud **hierarchyid**.  
  
## <a name="return-types"></a>Types de retour
 **Type de retour CLR : void**  
  
## <a name="remarks"></a>Notes  
 Read ne valide pas son entrée. Si une entrée binaire non valide est fournie, Read peut lever une exception. Elle peut aussi réussir et produire un objet **SqlHierarchyId** non valide dont les méthodes peuvent donner des résultats imprévisibles ou lever une exception.  
  
 Read peut être appelée uniquement sur un objet **SqlHierarchyId** créé récemment.  
  
 Read est utilisée en interne par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cas de nécessité, par exemple lors de l’écriture de données dans une colonne **hierarchyid**. Read est également appelée en interne quand une conversion est effectuée entre **varbinary** et **hierarchyid**.  
  
## <a name="examples"></a>Exemples  
  
```csharp
Byte[] encoding = new byte[] { 0x58 };  
MemoryStream stream = new MemoryStream(encoding, false /*not writable*/);  
BinaryReader br = new BinaryReader(stream);  
SqlHierarchyId hid = new SqlHierarchyId();  
hid.Read(br);   
```  
  
## <a name="see-also"></a>Voir aussi  
[Write &#40;moteur de base de données&#41;](../../t-sql/data-types/write-database-engine.md)  
[ToString &#40;moteur de base de données&#41;](../../t-sql/data-types/tostring-database-engine.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Référence de méthodes de type de données hierarchyid](./hierarchyid-data-type-method-reference.md)
  
