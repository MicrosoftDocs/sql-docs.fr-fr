---
description: SQLGetPoolID, fonction
title: SQLGetPoolID fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetPoolID function [ODBC]
ms.assetid: 95a8666a-ad68-4d89-bf65-f2cc797f8820
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e7bd06a289eab6499995e40a3c6ad9ea56c747e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180924"
---
# <a name="sqlgetpoolid-function"></a>SQLGetPoolID, fonction
**Conformité**  
 Version introduite : ODBC 3,81 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLGetPoolID** récupère l’ID du pool.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp
  
SQLRETURN  SQLGetPoolID (  
                SQLHDBC_INFO_TOKEN    hDbcInfoToken,  
                POOLID *              pPoolID );  
```  
  
## <a name="arguments"></a>Arguments  
 *hDbcInfoToken*  
 Entrée Handle de jeton qui contient toutes les informations de connexion.  
  
 *pPoolID*  
 Sortie L’ID du pool, qui est utilisé pour identifier un ensemble de connexions qui peuvent être utilisées de façon interchangeable (éventuellement nécessitant une réinitialisation supplémentaire).  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLGetPoolID** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, le gestionnaire de pilotes utilise un **comme HandleType** de SQL_HANDLE_DBC_INFO_TOKEN et un **handle** de *hDbcInfoToken*.  
  
## <a name="remarks"></a>Notes  
 **SQLGetPoolID** est utilisé pour obtenir l’ID du pool en fonction d’un ensemble d’informations de connexion (à partir de **SQLSetConnectAttrForDbcInfo**, **SQLSetDriverConnectInfo** et **SQLSetConnectInfo**). Cet ID de pool est utilisé pour identifier un ensemble de connexions qui peuvent être utilisées indifféremment (éventuellement nécessitant une réinitialisation supplémentaire). L’ID du pool est utilisé pour identifier le pool de connexions de ce groupe de connexions.  
  
 Chaque fois qu’un pilote retourne SQL_ERROR ou SQL_INVALID_HANDLE, le gestionnaire de pilotes renvoie l’erreur à l’application (dans [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) ou [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Chaque fois qu’un pilote retourne SQL_SUCCESS_WITH_INFO, le gestionnaire de pilotes obtient les informations de diagnostic à partir de *hDbcInfoToken* et renvoie SQL_SUCCESS_WITH_INFO à l’application dans [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) et [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Les applications ne doivent pas appeler cette fonction directement. Un pilote ODBC qui prend en charge le regroupement de connexions prenant en charge les pilotes doit implémenter cette fonction.  
  
 Incluez sqlspi. h pour le développement du pilote ODBC.  
  
## <a name="see-also"></a>Voir aussi  
 [Développement d’un pilote ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Regroupement de connexions prenant en charge les pilotes](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Développement de la reconnaissance des pools de connexions dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
