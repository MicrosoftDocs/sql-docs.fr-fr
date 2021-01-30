---
description: Fonction SQLDescribeCol
title: Fonction SQLDescribeCol | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDescribeCol
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDescribeCol
helpviewer_keywords:
- SQLDescribeCol function [ODBC]
ms.assetid: eddef353-83f3-4a3c-8f24-f9ed888890a4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ceffc2b0db32ccce75c566c89ac3d69b535abe29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192078"
---
# <a name="sqldescribecol-function"></a>Fonction SQLDescribeCol
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLDescribeCol** retourne le descripteur de résultat-nom de colonne, type, taille de colonne, chiffres décimaux et possibilité de valeur null pour une colonne du jeu de résultats. Ces informations sont également disponibles dans les champs de IRD.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLDescribeCol(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   ColumnNumber,  
      SQLCHAR *      ColumnName,  
      SQLSMALLINT    BufferLength,  
      SQLSMALLINT *  NameLengthPtr,  
      SQLSMALLINT *  DataTypePtr,  
      SQLULEN *      ColumnSizePtr,  
      SQLSMALLINT *  DecimalDigitsPtr,  
      SQLSMALLINT *  NullablePtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *ColumnNumber*  
 Entrée Numéro de colonne des données de résultat, ordonné séquentiellement dans l’ordre croissant des colonnes, à partir de 1. L’argument *ColumnNumber* peut également avoir la valeur 0 pour décrire la colonne de signets.  
  
 *ColumnName*  
 Sortie Pointeur vers une mémoire tampon se terminant par un caractère NULL dans laquelle retourner le nom de la colonne. Cette valeur est lue à partir du champ SQL_DESC_NAME de la IRD. Si la colonne n’est pas nommée ou si le nom de la colonne ne peut pas être déterminé, le pilote retourne une chaîne vide.  
  
 Si *ColumnName* a la valeur null, *NameLengthPtr* retourne toujours le nombre total de caractères (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon désignée par *ColumnName*.  
  
 *BufferLength*  
 Entrée Longueur de la mémoire tampon **ColumnName* , en caractères.  
  
 *NameLengthPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre total de caractères (à l’exception de la fin null) disponibles à retourner dans \* *ColumnName*. Si le nombre de caractères disponibles à retourner est supérieur ou égal à *BufferLength*, le nom de colonne dans \* *ColumnName* est tronqué en *BufferLength* moins la longueur d’un caractère de fin null.  
  
 *DataTypePtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le type de données SQL de la colonne. Cette valeur est lue à partir du champ SQL_DESC_CONCISE_TYPE de la IRD. Il s’agit de l’une des valeurs des [types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md)ou d’un type de données SQL spécifique au pilote. Si le type de données ne peut pas être déterminé, le pilote retourne SQL_UNKNOWN_TYPE.  
  
 Dans ODBC 3. *x*, SQL_TYPE_DATE, SQL_TYPE_TIME ou SQL_TYPE_TIMESTAMP sont retournés dans *\* DataTypePtr* pour les données de date, d’heure ou d’horodatage, respectivement ; dans ODBC 2.*x*, SQL_DATE, SQL_TIME ou SQL_TIMESTAMP sont retournés. Le gestionnaire de pilotes effectue les mappages requis lorsqu’un ODBC 2. l’application *x* fonctionne avec ODBC 3. *x* ou lorsqu’un pilote ODBC 3. l’application *x* fonctionne avec ODBC 2. pilote *x* .  
  
 Lorsque *ColumnNumber* est égal à 0 (pour une colonne de signet), SQL_BINARY est retourné dans *\* DataTypePtr* pour les signets de longueur variable. (SQL_INTEGER est renvoyé si les signets sont utilisés par ODBC 3. *x* qui fonctionne avec ODBC 2. *x* ou ODBC 2. *x* qui fonctionne avec une application ODBC 3. pilote *x* .)  
  
 Pour plus d’informations sur ces types de données, consultez types de données [SQL](../../../odbc/reference/appendixes/sql-data-types.md) dans l’annexe D : types de données. Pour plus d’informations sur les types de données SQL spécifiques au pilote, consultez la documentation du pilote.  
  
 *ColumnSizePtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner la taille (en caractères) de la colonne sur la source de données. Si la taille de la colonne ne peut pas être déterminée, le pilote retourne 0. Pour plus d’informations sur la taille des colonnes, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) dans l’annexe D : types de données.  
  
 *DecimalDigitsPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre de chiffres décimaux de la colonne sur la source de données. Si le nombre de chiffres décimaux ne peut pas être déterminé ou s’il n’est pas applicable, le pilote retourne 0. Pour plus d’informations sur les chiffres décimaux, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) dans l’annexe D : types de données.  
  
 *NullablePtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner une valeur qui indique si la colonne autorise les valeurs NULL. Cette valeur est lue à partir du champ SQL_DESC_NULLABLE de la IRD. Les valeurs possibles sont les suivantes :  
  
 SQL_NO_NULLS : la colonne n’autorise pas les valeurs NULL.  
  
 SQL_NULLABLE : la colonne autorise les valeurs NULL.  
  
 SQL_NULLABLE_UNKNOWN : le pilote ne peut pas déterminer si la colonne autorise les valeurs NULL.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLDescribeCol** retourne soit SQL_ERROR, soit SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLDescribeCol** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|La valeur \* *ColumnName* de la mémoire tampon n’est pas assez grande pour retourner le nom complet de la colonne, donc le nom de la colonne a été tronqué. La longueur du nom de colonne non tronquée est retournée dans **NameLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07005|L’instruction préparée n’est pas une *spécification de curseur*|L’instruction associée à *StatementHandle* n’a pas retourné de jeu de résultats. Aucune colonne à décrire.|  
