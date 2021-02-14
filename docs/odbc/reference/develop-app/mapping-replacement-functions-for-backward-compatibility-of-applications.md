---
description: Mappage des fonctions de remplacement pour la compatibilité descendante des applications
title: Mappage des fonctions de remplacement pour la compatibilité des applications-ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- mapping replacement functions [ODBC]
- upgrading applications [ODBC], mapping replacement functions
- compatibility [ODBC], mapping replacement functions
- ODBC drivers [ODBC], backward compatibility
- functions [ODBC], mapping replacement functions
- application upgrades [ODBC], mapping replacement functions
- backward compatibility [ODBC], mapping replacement functions
ms.assetid: f5e6d9da-76ef-42cb-b3f5-f640857df732
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e4c31d523a9fb50dd45bf8ab56c551d3830b0861
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082160"
---
# <a name="mapping-replacement-functions-for-backward-compatibility-of-applications"></a>Mappage des fonctions de remplacement pour la compatibilité descendante des applications
Une application ODBC *3. x* utilisant le gestionnaire de pilotes ODBC *3. x* fonctionnera sur un pilote ODBC *2. x* tant qu’aucune nouvelle fonctionnalité ne sera utilisée. Les modifications de fonctionnalités et de comportements en double affectent cependant la manière dont l’application ODBC *3. x* fonctionne sur un pilote ODBC *2. x* . Lorsque vous utilisez un pilote ODBC *2. x* , le gestionnaire de pilotes mappe les fonctions ODBC *3. x* suivantes, qui ont remplacé une ou plusieurs fonctions ODBC *2. x* dans les fonctions ODBC *2. x* correspondantes.  
  
|ODBC *3. x,* fonction|ODBC *2. x,* fonction|  
|-------------------------|-------------------------|  
|**SQLAllocHandle**|**SQLAllocEnv**, **SQLAllocConnect** ou **SQLAllocStmt,**|  
|**SQLBulkOperations**|**SQLSetPos**|  
|**SQLColAttribute**|**SQLColAttributes**|  
|**SQLEndTran**|**SQLTransact**|  
|**SQLFetch**|**SQLExtendedFetch**|  
|**SQLFetchScroll**|**SQLExtendedFetch**|  
|**SQLFreeHandle**|**Sqlfreeenv,**, **sqlfreeconnect,** ou **SQLFreeStmt**|  
|**SQLGetConnectAttr**|**Sqlgetconnectoption,**|  
|**SQLGetDiagRec**|**SQLError**|  
|**SQLGetStmtAttr**|**SQLGetStmtOption**[1]|  
|**SQLSetConnectAttr**|**SQLSetConnectOption**|  
|**SQLSetStmtAttr**|**SQLSetStmtOption**[1]|  
  
 [1] d’autres actions peuvent également être effectuées, en fonction de l’attribut demandé.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
 Le gestionnaire de pilotes mappe ce à **SQLAllocEnv**, **SQLAllocConnect** ou **SQLAllocStmt,**, selon le cas. L’appel suivant à **SQLAllocHandle**:  
  
```  
SQLAllocHandle(HandleType, InputHandle, OutputHandlePtr);  
```  
  
 permet au gestionnaire de pilotes d’effectuer les opérations suivantes (conceptuelles, pas de vérification des erreurs) :  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV: return (SQLAllocEnv(OutputHandlePtr));  
   case SQL_HANDLE_DBC: return (SQLAllocConnect (InputHandle, OutputHandlePtr));  
   case SQL_HANDLE_STMT: return (SQLAllocStmt (InputHandle, OutputHandlePtr));  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlbulkoperations"></a>SQLBulkOperations  
 Le gestionnaire de pilotes mappe ce à **SQLSetPos**. L’appel suivant à **SQLBulkOperations**:  
  
```  
SQLBulkOperations(hstmt, Operation);  
```  
  
 se traduira par la séquence d’étapes suivante :  
  
1.  Si l’argument Operation est SQL_ADD, le gestionnaire de pilotes appelle **SQLSetPos** comme suit :  
  
    ```  
    SQLSetPos (hstmt, 0, SQL_ADD, SQL_LOCK_NO_CHANGE);  
    ```  
  
2.  Si l’argument d’opération n’est pas SQL_ADD, le pilote retourne SQLSTATE HY092 (identificateur d’attribut/option non valide).  
  
