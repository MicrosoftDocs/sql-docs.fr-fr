---
description: SQLGetCursorName Function
title: SQLGetCursorName, fonction | Microsoft Docs
ms.custom: ''
ms.date: 06/12/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetCursorName
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetCursorName
helpviewer_keywords:
- SQLGetCursorName function [ODBC]
ms.assetid: e6e92199-7bb6-447c-8987-049a4c6ce05d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f413bb9fb5a11a9e09b932725219e47177aa0c2e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199206"
---
# <a name="sqlgetcursorname-function"></a>SQLGetCursorName Function
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLGetCursorName** retourne le nom du curseur associé à une instruction spécifiée.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLGetCursorName(  
     SQLHSTMT        StatementHandle,  
     SQLCHAR *       CursorName,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   NameLengthPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *CursorName*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nom du curseur.  
  
 Si *CursorName* a la valeur null, *NameLengthPtr* retourne toujours le nombre total de caractères (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe *CursorName*.  
  
 *BufferLength*  
 Entrée Longueur de \* *CursorName*, en caractères. 
  
 *NameLengthPtr*  
 Sortie Pointeur vers la mémoire dans laquelle retourner le nombre total de caractères (à l’exception du caractère de fin null) disponibles à retourner dans \* *CursorName*. Si le nombre de caractères disponibles à retourner est supérieur ou égal à *BufferLength*, le nom du curseur dans \* *CursorName* est tronqué à *BufferLength* moins la longueur d’un caractère de fin null.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLGetCursorName** retourne soit SQL_ERROR, soit SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLGetCursorName** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Le \* *CursorName* de mémoire tampon n’est pas assez grand pour retourner le nom de curseur entier, donc le nom de curseur a été tronqué. La longueur du nom de curseur non tronqué est retournée dans **NameLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLGetCursorName** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY015|Aucun nom de curseur disponible|(DM) le pilote était un pilote ODBC 2 *. x* , il n’existait aucun curseur ouvert sur l’instruction, et aucun nom de curseur n’avait été défini avec **SQLSetCursorName**.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur spécifiée dans l’argument *BufferLength* est inférieure à 0.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Les noms de curseurs sont utilisés uniquement dans les instructions Update et DELETE positionnées (par exemple, **Update** _table-Name_ ... **Où Current de** _Cursor-Name_). Pour plus d’informations, consultez [instructions Update et DELETE positionnées](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md). Si l’application n’appelle pas **SQLSetCursorName** pour définir un nom de curseur, le pilote génère un nom. Ce nom commence par les lettres SQL_CUR.  
  
> [!NOTE]
>  Dans ODBC 2 *. x*, lorsqu’aucun curseur n’est ouvert et qu’aucun nom n’a été défini par un appel à **SQLSetCursorName**, un appel à **SQLGetCursorName** a retourné SQLSTATE HY015 (aucun nom de curseur disponible). Dans ODBC 3 *. x*, ce n’est plus le cas. quel que soit le moment où **SQLGetCursorName** est appelé, le pilote retourne le nom du curseur.  
  
 **SQLGetCursorName** retourne le nom d’un curseur, que le nom ait été créé explicitement ou implicitement. Un nom de curseur est généré implicitement si **SQLSetCursorName** n’est pas appelé. **SQLSetCursorName** peut être appelé pour renommer un curseur sur une instruction tant que le curseur se trouve dans un État alloué ou préparé.  
  
 Un nom de curseur défini de manière explicite ou implicite reste défini jusqu’à ce que le *StatementHandle* auquel il est associé soit supprimé, en utilisant **SQLFreeHandle** avec un *comme HandleType* de SQL_HANDLE_STMT.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécution d’une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Préparation d’une instruction pour l’exécution|[Fonction SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|Définition d’un nom de curseur|[SQLSetCursorName, fonction](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
