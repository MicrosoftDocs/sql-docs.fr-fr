---
description: SQLCompleteAsync, fonction
title: SQLCompleteAsync fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
f1_keywords:
- SQLCompleteAsync
helpviewer_keywords:
- SQLCompleteAsync function [ODBC]
ms.assetid: 1b97c46a-d2e5-4540-8239-9d975e5321c6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ad9a84bcc5f8ae716394925d4935c384d29794c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207111"
---
# <a name="sqlcompleteasync-function"></a>SQLCompleteAsync, fonction
**Conformité**  
 Version introduite : ODBC 3,8 conformité aux normes : aucune  
  
 **Résumé**  
 **SQLCompleteAsync** peut être utilisé pour déterminer quand une fonction asynchrone est terminée à l’aide de la notification ou du traitement basé sur l’interrogation. Pour plus d’informations sur les opérations asynchrones, consultez [exécution asynchrone](../../../odbc/reference/develop-app/asynchronous-execution.md).  
  
 **SQLCompleteAsync** est uniquement implémenté dans le gestionnaire de pilotes ODBC.  
  
 En mode de traitement asynchrone basé sur les notifications, **SQLCompleteAsync** doit être appelé après que le gestionnaire de pilotes a déclenché l’objet d’événement utilisé pour la notification. **SQLCompleteAsync** termine le traitement asynchrone et la fonction asynchrone génère un code de retour.  
  
 En mode de traitement asynchrone basé sur les interrogations, **SQLCompleteAsync** est une alternative à l’appel de la fonction asynchrone d’origine, sans qu’il soit nécessaire de spécifier les arguments dans l’appel de fonction asynchrone d’origine. **SQLCompleteAsync** peut être utilisé, que la bibliothèque de curseurs ODBC soit activée ou non.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLCompleteAsync(  
      SQLSMALLINT HandleType,  
      SQLHANDLE   Handle,  
      RETCODE *   AsyncRetCodePtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *HandleType*  
 Entrée Type du descripteur sur lequel effectuer le traitement asynchrone. Les valeurs valides sont SQL_HANDLE_DBC ou SQL_HANDLE_STMT.  
  
 *Handle*  
 Entrée Handle sur lequel effectuer le traitement asynchrone. Si *handle* n’est pas un handle valide du type spécifié par *comme HandleType*, **SQLCompleteAsync** retourne SQL_INVALID_HANDLE.  
  
 Si *handle* n’est pas un handle valide du type spécifié par *comme HandleType*, **SQLCompleteAsync** retourne SQL_INVALID_HANDLE.  
  
 *AsyncRetCodePtr*  
 Sortie Pointeur vers une mémoire tampon qui contient le code de retour de l’API asynchrone. Si *AsyncRetCodePtr* a la valeur null, **SQLCompleteAsync** retourne SQL_ERROR.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_ERROR, SQL_NO_DATA ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Si **SQLCompleteAsync** retourne SQL_SUCCESS, une application doit obtenir le code de retour de la fonction asynchrone à partir de la mémoire tampon pointée par *AsyncRetCodePtr*. Le SQLSTATE associé, le cas échéant, peut être obtenu en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un descripteur d’instruction ou un *comme HandleType* de SQL_HANDLE_DBC et un handle de connexion. Ces enregistrements de diagnostic sont associés à la fonction asynchrone, et non à cette fonction **SQLCompleteAsync** .  
  
 **SQLCompleteAsync** retourne un code autre que SQL_SUCCESS pour indiquer que **SQLCompleteAsync** n’est pas appelé correctement. Dans ce cas, **SQLCompleteAsync** ne publie aucun enregistrement de diagnostic. Les codes de retour possibles sont les suivants :  
  
-   SQL_INVALID_HANDLE : le handle indiqué par *comme HandleType* et *handle* n’est pas un handle valide.  
  
-   SQL_ERROR : *AsyncRetCodePtr* est null ou le traitement asynchrone n’est pas activé sur le descripteur.  
  
-   SQL_NO_DATA : en mode de notification, une opération asynchrone n’est pas en cours ou le gestionnaire de pilotes n’a pas notifié l’application. En mode d’interrogation, une opération asynchrone n’est pas en cours.  
  
## <a name="comments"></a>Commentaires  
 En mode de traitement asynchrone basé sur les interrogations, *AsyncRetCodePtr* peut être SQL_STILL_EXECUTING lorsque **SQLCompleteAsync** retourne SQL_SUCCESS. L’application doit continuer à interroger jusqu’à ce que *AsyncRetCodePtr* ne soit pas SQL_STILL_EXECUTING. En mode de traitement asynchrone basé sur les notifications, *AsyncRetCodePtr* ne sera jamais SQL_STILL_EXECUTING.  
  
## <a name="see-also"></a>Voir aussi  
 [Exécution asynchrone (méthode d’interrogation)](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)
