---
description: '&#x40;&#x40;TEXTSIZE (Transact-SQL)'
title: '@@TEXTSIZE (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@TEXTSIZE'
- '@@TEXTSIZE_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- SET statement, TEXTSIZE option
- SELECT statement [SQL Server], text size returned
- TEXTSIZE option
- '@@TEXTSIZE function'
- text size returned [SQL Server]
ms.assetid: 4308a7b9-8e8f-49e9-8246-8224e32f4953
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: ac628c06693b46aec49654541723937ff317b18c
ms.sourcegitcommit: 197a6ffb643f93592edf9e90b04810a18be61133
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91380482"
---
# <a name="x40x40textsize-transact-sql"></a>&#x40;&#x40;TEXTSIZE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie la valeur actuelle de l’option [TEXTSIZE](../../t-sql/statements/set-textsize-transact-sql.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
@@TEXTSIZE  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 **integer**  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant utilise `SELECT` pour afficher la valeur `@@TEXTSIZE` avant et après sa modification avec l’instruction `SET``TEXTSIZE`.  
  
```sql
-- Set the TEXTSIZE option to the default size of 4096 bytes.  
SET TEXTSIZE 0  
SELECT @@TEXTSIZE AS 'Text Size'  
SET TEXTSIZE 2048  
SELECT @@TEXTSIZE AS 'Text Size'  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Text Size
-----------
4096
Text Size
-----------
2048
 ```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de configuration &#40;Transact-SQL&#41;](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [SET TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/statements/set-textsize-transact-sql.md)  
  
  
