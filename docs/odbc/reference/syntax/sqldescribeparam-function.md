---
description: Fonction SQLDescribeParam
title: SQLDescribeParam, fonction | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDescribeParam
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLDescribeParam
helpviewer_keywords:
- SQLDescribeParam function [ODBC]
ms.assetid: 1f5b63c4-2f3e-44da-b155-876405302281
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b8459ca7dd493ba8ab316f832951170f87a77664
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192040"
---
# <a name="sqldescribeparam-function"></a>Fonction SQLDescribeParam
**Conformité**  
 Version introduite : ODBC 1,0 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLDescribeParam** retourne la description d’un marqueur de paramètre associé à une instruction SQL préparée. Ces informations sont également disponibles dans les champs de l’IPD.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLDescribeParam(  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ParameterNumber,  
      SQLSMALLINT *   DataTypePtr,  
      SQLULEN *       ParameterSizePtr,  
      SQLSMALLINT *   DecimalDigitsPtr,  
      SQLSMALLINT *   NullablePtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *ParameterNumber*  
 Entrée Numéro de marqueur de paramètre ordonné séquentiellement dans l’ordre croissant des paramètres, à partir de 1.  
  
 *DataTypePtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le type de données SQL du paramètre. Cette valeur est lue à partir du champ d’enregistrement SQL_DESC_CONCISE_TYPE de l’IPD. Il s’agit de l’une des valeurs de la section [types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md) de l’annexe D : types de données, ou d’un type de données SQL spécifique au pilote.  
  
 Dans ODBC 3. *x*, SQL_TYPE_DATE, SQL_TYPE_TIME ou SQL_TYPE_TIMESTAMP sont retournés dans *\* DataTypePtr* pour les données de date, d’heure ou d’horodatage, respectivement ; dans ODBC 2.*x*, SQL_DATE, SQL_TIME ou SQL_TIMESTAMP sont retournés. Le gestionnaire de pilotes effectue les mappages requis lorsqu’un ODBC 2. l’application *x* fonctionne avec ODBC 3. *x* ou lorsqu’un pilote ODBC 3. l’application *x* fonctionne avec ODBC 2. pilote *x* .  
  
 Lorsque *ColumnNumber* est égal à 0 (pour une colonne de signet), SQL_BINARY est retourné dans *\* DataTypePtr* pour les signets de longueur variable. (SQL_INTEGER est renvoyé si les signets sont utilisés par ODBC 3. *x* qui fonctionne avec ODBC 2. *x* ou ODBC 2. *x* qui fonctionne avec une application ODBC 3. pilote *x* .)  
  
 Pour plus d’informations, consultez [types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md) dans l’annexe D : types de données. Pour plus d’informations sur les types de données SQL spécifiques au pilote, consultez la documentation du pilote.  
  
 *ParameterSizePtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner la taille, en caractères, de la colonne ou de l’expression du marqueur de paramètre correspondant, comme défini par la source de données. Pour plus d’informations sur la taille des colonnes, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
 *DecimalDigitsPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre de chiffres décimaux de la colonne ou de l’expression du paramètre correspondant, comme défini par la source de données. Pour plus d’informations sur les chiffres décimaux, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
 *NullablePtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner une valeur qui indique si le paramètre autorise les valeurs NULL. Cette valeur est lue à partir du champ SQL_DESC_NULLABLE de la IPD. Celui-ci peut avoir l'une des valeurs suivantes :  
  
-   SQL_NO_NULLS : le paramètre n’autorise pas les valeurs NULL (il s’agit de la valeur par défaut).  
  
-   SQL_NULLABLE : le paramètre autorise les valeurs NULL.  
  
-   SQL_NULLABLE_UNKNOWN : le pilote ne peut pas déterminer si le paramètre autorise les valeurs NULL.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLDescribeParam** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLDescribeParam** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07009|Index de descripteur non valide|(DM) la valeur spécifiée pour l’argument *ParameterNumber* est inférieure à 1.<br /><br /> La valeur spécifiée pour l’argument *ParameterNumber* est supérieure au nombre de paramètres dans l’instruction SQL associée.<br /><br /> Le marqueur de paramètre faisait partie d’une instruction non DML.<br /><br /> Le marqueur de paramètre faisait partie d’une liste de **sélection** .|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|21S01|La liste d’insertion de valeurs ne correspond pas à la liste de colonnes|Le nombre de paramètres dans l’instruction **Insert** ne correspondait pas au nombre de colonnes dans la table nommée dans l’instruction.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*. Ensuite, la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM) la fonction a été appelée avant d’appeler **SQLPrepare** ou **SQLExecDirect** pour *StatementHandle*.<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLDescribeParam** .<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 Les marqueurs de paramètres sont numérotés dans l’ordre des paramètres d’incrémentation, à partir de 1, dans l’ordre dans lequel ils apparaissent dans l’instruction SQL.  
  
 **SQLDescribeParam** ne retourne pas le type (entrée, entrée/sortie ou sortie) d’un paramètre dans une instruction SQL. Sauf dans les appels aux procédures, tous les paramètres dans les instructions SQL sont des paramètres d’entrée. Pour déterminer le type de chaque paramètre dans un appel à une procédure, une application appelle **SQLProcedureColumns**.  
  
 Pour plus d’informations, consultez [Description des paramètres](../../../odbc/reference/develop-app/describing-parameters.md).  
  