3.  Si l’application tente de modifier le SQL_ATTR_ROW_STATUS_PTR entre les appels à **SQLFetch** ou **SQLFetchScroll** et **SQLBulkOperations**, le gestionnaire de pilotes retourne SQLState HY011 (l’attribut ne peut pas être défini maintenant).  
  
4.  Si l’argument d’opération est SQL_ADD, l’application doit appeler **SQLBindCol** pour lier les données à insérer. Il ne peut pas appeler **SQLSetDescField** ou **SQLSetDescRec** pour lier les données à insérer.  
  
5.  Si l’argument Operation est SQL_ADD et que le nombre de lignes à insérer n’est pas le même que la taille de l’ensemble de lignes en cours, **SQLSetStmtAttr** doit être appelé pour affecter à l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE le nombre de lignes à insérer avant d’appeler **SQLBulkOperations**. Pour revenir à la taille de l’ensemble de lignes précédent, l’application doit définir l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE avant l’appel de **SQLFetch**, **SQLFetchScroll** ou **SQLSetPos** .  
  
## <a name="sqlcolattribute"></a>SQLColAttribute  
 Le gestionnaire de pilotes mappe ce à **SQLColAttributes**. L’appel suivant à **SQLColAttribute**:  
  
```  
SQLColAttribute(StatementHandle, ColumnNumber, FieldIdentifier, CharacterAttributePtr, BufferLength, StringLengthPtr, NumericAttributePtr);  
```  
  
 se traduira par la séquence d’étapes suivante :  
  
1.  Si *FieldIdentifier* est l’un des éléments suivants :  
  
     SQL_DESC_PRECISION, SQL_DESC_SCALE, SQL_DESC_LENGTH, SQL_DESC_OCTET_LENGTH, SQL_DESC_UNNAMED, SQL_DESC_BASE_COLUMN_NAME, SQL_DESC_LITERAL_PREFIX, SQL_DESC_LITERAL_SUFFIX ou SQL_DESC_LOCAL_TYPE_NAME  
  
     le gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY091 (identificateur de champ de descripteur non valide). Aucune autre règle de cette section ne s’applique.  
  
2.  Le gestionnaire de pilotes mappe SQL_COLUMN_COUNT, SQL_COLUMN_NAME ou SQL_COLUMN_NULLABLE à SQL_DESC_COUNT, SQL_DESC_NAME ou SQL_DESC_NULLABLE, respectivement. (Un pilote ODBC *2. x* doit uniquement prendre en charge SQL_COLUMN_COUNT, SQL_COLUMN_NAME et SQL_COLUMN_NULLABLE, et non SQL_DESC_COUNT, SQL_DESC_NAME et SQL_DESC_NULLABLE.) L’appel à SQLColAttribute est mappé à :  
  
    ```  
    SQLColAttributes(StatementHandle, ColumnNumber, FieldIdentifier, CharacterAttributePtr, BufferLength, StringLengthPtr, NumericAttributePtr);  
    ```  
  
3.  Toutes les autres valeurs *FieldIdentifier* sont transmises au pilote, avec **SQLColAttribute** mappé à **SQLColAttributes** , comme indiqué précédemment.  
  
4.  Si *BufferLength* est inférieur à 0, le gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY090 (longueur de chaîne ou de mémoire tampon non valide). Aucune autre règle de cette section ne s’applique.  
  
5.  Si *FieldIdentifier* est SQL_DESC_CONCISE_TYPE et que le type retourné est un type de données DateTime concis, le gestionnaire de pilotes mappe les valeurs de retour pour les codes de date, d’heure et d’horodatage.  
  
6.  Le gestionnaire de pilotes effectue les vérifications nécessaires pour déterminer si SQLSTATE HY010 (erreur de séquence de fonction) doit être déclenché. Dans ce cas, le gestionnaire de pilotes retourne SQL_ERROR et SQLSTATE HY010 (erreur de séquence de fonction). Aucune autre règle de cette section ne s’applique.  
  
## <a name="sqlendtran"></a>SQLEndTran  
 Le gestionnaire de pilotes mappe ce à **SQLTransact**. L’appel suivant à **SQLEndTran**:  
  
