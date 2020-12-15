---
description: SQLFreeStmt
title: SQLFreeStmt | Microsoft Docs
ms.custom: ''
ms.date: 11/23/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLFreeStmt function
ms.assetid: d9666d0b-3446-480e-bf1a-10b01213e411
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 956ce78ae72c39e1986955fcc562c4a2472eb4a1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465190"
---
# <a name="sqlfreestmt"></a>SQLFreeStmt
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Recommandé   
      **SQLFreeStmt** n'est pas recommandé dans ODBC 3.0 et les versions ultérieures. Toutefois, si l’application doit réutiliser l’instruction, vous devez toujours utiliser **SQLFreeStmt** avec les options **SQL_RESET_PARAMS** et **SQL_UNBIND** ). Vous pouvez également utiliser [SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md), [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md), [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md), [SQLSetDescField](../../relational-databases/native-client-odbc-api/sqlsetdescfield.md)et [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) pour remplacer ou dupliquer la fonction de **SQLFreeStmt** et les utiliser à la place.  
  
 En général, il est plus efficace de réutiliser des instructions que de les supprimer et d’en allouer de nouvelles. Toutefois, dans certaines situations, comme la réutilisation des instructions, SQLFreeStmt doit toujours être utilisé.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLFreeStmt fonction)](../../odbc/reference/syntax/sqlfreestmt-function.md)   
 [Détails de l’implémentation d’API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
