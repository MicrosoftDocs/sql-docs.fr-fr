---
description: Write (moteur de base de données)
title: Write (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Write_TSQL
- Write
dev_langs:
- TSQL
helpviewer_keywords:
- Write [Database Engine]
ms.assetid: 7c554334-d2d9-4eae-a4ae-097aa4020e1a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: aba1271dd68224d237595a81d61b23d8db38b7c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211487"
---
# <a name="write-database-engine"></a>Write (moteur de base de données)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Write écrit une représentation binaire de **SqlHierarchyId** dans le **BinaryWriter** passé. Write ne peut pas être appelée au moyen de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Utilisez plutôt CAST ou CONVERT.
  
## <a name="syntax"></a>Syntaxe  
  
```csharp
void Write( BinaryWriter w )
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*w*  
Objet **BinaryWriter** dans lequel la représentation binaire de ce nœud **hierarchyid** sera écrite.
  
## <a name="return-types"></a>Types de retour  
**Type de retour CLR : void**
  
## <a name="remarks"></a>Notes  
Write est utilisée en interne par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cas de nécessité, par exemple lors du chargement de données à partir d’une colonne **hierarchyid**. Write est également appelée en interne quand une conversion est effectuée entre **hierarchyid** et **varbinary**.
  
## <a name="examples"></a>Exemples  
  
```csharp
MemoryStream stream = new MemoryStream();  
BinaryWriter bw = new BinaryWriter(stream);  
hid.Write(bw);  
byte[] encoding = stream.ToArray();  
```  
  
## <a name="see-also"></a>Voir aussi
[Read &#40;moteur de base de données&#41;](../../t-sql/data-types/read-database-engine.md)  
[ToString &#40;moteur de base de données&#41;](../../t-sql/data-types/tostring-database-engine.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Référence de méthodes de type de données hierarchyid](./hierarchyid-data-type-method-reference.md)
  