## <a name="code-example"></a>Exemple de code  
 L’exemple suivant invite l’utilisateur à entrer une instruction SQL, puis prépare cette instruction. Ensuite, il appelle **SQLNumParams** pour déterminer si l’instruction contient des paramètres. Si l’instruction contient des paramètres, elle appelle **SQLDescribeParam** pour décrire ces paramètres et **SQLBindParameter** pour les lier. Enfin, il invite l’utilisateur à entrer les valeurs de tous les paramètres, puis il exécute l’instruction.  
  
```cpp  
SQLCHAR       Statement[100];  
SQLSMALLINT   NumParams, i, DataType, DecimalDigits, Nullable;  
SQLUINTEGER   ParamSize;  
SQLHSTMT      hstmt;  
  
// Prompt the user for an SQL statement and prepare it.  
GetSQLStatement(Statement);  
SQLPrepare(hstmt, Statement, SQL_NTS);  
  
// Check to see if there are any parameters. If so, process them.  
SQLNumParams(hstmt, &NumParams);  
if (NumParams) {  
   // Allocate memory for three arrays. The first holds pointers to buffers in which  
   // each parameter value will be stored in character form. The second contains the  
   // length of each buffer. The third contains the length/indicator value for each  
   // parameter.  
   SQLPOINTER * PtrArray = (SQLPOINTER *) malloc(NumParams * sizeof(SQLPOINTER));  
   SQLINTEGER * BufferLenArray = (SQLINTEGER *) malloc(NumParams * sizeof(SQLINTEGER));  
   SQLINTEGER * LenOrIndArray = (SQLINTEGER *) malloc(NumParams * sizeof(SQLINTEGER));  
  
   for (i = 0; i < NumParams; i++) {  
   // Describe the parameter.  
   SQLDescribeParam(hstmt, i + 1, &DataType, &ParamSize, &DecimalDigits, &Nullable);  
  
   // Call a helper function to allocate a buffer in which to store the parameter  
   // value in character form. The function determines the size of the buffer from  
   // the SQL data type and parameter size returned by SQLDescribeParam and returns  
   // a pointer to the buffer and the length of the buffer.  
   AllocParamBuffer(DataType, ParamSize, &PtrArray[i], &BufferLenArray[i]);  
  
   // Bind the memory to the parameter. Assume that we only have input parameters.  
   SQLBindParameter(hstmt, i + 1, SQL_PARAM_INPUT, SQL_C_CHAR, DataType, ParamSize,  
         DecimalDigits, PtrArray[i], BufferLenArray[i],  
         &LenOrIndArray[i]);  
  
   // Prompt the user for the value of the parameter and store it in the memory  
   // allocated earlier. For simplicity, this function does not check the value  
   // against the information returned by SQLDescribeParam. Instead, the driver does  
   // this when the statement is executed.  
   GetParamValue(PtrArray[i], BufferLenArray[i], &LenOrIndArray[i]);  
   }  
}  
  
// Execute the statement.  
SQLExecute(hstmt);  
  
// Process the statement further, such as retrieving results (if any) and closing the  
// cursor (if any). Code not shown.  
  
// Free the memory allocated for each parameter and the memory allocated for the arrays  
// of pointers, buffer lengths, and length/indicator values.  
for (i = 0; i < NumParams; i++) free(PtrArray[i]);  
free(PtrArray);  
free(BufferLenArray);  
free(LenOrIndArray);  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à un paramètre|[Fonction SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Exécution d’une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Préparation d’une instruction pour l’exécution|[Fonction SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
