---
description: Fonction SQLProcedures
title: SQLProcedures, fonction | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLProcedures
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLProcedures
helpviewer_keywords:
- SQLProcedures function [ODBC]
ms.assetid: d0d9ef10-2fd4-44a5-9334-649f186f4ba0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c8b3c39243e663f58975918abfa9938d098e6f54
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192569"
---
# <a name="sqlprocedures-function"></a>Fonction SQLProcedures
**Conformité**  
 Version introduite : ODBC 1,0 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLProcedures** renvoie la liste des noms de procédures stockés dans une source de données spécifique. La *procédure* est un terme générique utilisé pour décrire un *objet exécutable*, ou une entité nommée qui peut être appelée à l’aide de paramètres d’entrée et de sortie. Pour plus d’informations sur les procédures, consultez les [procédures](../../../odbc/reference/develop-app/procedures-odbc.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLProcedures(  
     SQLHSTMT       StatementHandle,  
     SQLCHAR *      CatalogName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      SchemaName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      ProcName,  
     SQLSMALLINT    NameLength3);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *Nomcatalogue*  
 Entrée Catalogue de procédures. Si un pilote prend en charge des catalogues pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") dénote les tables qui n’ont pas de catalogues. *Nomcatalogue* ne peut pas contenir un modèle de recherche de chaînes.  
  
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
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLProcedures** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLProcedures** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|24 000|État de curseur non valide|Un curseur a été ouvert sur *StatementHandle*, et **SQLFetch** ou **SQLFetchScroll** ont été appelés. Cette erreur est retournée par le gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA, et est retourné par le pilote si **sqlfetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un curseur a été ouvert sur le *StatementHandle*, mais **SQLFetch** ou **SQLFetchScroll** n’a pas été appelé.|  
|40001|Échec de la sérialisation|La transaction a été restaurée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*. Ensuite, la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|L’attribut de l’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE, l’argument *nomcatalogue* était un pointeur null, et l' *infotype* SQL_CATALOG_NAME retourne que les noms de catalogue sont pris en charge.<br /><br /> (DM) l’attribut d’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE et l’argument *SchemaName* ou *procname* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur de l’un des arguments de longueur de nom était inférieure à 0, mais n’est pas égale à SQL_NTS.<br /><br /> La valeur de l’un des arguments de longueur de nom dépasse la valeur de longueur maximale pour le nom correspondant.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Un catalogue de procédures a été spécifié et le pilote ou la source de données ne prend pas en charge les catalogues.<br /><br /> Un schéma de procédure a été spécifié, et le pilote ou la source de données ne prend pas en charge les schémas.<br /><br /> Un modèle de recherche de chaîne a été spécifié pour le schéma de procédure ou le nom de procédure, et la source de données ne prend pas en charge les modèles de recherche pour un ou plusieurs de ces arguments.<br /><br /> La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’a pas été prise en charge par le pilote ou la source de données.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini sur un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai expiré|Le délai d’expiration de la requête a expiré avant que la source de données n’ait retourné le jeu de résultats demandé. Le délai d’attente est défini à l’aide de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge cette fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 **SQLProcedures** répertorie toutes les procédures de la plage demandée. Un utilisateur peut avoir ou non l’autorisation d’exécuter l’une de ces procédures. Pour vérifier l’accessibilité, une application peut appeler **SQLGetInfo** et vérifier la valeur des informations de SQL_ACCESSIBLE_PROCEDURES. Dans le cas contraire, l’application doit être en mesure de gérer une situation dans laquelle l’utilisateur sélectionne une procédure qu’il ne peut pas exécuter. Pour plus d’informations sur la façon dont ces informations peuvent être utilisées, consultez [procédures](../../../odbc/reference/develop-app/procedures-odbc.md).  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données renvoyées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 **SQLProcedures** retourne les résultats sous la forme d’un jeu de résultats standard, classé par PROCEDURE_CAT, PROCEDURE_SCHEMA et PROCEDURE_NAME.  
  
> [!NOTE]  
>  Les **SQLProcedures** peuvent ne pas retourner toutes les procédures. Les applications peuvent utiliser n’importe quelle procédure valide, qu’elles soient retournées ou non par **SQLProcedures**.  
  
 Les colonnes suivantes ont été renommées pour ODBC 3 *. x*. Les modifications apportées aux noms de colonne n’affectent pas la compatibilité descendante, car les applications sont liées par numéro de colonne.  
  
|Colonne ODBC 2,0|Colonne ODBC 3 *. x*|  
|---------------------|-----------------------|  
|PROCEDURE_QUALIFIER|PROCEDURE_CAT|  
|_OWNER DE PROCÉDURE|_SCHEM DE PROCÉDURE|  
  
 Pour déterminer les longueurs réelles des colonnes PROCEDURE_CAT, PROCEDURE_SCHEM et PROCEDURE_NAME, une application peut appeler **SQLGetInfo** avec les options SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN et SQL_MAX_PROCEDURE_NAME_LEN.  
  
 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires au-delà de la colonne 8 (PROCEDURE_TYPE) peuvent être définies par le pilote. Une application doit accéder aux colonnes spécifiques aux pilotes en comptant à partir de la fin de l’ensemble de résultats au lieu de spécifier une position ordinale explicite. Pour plus d’informations, consultez [données retournées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nom de la colonne|Numéro de colonne|Type de données|Commentaires|  
|-----------------|-------------------|---------------|--------------|  
|PROCEDURE_CAT (ODBC 2,0)|1|Varchar|Identificateur du catalogue de procédures ; NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge les catalogues pour certaines procédures, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il renvoie une chaîne vide ("") pour les procédures qui n’ont pas de catalogues.|  
|PROCEDURE_SCHEM (ODBC 2,0)|2|Varchar|Identificateur du schéma de procédure ; NULL s’il n’est pas applicable à la source de données. Si un pilote prend en charge des schémas pour certaines procédures, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, il retourne une chaîne vide ("") pour les procédures qui n’ont pas de schémas.|  
|PROCEDURE_NAME (ODBC 2,0)|3|Varchar non NULL|Identificateur de la procédure.|  
|NUM_INPUT_PARAMS (ODBC 2,0)|4|N/A|Réservé pour un usage futur. Les applications ne doivent pas s’appuyer sur les données retournées dans ces colonnes de résultats.|  
|NUM_OUTPUT_PARAMS (ODBC 2,0)|5|N/A|Réservé pour un usage futur. Les applications ne doivent pas s’appuyer sur les données retournées dans ces colonnes de résultats.|  
|NUM_RESULT_SETS (ODBC 2,0)|6|N/A|Réservé pour un usage futur. Les applications ne doivent pas s’appuyer sur les données retournées dans ces colonnes de résultats.|  
|REMARQUES (ODBC 2,0)|7|Varchar|Description de la procédure.|  
|PROCEDURE_TYPE (ODBC 2,0)|8|Smallint|Définit le type de la procédure :<br /><br /> SQL_PT_UNKNOWN : il n’est pas possible de déterminer si la procédure retourne une valeur.<br /><br /> SQL_PT_PROCEDURE : l’objet retourné est une procédure ; autrement dit, il n’a pas de valeur de retour.<br /><br /> SQL_PT_FUNCTION : l’objet retourné est une fonction ; autrement dit, il a une valeur de retour.|  
  
 Les arguments *SchemaName* et *pacname* acceptent les modèles de recherche. Pour plus d’informations sur les modèles de recherche valides, consultez [arguments de valeur de modèle](../../../odbc/reference/develop-app/pattern-value-arguments.md).  
  
## <a name="code-example"></a>Exemple de code  
 Consultez [appels de procédure](../../../odbc/reference/develop-app/procedure-calls.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Extraction d’une seule ligne ou d’un bloc de données dans une direction vers l’avant uniquement|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Retour d’informations sur un pilote ou une source de données|[Fonction SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
|Retour des paramètres et des colonnes de l’ensemble de résultats d’une procédure|[Fonction SQLProcedureColumns](../../../odbc/reference/syntax/sqlprocedurecolumns-function.md)|  
|Syntaxe pour l’appel de procédures stockées|[Exécution d’instructions](../../../odbc/reference/develop-app/executing-statements-odbc.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
