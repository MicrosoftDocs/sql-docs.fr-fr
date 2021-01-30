---
description: Fonction SQLForeignKeys
title: SQLForeignKeys fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLForeignKeys
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLForeignKeys
helpviewer_keywords:
- SQLForeignKeys function [ODBC]
ms.assetid: 07f3f645-f643-4d39-9a10-70a72f24e608
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 12ed4d570ac635f2de0db8388a4f6f01efbb2608
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165307"
---
# <a name="sqlforeignkeys-function"></a>Fonction SQLForeignKeys
**Conformité**  
 Version introduite : ODBC 1,0 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLForeignKeys** peut retourner :  
  
-   Liste de clés étrangères dans la table spécifiée (colonnes de la table spécifiée qui font référence à des clés primaires dans d’autres tables).  
  
-   Liste de clés étrangères dans d’autres tables qui font référence à la clé primaire dans la table spécifiée.  
  
 Le pilote retourne chaque liste sous la forme d’un jeu de résultats sur l’instruction spécifiée.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLForeignKeys(  
     SQLHSTMT       StatementHandle,  
     SQLCHAR *      PKCatalogName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      PKSchemaName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      PKTableName,  
     SQLSMALLINT    NameLength3,  
     SQLCHAR *      FKCatalogName,  
     SQLSMALLINT    NameLength4,  
     SQLCHAR *      FKSchemaName,  
     SQLSMALLINT    NameLength5,  
     SQLCHAR *      FKTableName,  
     SQLSMALLINT    NameLength6);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *PKCatalogName*  
 Entrée Nom du catalogue de la table de clé primaire. Si un pilote prend en charge des catalogues pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") dénote les tables qui n’ont pas de catalogues. *PKCatalogName* ne peut pas contenir un modèle de recherche de chaînes.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *PKCatalogName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *PKCatalogName* est un argument ordinaire ; Il est traité littéralement et sa casse est importante. Pour plus d’informations, consultez [arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Entrée Longueur de **PKCatalogName*, en caractères.  
  
 *PKSchemaName*  
 Entrée Nom de schéma de la table de clé primaire. Si un pilote prend en charge des schémas pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") dénote les tables qui n’ont pas de schémas. *PKSchemaName* ne peut pas contenir un modèle de recherche de chaînes.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *PKSchemaName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *PKSchemaName* est un argument ordinaire ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength2*  
 Entrée Longueur de **PKSchemaName*, en caractères.  
  
 *PKTableName*  
 Entrée Nom de la table de clé primaire. *PKTableName* ne peut pas contenir un modèle de recherche de chaînes.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *PKTableName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *PKTableName* est un argument ordinaire ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength3*  
 Entrée Longueur de **PKTableName*, en caractères.  
  
 *FKCatalogName*  
 Entrée Nom du catalogue de la table de clés étrangères. Si un pilote prend en charge des catalogues pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") dénote les tables qui n’ont pas de catalogues. *FKCatalogName* ne peut pas contenir un modèle de recherche de chaînes.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *FKCatalogName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *FKCatalogName* est un argument ordinaire ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength4*  
 Entrée Longueur de **FKCatalogName*, en caractères.  
  
 *FKSchemaName*  
 Entrée Nom de schéma de la table de clé étrangère. Si un pilote prend en charge des schémas pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") dénote les tables qui n’ont pas de schémas. *FKSchemaName* ne peut pas contenir un modèle de recherche de chaînes.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *FKSchemaName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *FKSchemaName* est un argument ordinaire ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength5*  
 Entrée Longueur de **FKSchemaName*, en caractères.  
  
 *FKTableName*  
 Entrée Nom de la table de clé étrangère. *FKTableName* ne peut pas contenir un modèle de recherche de chaînes.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *FKTableName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *FKTableName* est un argument ordinaire ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength6*  
 Entrée Longueur de **FKTableName*, en caractères.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLForeignKeys** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLForeignKeys** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|24 000|État de curseur non valide|Un curseur a été ouvert sur *StatementHandle*, et **SQLFetch** ou **SQLFetchScroll** ont été appelés. Cette erreur est retournée par le gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA, et est retourné par le pilote si **sqlfetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un curseur a été ouvert sur le *StatementHandle*, mais **SQLFetch** ou **SQLFetchScroll** n’a pas été appelé.|  
