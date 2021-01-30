---
description: SQLCancelHandle, fonction
title: SQLCancelHandle fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
f1_keywords:
- SQLCancelHandle
helpviewer_keywords:
- SQLCancelHandle function [ODBC]
ms.assetid: 16049b5b-22a7-4640-9897-c25dd0f19d21
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 727f9c15809969560410cfa54bdf9b70ec765f53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181044"
---
# <a name="sqlcancelhandle-function"></a>SQLCancelHandle, fonction
**Conformité**  
 Version introduite : ODBC 3,8 conformité aux normes : aucune  
  
 La plupart des pilotes ODBC 3,8 (et versions ultérieures) implémentent cette fonction. Si un pilote ne le fait pas, un appel à **SQLCancelHandle** avec un handle de connexion dans le paramètre *descripteur* retourne SQL_ERROR avec une valeur SQLSTATE de IM001 et le message « le pilote ne prend pas en charge cette fonction « » un appel à **SQLCancelHandle** avec un descripteur d’instruction, car le paramètre *handle* sera mappé à un appel à **SQLCancel** par le gestionnaire de pilotes et peut être traité si le Une application peut utiliser **SQLGetFunctions** pour déterminer si un pilote prend en charge **SQLCancelHandle**.  
  
 **Résumé**  
 **SQLCancelHandle** annule le traitement sur une connexion ou une instruction. Le gestionnaire de pilotes mappe un appel à **SQLCancelHandle** à un appel à **SQLCancel** lorsque *comme HandleType* est SQL_HANDLE_STMT.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLCancelHandle(  
      SQLSMALLINT  HandleType,  
      SQLHANDLE    Handle);  
