---
description: Fonction SQLColumns
title: Fonction SQLColumns | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLColumns
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLColumns
helpviewer_keywords:
- SQLColumns function [ODBC]
ms.assetid: 4a3618b7-d2b8-43c6-a1fd-7a4e6fa8c7d0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 51a72000e071d89fc3f189b00c6d8491db1e6e9f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201979"
---
# <a name="sqlcolumns-function"></a>Fonction SQLColumns
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ouvrir un groupe  
  
 **Résumé**  
 **SQLColumns** retourne la liste des noms de colonnes dans les tables spécifiées. Le pilote retourne ces informations sous la forme d’un jeu de résultats sur le *StatementHandle* spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLColumns(  
     SQLHSTMT       StatementHandle,  
     SQLCHAR *      CatalogName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      SchemaName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      TableName,  
     SQLSMALLINT    NameLength3,  
     SQLCHAR *      ColumnName,  
     SQLSMALLINT    NameLength4);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *Nomcatalogue*  
 Entrée Nom du catalogue. Si un pilote prend en charge des catalogues pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") indique les tables qui n’ont pas de catalogues. *Nomcatalogue* ne peut pas contenir un modèle de recherche de chaînes.  
  
> [!NOTE]  
>  Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *nomcatalogue* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *nomcatalogue* est un argument ordinaire ; Il est traité littéralement et sa casse est importante. Pour plus d’informations, consultez [arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Entrée Longueur en caractères de **nomcatalogue*.  
  
 *SchemaName*  
 Entrée Modèle de recherche de chaînes pour les noms de schémas. Si un pilote prend en charge des schémas pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") indique les tables qui n’ont pas de schémas.  
  
> [!NOTE]  
>  Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *SchemaName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *SchemaName* est un argument de valeur de modèle ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength2*  
 Entrée Longueur en caractères de **SchemaName*.  
  
 *TableName*  
 Entrée Modèle de recherche de chaînes pour les noms de tables.  
  
> [!NOTE]  
>  Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *TableName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *TableName* est un argument de valeur de modèle ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength3*  
 Entrée Longueur en caractères de **TableName*.  
  
 *ColumnName*  
 Entrée Modèle de recherche de chaînes pour les noms de colonnes.  
  
> [!NOTE]  
>  Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *ColumnName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *ColumnName* est un argument de valeur de modèle ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength4*  
 Entrée Longueur en caractères de **ColumnName*.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLColumns** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLColumns** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|24 000|État de curseur non valide|Un curseur a été ouvert sur *StatementHandle*, et **SQLFetch** ou **SQLFetchScroll** ont été appelés. Cette erreur est retournée par le gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA, et est retourné par le pilote si **sqlfetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un curseur a été ouvert sur le *StatementHandle* , mais **SQLFetch** ou **SQLFetchScroll** n’a pas été appelé.|  
|40001|Échec de la sérialisation|La transaction a été restaurée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*. Ensuite, la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|L’attribut de l’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE, l’argument *nomcatalogue* était un pointeur null, et l' *infotype* SQL_CATALOG_NAME retourne que les noms de catalogue sont pris en charge.<br /><br /> (DM) l’attribut d’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE et l’argument *SchemaName*, *TableName* ou *ColumnName* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLColumns** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur de l’un des arguments de longueur de nom était inférieure à 0, mais n’est pas égale à SQL_NTS.|  
|||La valeur de l’un des arguments de longueur de nom dépasse la valeur de longueur maximale pour le catalogue ou le nom correspondant. La longueur maximale de chaque catalogue ou nom peut être obtenue en appelant **SQLGetInfo** avec les valeurs d' *infotype* . (Voir « commentaires ».)|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Un nom de catalogue a été spécifié, et le pilote ou la source de données ne prend pas en charge les catalogues.<br /><br /> Un nom de schéma a été spécifié, et le pilote ou la source de données ne prend pas en charge les schémas.<br /><br /> Un modèle de recherche de chaînes a été spécifié pour le nom du schéma, le nom de la table ou le nom de la colonne, et la source de données ne prend pas en charge les modèles de recherche pour un ou plusieurs de ces arguments.<br /><br /> La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’a pas été prise en charge par le pilote ou la source de données.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini sur un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai expiré|Le délai d’expiration de la requête a expiré avant que la source de données n’ait retourné le jeu de résultats. Le délai d’attente est défini à l’aide de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 Cette fonction est généralement utilisée avant l’exécution de l’instruction pour récupérer des informations sur les colonnes d’une table ou de tables à partir du catalogue de la source de données. **SQLColumns** peut être utilisé pour récupérer des données pour tous les types d’éléments retournés par **SQLTables**. En plus des tables de base, cela peut inclure (sans s’y limiter) des vues, des synonymes, des tables système, etc. En revanche, les fonctions **SQLColAttribute** et **SQLDescribeCol** décrivent les colonnes d’un jeu de résultats et la fonction **SQLNumResultCols** retourne le nombre de colonnes dans un jeu de résultats. Pour plus d’informations, consultez [utilisation des données de catalogue](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données renvoyées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 **SQLColumns** retourne les résultats sous la forme d’un jeu de résultats standard, classé par TABLE_CAT, TABLE_SCHEM, TABLE_NAME et ORDINAL_POSITION.  
  
> [!NOTE]  
>  Lorsqu’une application fonctionne avec ODBC 2. *x* , aucune ORDINAL_POSITION colonne n’est retournée dans le jeu de résultats. Par conséquent, lorsque vous travaillez avec ODBC 2. *x* , l’ordre des colonnes dans la liste de colonnes retournée par **SQLColumns** n’est pas nécessairement le même que celui des colonnes retournées lorsque l’application exécute une instruction SELECT sur toutes les colonnes de cette table.  
  
> [!NOTE]  
>  **SQLColumns** peut ne pas retourner toutes les colonnes. Par exemple, un pilote peut ne pas retourner d’informations sur les pseudo-colonnes, telles que l’ID de colonne Oracle. Les applications peuvent utiliser n’importe quelle colonne valide, qu’elles soient retournées par **SQLColumns**.  
>   
>  Certaines colonnes qui peuvent être retournées par **SQLStatistics** ne sont pas retournées par **SQLColumns**. Par exemple, **SQLColumns** ne retourne pas les colonnes d’un index créé sur une expression ou un filtre, par exemple Salary + Benefits ou Dept = 0012.  
  
 Les longueurs des colonnes VARCHAR ne sont pas indiquées dans le tableau. les longueurs réelles dépendent de la source de données. Pour déterminer les longueurs réelles des colonnes TABLE_CAT, TABLE_SCHEM, TABLE_NAME et COLUMN_NAME, une application peut appeler **SQLGetInfo** avec les options SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_TABLE_NAME_LEN et SQL_MAX_COLUMN_NAME_LEN.  
  
 Les colonnes suivantes ont été renommées pour ODBC 3. *x*. Les modifications apportées aux noms de colonne n’affectent pas la compatibilité descendante, car les applications sont liées par numéro de colonne.  
  
|Colonne ODBC 2,0|ODBC 3. colonne *x*|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
|PRECISION|COLUMN_SIZE|  
|LENGTH|BUFFER_LENGTH|  
|SCALE|DECIMAL_DIGITS|  
|RADIX|NUM_PREC_RADIX|  
  
 Les colonnes suivantes ont été ajoutées au jeu de résultats retourné par **SQLColumns** pour ODBC 3. *x*:  

:::row:::
    :::column:::
        CHAR_OCTET_LENGTH  
        COLUMN_DEF  
    :::column-end:::
    :::column:::
        IS_NULLABLE  
        ORDINAL_POSITION  
    :::column-end:::
    :::column:::
        SQL_DATA_TYPE  
        SQL_DATETIME_SUB  
    :::column-end:::
:::row-end:::

 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires au-delà de la colonne 18 (IS_NULLABLE) peuvent être définies par le pilote. Une application doit accéder aux colonnes spécifiques aux pilotes en comptant à partir de la fin de l’ensemble de résultats au lieu de spécifier une position ordinale explicite. Pour plus d’informations, consultez [données retournées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nom de la colonne|Colonne<br /><br /> nombre|Type de données|Commentaires|  
|-----------------|-----------------------|---------------|--------------|  
|TABLE_CAT (ODBC 1,0)|1|Varchar|Nom du catalogue ; NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge des catalogues pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il renvoie une chaîne vide ("") pour les tables qui n’ont pas de catalogues.|  
|TABLE_SCHEM (ODBC 1,0)|2|Varchar|Nom du schéma ; NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge des schémas pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il renvoie une chaîne vide ("") pour les tables qui n’ont pas de schémas.|  
|TABLE_NAME (ODBC 1,0)|3|Varchar non NULL|Nom de la table.|  
|COLUMN_NAME (ODBC 1,0)|4|Varchar non NULL|Nom de la colonne. Le pilote retourne une chaîne vide pour une colonne qui n’a pas de nom.|  
|DATA_TYPE (ODBC 1,0)|5|Smallint non NULL|Type de données SQL. Il peut s’agir d’un type de données SQL ODBC ou d’un type de données SQL spécifique au pilote. Pour les types de données DateTime et Interval, cette colonne retourne le type de données concis (par exemple, SQL_TYPE_DATE ou SQL_INTERVAL_YEAR_TO_MONTH, au lieu du type de données non concis comme SQL_DATETIME ou SQL_INTERVAL). Pour obtenir la liste des types de données SQL ODBC valides, consultez types de données [SQL](../../../odbc/reference/appendixes/sql-data-types.md) dans l’annexe D : types de données. Pour plus d’informations sur les types de données SQL spécifiques au pilote, consultez la documentation du pilote.<br /><br /> Types de données retournés pour ODBC 3. *x* et ODBC 2. les applications *x* peuvent être différentes. Pour plus d’informations, consultez [compatibilité descendante et conformité aux normes](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md).|  
|TYPE_NAME (ODBC 1,0)|6|Varchar non NULL|Nom du type de données dépendant de la source de données ; par exemple, « CHAR », « VARCHAR », « MONEY », « LONG VARBINAR » ou « CHAR () FOR BIT DATA ».|  
|COLUMN_SIZE (ODBC 1,0)|7|Integer|Si DATA_TYPE est SQL_CHAR ou SQL_VARCHAR, cette colonne contient la longueur maximale en caractères de la colonne. Pour les types de données DateTime, il s’agit du nombre total de caractères requis pour afficher la valeur lorsqu’elle est convertie en caractères. Pour les types de données numériques, il s’agit du nombre total de chiffres ou du nombre total de bits autorisés dans la colonne, en fonction de la NUM_PREC_RADIX colonne. Pour les types de données Interval, il s’agit du nombre de caractères dans la représentation de caractère du littéral d’intervalle (comme défini par la précision d’intervalle, consultez [longueur de type de données d’intervalle](../../../odbc/reference/appendixes/interval-data-type-length.md) dans l’annexe D : types de données). Pour plus d’informations, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) dans l’annexe D : types de données.|  
|BUFFER_LENGTH (ODBC 1,0)|8|Integer|Longueur, en octets, des données transférées sur une opération SQLGetData, SQLFetch ou SQLFetchScroll si SQL_C_DEFAULT est spécifié. Pour les données numériques, cette taille peut différer de la taille des données stockées dans la source de données. Cette valeur peut être différente de COLUMN_SIZE colonne pour les données de caractères. Pour plus d’informations sur la longueur, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) dans l’annexe D : types de données.|  
|DECIMAL_DIGITS (ODBC 1,0)|9|Smallint|Nombre total de chiffres significatifs à droite de la virgule décimale. Pour SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP, cette colonne contient le nombre de chiffres du composant fractions de seconde. Pour les autres types de données, il s’agit des chiffres décimaux de la colonne sur la source de données. Pour les types de données Interval qui contiennent un composant d’heure, cette colonne contient le nombre de chiffres à droite de la virgule décimale (fractions de seconde). Pour les types de données Interval qui ne contiennent pas de composant heure, cette colonne a la valeur 0. Pour plus d’informations sur les chiffres décimaux, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) dans l’annexe D : types de données. La valeur NULL est retournée pour les types de données où DECIMAL_DIGITS n’est pas applicable.|  
|NUM_PREC_RADIX (ODBC 1,0)|10|Smallint|Pour les types de données numériques, 10 ou 2. Si la valeur est 10, les valeurs de COLUMN_SIZE et DECIMAL_DIGITS donnent le nombre de chiffres décimaux autorisés pour la colonne. Par exemple, une colonne DÉCIMALe (12, 5) retourne une NUM_PREC_RADIX de 10, un COLUMN_SIZE de 12 et un DECIMAL_DIGITS de 5 ; une colonne FLOAT peut retourner un NUM_PREC_RADIX de 10, un COLUMN_SIZE de 15 et un DECIMAL_DIGITS NULL.<br /><br /> Si la valeur est 2, les valeurs de COLUMN_SIZE et DECIMAL_DIGITS donnent le nombre de bits autorisés dans la colonne. Par exemple, une colonne de type FLOAT peut renvoyer une base de 2, une COLUMN_SIZE de 53 et une DECIMAL_DIGITS de NULL.<br /><br /> La valeur NULL est retournée pour les types de données où NUM_PREC_RADIX n’est pas applicable.|  
|NULLABLE (ODBC 1,0)|11|Smallint non NULL|SQL_NO_NULLS si la colonne ne peut pas inclure de valeurs NULL.<br /><br /> SQL_NULLABLE si la colonne accepte les valeurs NULL.<br /><br /> SQL_NULLABLE_UNKNOWN s’il n’est pas connu que la colonne accepte les valeurs NULL.<br /><br /> La valeur retournée pour cette colonne est différente de la valeur renvoyée pour la colonne IS_NULLABLE. La colonne NULLABLE indique avec certitude qu’une colonne peut accepter des valeurs NULL, mais ne peut pas indiquer avec certitude qu’une colonne n’accepte pas les valeurs NULL. La colonne IS_NULLABLE indique avec certitude qu’une colonne ne peut pas accepter les valeurs NULL, mais ne peut pas indiquer avec certitude qu’une colonne accepte les valeurs NULL.|  
|REMARQUES (ODBC 1,0)|12|Varchar|Description de la colonne.|  
|COLUMN_DEF (ODBC 3,0)|13|Varchar|Valeur par défaut de la colonne. La valeur de cette colonne doit être interprétée comme une chaîne si elle est placée entre guillemets.<br /><br /> Si NULL a été spécifié en tant que valeur par défaut, cette colonne est le mot NULL, non placé entre guillemets. Si la valeur par défaut ne peut pas être représentée sans troncation, cette colonne contient tronqué, sans les guillemets simples. Si aucune valeur par défaut n’a été spécifiée, cette colonne a la valeur NULL.<br /><br /> La valeur de COLUMN_DEF peut être utilisée pour générer une nouvelle définition de colonne, sauf si elle contient la valeur TRONQUÉe.|  
|SQL_DATA_TYPE (ODBC 3,0)|14|Smallint non NULL|Type de données SQL, tel qu’il apparaît dans le champ enregistrement SQL_DESC_TYPE dans le IRD. Il peut s’agir d’un type de données SQL ODBC ou d’un type de données SQL spécifique au pilote. Cette colonne est la même que la colonne DATA_TYPE, à l’exception des types de données DateTime et Interval. Cette colonne retourne le type de données non concis (par exemple, SQL_DATETIME ou SQL_INTERVAL), au lieu du type de données concis (tel que SQL_TYPE_DATE ou SQL_INTERVAL_YEAR_TO_MONTH) pour les types de données DateTime et Interval. Si cette colonne retourne SQL_DATETIME ou SQL_INTERVAL, le type de données spécifique peut être déterminé à partir de la colonne SQL_DATETIME_SUB. Pour obtenir la liste des types de données SQL ODBC valides, consultez types de données [SQL](../../../odbc/reference/appendixes/sql-data-types.md) dans l’annexe D : types de données. Pour plus d’informations sur les types de données SQL spécifiques au pilote, consultez la documentation du pilote.<br /><br /> Types de données retournés pour ODBC 3. *x* et ODBC 2. les applications *x* peuvent être différentes. Pour plus d’informations, consultez [compatibilité descendante et conformité aux normes](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md).|  
|SQL_DATETIME_SUB (ODBC 3,0)|15|Smallint|Code de sous-type pour les types de données DateTime et Interval. Pour les autres types de données, cette colonne retourne une valeur NULL. Pour plus d’informations sur les sous-codes DateTime et Interval, consultez « SQL_DESC_DATETIME_INTERVAL_CODE » dans [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).|  
|CHAR_OCTET_LENGTH (ODBC 3,0)|16|Integer|Longueur maximale, en octets, d’une colonne de type de données binaire ou caractère. Pour tous les autres types de données, cette colonne retourne une valeur NULL.|  
|ORDINAL_POSITION (ODBC 3,0)|17|Entier non NULL|Position ordinale de la colonne dans la table. La première colonne de la table est le numéro 1.|  
|IS_NULLABLE (ODBC 3,0)|18|Varchar|« NON » si la colonne n’inclut pas de valeurs NULL.<br /><br /> « Oui » si la colonne peut inclure des valeurs NULL.<br /><br /> Cette colonne renvoie une chaîne de longueur zéro si la possibilité de valeurs Null n'est pas connue.<br /><br /> Les règles ISO sont utilisées pour déterminer la possibilité de valeur Null. Un SGBD compatible avec la norme ISO SQL ne peut pas renvoyer de chaîne vide.<br /><br /> La valeur retournée pour cette colonne est différente de la valeur renvoyée pour la colonne NULLABLE. (Consultez la description de la colonne NULLABLE.)|  
  