|07009|Index de descripteur non valide|(DM) la valeur spécifiée pour l’argument *ColumnNumber* était égale à 0 et l’option d’instruction SQL_ATTR_USE_BOOKMARKS était SQL_UB_OFF.<br /><br /> La valeur spécifiée pour l’argument *ColumnNumber* est supérieure au nombre de colonnes dans le jeu de résultats.|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Échec d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*. Ensuite, la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lorsque **SQLDescribeCol** était appelé.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) la fonction a été appelée avant d’appeler **SQLPrepare**, **SQLExecute** ou une fonction de catalogue sur le descripteur d’instruction.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur spécifiée pour l’argument *BufferLength* est inférieure à 0.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
 **SQLDescribeCol** peut retourner n’importe quel SQLSTATE pouvant être retourné par **SQLPrepare** ou **SQLExecute** lorsqu’il est appelé après **SQLPrepare** et avant **SQLExecute**, selon le moment où la source de données évalue l’instruction SQL associée au descripteur d’instruction.  
  
 Pour des raisons de performances, une application ne doit pas appeler **SQLDescribeCol** avant d’exécuter une instruction.  
  
## <a name="comments"></a>Commentaires  
 Une application appelle généralement **SQLDescribeCol** après un appel à **SQLPrepare** et avant ou après l’appel associé à **SQLExecute**. Une application peut également appeler **SQLDescribeCol** après un appel à **SQLExecDirect**. Pour plus d’informations, consultez [métadonnées du jeu de résultats](../../../odbc/reference/develop-app/result-set-metadata.md).  
  
 **SQLDescribeCol** récupère le nom de colonne, le type et la longueur générés par une instruction **Select** . Si la colonne est une expression, **ColumnName* est soit une chaîne vide, soit un nom défini par le pilote.  
  
> [!NOTE]  
>  ODBC prend en charge SQL_NULLABLE_UNKNOWN en tant qu’extension, même si la spécification de l’interface d’ouverture de groupe et d’accès au groupe d’accès SQL ne spécifie pas l’option pour **SQLDescribeCol**.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annulation du traitement des instructions|[SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retour d’informations sur une colonne dans un jeu de résultats|[SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)|  
|Extraction de plusieurs lignes de données|[SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Retour du nombre de colonnes du jeu de résultats|[SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Préparation d’une instruction pour l’exécution|[SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