|40001|Échec de la sérialisation|La transaction a été restaurée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*, puis la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|(DM) les arguments *PKTableName* et *FKTableName* ont tous deux des pointeurs null.<br /><br /> L’attribut d’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE, l’argument *FKCatalogName* ou *PKCatalogName* était un pointeur null, et l' *infotype* SQL_CATALOG_NAME retourne que les noms de catalogue sont pris en charge.<br /><br /> (DM) l’attribut d’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE, et l’argument *FKSchemaName*, *PKSchemaName*, *FKTableName* ou *PKTableName* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction SQLForeignKeys.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur de l’un des arguments de longueur de nom était inférieure à 0, mais n’est pas égale à SQL_NTS.|  
|||La valeur de l’un des arguments de longueur de nom dépasse la valeur de longueur maximale pour le nom correspondant. (Voir « commentaires ».)|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Un nom de catalogue a été spécifié, et le pilote ou la source de données ne prend pas en charge les catalogues.<br /><br /> Un nom de schéma a été spécifié, et le pilote ou la source de données ne prend pas en charge les schémas.|  
|||La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’a pas été prise en charge par le pilote ou la source de données.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini sur un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai expiré|Le délai d’expiration de la requête a expiré avant que la source de données n’ait retourné le jeu de résultats. Le délai d’attente est défini à l’aide de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 Pour plus d’informations sur la façon dont les informations retournées par cette fonction peuvent être utilisées, consultez [utilisations des données de catalogue](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
 Si \* *PKTableName* contient un nom de table, **SQLForeignKeys** retourne un jeu de résultats qui contient la clé primaire de la table spécifiée et toutes les clés étrangères qui y font référence. La liste de clés étrangères dans d’autres tables n’inclut pas les clés étrangères qui pointent vers des contraintes uniques dans la table spécifiée.  
  
 Si \* *FKTableName* contient un nom de table, **SQLForeignKeys** retourne un jeu de résultats qui contient toutes les clés étrangères de la table spécifiée qui pointent vers les clés primaires d’autres tables, et les clés primaires dans les autres tables auxquelles ils font référence. La liste de clés étrangères dans la table spécifiée ne contient pas de clés étrangères qui font référence à des contraintes uniques dans d’autres tables.  
  
 Si \* *PKTableName* et \* *FKTableName* contiennent des noms de tables, **SQLForeignKeys** retourne les clés étrangères dans la table spécifiée dans \* *FKTableName* qui font référence à la clé primaire de la table spécifiée dans **PKTableName*. Ce doit être une clé au plus.  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données renvoyées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 **SQLForeignKeys** retourne les résultats sous la forme d’un jeu de résultats standard. Si les clés étrangères associées à une clé primaire sont demandées, le jeu de résultats est trié par FKTABLE_CAT, FKTABLE_SCHEM, FKTABLE_NAME et KEY_SEQ. Si les clés primaires associées à une clé étrangère sont demandées, le jeu de résultats est trié par PKTABLE_CAT, PKTABLE_SCHEM, PKTABLE_NAME et KEY_SEQ. Le tableau suivant répertorie les colonnes du jeu de résultats.  
  
 Les longueurs des colonnes VARCHAR ne sont pas indiquées dans le tableau. les longueurs réelles dépendent de la source de données. Pour déterminer les longueurs réelles des colonnes PKTABLE_CAT ou FKTABLE_CAT, PKTABLE_SCHEM ou FKTABLE_SCHEM, PKTABLE_NAME ou FKTABLE_NAME et PKCOLUMN_NAME ou FKCOLUMN_NAME, une application peut appeler **SQLGetInfo** avec les options SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_TABLE_NAME_LEN et SQL_MAX_COLUMN_NAME_LEN.  
  
 Les colonnes suivantes ont été renommées pour ODBC 3 *. x.* Les modifications apportées aux noms de colonne n’affectent pas la compatibilité descendante, car les applications sont liées par numéro de colonne.  
  
|Colonne ODBC 2,0|Colonne ODBC 3 *. x*|  
|---------------------|-----------------------|  
|PKTABLE_QUALIFIER|PKTABLE_CAT|  
|PKTABLE_OWNER|PKTABLE_SCHEM|  
|FKTABLE_QUALIFIER|FK_TABLE_CAT|  
|FKTABLE_OWNER|FKTABLE_SCHEM|  
  
 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires qui dépassent la colonne 14 (remarques) peuvent être définies par le pilote. Une application doit accéder aux colonnes spécifiques aux pilotes en comptant à partir de la fin de l’ensemble de résultats au lieu de spécifier une position ordinale explicite. Pour plus d’informations, consultez [données retournées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nom de la colonne|Numéro de colonne|Type de données|Commentaires|  
|-----------------|-------------------|---------------|--------------|  
|PKTABLE_CAT (ODBC 1,0)|1|Varchar|Nom du catalogue de la table de clé primaire ; NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge des catalogues pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il renvoie une chaîne vide ("") pour les tables qui n’ont pas de catalogues.|  
|PKTABLE_SCHEM (ODBC 1,0)|2|Varchar|Nom de schéma de la table de clé primaire ; NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge des schémas pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il renvoie une chaîne vide ("") pour les tables qui n’ont pas de schémas.|  
|PKTABLE_NAME (ODBC 1,0)|3|Varchar non NULL|Nom de la table de clé primaire.|  
|PKCOLUMN_NAME (ODBC 1,0)|4|Varchar non NULL|Nom de la colonne de clé primaire. Le pilote retourne une chaîne vide pour une colonne qui n’a pas de nom.|  
|FKTABLE_CAT (ODBC 1,0)|5|Varchar|Nom du catalogue de la table de clés étrangères ; NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge des catalogues pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il renvoie une chaîne vide ("") pour les tables qui n’ont pas de catalogues.|  
|FKTABLE_SCHEM (ODBC 1,0)|6|Varchar|Nom de schéma de la table de clés étrangères ; NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge des schémas pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il renvoie une chaîne vide ("") pour les tables qui n’ont pas de schémas.|  
|FKTABLE_NAME (ODBC 1,0)|7|Varchar non NULL|Nom de la table de clé étrangère.|  
|FKCOLUMN_NAME (ODBC 1,0)|8|Varchar non NULL|Nom de la colonne de clé étrangère. Le pilote retourne une chaîne vide pour une colonne qui n’a pas de nom.|  
|KEY_SEQ (ODBC 1,0)|9|Smallint non NULL|Numéro de séquence de la colonne dans la clé (à partir de 1).|  
|UPDATE_RULE (ODBC 1,0)|10|Smallint|Action à appliquer à la clé étrangère lorsque l’opération SQL est **mise à jour**. Peut avoir l’une des valeurs suivantes. (La table référencée est la table qui contient la clé primaire ; la table de référence est la table qui contient la clé étrangère.)<br /><br /> SQL_CASCADE : lorsque la clé primaire de la table référencée est mise à jour, la clé étrangère de la table de référence est également mise à jour.<br /><br /> SQL_NO_ACTION : si une mise à jour de la clé primaire de la table référencée provoquerait une « référence non résolue » dans la table de référence (autrement dit, les lignes de la table de référence n’auraient pas d’équivalents dans la table référencée), la mise à jour est rejetée. Si une mise à jour de la clé étrangère de la table de référence introduirait une valeur qui n’existe pas en tant que valeur de la clé primaire de la table référencée, la mise à jour est rejetée. (Cette action est identique à l’action SQL_RESTRICT dans ODBC 2 *. x*.)<br /><br /> SQL_SET_NULL : quand une ou plusieurs lignes de la table référencée sont mises à jour de telle sorte qu’un ou plusieurs composants de la clé primaire sont modifiés, les composants de la clé étrangère de la table de référence qui correspondent aux composants modifiés de la clé primaire ont la valeur NULL dans toutes les lignes correspondantes de la table de référence.<br /><br /> SQL_SET_DEFAULT : quand une ou plusieurs lignes de la table référencée sont mises à jour de telle sorte qu’un ou plusieurs composants de la clé primaire sont modifiés, les composants de la clé étrangère de la table de référence qui correspondent aux composants modifiés de la clé primaire sont définis sur les valeurs par défaut applicables dans toutes les lignes correspondantes de la table de référence.<br /><br /> NULL s’il n’est pas applicable à la source de données.|  
|DELETE_RULE (ODBC 1,0)|11|Smallint|Action à appliquer à la clé étrangère lorsque l’opération SQL est **supprimée**. Peut avoir l’une des valeurs suivantes. (La table référencée est la table qui contient la clé primaire ; la table de référence est la table qui contient la clé étrangère.)<br /><br /> SQL_CASCADE : lorsqu’une ligne de la table référencée est supprimée, toutes les lignes correspondantes dans les tables de référence sont également supprimées.<br /><br /> SQL_NO_ACTION : si la suppression d’une ligne dans la table référencée provoquerait une « référence non résolue » dans la table de référence (autrement dit, les lignes de la table de référence n’auraient pas d’équivalents dans la table référencée), la mise à jour est rejetée. (Cette action est identique à l’action SQL_RESTRICT dans ODBC 2 *. x*.)<br /><br /> SQL_SET_NULL : quand une ou plusieurs lignes de la table référencée sont supprimées, chaque composant de la clé étrangère de la table de référence a la valeur NULL dans toutes les lignes correspondantes de la table de référence.<br /><br /> SQL_SET_DEFAULT : quand une ou plusieurs lignes de la table référencée sont supprimées, chaque composant de la clé étrangère de la table de référencement est défini sur la valeur par défaut applicable dans toutes les lignes correspondantes de la table de référence.<br /><br /> NULL s’il n’est pas applicable à la source de données.|  
|FK_NAME (ODBC 2,0)|12|Varchar|Nom de la clé étrangère. NULL s’il n’est pas applicable à la source de données.|  
|PK_NAME (ODBC 2,0)|13|Varchar|Nom de la clé primaire. NULL s’il n’est pas applicable à la source de données.|  
|REPORTABILITÉ (ODBC 3,0)|14|Smallint|SQL_INITIALLY_DEFERRED, SQL_INITIALLY_IMMEDIATE SQL_NOT_DEFERRABLE.|  
  
## <a name="code-example"></a>Exemple de code  
 Comme illustré dans le tableau suivant, cet exemple utilise trois tables, nommées ORDERs, LINEs et CUSTOMers.  
  
|ORDERS|COURBES|ACHETEURS|  
|------------|-----------|---------------|  
|ORDERID|ORDERID|CUSTID|  
|CUSTID|COURBES|NAME|  
|OPENDATE|PARTID|-|  
|Equipe|SPÉCIFIÉE|TÉLÉPHONE|  
|STATUT|||  
  
 Dans la table ORDERs, CUSTID identifie le client auquel la vente a été effectuée. Il s’agit d’une clé étrangère qui fait référence à CUSTID dans la table CUSTOMers.  
  
 Dans la table LINEs, ORDERID identifie la commande client à laquelle la ligne est associée. Il s’agit d’une clé étrangère qui fait référence à ORDERID dans la table ORDERs.  
  
 Cet exemple appelle **SQLPrimaryKeys** pour récupérer la clé primaire de la table Orders. Le jeu de résultats aura une ligne ; les colonnes significatives sont indiquées dans le tableau suivant.  
  
|TABLE_NAME|COLUMN_NAME|KEY_SEQ|  
|-----------------|------------------|--------------|  
|ORDERS|ORDERID|1|  
  
 L’exemple appelle ensuite **SQLForeignKeys** pour récupérer les clés étrangères dans d’autres tables qui font référence à la clé primaire de la table Orders. Le jeu de résultats aura une ligne ; les colonnes significatives sont indiquées dans le tableau suivant.  
  
|PKTABLE_NAME|PKCOLUMN_NAME|FKTABLE_NAME|FKCOLUMN_NAME|KEY_SEQ|  
|-------------------|--------------------|-------------------|--------------------|--------------|  
|ORDERS|CUSTID|COURBES|CUSTID|1|  
  
 Enfin, l’exemple appelle **SQLForeignKeys** pour récupérer les clés étrangères dans la table Orders qui font référence aux clés primaires d’autres tables. Le jeu de résultats aura une ligne ; les colonnes significatives sont indiquées dans le tableau suivant.  
  
|PKTABLE_NAME|PKCOLUMN_NAME|FKTABLE_NAME|FKCOLUMN_NAME|KEY_SEQ|  
|-------------------|--------------------|-------------------|--------------------|--------------|  
|ACHETEURS|CUSTID|ORDERS|CUSTID|1|  
  
```cpp  
#define TAB_LEN SQL_MAX_TABLE_NAME_LEN + 1  
#define COL_LEN SQL_MAX_COLUMN_NAME_LEN + 1  
  
LPSTR   szTable;              /* Table to display */  
  
UCHAR szPkTable[TAB_LEN];   /* Primary key table name */  
UCHAR szFkTable[TAB_LEN];   /* Foreign key table name */  
UCHAR szPkCol[COL_LEN];     /* Primary key column */  
UCHAR szFkCol[COL_LEN];     /* Foreign key column */  
  
SQLHSTMT      hstmt;  
SQLINTEGER    cbPkTable, cbPkCol, cbFkTable, cbFkCol, cbKeySeq;  
SQLSMALLINT   iKeySeq;  
SQLRETURN     retcode;  
  
// Bind the columns that describe the primary and foreign keys.  
// Ignore the table schema, name, and catalog for this example.  
  
SQLBindCol(hstmt, 3, SQL_C_CHAR, szPkTable, TAB_LEN, &cbPkTable);  
SQLBindCol(hstmt, 4, SQL_C_CHAR, szPkCol, COL_LEN, &cbPkCol);  
SQLBindCol(hstmt, 5, SQL_C_SSHORT, &iKeySeq, TAB_LEN, &cbKeySeq);  
SQLBindCol(hstmt, 7, SQL_C_CHAR, szFkTable, TAB_LEN, &cbFkTable);  
SQLBindCol(hstmt, 8, SQL_C_CHAR, szFkCol, COL_LEN, &cbFkCol);  
  
strcpy_s(szTable, sizeof(szTable), "ORDERS");  
  
/* Get the names of the columns in the primary key. */  
  
retcode = SQLPrimaryKeys(hstmt,  
         NULL, 0,             /* Catalog name */  
         NULL, 0,             /* Schema name */  
         szTable, SQL_NTS);   /* Table name */  
  
while ((retcode == SQL_SUCCESS) || (retcode == SQL SUCCESS_WITH_INFO)) {  
  
   /* Fetch and display the result set. This will be a list of the */  
   /* columns in the primary key of the ORDERS table. */  
  
   retcode = SQLFetch(hstmt);  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
      fprintf(out, "Table: %s Column: %s Key Seq: %hd \n", szPkTable, szPkCol,  
      iKeySeq);  
}  
  
/* Close the cursor (the hstmt is still allocated). */  
  
SQLFreeStmt(hstmt, SQL_CLOSE);  
  
/* Get all the foreign keys that refer to ORDERS primary key.*/   
  
retcode = SQLForeignKeys(hstmt,  
         NULL, 0,            /* Primary catalog */  
         NULL, 0,            /* Primary schema */  
         szTable, SQL_NTS,   /* Primary table */  
         NULL, 0,            /* Foreign catalog */  
         NULL, 0,            /* Foreign schema */  
         NULL, 0);           /* Foreign table */  
  
while ((retcode == SQL_SUCCESS) || (retcode == SQL_SUCCESS_WITH_INFO)) {  
  
/* Fetch and display the result set. This will be all of the */  
/* foreign keys in other tables that refer to the ORDERS */  
/* primary key. */  
  
   retcode = SQLFetch(hstmt);  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
      fprintf(out, "%-s ( %-s ) <-- %-s ( %-s )\n", szPkTable,  
               szPkCol, szFkTable, szFkCol);  
}  
  
/* Close the cursor (the hstmt is still allocated). */  
  
SQLFreeStmt(hstmt, SQL_CLOSE);  
  
/* Get all the foreign keys in the ORDERS table. */  
  
retcode = SQLForeignKeys(hstmt,  
         NULL, 0,             /* Primary catalog */  
         NULL, 0,             /* Primary schema */  
         NULL, 0,             /* Primary table */  
         NULL, 0,             /* Foreign catalog */  
         NULL, 0,             /* Foreign schema */  
         szTable, SQL_NTS);   /* Foreign table */  
  
while ((retcode == SQL_SUCCESS) || (retcode == SQL_SUCCESS_WITH_INFO)) {  
  
/* Fetch and display the result set. This will be all of the */  
/* primary keys in other tables that are referred to by foreign */  
/* keys in the ORDERS table. */  
  
   retcode = SQLFetch(hstmt);  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
      fprintf(out, "%-s ( %-s )--> %-s ( %-s )\n", szFkTable, szFkCol, szPkTable, szPkCol);  
}  
  
/* Free the hstmt. */  
SQLFreeStmt(hstmt, SQL_DROP);  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Extraction d’une seule ligne ou d’un bloc de données dans une direction vers l’avant uniquement|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Retour des colonnes d’une clé primaire|[Fonction SQLPrimaryKeys](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|  
|Retour de statistiques de table et d’index|[Fonction SQLStatistics](../../../odbc/reference/syntax/sqlstatistics-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