```  
SQLEndTran(HandleType, Handle, CompletionType);  
```  
  
 permet au gestionnaire de pilotes d’effectuer les opérations suivantes (conceptuelles, pas de vérification des erreurs) :  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV:return(SQLTransact(Handle, SQL_NULL_HDBC, CompletionType));  
   case SQL_HANDLE_DBC:return(SQLTransact(SQL_NULL_HENV, Handle, CompletionType);  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlfetch"></a>SQLFetch  
 Le gestionnaire de pilotes mappe ce à **SQLExtendedFetch** avec un argument *FetchOrientation* de SQL_FETCH_NEXT. L’appel suivant à **SQLFetch**:  
  
```  
SQLFetch (StatementHandle);  
```  
  
 entraîne l’appel de **SQLExtendedFetch** par le gestionnaire de pilotes, comme suit :  
  
```  
rc = SQLExtendedFetch(StatementHandle, FetchOrientation, FetchOffset, &RowCount, RowStatusArray);  
```  
  
 Dans cet appel, l’argument *pcRow* est défini sur la valeur que l’application affecte à l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR par le biais d’un appel à **SQLSetStmtAttr**.  
  
> [!NOTE]  
>  Lorsque l’application appelle **SQLSetStmtAttr** pour définir SQL_ATTR_ROW_STATUS_PTR de façon à ce qu’elle pointe vers un tableau d’État, le gestionnaire de pilotes met en cache le pointeur. *RowStatusArray* peut être égal à un pointeur null.  
  
 Si le pilote ne prend pas en charge **SQLExtendedFetch** et que la bibliothèque de curseurs est chargée, le gestionnaire de pilotes utilise le **SQLExtendedFetch** de la bibliothèque de curseurs pour mapper **SQLFetch** à **SQLExtendedFetch**. Si le pilote ne prend pas en charge **SQLExtendedFetch** et que la bibliothèque de curseurs n’est pas chargée, le gestionnaire de pilotes passe l’appel de **SQLFetch** à le pilote. Si l’application appelle **SQLSetStmtAttr** pour définir SQL_ATTR_ROW_STATUS_PTR, le gestionnaire de pilotes s’assure que le tableau est rempli. Si l’application appelle **SQLSetStmtAttr** pour définir SQL_ATTR_ROWS_FETCHED_PTR, le gestionnaire de pilotes définit ce champ sur 1.  
  
## <a name="sqlfetchscroll"></a>SQLFetchScroll  
 Le gestionnaire de pilotes mappe ce à **SQLExtendedFetch**. L’appel suivant à **SQLFetchScroll**:  
  
```  
SQLFetchScroll(StatementHandle, FetchOrientation, FetchOffset);  
```  
  
 se traduira par la séquence d’étapes suivante :  
  
1.  Lorsque l’application appelle **SQLSetStmtAttr** pour définir SQL_ATTR_ROW_STATUS_PTR (qui définit le champ SQL_DESC_ARRAY_STATUS_PTR dans IRD) de façon à ce qu’elle pointe vers un tableau d’État, le gestionnaire de pilotes met en cache ce pointeur. Laissez ce pointeur *RowStatusArray*. dans le cas contraire, laissez *RowStatusArray* égal à un pointeur null. Si l’argument *RowStatusArray* est défini sur un pointeur null, le gestionnaire de pilotes génère un tableau d’état de ligne.  
  
2.  Si *FetchOrientation* n’est pas l’un des SQL_FETCH_NEXT, SQL_FETCH_PRIOR, SQL_FETCH_ABSOLUTE, SQL_FETCH_RELATIVE, SQL_FETCH_FIRST, SQL_FETCH_LAST ou SQL_FETCH_BOOKMARK, le gestionnaire de pilotes retourne avec SQL_ERROR et SQLSTATE HY106 (type d’extraction hors limites). Aucune autre règle de cette section ne s’applique.  
  
3.  Casse :  
  
-   Si *FetchOrientation* est égal à SQL_FETCH_BOOKMARK, alors :  
  
    -   Si **SQLSetStmtAttr** a été appelé précédemment pour définir la valeur de SQL_ATTR_FETCH_BOOKMARK_PTR, laissez *BMK* la valeur obtenue en déréférençant le pointeur SQL_DESC_FETCH_BOOKMARK_PTR.  
  
    -   Sinon, retournez SQL_ERROR avec SQLSTATE HY111 (valeur de signet non valide). Aucune autre règle de cette section ne s’applique.  
  
     Désormais, le gestionnaire de pilotes appelle **SQLExtendedFetch**, comme suit :  
  
    ```  
    rc = SQLExtendedFetch(StatementHandle, FetchOrientation, Bmk, pcRow, RowStatusArray);  
    ```  
  
-   Dans le cas contraire, le gestionnaire de pilotes appelle **SQLExtendedFetch**, comme suit :  
  
    ```  
    rc = SQLExtendedFetch(StatementHandle, FetchOrientation, FetchOffset, pcRow, RowStatusArray);  
    ```  
  
     Dans ces appels, l’argument *pcRow* est défini sur la valeur que l’application affecte à l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR par le biais d’un appel à **SQLSetStmtAttr**.  
  
-   SQL_ATTR_ROW_ARRAY_SIZE est mappé à SQL_ROWSET_SIZE.  
  
-   Si *RC* est égal à SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO, et si *FetchOrientation* est égal à SQL_FETCH_BOOKMARK et *FetchOffset* n’est pas égal à 0, le gestionnaire de pilotes publie un avertissement, SQLSTATE 01S10 (tentative de récupération par un décalage de signet, valeur de décalage ignorée) et retourne SQL_SUCCESS_WITH_INFO.  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
 Le gestionnaire de pilotes mappe cela à **sqlfreeenv,**, **sqlfreeconnect,** ou **SQLFreeStmt** , selon le cas. L’appel suivant à **SQLFreeHandle**:  
  
```  
SQLFreeHandle(HandleType, Handle);  
```  
  
 permet au gestionnaire de pilotes d’effectuer les opérations suivantes (conceptuelles, pas de vérification des erreurs) :  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV: return (SQLFreeEnv(Handle));  
   case SQL_HANDLE_DBC: return (SQLFreeConnect(Handle));  
   case SQL_HANDLE_STMT: return (SQLFreeStmt(Handle, SQL_DROP));  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
 Le gestionnaire de pilotes mappe ce à **sqlgetconnectoption,**. L’appel suivant à **SQLGetConnectAttr**:  
  
```  
SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength, StringLengthPtr);  
```  
  
 se traduira par la séquence d’étapes suivante :  
  
1.  Si l' *attribut* n’est pas un attribut de connexion ou d’instruction défini par le pilote et qu’il n’est pas un attribut défini dans ODBC *2. x*, le gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’option/d’attribut non valide). Aucune autre règle de cette section ne s’applique.  
  
2.  Si l' *attribut* est égal à SQL_ATTR_AUTO_IPD ou SQL_ATTR_METADATA_ID, le gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’option/attribut non valide).  
  
3.  Le gestionnaire de pilotes effectue les vérifications nécessaires pour déterminer si SQLSTATE 08003 (connexion non ouverte) ou SQLSTATE HY010 (erreur de séquence de fonction) doit être déclenché. Dans ce cas, le gestionnaire de pilotes retourne SQL_ERROR et publie le message d’erreur approprié. Aucune autre règle de cette section ne s’applique.  
  
4.  Le gestionnaire de pilotes appelle **sqlgetconnectoption,** comme suit :  
  
    ```  
    SQLGetConnectOption (ConnectionHandle, Attribute, ValuePtr);  
    ```  
  
     Notez que les *BufferLength* et *StringLengthPtr* sont ignorés.  
  
## <a name="sqlgetdata"></a>SQLGetData  
 Quand une application ODBC *3. x* qui utilise un pilote ODBC *2. x* appelle **SQLGetData** avec l’argument *ColumnNumber* égal à 0, le gestionnaire de pilotes ODBC *3. x* mappe ce à un appel à **SQLGetStmtOption** avec l’attribut *option* défini sur SQL_GET_BOOKMARK.  
  
## <a name="sqlgetstmtattr"></a>SQLGetStmtAttr  
 Le gestionnaire de pilotes mappe ce à **SQLGetStmtOption**. L’appel suivant à **SQLGetStmtAttr**:  
  
```  
SQLGetStmtAttr(StatementHandle, Attribute, ValuePtr, BufferLength, StringLengthPtr);  
```  
  
 se traduira par la séquence d’étapes suivante :  
  
1.  Si l' *attribut* n’est pas un attribut de connexion ou d’instruction défini par le pilote et qu’il n’est pas un attribut défini dans ODBC *2. x*, le gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’option/d’attribut non valide). Aucune autre règle de cette section ne s’applique.  
  
2.  Si l' *attribut* est l’un des éléments suivants :  
  
     SQL_ATTR_APP_ROW_DESC  
  
     SQL_ATTR_APP_PARAM_DESC  
  
     SQL_ATTR_AUTO_IPD  
  
     SQL_ATTR_ROW_BIND_TYPE  
  
     SQL_ATTR_IMP_ROW_DESC  
  
     SQL_ATTR_IMP_PARAM_DESC  
  
     SQL_ATTR_METADATA_ID  
  
     SQL_ATTR_PARAM_BIND_TYPE  
  
     SQL_ATTR_PREDICATE_PTR  
  
     SQL_ATTR_PREDICATE_OCTET_LENGTH_PTR  
  
     SQL_ATTR_PARAM_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_OPERATION_PTR  
  
     SQL_ATTR_PARAM_OPERATION_PTR  
  
     le gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’attribut/option non valide). Aucune autre règle de cette section ne s’applique.  
  
