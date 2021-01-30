---
description: Fonction SQLTablePrivileges
title: SQLTablePrivileges fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLTablePrivileges
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLTablePrivileges
helpviewer_keywords:
- SQLTablePrivileges function [ODBC]
ms.assetid: 8cfdb64f-64c5-47e6-ad57-0533ac630afa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 16c813c1859e139c3d85ebf901e673f1164073e5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191736"
---
# <a name="sqltableprivileges-function"></a>Fonction SQLTablePrivileges
**Conformité**  
 Version introduite : ODBC 1,0 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLTablePrivileges** retourne une liste de tables et les privilèges associés à chaque table. Le pilote retourne les informations sous la forme d’un jeu de résultats sur l’instruction spécifiée.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLTablePrivileges(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     CatalogName,  
     SQLSMALLINT   NameLength1,  
     SQLCHAR *     SchemaName,  
     SQLSMALLINT   NameLength2,  
     SQLCHAR *     TableName,  
     SQLSMALLINT   NameLength3);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *Nomcatalogue*  
 Entrée Catalogue de tables. Si un pilote prend en charge des catalogues pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") dénote les tables qui n’ont pas de catalogues. *Nomcatalogue* ne peut pas contenir un modèle de recherche de chaînes.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *nomcatalogue* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *nomcatalogue* est un argument ordinaire ; Il est traité littéralement et sa casse est importante. Pour plus d’informations, consultez [arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Entrée Longueur en caractères de **nomcatalogue*.  
  
 *SchemaName*  
 Entrée Modèle de recherche de chaînes pour les noms de schémas. Si un pilote prend en charge des schémas pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") dénote les tables qui n’ont pas de schémas.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *SchemaName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *SchemaName* est un argument de valeur de modèle ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength2*  
 Entrée Longueur en caractères de **SchemaName*.  
  
 *TableName*  
 Entrée Modèle de recherche de chaînes pour les noms de tables.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *TableName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *TableName* est un argument de valeur de modèle ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength3*  
 Entrée Longueur en caractères de **TableName*.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLTablePrivileges** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLTablePrivileges** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|24 000|État de curseur non valide|Un curseur a été ouvert sur *StatementHandle*, et **SQLFetch** ou **SQLFetchScroll** ont été appelés. Cette erreur est retournée par le gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA et est retourné par le pilote si **sqlfetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un curseur a été ouvert sur le *StatementHandle*, mais **SQLFetch** ou **SQLFetchScroll** n’a pas été appelé.|  
|40001|Échec de la sérialisation|La transaction a été restaurée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction **SQLTablePrivileges** a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelée sur le *StatementHandle*. La fonction **SQLTablePrivileges** a ensuite été appelée à nouveau sur *StatementHandle*.<br /><br /> La fonction **SQLTablePrivileges** a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelée sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|L’attribut de l’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE, l’argument *nomcatalogue* était un pointeur null, et l' *infotype* SQL_CATALOG_NAME retourne que les noms de catalogue sont pris en charge.<br /><br /> (DM) l’attribut d’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE et l’argument *SchemaName* ou *TableName* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLTablePrivileges** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur de l’un des arguments de longueur de nom était inférieure à 0, mais n’est pas égale à SQL_NTS.<br /><br /> La valeur de l’un des arguments de longueur de nom dépasse la valeur de longueur maximale pour le qualificateur ou le nom correspondant.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Un catalogue a été spécifié et le pilote ou la source de données ne prend pas en charge les catalogues.<br /><br /> Un schéma a été spécifié, et le pilote ou la source de données ne prend pas en charge les schémas.<br /><br /> Un modèle de recherche de chaînes a été spécifié pour le schéma de la table, le nom de la table ou le nom de la colonne, et la source de données ne prend pas en charge les modèles de recherche pour un ou plusieurs de ces arguments.<br /><br /> La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’a pas été prise en charge par le pilote ou la source de données.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini sur un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai expiré|Le délai d’expiration de la requête a expiré avant que la source de données n’ait retourné le jeu de résultats. Le délai d’attente est défini à l’aide de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 Les arguments *SchemaName* et *TableName* acceptent les modèles de recherche. Pour plus d’informations sur les modèles de recherche valides, consultez [arguments de valeur de modèle](../../../odbc/reference/develop-app/pattern-value-arguments.md).  
  
 **SQLTablePrivileges** retourne les résultats sous la forme d’un jeu de résultats standard, classé par TABLE_CAT, TABLE_SCHEM, table_name, Privilege et GRANTEE.  
  
 Pour déterminer les longueurs réelles des colonnes TABLE_CAT, TABLE_SCHEM et TABLE_NAME, une application peut appeler **SQLGetInfo** avec les options SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN et SQL_MAX_TABLE_NAME_LEN.  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données renvoyées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Les colonnes suivantes ont été renommées pour ODBC *3. x*. Les modifications apportées aux noms de colonne n’affectent pas la compatibilité descendante, car les applications sont liées par numéro de colonne.  
  
