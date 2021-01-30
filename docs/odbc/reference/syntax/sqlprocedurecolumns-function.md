---
description: Fonction SQLProcedureColumns
title: SQLProcedureColumns fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLProcedureColumns
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLProcedureColumns
helpviewer_keywords:
- SQLProcedureColumns function [ODBC]
ms.assetid: 4ca37b28-a6df-465b-8988-d422d37fc025
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 63025751b1ec5e93f5889d77c373cbedb19bf543
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192577"
---
# <a name="sqlprocedurecolumns-function"></a>Fonction SQLProcedureColumns
**Conformité**  
 Version introduite : ODBC 1,0 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLProcedureColumns** retourne la liste des paramètres d’entrée et de sortie, ainsi que les colonnes qui composent le jeu de résultats pour les procédures spécifiées. Le pilote retourne les informations sous la forme d’un jeu de résultats sur l’instruction spécifiée.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLProcedureColumns(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     CatalogName,  
     SQLSMALLINT   NameLength1,  
     SQLCHAR *     SchemaName,  
     SQLSMALLINT   NameLength2,  
     SQLCHAR *     ProcName,  
     SQLSMALLINT   NameLength3,  
     SQLCHAR *     ColumnName,  
     SQLSMALLINT   NameLength4);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *Nomcatalogue*  
 Entrée Nom du catalogue de procédures. Si un pilote prend en charge les catalogues pour certaines procédures, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") indique les procédures qui n’ont pas de catalogues. *Nomcatalogue* ne peut pas contenir un modèle de recherche de chaînes.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *nomcatalogue* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *nomcatalogue* est un argument ordinaire ; Il est traité littéralement et sa casse est importante. Pour plus d’informations, consultez [arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Entrée Longueur en caractères de **nomcatalogue*.  
  
 *SchemaName*  
 Entrée Modèle de recherche de chaînes pour les noms de schéma de procédure. Si un pilote prend en charge des schémas pour certaines procédures, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") indique les procédures qui n’ont pas de schémas.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *SchemaName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *SchemaName* est un argument de valeur de modèle ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength2*  
 Entrée Longueur en caractères de **SchemaName*.  
  
 *ProcName*  
 Entrée Modèle de recherche de chaînes pour les noms de procédure.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *procname* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *procname* est un argument de valeur de modèle ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength3*  
 Entrée Longueur en caractères de **procname*.  
  
 *ColumnName*  
 Entrée Modèle de recherche de chaînes pour les noms de colonnes.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *ColumnName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *ColumnName* est un argument de valeur de modèle ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength4*  
 Entrée Longueur en caractères de **ColumnName*.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLProcedureColumns** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLProcedureColumns** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|24 000|État de curseur non valide|Un curseur a été ouvert sur *StatementHandle*, et **SQLFetch** ou **SQLFetchScroll** ont été appelés. Cette erreur est retournée par le gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA, et est retourné par le pilote si **sqlfetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un curseur a été ouvert sur le *StatementHandle*, mais **SQLFetch** ou **SQLFetchScroll** n’a pas été appelé.|  
|40001|Échec de la sérialisation|La transaction a été restaurée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLError** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*. Ensuite, la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|L’attribut de l’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE, l’argument *nomcatalogue* était un pointeur null, et l' *infotype* SQL_CATALOG_NAME retourne que les noms de catalogue sont pris en charge.<br /><br /> (DM) l’attribut d’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE et l’argument *SchemaName*, *procname* ou *ColumnName* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction aynschronous était toujours en cours d’exécution lors de l’appel de la fonction SQLProcedureColumns.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur de l’un des arguments de longueur de nom était inférieure à 0, mais n’est pas égale à SQL_NTS.<br /><br /> La valeur de l’un des arguments de longueur de nom dépasse la valeur de longueur maximale pour le catalogue, le schéma, la procédure ou le nom de colonne correspondant.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Un catalogue de procédures a été spécifié et le pilote ou la source de données ne prend pas en charge les catalogues.<br /><br /> Un schéma de procédure a été spécifié, et le pilote ou la source de données ne prend pas en charge les schémas.<br /><br /> Un modèle de recherche de chaînes a été spécifié pour le schéma de procédure, le nom de la procédure ou le nom de la colonne, et la source de données ne prend pas en charge les modèles de recherche pour un ou plusieurs de ces arguments.<br /><br /> La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’a pas été prise en charge par le pilote ou la source de données.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini sur un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai expiré|Le délai d’attente a expiré avant que la source de données n’ait retourné le jeu de résultats. Le délai d’attente est défini à l’aide de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 Cette fonction est généralement utilisée avant l’exécution de l’instruction pour récupérer des informations sur les paramètres de procédure et les colonnes qui composent le ou les jeux de résultats retournés par la procédure, le cas échéant. Pour plus d’informations, consultez [Procédures](../../../odbc/reference/develop-app/procedures-odbc.md).  
  
