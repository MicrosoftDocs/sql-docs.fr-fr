---
description: Fonction SQLStatistics
title: SQLStatistics, fonction | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLStatistics
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLStatistics
helpviewer_keywords:
- SQLStatistics function [ODBC]
ms.assetid: 45210682-cfea-4e5d-9951-bcf1cbe10f41
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 711efa4797855e42b278e519106c38d62fda1817
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191744"
---
# <a name="sqlstatistics-function"></a>Fonction SQLStatistics
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLStatistics** récupère une liste de statistiques sur une table unique et les index associés à la table. Le pilote retourne les informations sous la forme d’un jeu de résultats.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLStatistics(  
     SQLHSTMT        StatementHandle,  
     SQLCHAR *       CatalogName,  
     SQLSMALLINT     NameLength1,  
     SQLCHAR *       SchemaName,  
     SQLSMALLINT     NameLength2,  
     SQLCHAR *       TableName,  
     SQLSMALLINT     NameLength3,  
     SQLUSMALLINT    Unique,  
     SQLUSMALLINT    Reserved);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *Nomcatalogue*  
 Entrée Nom du catalogue. Si un pilote prend en charge des catalogues pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") indique les tables qui n’ont pas de catalogues. *Nomcatalogue* ne peut pas contenir un modèle de recherche de chaînes.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *nomcatalogue* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *nomcatalogue* est un argument ordinaire ; Il est traité littéralement et sa casse est importante. Pour plus d’informations, consultez [arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Entrée Longueur en caractères de **nomcatalogue*.  
  
 *SchemaName*  
 Entrée Nom du schéma. Si un pilote prend en charge des schémas pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") indique les tables qui n’ont pas de schémas. *SchemaName* ne peut pas contenir un modèle de recherche de chaînes.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *SchemaName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *SchemaName* est un argument ordinaire ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength2*  
 Entrée Longueur en caractères de **SchemaName*.  
  
 *TableName*  
 Entrée Nom de la table. Cet argument ne peut pas être un pointeur null. *SchemaName* ne peut pas contenir un modèle de recherche de chaînes.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *TableName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *TableName* est un argument ordinaire ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength3*  
 Entrée Longueur en caractères de **TableName*.  
  
 *Unique*  
 Entrée Type d’index : SQL_INDEX_UNIQUE ou SQL_INDEX_ALL.  
  
 *Reserved*  
 Entrée Indique l’importance des colonnes de CARDINALité et de PAGES dans le jeu de résultats. Les options suivantes affectent uniquement le retour des colonnes de CARDINALité et de PAGES. les informations d’index sont retournées même si la CARDINALité et les PAGES ne sont pas retournées.  
  
 SQL_ENSURE demande que le pilote récupère les statistiques de manière inconditionnelle. (Les pilotes qui se conforment uniquement à l’Open Group standard et ne prennent pas en charge les extensions ODBC ne sont pas en mesure de prendre en charge SQL_ENSURE.)  
  
 SQL_QUICK demande que le pilote récupère la CARDINALité et les PAGES uniquement s’ils sont facilement disponibles à partir du serveur. Dans ce cas, le pilote ne s'assure pas que les valeurs sont actuelles. (Les applications qui sont écrites dans le groupe Open Group standard obtiendront toujours SQL_QUICK comportement des pilotes ODBC *3. x*.)  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLStatistics** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLStatistics** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|24 000|État de curseur non valide|Un curseur a été ouvert sur *StatementHandle*, et **SQLFetch** ou **SQLFetchScroll** ont été appelés. Cette erreur est retournée par le gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA et est retourné par le pilote si **sqlfetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un curseur a été ouvert sur le *StatementHandle*, mais **SQLFetch** ou **SQLFetchScroll** n’a pas été appelé.|  