3.  Le gestionnaire de pilotes effectue les vérifications nécessaires pour déterminer si SQLSTATE HY010 (erreur de séquence de fonction) doit être déclenché. Dans ce cas, le gestionnaire de pilotes retourne SQL_ERROR et SQLSTATE HY010 (erreur de séquence de fonction). Aucune autre règle de cette section ne s’applique.  
  
4.  Si l' *attribut* est égal à SQL_ATTR_ROWS_FETCHED_PTR, le gestionnaire de pilotes retourne un pointeur vers la variable du gestionnaire de pilotes interne *cRow*, qu’elle utilise ou utilisera dans un appel à **SQLExtendedFetch**. Aucune autre règle de cette section ne s’applique.  
  
5.  Si l' *attribut* est égal à SQL_DESC_FETCH_BOOKMARK_PTR, le gestionnaire de pilotes retourne le pointeur approprié qu’il avait mis en cache pendant un appel à **SQLSetStmtAttr**.  
  
6.  Si l' *attribut* est égal à SQL_ATTR_ROW_STATUS_PTR, le gestionnaire de pilotes retourne le pointeur approprié qu’il avait mis en cache pendant un appel à **SQLSetStmtAttr**.  
  
7.  Le gestionnaire de pilotes appelle **SQLGetStmtOption** comme suit :  
  
    ```  
    SQLGetStmtOption (hstmt, fOption, pvParam);  
    ```  
  
     où *HSTMT*, *fOption* et *pvParam* seront définis respectivement sur les valeurs de *StatementHandle*, *attribute* et *ValuePtr*. *BufferLength* et *StringLengthPtr* sont ignorés.  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
 Le gestionnaire de pilotes mappe ce à **SQLSetConnectOption**. L’appel suivant à **SQLSetConnectAttr**:  
  