```  
  
## <a name="arguments"></a>Arguments  
 *HandleType*  
 Entrée Type du handle sur lequel cacel le traitement. Les valeurs valides sont SQL_HANDLE_DBC ou SQL_HANDLE_STMT.  
  
 *Handle*  
 Entrée Handle sur lequel annuler le traitement.  
  
 Si *handle* n’est pas un handle valide du type spécifié par *comme HandleType*, **SQLCancelHandle** retourne SQL_INVALID_HANDLE.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLCancelHandle** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de handle d’instruction ou un *comme HandleType* de SQL_HANDLE_DBC et un *handle* de handle de connexion.  
  
 Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLCancelHandle** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par [SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md) dans l’argument *\* MessageText* buffer décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|Une fonction d’instruction en cours d’exécution asynchrone a été appelée pour l’un des descripteurs d’instruction associés au *handle*, et *comme HandleType* a la valeur SQL_HANDLE_DBC. La fonction asynchrone était toujours en cours d’exécution lors de l’appel de **SQLCancelHandle** .<br /><br /> (DM) l’argument *comme HandleType* a été SQL_HANDLE_STMT ; une fonction d’exécution asynchrone a été appelée sur le handle de connexion associé ; et la fonction était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour l’un des descripteurs d’instruction associés au *Handle* et *comme HandleType* a été défini sur SQL_HANDLE_DBC, et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> **SQLBrowseConnect** a été appelé pour *ConnectionHandle* et a retourné SQL_NEED_DATA. Cette fonction a été appelée avant la fin du processus de navigation.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY092|Identificateur d’attribut/option non valide|*Comme HandleType* a été défini sur SQL_HANDLE_ENV ou SQL_HANDLE_DESC.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *descripteur* ne prend pas en charge la fonction.|  
  
 Si **SQLCancelHandle** est appelé avec *comme handletype* défini sur SQL_HANDLE_STMT, il peut retourner tout SQLSTATE pouvant être retourné par la fonction **SQLCancel**.  
  
## <a name="comments"></a>Commentaires  
 Cette fonction est similaire à **SQLCancel** , mais peut accepter un handle de connexion ou d’instruction en tant que paramètre plutôt qu’un descripteur d’instruction. Le gestionnaire de pilotes mappe un appel à **SQLCancelHandle** à un appel à **SQLCancel** lorsque *comme HandleType* est SQL_HANDLE_STMT. Cela permet aux applications d’utiliser **SQLCancelHandle** pour annuler des opérations d’instruction, même si le pilote n’implémente pas **SQLCancelHandle**.  
  
 Pour plus d’informations sur l’annulation d’une opération d’instruction, consultez [fonction SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md).  
  
 Si aucune opération n’est en cours sur le *handle* , l’appel à **SQLCancelHandle** n’a aucun effet.  
  
 **SQLCancelHandle** sur un handle de connexion peut annuler les types de traitement suivants :  
  
-   Une fonction s’exécutant de façon asynchrone sur la connexion.  
  
-   Fonction en cours d’exécution sur le handle de connexion sur un autre thread.  
  
 Quand **SQLCancelHandle** est appelé pour annuler une fonction qui s’exécute de façon asynchrone dans une connexion, les enregistrements de diagnostic publiés par **SQLCancelHandle** sont ajoutés à ceux retournés par l’opération qui est annulée ; Toutefois, **SQLCancelHandle** ne retourne pas d’enregistrements de diagnostic lors de l’annulation d’une fonction qui s’exécute sur une connexion sur un autre thread.  
  
 L’utilisation de **SQLCancelHandle** pour annuler **SQLEndTran** peut mettre la connexion en état suspendu. Pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation de **SQLCancelHandle** dans une application qui sera déployée sur un système d’exploitation Windows antérieur à Windows 7, consultez [matrice de compatibilité](../../../odbc/reference/develop-app/compatibility-matrix.md).  
  
#### <a name="canceling-connection-related-asynchronous-processing"></a>Annulation Connection-Related traitement asynchrone  
 Si une fonction retourne SQL_STILL_EXECUTING, une application peut appeler **SQLCancelHandle** pour annuler l’opération. Si la demande d’annulation réussit, **SQLCancelHandle** retourne SQL_SUCCESS. Cela ne signifie pas que la fonction d’origine a été annulée ; elle indique que la demande d’annulation a été traitée. Le pilote et la source de données déterminent à quel moment ou si l’opération est annulée. L’application doit continuer à appeler la fonction d’origine jusqu’à ce que le code de retour ne soit pas SQL_STILL_EXECUTING. Si la fonction d’origine a été annulée, le code de retour est SQL_ERROR et SQLSTATE HY008 (opération annulée). Si la fonction d’origine a terminé son traitement normal (n’a pas été annulé), le code de retour est SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO, ou SQL_ERROR et un SQLSTATE autre que HY008 (opération annulée), si la fonction d’origine a échoué.  
  
#### <a name="canceling-functions-executing-on-another-thread"></a>Annulation de fonctions s’exécutant sur un autre thread  
 Dans une application multithread, l’application peut annuler une opération qui s’exécute sur un autre thread. Pour annuler l’opération, l’application appelle **SQLCancelHandle** avec le handle utilisé par la fonction, mais sur un thread différent. Le pilote et le système d’exploitation déterminent la façon dont l’opération est annulée. Le code de retour **SQLCancelHandle** indique si le pilote a traité la demande, en retournant SQL_SUCCESS ou SQL_ERROR (aucune information de diagnostic n’est retournée). Si le traitement sur la fonction d’origine est annulé, la fonction d’origine retourne SQL_ERROR et SQLSTATE HY008 (opération annulée).  
  
 Si une fonction est exécutée lorsque **SQLCancelHandle** est appelé sur un autre thread pour annuler la fonction, il est possible que la fonction aboutisse et retourne SQL_SUCCESS avant que l’annulation ne prenne effet. Un appel à **SQLCancelHandle** n’a aucun effet si l’opération s’est terminée avant que **SQLCancelHandle** ait pu annuler l’opération.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Annulation d’une fonction qui s’exécute de façon asynchrone sur un descripteur d’instruction, annulation d’une fonction sur une instruction qui a besoin de données ou annulation d’une fonction en cours d’exécution sur une instruction sur un autre thread.|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Exécution asynchrone (méthode d’interrogation)](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)
