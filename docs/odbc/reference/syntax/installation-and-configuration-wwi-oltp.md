---
description: SQLSetDriverConnectInfo, fonction
title: SQLSetDriverConnectInfo fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetDriverConnectInfo function [ODBC]
ms.assetid: bfd4dfc2-fbca-4ef3-81e5-2706f2389256
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: de0a21b8fa3e620f4a6884311c10c93c36518849
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174689"
---
# <a name="sqlsetdriverconnectinfo-function"></a>SQLSetDriverConnectInfo, fonction
**Conformité**  
 Version introduite : ODBC 3,81 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLSetDriverConnectInfo** est utilisé pour définir la chaîne de connexion dans le jeton d’informations de connexion pour l’appel **SQLDriverConnect** d’une application.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp
  
SQLRETURN SQLSetDriverConnectInfo(  
                SQLHDBC_INFO_TOKEN   hDbcInfoToken,  
                WCHAR *              InConnectionString,  
                SQLSMALLINT          StringLength1 );  
```  
  
## <a name="arguments"></a>Arguments  
 *TokenHandle*  
 Entrée Handle de jeton.  
  
 *InConnectionString*  
 Entrée Une chaîne de connexion complète (consultez la syntaxe dans « Comments » dans [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)), une chaîne de connexion partielle ou une chaîne vide.  
  
 *StringLength1*  
 Entrée Longueur de **InConnectionString*, en caractères si la chaîne est Unicode, ou octets si la chaîne est ANSI ou DBCS.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Identique à [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) lié à une erreur de validation d’entrée, à ceci près que le gestionnaire de pilotes utilisera un **comme HandleType** de SQL_HANDLE_DBC_INFO_TOKEN et un **handle** de *hDbcInfoToken*.  
  
## <a name="remarks"></a>Notes  
 Chaque fois qu’un pilote retourne SQL_ERROR ou SQL_INVALID_HANDLE, le gestionnaire de pilotes renvoie l’erreur à l’application (dans [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) ou [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Chaque fois qu’un pilote retourne SQL_SUCCESS_WITH_INFO, le gestionnaire de pilotes obtient les informations de diagnostic à partir de *hDbcInfoToken* et renvoie SQL_SUCCESS_WITH_INFO à l’application dans [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) et [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Les applications ne doivent pas appeler cette fonction directement. Un pilote ODBC qui prend en charge le regroupement de connexions prenant en charge les pilotes doit implémenter cette fonction.  
  
 Incluez sqlspi. h pour le développement du pilote ODBC.  
  
## <a name="see-also"></a>Voir aussi  
 [Développement d’un pilote ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Regroupement de connexions prenant en charge les pilotes](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Développement de la reconnaissance des pools de connexions dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