```  
SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, StringLength);  
```  
  
 se traduira par la séquence d’étapes suivante :  
  
1.  Si l' *attribut* n’est pas un attribut de connexion ou d’instruction défini par le pilote et qu’il n’est pas un attribut défini dans ODBC *2. x*, le gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’option/d’attribut non valide). Aucune autre règle de cette section ne s’applique.  
  
2.  Si l' *attribut* est égal à SQL_ATTR_AUTO_IPD, le gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’attribut/option non valide).  
  
3.  Le gestionnaire de pilotes effectue les vérifications nécessaires pour déterminer si SQLSTATE 08003 (connexion non ouverte) ou SQLSTATE HY010 (erreur de séquence de fonction) doit être déclenché. Si l’une de ces erreurs doit être déclenchée, le gestionnaire de pilotes retourne SQL_ERROR et publie le message d’erreur approprié. Aucune autre règle de cette section ne s’applique.  
  
4.  Le gestionnaire de pilotes appelle **SQLSetConnectOption** comme suit :  
  
    ```  
    SQLSetConnectOption (hdbc, fOption, vParam);  
    ```  
  
     où *hdbc*, *fOption* et *vParam* sont définis sur les valeurs de *ConnectionHandle*, *attribute* et *ValuePtr*, respectivement. *StringLengthPtr* est ignoré.  
  
> [!NOTE]  
>  La possibilité de définir des attributs d’instruction sur le niveau de connexion est dépréciée. Les attributs d’instruction ne doivent jamais être définis au niveau de la connexion par une application ODBC *3. x* .  
  
## <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
 Le gestionnaire de pilotes mappe ce à **SQLSetStmtOption**. L’appel suivant à **SQLSetStmtAttr**:  
  
```  
SQLSetStmtAttr(StatementHandle, Attribute, ValuePtr, StringLength);  
```  
  
 se traduira par la séquence d’étapes suivante :  
  
1.  Si l' *attribut* n’est pas un attribut de connexion ou d’instruction défini par le pilote et qu’il n’est pas un attribut défini dans ODBC *2. x*, le gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’option/d’attribut non valide). Aucune autre règle de cette section ne s’applique.  
  
