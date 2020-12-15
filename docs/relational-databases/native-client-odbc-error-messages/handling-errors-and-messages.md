---
title: Gestion des erreurs et des messages | Microsoft Docs
description: Découvrez les informations de diagnostic retournées lorsqu’une application appelle une fonction ODBC, y compris la réussite ou l’échec, ainsi que des informations détaillées.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC error handling, about error handling
- errors [ODBC]
- SQL Server Native Client ODBC driver, errors
- messages [ODBC], about messages
- ODBC error handling
- SQL_INVALID_HANDLE return code
- errors [ODBC], about error handling
- messages [ODBC]
ms.assetid: 74ea9630-e482-4a46-bb45-f5234f079b48
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 367142f4a605f95c81cbac34d4696a77f7a693ce
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438475"
---
# <a name="handling-errors-and-messages"></a>Gestion des erreurs et des messages
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Lorsqu'une application appelle une fonction ODBC, le pilote exécute la fonction et retourne les informations de diagnostic de deux façons : un code de retour indique le succès ou l'échec total d'une fonction ODBC et les enregistrements de diagnostic fournissent des informations détaillées sur la fonction. Les enregistrements de diagnostic comportent un enregistrement d'en-tête et des enregistrements d'état. Au moins un enregistrement de diagnostic, l'enregistrement d'en-tête, est retourné même si la fonction réussit.  
  
 Les informations de diagnostic sont utilisées au moment du développement pour intercepter les erreurs de programmation, telles que les handles non valides et les erreurs de syntaxe dans les instructions SQL codées de manière irréversible. Elles sont également utilisées au moment de l'exécution pour intercepter les avertissements et les erreurs d'exécution, tels que la troncation de données, les violations de règle et les erreurs de syntaxe des instructions SQL saisies par l'utilisateur. La logique du programme repose en général sur des codes de retour.  
  
 Par exemple, après qu’une application a appelé **SQLFetch** pour extraire les lignes d’un jeu de résultats, le code de retour indique si la fin du jeu de résultats a été atteinte (SQL_NO_DATA), si des messages d’information ont été retournés (SQL_SUCCESS_WITH_INFO) ou si une erreur s’est produite (SQL_ERROR).  
  
 Si le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client retourne une valeur autre que SQL_SUCCESS, l’application peut **appeler SQLGetDiagRec** pour récupérer les messages d’information ou d’erreur. Utilisez **SQLGetDiagRec** pour faire défiler vers le haut et vers le haut l’ensemble de messages s’il y a plusieurs messages.  
  
 Le code de retour SQL_INVALID_HANDLE indique toujours une erreur de programmation et ne doit jamais se rencontrer au moment de l'exécution. Tous les autres codes de retour fournissent des informations d'exécution, bien que SQL_ERROR puisse indiquer une erreur de programmation.  
  
 L' [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] API native d’origine, DB-Library pour C, permet à une application d’installer les fonctions de gestion des erreurs de rappel et de gestion des messages qui retournent des erreurs ou des messages. Certaines instructions [!INCLUDE[tsql](../../includes/tsql-md.md)], telles que PRINT, RAISERROR, DBCC et SET, retournent leurs résultats à la fonction gestionnaire des messages de DB-Library et non dans un jeu de résultats. Toutefois, l'API ODBC n'a aucune fonction de rappel similaire. Lorsque le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client détecte des messages provenant de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il définit le code de retour odbc sur SQL_SUCCESS_WITH_INFO ou SQL_ERROR et retourne le message sous la forme d’un ou de plusieurs enregistrements de diagnostic. Par conséquent, une application ODBC doit tester avec soin ces codes de retour et appeler **SQLGetDiagRec** pour extraire les données de message.  
  
 Pour plus d’informations sur le suivi des erreurs, consultez [Suivi de l’accès aux données](/previous-versions/sql/sql-server-2008/cc765421(v=sql.100)). Pour plus d’informations sur les améliorations du suivi des erreurs ajoutées dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], consultez [Accès aux informations de diagnostic dans le journal des événements étendus](../../relational-databases/native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Traitement des instructions qui génèrent des messages](../../relational-databases/native-client-odbc-error-messages/processing-statements-that-generate-messages.md)  
  
-   [Enregistrements et champs de diagnostic](../../relational-databases/native-client-odbc-error-messages/diagnostic-records-and-fields.md)  
  
-   [Numéros des erreurs natives](../../relational-databases/native-client-odbc-error-messages/native-error-numbers.md)  
  
-   [SQLSTATE &#40;codes d’erreur ODBC&#41;](../../relational-databases/native-client-odbc-error-messages/sqlstate-odbc-error-codes.md)  
  
-   [Messages d’erreur](../../relational-databases/native-client-odbc-error-messages/error-messages.md)  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
  