|40001|Échec de la sérialisation|La transaction a été restaurée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*, puis la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|L’argument *TableName* était un pointeur null.<br /><br /> L’attribut de l’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE, l’argument *nomcatalogue* était un pointeur null, et l' *infotype* SQL_CATALOG_NAME retourne que les noms de catalogue sont pris en charge.<br /><br /> (DM) l’attribut d’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE et l’argument *SchemaName* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLStatistics** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur de l’un des arguments de longueur de nom était inférieure à 0, mais n’est pas égale à SQL_NTS.<br /><br /> La valeur de l’un des arguments de longueur de nom dépasse la valeur de longueur maximale pour le nom correspondant.|  
|HY100|Type d’option d’unicité hors limites|(DM) une valeur *unique* non valide a été spécifiée.|  
|HY101|Type d’option de précision hors limites|(DM) une valeur *réservée* non valide a été spécifiée.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Un catalogue a été spécifié et le pilote ou la source de données ne prend pas en charge les catalogues.<br /><br /> Un schéma a été spécifié, et le pilote ou la source de données ne prend pas en charge les schémas.<br /><br /> La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’a pas été prise en charge par le pilote ou la source de données.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini sur un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai expiré|Le délai d’expiration de la requête a expiré avant que la source de données n’ait retourné le jeu de résultats demandé. Le délai d’attente est défini à l’aide de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 **SQLStatistics** retourne des informations sur une table unique sous la forme d’un jeu de résultats standard, classé par NON_UNIQUE, TYPE, INDEX_QUALIFIER, INDEX_NAME et ORDINAL_POSITION. Le jeu de résultats combine les informations statistiques (dans les colonnes CARDINALité et PAGES du jeu de résultats) de la table avec les informations relatives à chaque index. Pour plus d’informations sur la façon dont ces informations peuvent être utilisées, consultez [utilisation des données de catalogue](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
 Pour déterminer les longueurs réelles des colonnes TABLE_CAT, TABLE_SCHEM, TABLE_NAME et COLUMN_NAME, une application peut appeler **SQLGetInfo** avec les options SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_TABLE_NAME_LEN et SQL_MAX_COLUMN_NAME_LEN.  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données renvoyées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Les colonnes suivantes ont été renommées pour ODBC *3. x*. Les modifications apportées aux noms de colonne n’affectent pas la compatibilité descendante, car les applications sont liées par numéro de colonne.  
  
|Colonne ODBC 2,0|Colonne ODBC *3. x*|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
|SEQ_IN_INDEX|ORDINAL_POSITION|  
|COLLATION|ASC_OR_DESC|  
  
 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires au-delà de la colonne 13 (FILTER_CONDITION) peuvent être définies par le pilote. Une application doit accéder aux colonnes spécifiques aux pilotes en comptant à partir de la fin de l’ensemble de résultats au lieu de spécifier une position ordinale explicite. Pour plus d’informations, consultez [données retournées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nom de la colonne|Numéro de colonne|Type de données|Commentaires|  
|-----------------|-------------------|---------------|--------------|  
|TABLE_CAT (ODBC 1,0)|1|Varchar|Nom du catalogue de la table à laquelle s’applique la statistique ou l’index. NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge des catalogues pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il renvoie une chaîne vide ("") pour les tables qui n’ont pas de catalogues.|  
|TABLE_SCHEM (ODBC 1,0)|2|Varchar|Nom de schéma de la table à laquelle s’applique la statistique ou l’index ; NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge des schémas pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il renvoie une chaîne vide ("") pour les tables qui n’ont pas de schémas.|  
|TABLE_NAME (ODBC 1,0)|3|Varchar non NULL|Nom de la table à laquelle s’applique la statistique ou l’index.|  
|NON_UNIQUE (ODBC 1,0)|4|Smallint|Indique si l’index n’autorise pas les valeurs en double :<br /><br /> SQL_TRUE si les valeurs d’index peuvent être non uniques.<br /><br /> SQL_FALSE si les valeurs d’index doivent être uniques.<br /><br /> La valeur NULL est retournée si le TYPE est SQL_TABLE_STAT.|  
|INDEX_QUALIFIER (ODBC 1,0)|5|Varchar|Identificateur utilisé pour qualifier le nom d’index qui fait une **Drop index**; La valeur NULL est retournée si un qualificateur d’index n’est pas pris en charge par la source de données ou si le TYPE est SQL_TABLE_STAT. Si une valeur non null est retournée dans cette colonne, elle doit être utilisée pour qualifier le nom de l’index sur une instruction **Drop index** ; dans le cas contraire, le TABLE_SCHEM doit être utilisé pour qualifier le nom de l’index.|  
|INDEX_NAME (ODBC 1,0)|6|Varchar|Nom de l’index ; La valeur NULL est retournée si le TYPE est SQL_TABLE_STAT.|  
|TYPE (ODBC 1,0)|7|Smallint non NULL|Type d’informations renvoyées :<br /><br /> SQL_TABLE_STAT indique une statistique pour la table (dans la colonne CARDINALité ou PAGES).<br /><br /> SQL_INDEX_BTREE indique un index d’arbre B (B-Tree).<br /><br /> SQL_INDEX_CLUSTERED indique un index cluster.<br /><br /> SQL_INDEX_CONTENT indique un index de contenu.<br /><br /> SQL_INDEX_HASHED indique un index haché.<br /><br /> SQL_INDEX_OTHER indique un autre type d’index.|  
|ORDINAL_POSITION (ODBC 1,0)|8|Smallint|Numéro de séquence de la colonne dans l’index (à partir de 1); La valeur NULL est retournée si le TYPE est SQL_TABLE_STAT.|  
|COLUMN_NAME (ODBC 1,0)|9|Varchar|Nom de la colonne. Si la colonne est basée sur une expression, telle que SALARY + BENEFITs, l’expression est retournée ; Si l’expression ne peut pas être déterminée, une chaîne vide est retournée. La valeur NULL est retournée si le TYPE est SQL_TABLE_STAT.|  
|ASC_OR_DESC (ODBC 1,0)|10|Char (1)|Ordre de tri pour la colonne : « A » pour l’ordre croissant ; « D » pour l’ordre décroissant ; La valeur NULL est retournée si la séquence de tri de la colonne n’est pas prise en charge par la source de données ou si le TYPE est SQL_TABLE_STAT.|  
|CARDINALITÉ (ODBC 1,0)|11|Integer|Cardinalité de la table ou de l’index ; nombre de lignes dans la table si le TYPE est SQL_TABLE_STAT ; nombre de valeurs uniques dans l’index si le TYPE n’est pas SQL_TABLE_STAT ; La valeur NULL est retournée si la valeur n’est pas disponible à partir de la source de données.|  
|PAGES (ODBC 1,0)|12|Integer|Nombre de pages utilisées pour stocker l’index ou la table ; nombre de pages pour la table si le TYPE est SQL_TABLE_STAT ; nombre de pages pour l’index si le TYPE n’est pas SQL_TABLE_STAT ; La valeur NULL est retournée si la valeur n’est pas disponible à partir de la source de données ou si elle n’est pas applicable à la source de données.|  
|FILTER_CONDITION (ODBC 2,0)|13|Varchar|Si l’index est un index filtré, il s’agit de la condition de filtre, par exemple SALARY > 30000 ; Si la condition de filtre ne peut pas être déterminée, il s’agit d’une chaîne vide.<br /><br /> NULL si l’index n’est pas un index filtré, il ne peut pas être déterminé si l’index est un index filtré, ou si le TYPE est SQL_TABLE_STAT.|  
  
 Si la ligne dans le jeu de résultats correspond à une table, le pilote définit le TYPE sur SQL_TABLE_STAT et définit NON_UNIQUE, INDEX_QUALIFIER, INDEX_NAME, ORDINAL_POSITION, COLUMN_NAME et ASC_OR_DESC sur NULL. Si la CARDINALité ou les PAGES ne sont pas disponibles à partir de la source de données, le pilote les affecte à la valeur NULL.  
  
## <a name="code-example"></a>Exemple de code  
 Pour obtenir un exemple de code d’une fonction similaire, consultez [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Extraction d’une seule ligne ou d’un bloc de données dans une direction vers l’avant uniquement.|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Retour des colonnes de clés étrangères|[Fonction SQLForeignKeys](../../../odbc/reference/syntax/sqlforeignkeys-function.md)|  
|Retour des colonnes d’une clé primaire|[Fonction SQLPrimaryKeys](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