2.  Si l' *attribut* est l’un des éléments suivants :  
  
     SQL_ATTR_APP_ROW_DESC  
  
     SQL_ATTR_APP_PARAM_DESC  
  
     SQL_ATTR_AUTO_IPD  
  
     SQL_ATTR_ROW_BIND_TYPE  
  
     SQL_ATTR_IMP_ROW_DESC  
  
     SQL_ATTR_IMP_PARAM_DESC  
  
     SQL_ATTR_METADATA_ID  
  
     SQL_ATTR_PARAM_BIND_TYPE  
  
     SQL_ATTR_PREDICATE_PTR  
  
     SQL_ATTR_PREDICATE_OCTET_LENGTH_PTR  
  
     SQL_ATTR_PARAM_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_OPERATION_PTR  
  
     SQL_ATTR_PARAM_OPERATION_PTR  
  
     le gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’attribut/option non valide). Aucune autre règle de cette section ne s’applique.  
  
3.  Le gestionnaire de pilotes effectue les vérifications nécessaires pour déterminer si SQLSTATE HY010 (erreur de séquence de fonction) doit être déclenché. Dans ce cas, le gestionnaire de pilotes retourne SQL_ERROR et SQLSTATE HY010 (erreur de séquence de fonction). Aucune autre règle de cette section ne s’applique.  
  
4.  Si l' *attribut* est égal à SQL_ATTR_PARAMSET_SIZE ou SQL_ATTR_PARAMS_PROCESSED_PTR, consultez la section « mappages pour la gestion des tableaux de paramètres », plus loin dans cette rubrique. Aucune autre règle de cette section ne s’applique.  
  
5.  Si l' *attribut* est égal à SQL_ATTR_ROWS_FETCHED_PTR, le gestionnaire de pilotes met en cache la valeur du pointeur pour une utilisation ultérieure avec **SQLFetchScroll**.  
  
6.  Si l' *attribut* est égal à SQL_ATTR_ROW_STATUS_PTR, le gestionnaire de pilotes met en cache la valeur du pointeur pour une utilisation ultérieure avec **SQLFetchScroll** ou **SQLSetPos**. Aucune autre règle de cette section ne s’applique.  
  
7.  Si l' *attribut* est égal à SQL_ATTR_FETCH_BOOKMARK_PTR, le gestionnaire de pilotes met en cache *ValuePtr* et utilise la valeur mise en cache ultérieurement dans un appel à **SQLFetchScroll**. Aucune autre règle de cette section ne s’applique.  
  
8.  Le gestionnaire de pilotes appelle **SQLSetStmtOption** comme suit :  
  
    ```  
    SQLSetStmtOption (hstmt, fOption, vParam);  
    ```  
  
     où *HSTMT*, *fOption* et *vParam* seront définis sur les valeurs de *StatementHandle*, *attribute* et *ValuePtr*, respectivement. L’argument *StringLength* est ignoré.  
  
     Si un pilote ODBC *2. x* prend en charge les options de chaîne de caractères, les instructions spécifiques au pilote, une application ODBC *3. x* doit appeler **SQLSetStmtOption** pour définir ces options.  
  
## <a name="mappings-for-handling-parameter-arrays"></a>Mappages pour la gestion des tableaux de paramètres  
 Lorsque l’application appelle :  
  
```  
SQLSetStmtAttr (StatementHandle, SQL_ATTR_PARAMSET_SIZE, Size, StringLength);  
```  
  
 le gestionnaire de pilotes appelle :  
  