|Colonne ODBC 2,0|Colonne ODBC *3. x*|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
  
 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires au-delà de la colonne 7 (IS_GRANTABLE) peuvent être définies par le pilote. Une application doit accéder aux colonnes spécifiques aux pilotes en comptant à partir de la fin de l’ensemble de résultats au lieu de spécifier une position ordinale explicite. Pour plus d’informations, consultez [données retournées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nom de la colonne|Numéro de colonne|Type de données|Commentaires|  
|-----------------|-------------------|---------------|--------------|  
|TABLE_CAT (ODBC 1,0)|1|Varchar|Nom du catalogue ; NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge des catalogues pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il renvoie une chaîne vide ("") pour les tables qui n’ont pas de catalogues.|  
|TABLE_SCHEM (ODBC 1,0)|2|Varchar|Nom du schéma ; NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge des schémas pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il renvoie une chaîne vide ("") pour les tables qui n’ont pas de schémas.|  
|TABLE_NAME (ODBC 1,0)|3|Varchar non NULL|Nom de la table.|  
|FOURNISSEUR D’AUTORISATIONS (ODBC 1,0)|4|Varchar|Nom de l’utilisateur qui a accordé le privilège ; NULL s’il n’est pas applicable à la source de données.<br /><br /> Pour toutes les lignes dans lesquelles la valeur de la colonne GRANTee est le propriétaire de l’objet, la colonne GRANTOR est « _SYSTEM ».|  
|BÉNÉFICIAIRE (ODBC 1,0)|5|Varchar non NULL|Nom de l’utilisateur auquel le privilège a été accordé.|  
|PRIVILÈGE (ODBC 1,0)|6|Varchar non NULL|Privilège de table. Peut être l’un des privilèges suivants ou un privilège spécifique à la source de données.<br /><br /> SELECT : le bénéficiaire est autorisé à récupérer des données pour une ou plusieurs colonnes de la table.<br /><br /> INSERT : le bénéficiaire est autorisé à insérer de nouvelles lignes contenant des données pour une ou plusieurs colonnes dans la table.<br /><br /> MISE à jour : le bénéficiaire est autorisé à mettre à jour les données dans une ou plusieurs colonnes de la table.<br /><br /> DELETE : le bénéficiaire est autorisé à supprimer des lignes de données de la table.<br /><br /> Références : le bénéficiaire est autorisé à faire référence à une ou plusieurs colonnes de la table au sein d’une contrainte (par exemple, une contrainte unique, référentielle ou de validation de table).<br /><br /> La portée de l’action autorisant le bénéficiaire par un privilège de table donné est dépendante de la source de données. Par exemple, le privilège mettre à jour peut autoriser le bénéficiaire à mettre à jour toutes les colonnes d’une table sur une source de données et uniquement les colonnes pour lesquelles le fournisseur d’autorisations a le privilège mettre à jour sur une autre source de données.|  
|IS_GRANTABLE (ODBC 1,0)|7|Varchar|Indique si le bénéficiaire est autorisé à accorder le privilège à d’autres utilisateurs ; « Oui », « non » ou NULL s’ils sont inconnus ou non applicables à la source de données.<br /><br /> Un privilège peut être accordé ou non, mais pas les deux. Le jeu de résultats retourné par **SQLColumnPrivileges** ne contient jamais deux lignes pour lesquelles toutes les colonnes à l’exception de la colonne IS_GRANTABLE contiennent la même valeur.|  
  
## <a name="code-example"></a>Exemple de code  
 Pour obtenir un exemple de code d’une fonction similaire, consultez [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md).  
  
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
|Renvoi d’une liste de tables dans une source de données|[Fonction SQLTables](../../../odbc/reference/syntax/sqltables-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
