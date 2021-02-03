---
description: OPEN (Transact-SQL)
title: OPEN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPEN_TSQL
- OPEN
dev_langs:
- TSQL
helpviewer_keywords:
- opening cursors
- cursors [SQL Server], opening
- populating cursors [SQL Server]
- OPEN statement
- Transact-SQL cursors, opening
ms.assetid: fd1c5e3b-6045-4a42-b646-3fca76e874c1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 23d3a4c306d61f2fdc7f71102a3a786dd9856941
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191927"
---
# <a name="open-transact-sql"></a>OPEN (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Ouvre un curseur côté serveur [!INCLUDE[tsql](../../includes/tsql-md.md)] et remplit ce dernier en exécutant l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] spécifiée dans l’instruction DECLARE CURSOR ou SET *cursor_variable*.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
OPEN { { [ GLOBAL ] cursor_name } | cursor_variable_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 GLOBAL  
 Indique que *cursor_name* fait référence à un curseur global.  
  
 *cursor_name*  
 Représente le nom d'un curseur déclaré. Si un curseur global et un curseur local ont tous les deux le même nom *cursor_name*, *cursor_name* fait référence au curseur global si GLOBAL est spécifié ; sinon, *cursor_name* fait référence au curseur local.  
  
 *cursor_variable_name*  
 Nom d'une variable de curseur faisant référence à un curseur.  
  
## <a name="remarks"></a>Remarques  
 Si le curseur est déclaré avec l'option INSENSITIVE ou STATIC, OPEN crée une table temporaire pour recueillir l'ensemble de résultats. OPEN échoue si la longueur d'une ligne de l'ensemble de résultats est supérieure à la longueur de ligne maximale des tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si le curseur est déclaré avec l'option KEYSET, OPEN crée une table temporaire pour recueillir le jeu de clés. Les tables temporaires sont stockées dans tempdb.  
  
 Après avoir ouvert un curseur, utilisez la fonction @@CURSOR_ROWS pour recevoir le nombre de lignes éligibles dans le dernier curseur ouvert.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge la génération asynchrone de curseurs [!INCLUDE[tsql](../../includes/tsql-md.md)] pilotés par jeux de clés ou statiques. [!INCLUDE[tsql](../../includes/tsql-md.md)] sur les curseurs telles que OPEN ou FETCH sont exécutées par lot : la génération asynchrone de curseurs [!INCLUDE[tsql](../../includes/tsql-md.md)] n'est donc pas nécessaire. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] continue de prendre en charge les curseurs côté serveur d’API pilotés par jeux de clés ou statiques asynchrones si l’opération de curseur OPEN présente une latence trop faible, en raison des boucles clientes de chaque opération de curseur.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant ouvre un curseur et extrait toutes les lignes.  
  
```sql  
DECLARE Employee_Cursor CURSOR FOR  
SELECT LastName, FirstName  
FROM AdventureWorks2012.HumanResources.vEmployee  
WHERE LastName like 'B%';  
  
OPEN Employee_Cursor;  
  
FETCH NEXT FROM Employee_Cursor;  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    FETCH NEXT FROM Employee_Cursor  
END;  
  
CLOSE Employee_Cursor;  
DEALLOCATE Employee_Cursor;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CLOSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/close-transact-sql.md)   
 [@@CURSOR_ROWS &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-rows-transact-sql.md)   
 [DEALLOCATE &#40;Transact-SQL&#41;](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [DECLARE CURSOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-cursor-transact-sql.md)   
 [FETCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/fetch-transact-sql.md)  
  
  
