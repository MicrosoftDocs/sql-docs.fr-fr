---
description: SQLPoolConnect, fonction
title: SQLPoolConnect fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLPoolConnect function [ODBC]
ms.assetid: 41322737-890d-4a81-aed2-06cc3d546962
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8317e159a5a438bb22e8077f3a6635a79f9baefd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204557"
---
# <a name="sqlpoolconnect-function"></a>SQLPoolConnect, fonction
**Conformité**  
 Version introduite : ODBC 3,8 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLPoolConnect** est utilisé pour créer une nouvelle connexion si aucune connexion dans le pool ne peut être réutilisée.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp
  
SQLRETURN  SQLPoolConnect(  
                SQLHDBC              hDbc,  
                SQLHDBC_INFO_TOKEN   hDbcInfoToken,  
                WCHAR *              wszOutConnectString,  
                SQLSMALLINT          cchConnectStringBuffer,  
                SQLSMALLINT *        cchConnectStringLen );  
```  
  
## <a name="arguments"></a>Arguments  
 *hDbc*  
 Entrée Handle de connexion.  
  
 *hDbcInfoToken*  
 Entrée Handle de jeton pour la nouvelle demande de connexion d’application.  
  
 *wszOutConnectString*  
 Sortie Pointeur vers une mémoire tampon pour la chaîne de connexion terminée. Une fois la connexion établie à la source de données cible, cette mémoire tampon contient la chaîne de connexion terminée. Les applications doivent allouer au moins 1 024 caractères pour cette mémoire tampon.  
  
 Si *wszOutConnectString* a la valeur null, *cchConnectStringLen* retourne toujours le nombre total de caractères (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe *wszOutConnectString*.  
  
 *cchConnectStringBuffer*  
 Entrée Longueur de la mémoire tampon **wszOutConnectString* , en caractères.  
  
 *cchConnectStringLen*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre total de caractères (à l’exception du caractère de fin null) disponibles à retourner dans \* *wszOutConnectString*. Si le nombre de caractères disponibles à retourner est supérieur ou égal à *cchConnectStringBuffer*, la chaîne de connexion terminée dans \* *wszOutConnectString* est tronquée à *cchConnectStringBuffer* moins la longueur d’un caractère de fin null.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Semblable à [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) pour toute erreur de validation d’entrée, à ceci près que le gestionnaire de pilotes utilisera un **comme HandleType** de SQL_HANDLE_DBC_INFO_TOKEN et un **handle** de *hDbcInfoToken*.  
  
## <a name="remarks"></a>Notes  
 Le gestionnaire de pilotes garantit que le handle HENV parent de *hDbc* et *hDbcInfoToken* est identique.  
  
 Contrairement à [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md), il n’existe aucun argument *DriverCompletion* pour inviter les utilisateurs à entrer des informations de connexion. Une boîte de dialogue d’invite de commandes n’est pas autorisée dans le scénario de regroupement.  
  
 Les applications ne doivent pas appeler cette fonction directement. Un pilote ODBC qui prend en charge le regroupement de connexions prenant en charge les pilotes doit implémenter cette fonction.  
  
 Chaque fois qu’un pilote retourne SQL_ERROR ou SQL_INVALID_HANDLE, le gestionnaire de pilotes renvoie l’erreur à l’application (dans [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) ou [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Chaque fois qu’un pilote retourne SQL_SUCCESS_WITH_INFO, le gestionnaire de pilotes obtient les informations de diagnostic à partir de *hDbcInfoToken* et renvoie SQL_SUCCESS_WITH_INFO à l’application dans [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) et [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Quand une application utilise [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md), *wszOutConnectString* est une mémoire tampon null (les trois derniers paramètres ont tous la valeur NULL, 0, null). Dans le cas contraire, le pilote doit retourner la chaîne de connexion de sortie, qui sera retournée à l’appel de [fonction SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) de l’application.  
  
 Incluez sqlspi. h pour le développement du pilote ODBC.  
  
## <a name="see-also"></a>Voir aussi  
 [Développement d’un pilote ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Regroupement de connexions prenant en charge les pilotes](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Développement de la reconnaissance des pools de connexions dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
