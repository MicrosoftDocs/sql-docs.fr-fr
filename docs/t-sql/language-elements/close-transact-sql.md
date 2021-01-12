---
description: CLOSE (Transact-SQL)
title: CLOSE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CLOSE_TSQL
- CLOSE
dev_langs:
- TSQL
helpviewer_keywords:
- closing cursors
- cursors [SQL Server], closing
- CLOSE statement
ms.assetid: 21546874-97e3-4b93-970f-87c27f6b78c7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7d4f487bb98745a8e9e182ab31fdeea8e66e3b76
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98085301"
---
# <a name="close-transact-sql"></a>CLOSE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Ferme un curseur ouvert en libérant le jeu de résultats actuel et tous les verrous de curseur dans les lignes sur lesquelles le curseur est positionné. `CLOSE` garde les structures de données disponibles pour une réouverture éventuelle, mais les extractions et les mises à jour positionnées ne sont pas autorisées jusqu’à la réouverture du curseur. `CLOSE` doit être émise sur un curseur ouvert. Elle n’est pas autorisée sur un curseur qui a seulement été déclaré ou qui est déjà fermé.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CLOSE { { [ GLOBAL ] cursor_name } | cursor_variable_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 GLOBAL  
 Indique que *cursor_name* fait référence à un curseur global.  
  
 *cursor_name*  
 Nom d'un curseur ouvert. Si un curseur global et un curseur local ont tous les deux le même nom *cursor_name*, *cursor_name* fait référence au curseur global si GLOBAL est spécifié ; sinon, *cursor_name* fait référence au curseur local.  
  
 *cursor_variable_name*  
 Nom d'une variable de curseur associée à un curseur ouvert.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre comment positionner correctement l'instruction `CLOSE` dans un processus basé sur un curseur.  
  
```sql  
DECLARE Employee_Cursor CURSOR FOR  
SELECT EmployeeID, Title FROM AdventureWorks2012.HumanResources.Employee;  
OPEN Employee_Cursor;  
FETCH NEXT FROM Employee_Cursor;  
WHILE @@FETCH_STATUS = 0  
   BEGIN  
      FETCH NEXT FROM Employee_Cursor;  
   END;  
CLOSE Employee_Cursor;  
DEALLOCATE Employee_Cursor;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Curseurs](../../relational-databases/cursors.md)   
 [Curseurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DEALLOCATE &#40;Transact-SQL&#41;](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [FETCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/fetch-transact-sql.md)   
 [OPEN &#40;Transact-SQL&#41;](../../t-sql/language-elements/open-transact-sql.md)  
  
  