## <a name="code-example"></a>Exemple de code  
 Dans l’exemple suivant, une application déclare des tampons pour le jeu de résultats retourné par **SQLColumns**. Elle appelle **SQLColumns** pour retourner un jeu de résultats qui décrit chaque colonne de la table Employee. Il appelle ensuite **SQLBindCol** pour lier les colonnes du jeu de résultats aux mémoires tampons. Enfin, l’application extrait chaque ligne de données avec **SQLFetch** et la traite.  
  
```cpp  
// SQLColumns_Function.cpp  
// compile with: ODBC32.lib  
#include <windows.h>  
#include <sqlext.h>  
#define STR_LEN 128 + 1  
#define REM_LEN 254 + 1  
  
// Declare buffers for result set data  
SQLCHAR szSchema[STR_LEN];  
SQLCHAR szCatalog[STR_LEN];  
SQLCHAR szColumnName[STR_LEN];  
SQLCHAR szTableName[STR_LEN];  
SQLCHAR szTypeName[STR_LEN];  
SQLCHAR szRemarks[REM_LEN];  
SQLCHAR szColumnDefault[STR_LEN];  
SQLCHAR szIsNullable[STR_LEN];  
  
SQLINTEGER ColumnSize;  
SQLINTEGER BufferLength;  
SQLINTEGER CharOctetLength;  
SQLINTEGER OrdinalPosition;  
  
SQLSMALLINT DataType;  
SQLSMALLINT DecimalDigits;  
SQLSMALLINT NumPrecRadix;  
SQLSMALLINT Nullable;  
SQLSMALLINT SQLDataType;  
SQLSMALLINT DatetimeSubtypeCode;  
  
SQLHSTMT hstmt = NULL;  
  
// Declare buffers for bytes available to return  
SQLINTEGER cbCatalog;  
SQLINTEGER cbSchema;  
SQLINTEGER cbTableName;  
SQLINTEGER cbColumnName;  
SQLINTEGER cbDataType;  
SQLINTEGER cbTypeName;  
SQLINTEGER cbColumnSize;  
SQLLEN cbBufferLength;  
SQLINTEGER cbDecimalDigits;  
SQLINTEGER cbNumPrecRadix;  
SQLINTEGER cbNullable;  
SQLINTEGER cbRemarks;  
SQLINTEGER cbColumnDefault;  
SQLINTEGER cbSQLDataType;  
SQLINTEGER cbDatetimeSubtypeCode;  
SQLINTEGER cbCharOctetLength;  
SQLINTEGER cbOrdinalPosition;  
SQLINTEGER cbIsNullable;  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt = 0;  
   SQLRETURN retcode;  
  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
   retcode = SQLColumns(hstmt, NULL, 0, NULL, 0, (SQLCHAR*)"CUSTOMERS", SQL_NTS, NULL, 0);  
  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      // Bind columns in result set to buffers  
      SQLBindCol(hstmt, 1, SQL_C_CHAR, szCatalog, STR_LEN,&cbCatalog);  
      SQLBindCol(hstmt, 2, SQL_C_CHAR, szSchema, STR_LEN, &cbSchema);  
      SQLBindCol(hstmt, 3, SQL_C_CHAR, szTableName, STR_LEN,&cbTableName);  
      SQLBindCol(hstmt, 4, SQL_C_CHAR, szColumnName, STR_LEN, &cbColumnName);  
      SQLBindCol(hstmt, 5, SQL_C_SSHORT, &DataType, 0, &cbDataType);  
      SQLBindCol(hstmt, 6, SQL_C_CHAR, szTypeName, STR_LEN, &cbTypeName);  
      SQLBindCol(hstmt, 7, SQL_C_SLONG, &ColumnSize, 0, &cbColumnSize);  
      SQLBindCol(hstmt, 8, SQL_C_SLONG, &BufferLength, 0, &cbBufferLength);  
      SQLBindCol(hstmt, 9, SQL_C_SSHORT, &DecimalDigits, 0, &cbDecimalDigits);  
      SQLBindCol(hstmt, 10, SQL_C_SSHORT, &NumPrecRadix, 0, &cbNumPrecRadix);  
      SQLBindCol(hstmt, 11, SQL_C_SSHORT, &Nullable, 0, &cbNullable);  
      SQLBindCol(hstmt, 12, SQL_C_CHAR, szRemarks, REM_LEN, &cbRemarks);  
      SQLBindCol(hstmt, 13, SQL_C_CHAR, szColumnDefault, STR_LEN, &cbColumnDefault);  
      SQLBindCol(hstmt, 14, SQL_C_SSHORT, &SQLDataType, 0, &cbSQLDataType);  
      SQLBindCol(hstmt, 15, SQL_C_SSHORT, &DatetimeSubtypeCode, 0, &cbDatetimeSubtypeCode);  
      SQLBindCol(hstmt, 16, SQL_C_SLONG, &CharOctetLength, 0, &cbCharOctetLength);  
      SQLBindCol(hstmt, 17, SQL_C_SLONG, &OrdinalPosition, 0, &cbOrdinalPosition);  
      SQLBindCol(hstmt, 18, SQL_C_CHAR, szIsNullable, STR_LEN, &cbIsNullable);  
  
      while (SQL_SUCCESS == retcode) {  
         retcode = SQLFetch(hstmt);  
         /*  
         if (retcode == SQL_ERROR || retcode == SQL_SUCCESS_WITH_INFO)  
            0;   // show_error();  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
            0;   // Process fetched data  
         else  
            break;  
        */  
      }  
   }  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Renvoi des privilèges pour une ou plusieurs colonnes|[Fonction SQLColumnPrivileges](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Extraction de plusieurs lignes de données|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Retour de colonnes qui identifient de façon unique une ligne ou des colonnes mises à jour automatiquement par une transaction|[Fonction SQLSpecialColumns](../../../odbc/reference/syntax/sqlspecialcolumns-function.md)|  
|Retour de statistiques de table et d’index|[Fonction SQLStatistics](../../../odbc/reference/syntax/sqlstatistics-function.md)|  
|Renvoi d’une liste de tables dans une source de données|[Fonction SQLTables](../../../odbc/reference/syntax/sqltables-function.md)|  
|Renvoi de privilèges pour une table ou des tables|[Fonction SQLTablePrivileges](../../../odbc/reference/syntax/sqltableprivileges-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
