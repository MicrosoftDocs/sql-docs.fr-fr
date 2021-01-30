---
description: Fonction SQLTables
title: SQLTables, fonction | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLTables
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLTables
helpviewer_keywords:
- SQLTables function [ODBC]
ms.assetid: 60d5068a-7d7c-447c-acc6-f3f2cf73440c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 537323651f0759a435c574fe6234c3050fb57536
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191731"
---
# <a name="sqltables-function"></a>Fonction SQLTables
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ouvrir un groupe  
  
 **Résumé**  
 **SQLTables** retourne la liste des noms de table, de catalogue ou de schéma, ainsi que les types de table, stockés dans une source de données spécifique. Le pilote retourne les informations sous la forme d’un jeu de résultats.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLTables(  
     SQLHSTMT       StatementHandle,  
     SQLCHAR *      CatalogName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      SchemaName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      TableName,  
     SQLSMALLINT    NameLength3,  
     SQLCHAR *      TableType,  
     SQLSMALLINT    NameLength4);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction pour les résultats récupérés.  
  
 *Nomcatalogue*  
 Entrée Nom du catalogue. L’argument *nomcatalogue* accepte les modèles de recherche si l’attribut d’environnement SQL_ODBC_VERSION est SQL_OV_ODBC3 ; elle n’accepte pas les modèles de recherche si SQL_OV_ODBC2 est définie. Si un pilote prend en charge des catalogues pour certaines tables, mais pas pour d’autres, par exemple lorsqu’un pilote récupère des données à partir de différents SGBD, une chaîne vide ("") indique les tables qui n’ont pas de catalogues.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *nomcatalogue* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *nomcatalogue* est un argument de valeur de modèle ; Il est traité littéralement et sa casse est importante. Pour plus d’informations, consultez [arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Entrée Longueur en caractères de **nomcatalogue*.  
  
 *SchemaName*  
 Entrée Modèle de recherche de chaînes pour les noms de schémas. Si un pilote prend en charge des schémas pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") indique les tables qui n’ont pas de schémas.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *SchemaName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *SchemaName* est un argument de valeur de modèle ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength2*  
 Entrée Longueur en caractères de **SchemaName*.  
  
 *TableName*  
 Entrée Modèle de recherche de chaînes pour les noms de tables.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *TableName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *TableName* est un argument de valeur de modèle ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength3*  
 Entrée Longueur en caractères de **TableName*.  
  
 *TableType*  
 Entrée Liste des types de tables à faire correspondre.  
  
 Notez que l’attribut d’instruction SQL_ATTR_METADATA_ID n’a aucun effet sur l’argument *TABLETYPE* . *TABLETYPE* est un argument de liste de valeurs, quel que soit le paramètre de SQL_ATTR_METADATA_ID.  
  
 *NameLength4*  
 Entrée Longueur en caractères de **TABLETYPE*.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLTables** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLTables** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|24 000|État de curseur non valide|Un curseur a été ouvert sur *StatementHandle*, et **SQLFetch** ou **SQLFetchScroll** ont été appelés. Cette erreur est retournée par le gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA et est retourné par le pilote si **sqlfetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un curseur a été ouvert sur le *StatementHandle*, mais **SQLFetch** ou **SQLFetchScroll** n’a pas été appelé.|  
|40001|Échec de la sérialisation|La transaction a été restaurée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*. Ensuite, la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|L’attribut de l’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE, l’argument *nomcatalogue* était un pointeur null, et l' *infotype* SQL_CATALOG_NAME retourne que les noms de catalogue sont pris en charge.<br /><br /> (DM) l’attribut d’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE et l’argument *SchemaName* ou *TableName* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de SQLTables.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur de l’un des arguments de longueur était inférieure à 0, mais n’est pas égale à SQL_NTS.<br /><br /> La valeur de l’un des arguments de longueur de nom dépasse la valeur de longueur maximale pour le nom correspondant.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Un catalogue a été spécifié et le pilote ou la source de données ne prend pas en charge les catalogues.<br /><br /> Un schéma a été spécifié, et le pilote ou la source de données ne prend pas en charge les schémas.<br /><br /> Un modèle de recherche de chaînes a été spécifié pour le nom du catalogue, le schéma de la table ou le nom de la table, et la source de données ne prend pas en charge les modèles de recherche pour un ou plusieurs de ces arguments.<br /><br /> La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’a pas été prise en charge par le pilote ou la source de données.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini sur un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai expiré|Le délai d’expiration de la requête a expiré avant que la source de données n’ait retourné le jeu de résultats demandé. Le délai d’attente est défini à l’aide de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 **SQLTables** répertorie toutes les tables de la plage demandée. Un utilisateur peut avoir ou non des privilèges SELECT pour ces tables. Pour vérifier l’accessibilité, une application peut :  
  
-   Appelez **SQLGetInfo** et vérifiez le type d’informations SQL_ACCESSIBLE_TABLES.  
  
-   Appelez **SQLTablePrivileges** pour vérifier les privilèges de chaque table.  
  
 Dans le cas contraire, l’application doit être en mesure de gérer une situation dans laquelle l’utilisateur sélectionne une table pour laquelle des privilèges **Select** ne sont pas accordés.  
  
 Les arguments *SchemaName* et *TableName* acceptent les modèles de recherche. L’argument *nomcatalogue* accepte les modèles de recherche si l’attribut d’environnement SQL_ODBC_VERSION est SQL_OV_ODBC3 ; elle n’accepte pas les modèles de recherche si SQL_OV_ODBC2 est définie. Si SQL_OV_ODBC3 est définie, un pilote ODBC 3 *. x* exige que les caractères génériques dans l’argument *nomcatalogue* soient traités littéralement. Pour plus d’informations sur les modèles de recherche valides, consultez [arguments de valeur de modèle](../../../odbc/reference/develop-app/pattern-value-arguments.md).  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données renvoyées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Pour prendre en charge l’énumération de catalogues, de schémas et de types de tables, la sémantique spéciale suivante est définie pour les arguments *nomcatalogue*, *SchemaName*, *TableName* et *TABLETYPE* de **SQLTables**:  
  
-   Si *nomcatalogue* est SQL_ALL_CATALOGS et *SchemaName* et *TableName* sont des chaînes vides, le jeu de résultats contient une liste de catalogues valides pour la source de données. (Toutes les colonnes à l’exception de la TABLE_CAT colonne contiennent des valeurs NULL.)  
  
-   Si *SchemaName* est SQL_ALL_SCHEMAS et si *nomcatalogue* et *TableName* sont des chaînes vides, le jeu de résultats contient une liste de schémas valides pour la source de données. (Toutes les colonnes à l’exception de la TABLE_SCHEM colonne contiennent des valeurs NULL.)  
  
-   Si *TABLETYPE* est SQL_ALL_TABLE_TYPES et *nomcatalogue*, *SchemaName* et *TableName* sont des chaînes vides, le jeu de résultats contient une liste de types de tables valides pour la source de données. (Toutes les colonnes à l’exception de la TABLE_TYPE colonne contiennent des valeurs NULL.)  
  
 Si *TABLETYPE* n’est pas une chaîne vide, il doit contenir une liste de valeurs séparées par des virgules pour les types intéressants. chaque valeur peut être placée entre des guillemets simples (') ou sans guillemets, par exemple, 'TABLE', 'VIEW’ou TABLE, VIEW. Une application doit toujours spécifier le type de table en majuscules. le pilote doit convertir le type de table en cas de besoin par la source de données. Si la source de données ne prend pas en charge un type de table spécifié, **SQLTables** ne retourne aucun résultat pour ce type.  
  
 **SQLTables** retourne les résultats sous la forme d’un jeu de résultats standard, classé par TABLE_TYPE, TABLE_CAT, TABLE_SCHEM et table_name. Pour plus d’informations sur la façon dont ces informations peuvent être utilisées, consultez [utilisation des données de catalogue](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
 Pour déterminer les longueurs réelles des colonnes TABLE_CAT, TABLE_SCHEM et TABLE_NAME, une application peut appeler **SQLGetInfo** avec les types d’informations SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN et SQL_MAX_TABLE_NAME_LEN.  
  
 Les colonnes suivantes ont été renommées pour ODBC 3 *. x*. Les modifications apportées aux noms de colonne n’affectent pas la compatibilité descendante, car les applications sont liées par numéro de colonne.  
  
|Colonne ODBC 2,0|Colonne ODBC 3 *. x*|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
  
 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires au-delà de la colonne 5 (remarques) peuvent être définies par le pilote. Une application doit accéder aux colonnes spécifiques aux pilotes en comptant à partir de la fin de l’ensemble de résultats au lieu de spécifier une position ordinale explicite. Pour plus d’informations, consultez [données retournées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nom de la colonne|Numéro de colonne|Type de données|Commentaires|  
|-----------------|-------------------|---------------|--------------|  
|TABLE_CAT (ODBC 1,0)|1|Varchar|Nom du catalogue ; NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge des catalogues pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il renvoie une chaîne vide ("") pour les tables qui n’ont pas de catalogues.|  
|TABLE_SCHEM (ODBC 1,0)|2|Varchar|Nom du schéma ; NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge des schémas pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il renvoie une chaîne vide ("") pour les tables qui n’ont pas de schémas.|  
|TABLE_NAME (ODBC 1,0)|3|Varchar|Nom de la table.|  
|TABLE_TYPE (ODBC 1,0)|4|Varchar|Nom du type de table ; l’une des valeurs suivantes : « TABLE », « VIEW », « SYSTEM TABLE », « GLOBAL TEMPORARY », « LOCAL TEMPORARY », « ALIAS », « synonyme » ou un nom de type spécifique à la source de données.<br /><br /> Les significations de « ALIAS » et de « synonyme » sont spécifiques au pilote.|  
|REMARQUES (ODBC 1,0)|5|Varchar|Description de la table.|  
  
## <a name="example"></a>Exemple  
 L’exemple de code suivant ne libère pas les handles et les connexions. Consultez la fonction [SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md) et la [fonction SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md) pour obtenir des exemples de code afin de libérer des handles et des instructions.  
  
```cpp  
// SQLTables.cpp  
// compile with: user32.lib odbc32.lib  
#include <windows.h>  
#include <sqlext.h>  
#include <strsafe.h>  
  
// simple helper functions  
int MySQLSuccess(SQLRETURN rc) {  
   return (rc == SQL_SUCCESS || rc == SQL_SUCCESS_WITH_INFO);  
}  
  
struct DataBinding {  
   SQLSMALLINT TargetType;  
   SQLPOINTER TargetValuePtr;  
   SQLINTEGER BufferLength;  
   SQLLEN StrLen_or_Ind;  
};  
  
void printCatalog(const struct DataBinding* catalogResult) {  
   if (catalogResult[0].StrLen_or_Ind != SQL_NULL_DATA)   
      printf("Catalog Name = %s\n", (char *)catalogResult[0].TargetValuePtr);  
}  
  
// remember to disconnect and free memory, and free statements and handles  
int main() {  
   int bufferSize = 1024, i, numCols = 5;  
   struct DataBinding* catalogResult = (struct DataBinding*) malloc( numCols * sizeof(struct DataBinding) );  
   wchar_t* dbName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize );  
   wchar_t* userName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize );  
  
   // declare and initialize the environment, connection, statement handles  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   SQLRETURN retCode;  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLWCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen;  
  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, -1);  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
   retCode = SQLDriverConnect(hdbc, desktopHandle, (SQLCHAR*)"Driver={SQL Server}", SQL_NTS, (SQLCHAR*)connStrbuffer, 1024 + 1, &connStrBufferLen, SQL_DRIVER_PROMPT);  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
   retCode = SQLGetInfo(hdbc, SQL_DATABASE_NAME, dbName, (SQLSMALLINT)bufferSize, (SQLSMALLINT *)&bufferSize);  
   retCode = SQLGetInfo(hdbc, SQL_USER_NAME, userName, (SQLSMALLINT)bufferSize, (SQLSMALLINT *)&bufferSize);  
  
   bufferSize = 1024;  
  
   // allocate memory for the binding  
   // free this memory when done  
   for ( i = 0 ; i < numCols ; i++ ) {  
      catalogResult[i].TargetType = SQL_C_CHAR;  
      catalogResult[i].BufferLength = (bufferSize + 1);  
      catalogResult[i].TargetValuePtr = malloc( sizeof(unsigned char)*catalogResult[i].BufferLength );  
   }  
  
   // setup the binding (can be used even if the statement is closed by closeStatementHandle)  
   for ( i = 0 ; i < numCols ; i++ )  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, catalogResult[i].TargetType, catalogResult[i].TargetValuePtr, catalogResult[i].BufferLength, &(catalogResult[i].StrLen_or_Ind));  
  
   // all catalogs query  
   printf( "A list of names of all catalogs\n" );  
   retCode = SQLTables( hstmt, (SQLCHAR*)SQL_ALL_CATALOGS, SQL_NTS, (SQLCHAR*)"", SQL_NTS, (SQLCHAR*)"", SQL_NTS, (SQLCHAR*)"", SQL_NTS );  
   for ( retCode = SQLFetch(hstmt) ;  MySQLSuccess(retCode) ; retCode = SQLFetch(hstmt) )  
      printCatalog( catalogResult );  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Renvoi des privilèges pour une ou plusieurs colonnes|[Fonction SQLColumnPrivileges](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)|  
|Retour des colonnes dans une table ou des tables|[Fonction SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)|  
|Extraction d’une seule ligne ou d’un bloc de données dans une direction vers l’avant uniquement|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Retour de statistiques de table et d’index|[Fonction SQLStatistics](../../../odbc/reference/syntax/sqlstatistics-function.md)|  
|Renvoi de privilèges pour une table ou des tables|[Fonction SQLTablePrivileges](../../../odbc/reference/syntax/sqltableprivileges-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
