---
description: GETANSINULL (Transact-SQL)
title: GETANSINULL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- GETANSINULL
- GETANSINULL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- null values [SQL Server], default
- GETANSINULL function
- default nullability
- database nullability [SQL Server]
ms.assetid: 189399e4-428d-4902-b3a8-94f07fdefc6a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 501e9b234c33daf6c5c9c64f79a024e86ea3654b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98086589"
---
# <a name="getansinull-transact-sql"></a>GETANSINULL (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne la possibilité de valeur NULL par défaut de la base de données pour cette session.  
  
 ![Icône Lien d’article](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
GETANSINULL ( [ 'database' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 '*database*'  
 Nom de la base de données pour laquelle retourner des informations sur les possibilités de valeur NULL. *la base de données est de type **char** ou **ncha**. Si son type est **char**, *database* est implicitement converti en **nchar**.  
  
## <a name="return-types"></a>Types de retour  
 **int**  
  
## <a name="remarks"></a>Notes  
GETANSINULL retourne 1 si la possibilité de valeur Null de la base de données autorise les valeurs Null. Cette valeur renvoyée nécessite également que la possibilité de valeur Null de la colonne ou du type données ne soit pas explicitement définie. La valeur ANSI NULL par défaut est 1. 
  
 Pour activer le comportement par défaut de ANSI NULL, l'une des conditions suivantes doit être définie :  
  
-   ALTER DATABASE *database_name* SET ANSI_NULL_DEFAULT ON  
  
-   SET ANSI_NULL_DFLT_ON ON  
  
-   SET ANSI_NULL_DFLT_OFF OFF  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne la possibilité de valeur NULL par défaut pour la base de données `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT GETANSINULL('AdventureWorks2012')  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 ------  
1  

(1 row(s) affected)
 ```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  