```  
SQLParamOptions (StatementHandle, Size, &RowCount);  
```  
  
 Le gestionnaire de pilotes retourne ensuite un pointeur vers cette variable lorsque l’application appelle **SQLGetStmtAttr** pour récupérer SQL_ATTR_PARAMS_PROCESSED_PTR. Le gestionnaire de pilotes ne peut pas modifier cette variable interne tant que le descripteur d’instruction n’est pas retourné à l’État prepared ou allocated.  
  
 Une application ODBC *3. x* peut appeler **SQLGetStmtAttr** pour obtenir la valeur de SQL_ATTR_PARAMS_PROCESSED_PTR même s’il n’a pas défini explicitement le champ SQL_DESC_ARRAY_SIZE dans le APD. Cette situation peut survenir, par exemple, si l’application a une routine générique qui vérifie la « ligne » actuelle des paramètres en cours de traitement lorsque **SQLExecute** retourne SQL_NEED_DATA. Cette routine est appelée que le SQL_DESC_ARRAY_SIZE soit ou non égal à 1 ou supérieur à 1. Pour tenir compte de cela, le gestionnaire de pilotes doit définir cette variable interne, que l’application ait ou non appelé **SQLSetStmtAttr** pour définir le champ SQL_DESC_ARRAY_SIZE dans APD. Si SQL_DESC_ARRAY_SIZE n’a pas été défini, le gestionnaire de pilotes doit s’assurer que cette variable contient la valeur 1 avant de retourner **SQLExecDirect** ou **SQLExecute**.  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Dans ODBC *3. x*, l’appel de **SQLFetch** ou **SQLFETCHSCROLL** remplit le SQL_DESC_ARRAY_STATUS_PTR dans IRD, et le champ SQL_DIAG_ROW_NUMBER d’un enregistrement de diagnostic donné contient le numéro de la ligne de l’ensemble de lignes auquel cet enregistrement appartient. À l’aide de cela, l’application peut mettre en corrélation un message d’erreur avec une position de ligne donnée.  
  
 Un pilote ODBC *2. x* ne pourra pas fournir cette fonctionnalité. Toutefois, elle fournira la délimitation des erreurs avec SQLSTATE 01S01 (erreur dans la ligne). Une application ODBC *3. x* qui utilise **SQLFetch** ou **SQLFetchScroll** tout en accédant à un pilote ODBC *2. x* doit être consciente de ce fait. Notez également qu’une telle application n’est pas en mesure d’appeler **SQLGetDiagField** pour réellement récupérer le champ SQL_DIAG_ROW_NUMBER. Une application ODBC *3. x* fonctionnant avec un pilote ODBC *2. x* pourra appeler **SQLGetDiagField** uniquement avec un argument *DiagIdentifier* de SQL_DIAG_MESSAGE_TEXT, SQL_DIAG_NATIVE, SQL_DIAG_RETURNCODE ou SQL_DIAG_SQLSTATE. Le gestionnaire de pilotes ODBC *3. x* gère la structure des données de diagnostic lors de l’utilisation d’un pilote ODBC *2. x* , mais le pilote ODBC *2. x* retourne uniquement ces quatre champs.  
  
 Lorsqu’une application ODBC *2. x* utilise un pilote ODBC *2. x* , si une opération peut provoquer le renvoi de plusieurs erreurs par le gestionnaire de pilotes, des erreurs différentes peuvent être retournées par le gestionnaire de pilotes ODBC *3. x* par rapport à ODBC *2. x* Driver Manager.  
  
## <a name="mappings-for-bookmark-operations"></a>Mappages pour les opérations de signet  
 Le gestionnaire de pilotes ODBC *3. x* effectue les mappages suivants lorsqu’une application ODBC *3. x* qui utilise un pilote ODBC *2. x* effectue des opérations de signet.  
  
### <a name="sqlbindcol"></a>SQLBindCol  
 Lorsqu’une application *ODBC 3. x* qui utilise un pilote *ODBC 2. x* appelle **SQLBindCol** pour établir une liaison à la colonne 0 avec *fCType* égal à SQL_C_VARBOOKMARK, le gestionnaire de pilotes ODBC *3. x* vérifie si l’argument *BufferLength* est inférieur à 4 ou supérieur à 4, et si tel est le cas, retourne SQLState HY090 (chaîne ou longueur de mémoire tampon non valide). Si l’argument *BufferLength* est égal à 4, le gestionnaire de pilotes appelle **SQLBindCol** dans le pilote, après avoir remplacé *fCType* par SQL_C_BOOKMARK.  
  
### <a name="sqlcolattribute"></a>SQLColAttribute  
 Quand une application ODBC *3. x* qui utilise un pilote *ODBC 2. x* appelle **SQLColAttribute** avec l’argument *ColumnNumber* défini sur 0, le gestionnaire de pilotes retourne les valeurs *FieldIdentifier* indiquées dans le tableau suivant.  
  
