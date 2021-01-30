---
description: SQLRowCount (fonction)
title: SQLRowCount fonction) | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRowCount
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLRowCount
helpviewer_keywords:
- SQLRowCount function [ODBC]
ms.assetid: 61e00a8a-9b3b-45b9-b397-7fe818822416
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1fea56b7bf7b54bf4d1aa02efa2c4cb9ea1978e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192465"
---
# <a name="sqlrowcount-function"></a>SQLRowCount (fonction)
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLRowCount** retourne le nombre de lignes affectées par une instruction **Update**, **Insert** ou **Delete** . opération SQL_ADD, SQL_UPDATE_BY_BOOKMARK ou SQL_DELETE_BY_BOOKMARK dans **SQLBulkOperations**; ou une opération SQL_UPDATE ou SQL_DELETE dans **SQLSetPos**.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLRowCount(  
      SQLHSTMT   StatementHandle,  
      SQLLEN *   RowCountPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *RowCountPtr*  
 Sortie Pointe vers une mémoire tampon dans laquelle retourner un nombre de lignes. Pour les instructions **Update**, **Insert** et **DELETE** , pour les opérations SQL_ADD, SQL_UPDATE_BY_BOOKMARK et SQL_DELETE_BY_BOOKMARK dans **SQLBulkOperations**, et pour les opérations SQL_UPDATE ou SQL_DELETE dans **SQLSetPos**, la valeur retournée dans **RowCountPtr* est le nombre de lignes affectées par la demande ou-1 si le nombre de lignes affectées n’est pas disponible.  
  
 Lorsque **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, **SQLSetPos ou SQLMoreResults** est appelé, le champ SQL_DIAG_ROW_COUNT de la structure de données de diagnostic est défini sur le nombre de lignes et le nombre de lignes est mis en cache selon une méthode dépendante de l’implémentation. **SQLRowCount** retourne la valeur du nombre de lignes mises en cache. La valeur du nombre de lignes mises en cache est valide jusqu’à ce que le descripteur d’instruction soit rétabli à l’état préparé ou alloué, que l’instruction soit réexécutée ou que **SQLCloseCursor** soit appelé. Notez que si une fonction a été appelée depuis que le champ SQL_DIAG_ROW_COUNT a été défini, la valeur retournée par **SQLRowCount** peut être différente de la valeur dans le champ SQL_DIAG_ROW_COUNT, car le champ SQL_DIAG_ROW_COUNT est réinitialisé à 0 par un appel de fonction.  
  
 Pour les autres instructions et fonctions, le pilote peut définir la valeur retournée dans \* *RowCountPtr*. Par exemple, certaines sources de données peuvent être en mesure de retourner le nombre de lignes retournées par une instruction **Select** ou une fonction de catalogue avant d’extraire les lignes.  
  
> [!NOTE]  
>  De nombreuses sources de données ne peuvent pas retourner le nombre de lignes d’un jeu de résultats avant de les récupérer. pour une interopérabilité maximale, les applications ne doivent pas s’appuyer sur ce comportement.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLRowCount** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLRowCount** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLRowCount** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) la fonction a été appelée avant l’appel de **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** pour *StatementHandle*.<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Si la dernière instruction SQL exécutée sur le descripteur d’instruction n’est pas une instruction **Update**, **Insert** ou **Delete** , ou si l’argument *operation* dans l’appel précédent à **SQLBulkOperations** n’était pas SQL_ADD, SQL_UPDATE_BY_BOOKMARK ou SQL_DELETE_BY_BOOKMARK, ou si l’argument *operation* de l’appel précédent à **SQLSetPos** n’était pas SQL_UPDATE ou SQL_DELETE, la valeur de **RowCountPtr* est définie par le pilote. Pour plus d’informations, consultez [détermination du nombre de lignes affectées](../../../odbc/reference/develop-app/determining-the-number-of-affected-rows.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécution d’une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