> [!NOTE]  
>  **SQLProcedureColumns** peut ne pas retourner toutes les colonnes utilisées par une procédure. Par exemple, un pilote peut retourner uniquement des informations sur les paramètres utilisés par une procédure, et non sur les colonnes d’un jeu de résultats qu’il génère.  
  
 Les arguments *SchemaName*, *procname* et *ColumnName* acceptent les modèles de recherche. Pour plus d’informations sur les modèles de recherche valides, consultez [arguments de valeur de modèle](../../../odbc/reference/develop-app/pattern-value-arguments.md).  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données renvoyées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 **SQLProcedureColumns** retourne les résultats sous la forme d’un jeu de résultats standard, classé par PROCEDURE_CAT, PROCEDURE_SCHEM, PROCEDURE_NAME et COLUMN_TYPE. Les noms de colonnes sont retournés pour chaque procédure dans l’ordre suivant : le nom de la valeur de retour, le nom de chaque paramètre dans l’appel de procédure (dans l’ordre des appels), puis les noms de chaque colonne dans le jeu de résultats retourné par la procédure (dans l’ordre des colonnes).  
  
 Les applications doivent lier des colonnes spécifiques au pilote par rapport à la fin du jeu de résultats. Pour plus d’informations, consultez [données retournées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
 Pour déterminer les longueurs réelles des colonnes PROCEDURE_CAT, PROCEDURE_SCHEM, PROCEDURE_NAME et COLUMN_NAME, une application peut appeler **SQLGetInfo** avec les options SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_PROCEDURE_NAME_LEN et SQL_MAX_COLUMN_NAME_LEN.  
  
 Les colonnes suivantes ont été renommées pour ODBC 3. *x*. Les modifications apportées aux noms de colonne n’affectent pas la compatibilité descendante, car les applications sont liées par numéro de colonne.  
  
|Colonne ODBC 2,0|ODBC 3. colonne *x*|  
|---------------------|-----------------------|  
|PROCEDURE_QUALIFIER|PROCEDURE_CAT|  
|_OWNER DE PROCÉDURE|PROCEDURE_SCHEM|  
|PRECISION|COLUMN_SIZE|  
|LENGTH|BUFFER_LENGTH|  
|SCALE|DECIMAL_DIGITS|  
|RADIX|NUM_PREC_RADIX|  
  
 Les colonnes suivantes ont été ajoutées au jeu de résultats retourné par **SQLProcedureColumns** pour ODBC 3. *x*:  
  
-   COLUMN_DEF  
  
-   DATETIME_CODE  
  
-   CHAR_OCTET_LENGTH  
  
-   ORDINAL_POSITION  
  
-   IS_NULLABLE  
  
 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires au-delà de la colonne 19 (IS_NULLABLE) peuvent être définies par le pilote. Une application doit accéder aux colonnes spécifiques aux pilotes en comptant à partir de la fin de l’ensemble de résultats au lieu de spécifier une position ordinale explicite. Pour plus d’informations, consultez [données retournées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nom de la colonne|Numéro de colonne|Type de données|Commentaires|  
|-----------------|-------------------|---------------|--------------|  
|PROCEDURE_CAT (ODBC 2,0)|1|Varchar|Nom du catalogue de procédures ; NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge les catalogues pour certaines procédures, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il renvoie une chaîne vide ("") pour les procédures qui n’ont pas de catalogues.|  
|PROCEDURE_SCHEM (ODBC 2,0)|2|Varchar|Nom du schéma de procédure ; NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge des schémas pour certaines procédures, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il retourne une chaîne vide ("") pour les procédures qui n’ont pas de schémas.|  
|PROCEDURE_NAME (ODBC 2,0)|3|Varchar non NULL|Nom de la procédure. Une chaîne vide est retournée pour une procédure qui n’a pas de nom.|  
|COLUMN_NAME (ODBC 2,0)|4|Varchar non NULL|Nom de la colonne de procédure. Le pilote retourne une chaîne vide pour une colonne de procédure qui n’a pas de nom.|  
|COLUMN_TYPE (ODBC 2,0)|5|Smallint non NULL|Définit la colonne de procédure en tant que paramètre ou colonne de jeu de résultats :<br /><br /> SQL_PARAM_TYPE_UNKNOWN : la colonne de procédure est un paramètre dont le type est inconnu. (ODBC 1,0)<br /><br /> SQL_PARAM_INPUT : la colonne de procédure est un paramètre d’entrée. (ODBC 1,0)<br /><br /> SQL_PARAM_INPUT_OUTPUT : la colonne de procédure est un paramètre d’entrée/sortie. (ODBC 1,0)<br /><br /> SQL_PARAM_OUTPUT : la colonne de procédure est un paramètre de sortie. (ODBC 2,0)<br /><br /> SQL_RETURN_VALUE : la colonne de procédure est la valeur de retour de la procédure. (ODBC 2,0)<br /><br /> SQL_RESULT_COL : la colonne de procédure est une colonne de jeu de résultats. (ODBC 1,0)|  
|DATA_TYPE (ODBC 2,0)|6|Smallint non NULL|Type de données SQL. Il peut s’agir d’un type de données SQL ODBC ou d’un type de données SQL spécifique au pilote. Pour les types de données DateTime et Interval, cette colonne retourne les types de données concis (par exemple, SQL_TYPE_TIME ou SQL_INTERVAL_YEAR_TO_MONTH). Pour obtenir la liste des types de données SQL ODBC valides, consultez types de données [SQL](../../../odbc/reference/appendixes/sql-data-types.md) dans l’annexe D : types de données. Pour plus d’informations sur les types de données SQL spécifiques au pilote, consultez la documentation du pilote.|  
|TYPE_NAME (ODBC 2,0)|7|Varchar non NULL|Nom du type de données dépendant de la source de données ; par exemple, « CHAR », « VARCHAR », « MONEY », « LONG VARBINARY » ou « CHAR () FOR BIT DATA ».|  
|COLUMN_SIZE (ODBC 2,0)|8|Integer|Taille de colonne de la colonne de procédure sur la source de données. La valeur NULL est retournée pour les types de données où la taille de colonne n’est pas applicable. Pour plus d’informations sur la précision, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) dans l’annexe D : types de données.|  
|BUFFER_LENGTH (ODBC 2,0)|9|Integer|Longueur, en octets, des données transférées sur une opération **SQLGetData** ou **SQLFetch** si SQL_C_DEFAULT est spécifié. Pour les données numériques, cette taille peut être différente de la taille des données stockées dans la source de données. Pour plus d’informations, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md), dans l’annexe D : types de données.|  
|DECIMAL_DIGITS (ODBC 2,0)|10|Smallint|Chiffres décimaux de la colonne de procédure sur la source de données. La valeur NULL est retournée pour les types de données où les chiffres décimaux ne sont pas applicables. Pour plus d’informations sur les chiffres décimaux, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md), dans annexe D : types de données.|  
|NUM_PREC_RADIX (ODBC 2,0)|11|Smallint|Pour les types de données numériques, 10 ou 2.<br /><br /> Si la valeur est 10, les valeurs de COLUMN_SIZE et DECIMAL_DIGITS donnent le nombre de chiffres décimaux autorisés pour la colonne. Par exemple, une colonne DÉCIMALe (12, 5) retourne une NUM_PREC_RADIX de 10, un COLUMN_SIZE de 12 et un DECIMAL_DIGITS de 5 ; une colonne FLOAT peut retourner un NUM_PREC_RADIX de 10, un COLUMN_SIZE de 15 et un DECIMAL_DIGITS NULL.<br /><br /> Si la valeur est 2, les valeurs de COLUMN_SIZE et DECIMAL_DIGITS donnent le nombre de bits autorisés dans la colonne. Par exemple, une colonne de type FLOAT peut retourner un NUM_PREC_RADIX de 2, un COLUMN_SIZE de 53 et un DECIMAL_DIGITS NULL.<br /><br /> La valeur NULL est retournée pour les types de données où NUM_PREC_RADIX n’est pas applicable.|  
|NULLABLE (ODBC 2,0)|12|Smallint non NULL|Indique si la colonne de procédure accepte une valeur NULL :<br /><br /> SQL_NO_NULLS : la colonne de procédure n’accepte pas les valeurs NULL.<br /><br /> SQL_NULLABLE : la colonne de procédure accepte les valeurs NULL.<br /><br /> SQL_NULLABLE_UNKNOWN : il est inconnu si la colonne de procédure accepte les valeurs NULL.|  
|REMARQUES (ODBC 2,0)|13|Varchar|Description de la colonne de procédure.|  
|COLUMN_DEF (ODBC 3,0)|14|Varchar|Valeur par défaut de la colonne.<br /><br /> Si NULL a été spécifié en tant que valeur par défaut, cette colonne est le mot NULL, non placé entre guillemets. Si la valeur par défaut ne peut pas être représentée sans troncation, cette colonne contient tronqué, sans guillemets simples. Si aucune valeur par défaut n’a été spécifiée, cette colonne a la valeur NULL.<br /><br /> La valeur de COLUMN_DEF peut être utilisée pour générer une nouvelle définition de colonne, sauf si elle contient la valeur TRONQUÉe.|  
|SQL_DATA_TYPE (ODBC 3,0)|15|Smallint non NULL|Valeur du type de données SQL tel qu’il apparaît dans le champ SQL_DESC_TYPE du descripteur. Cette colonne est la même que la colonne DATA_TYPE, à l’exception des types de données DateTime et Interval.<br /><br /> Pour les types de données DateTime et Interval, le champ SQL_DATA_TYPE dans le jeu de résultats retourne SQL_INTERVAL ou SQL_DATETIME, et le champ SQL_DATETIME_SUB retourne le sous-code pour l’intervalle ou le type de données DateTime spécifique. (Voir l' [annexe D : types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).)|  
|SQL_DATETIME_SUB (ODBC 3,0)|16|Smallint|Code de sous-type pour les types de données DateTime et Interval. Pour les autres types de données, cette colonne retourne une valeur NULL.|  
|CHAR_OCTET_LENGTH (ODBC 3,0)|17|Integer|Longueur maximale, en octets, d’une colonne de type de données binaire ou caractère. Pour tous les autres types de données, cette colonne retourne une valeur NULL.|  
|ORDINAL_POSITION (ODBC 3,0)|18|Entier non NULL|Pour les paramètres d’entrée et de sortie, la position ordinale du paramètre dans la définition de la procédure (en accroissant l’ordre des paramètres, à partir de 1). Pour une valeur de retour (le cas échéant), la valeur 0 est retournée. Pour les colonnes de jeu de résultats, il s’agit de la position ordinale de la colonne dans le jeu de résultats, la première colonne du jeu de résultats étant le numéro 1. S’il existe plusieurs jeux de résultats, les positions ordinales des colonnes sont retournées de manière spécifique au pilote.|  
|IS_NULLABLE (ODBC 3,0)|19|Varchar|« NON » si la colonne n’inclut pas de valeurs NULL.<br /><br /> « Oui » si la colonne peut inclure des valeurs NULL.<br /><br /> Cette colonne renvoie une chaîne de longueur zéro si la possibilité de valeurs Null n'est pas connue.<br /><br /> Les règles ISO sont utilisées pour déterminer la possibilité de valeur Null. Un SGBD compatible avec la norme ISO SQL ne peut pas renvoyer de chaîne vide.<br /><br /> La valeur renvoyée pour cette colonne est différente de celle renvoyée pour la colonne NULLABLE. (Consultez la description de la colonne NULLABLE.)|  
  
## <a name="code-example"></a>Exemple de code  
 Consultez [appels de procédure](../../../odbc/reference/develop-app/procedure-calls.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Extraction d’une seule ligne ou d’un bloc de données dans une direction vers l’avant uniquement|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Retour d’une liste de procédures dans une source de données|[Fonction SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