|*FieldIdentifier*|Valeur|  
|-----------------------|-----------|  
|SQL_DESC_AUTO_UNIQUE_VALUE|SQL_FALSE|  
|SQL_DESC_CASE_SENSITIVE|SQL_FALSE|  
|SQL_DESC_CATALOG_NAME|"" (chaîne vide)|  
|SQL_DESC_CONCISE_TYPE|SQL_BINARY|  
|SQL_DESC_COUNT|La même valeur retournée par **SQLNumResultCols**|  
|SQL_DESC_DATETIME_INTERVAL_CODE|0|  
|SQL_DESC_DISPLAY_SIZE|8|  
|SQL_DESC_FIXED_PREC_SCALE|SQL_FALSE|  
|SQL_DESC_LABEL|"" (chaîne vide)|  
|SQL_DESC_LENGTH|0|  
|SQL_DESC_LITERAL_PREFIX|"" (chaîne vide)|  
|SQL_DESC_LITERAL_SUFFIX|"" (chaîne vide)|  
|SQL_DESC_LOCAL_TYPE_NAME|"" (chaîne vide)|  
|SQL_DESC_NAME|"" (chaîne vide)|  
|SQL_DESC_NULLABLE|SQL_NO_NULLS|  
|SQL_DESC_OCTET_LENGTH|4|  
|SQL_DESC_PRECISION|4|  
|SQL_DESC_SCALE|0|  
|SQL_DESC_SCHEMA_NAME|"" (chaîne vide)|  
|SQL_DESC_SEARCHABLE|SQL_PRED_NONE|  
|SQL_DESC_TABLE_NAME|"" (chaîne vide)|  
|SQL_DESC_TYPE|SQL_BINARY|  
|SQL_DESC_TYPE_NAME|"" (chaîne vide)|  
|SQL_DESC_UNNAMED|SQL_UNNAMED|  
|SQL_DESC_UNSIGNED|SQL_FALSE|  
|SQL_DESC_UPDATEABLE|SQL_ATTR_READ_ONLY|  
  
### <a name="sqldescribecol"></a>SQLDescribeCol  
 Quand une application ODBC *3. x* qui utilise un pilote ODBC *2. x* appelle **SQLDescribeCol** avec l’argument *ColumnNumber* défini sur 0, le gestionnaire de pilotes retourne les valeurs indiquées dans le tableau suivant.  
  
|Buffer|Valeur|  
|------------|-----------|  
|ColumnName|"" (chaîne vide)|  
|*NameLengthPtr|0|  
|*DataTypePtr|SQL_BINARY|  
|*ColumnSizePtr|4|  
|*DecimalDigitsPtr|0|  
|*NullablePtr|SQL_NO_NULLS|  
  
### <a name="sqlgetdata"></a>SQLGetData  
 Lorsqu’une application ODBC *3. x* qui utilise un pilote ODBC *2. x* effectue l’appel suivant à **SQLGetData** pour récupérer un signet :  
  
```  
SQLGetData(StatementHandle, 0, SQL_C_VARBOOKMARK, TargetValuePtr, BufferLength, StrLen_or_IndPtr)  
```  
  
 l’appel est mappé à **SQLGetStmtOption** avec un *fOption* de SQL_GET_BOOKMARK, comme suit :  
  
```  
SQLGetStmtOption(hstmt, SQL_GET_BOOKMARK, TargetValuePtr)  
```  
  
 où *HSTMT* et *pvParam* sont définis sur les valeurs de *StatementHandle* et *TargetValuePtr*, respectivement. Le signet est retourné dans la mémoire tampon vers laquelle pointe l’argument *pvParam* (*TargetValuePtr*). La valeur de la mémoire tampon vers laquelle pointe l’argument *StrLen_or_IndPtr* dans l’appel à **SQLGetData** est définie sur 4.  
  
 Ce mappage est nécessaire pour prendre en compte le cas dans lequel **SQLFetch** a été appelé avant l’appel à **SQLGetData** et le pilote ODBC *2. x* ne prenait pas en charge **SQLExtendedFetch**. Dans ce cas, **SQLFetch** est passé au pilote ODBC *2. x* , auquel cas la récupération de signet n’est pas prise en charge.  
  
 **SQLGetData** ne peut pas être appelé plusieurs fois dans un pilote ODBC *2. x* pour récupérer un signet en parties. par conséquent, l’appel de **SQLGetData** avec l’argument *BufferLength* défini sur une valeur inférieure à 4 et l’argument *ColumnNumber* défini sur 0 retourne SQLState HY090 (chaîne non valide ou longueur de la mémoire tampon). **SQLGetData** peut cependant être appelé plusieurs fois pour récupérer le même signet.  
  
### <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
 Lorsqu’une application ODBC *3. x* qui utilise un pilote ODBC *2. x* appelle **SQLSetStmtAttr** pour définir l’attribut SQL_ATTR_USE_BOOKMARKS sur SQL_UB_VARIABLE, le gestionnaire de pilotes affecte à l’attribut la valeur SQL_UB_ON dans le pilote ODBC *2. x* sous-jacent.
