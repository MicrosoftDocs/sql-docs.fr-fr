---
description: SQLSetConnectInfo, fonction
title: SQLSetConnectInfo fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetConnectInfo function [ODBC]
ms.assetid: 0782a1c3-c5d1-499b-a8ba-134162db9990
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1930d16605c1b2a02db08560549019afa656ca52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192424"
---
# <a name="sqlsetconnectinfo-function"></a>SQLSetConnectInfo, fonction
**Conformité**  
 Version introduite : ODBC 3,81 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLSetConnectInfo** est utilisé pour définir la source de données, l’ID d’utilisateur et le mot de passe dans le jeton d’informations de connexion pour l’appel de [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) d’une application.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp
  
SQLRETURN  SQLSetConnectInfo(  
                SQLHDBC_INFO_TOKEN   TokenHandle,  
                WCHAR *              ServerName,  
                SQLSMALLINT          NameLength1,  
                WCHAR *              UserName,  
                SQLSMALLINT          NameLength2,  
                WCHAR *              Authentication,  
                SQLSMALLINT          NameLength3 );  
```  
  
## <a name="arguments"></a>Arguments  
 *TokenHandle*  
 Entrée Handle de jeton.  
  
 *ServerName*  
 Entrée Nom de la source de données. Les données peuvent se trouver sur le même ordinateur que le programme, ou sur un autre ordinateur sur un réseau. Pour plus d’informations sur la façon dont une application choisit une source de données, consultez [choix d’une source de données ou](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md)d’un pilote.  
  
 *NameLength1*  
 Entrée Longueur de **ServerName* en caractères.  
  
 *UserName*  
 Entrée Identificateur de l’utilisateur.  
  
 *NameLength2*  
 Entrée Longueur de **nom d’utilisateur* en caractères.  
  
 *Authentification*  
 Entrée Chaîne d’authentification (généralement le mot de passe).  
  
 *NameLength3*  
 Entrée Longueur de **authentification* en caractères.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Identique à [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) pour les erreurs de validation des entrées, à ceci près que le gestionnaire de pilotes utilisera un **comme HandleType** de SQL_HANDLE_DBC_INFO_TOKEN et un **handle** de *hDbcInfoToken*.  
  
## <a name="remarks"></a>Notes  
 Chaque fois qu’un pilote retourne SQL_ERROR ou SQL_INVALID_HANDLE, le gestionnaire de pilotes renvoie l’erreur à l’application (dans [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) ou [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Chaque fois qu’un pilote retourne SQL_SUCCESS_WITH_INFO, le gestionnaire de pilotes obtient les informations de diagnostic à partir de *hDbcInfoToken* et renvoie SQL_SUCCESS_WITH_INFO à l’application dans [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) et [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Les applications ne doivent pas appeler cette fonction directement. Un pilote ODBC qui prend en charge le regroupement de connexions prenant en charge les pilotes doit implémenter cette fonction.  
  
 Incluez sqlspi. h pour le développement du pilote ODBC.  
  
## <a name="see-also"></a>Voir aussi  
 [Développement d’un pilote ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Regroupement de connexions prenant en charge les pilotes](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Développement de la reconnaissance des pools de connexions dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
