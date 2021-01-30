---
description: Fonction SQLSpecialColumns
title: SQLSpecialColumns fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSpecialColumns
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSpecialColumns
helpviewer_keywords:
- SQLSpecialColumns function [ODBC]
ms.assetid: bb2d9f21-bda0-4e50-a8be-f710db660034
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cf4f810499c4a3409d134806ead739096c5b1b89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191758"
---
# <a name="sqlspecialcolumns-function"></a>Fonction SQLSpecialColumns
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ouvrir un groupe  
  
 **Résumé**  
 **SQLSpecialColumns** récupère les informations suivantes sur les colonnes d’une table spécifiée :  
  
-   Ensemble optimal de colonnes qui identifie de façon unique une ligne dans la table.  
  
-   Les colonnes qui sont mises à jour automatiquement lorsqu’une valeur de la ligne est mise à jour par une transaction.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLSpecialColumns(  
     SQLHSTMT      StatementHandle,  
     SQLSMALLINT   IdentifierType,  
     SQLCHAR *     CatalogName,  
     SQLSMALLINT   NameLength1,  
     SQLCHAR *     SchemaName,  
     SQLSMALLINT   NameLength2,  
     SQLCHAR *     TableName,  
     SQLSMALLINT   NameLength3,  
     SQLSMALLINT   Scope,  
     SQLSMALLINT   Nullable);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *IdentifierType*  
 Entrée Type de colonne à retourner. Il doit s’agir de l’une des valeurs suivantes :   
  
 SQL_BEST_ROWID : retourne la colonne ou l’ensemble de colonnes optimal qui, en extrayant des valeurs de la ou des colonnes, permet d’identifier de manière unique toutes les lignes de la table spécifiée. Une colonne peut être soit une pseudo-colonne spécifiquement conçue à cet effet (comme dans Oracle ROWID ou Ingres TID), soit la ou les colonnes d’un index unique pour la table.  
  
 SQL_ROWVER : retourne la ou les colonnes de la table spécifiée, le cas échéant, qui sont automatiquement mises à jour par la source de données lorsqu’une valeur de la ligne est mise à jour par une transaction (comme dans SQLBase ROWID ou Sybase TIMESTAMP).  
  
 *Nomcatalogue*  
 Entrée Nom de catalogue de la table. Si un pilote prend en charge des catalogues pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") dénote les tables qui n’ont pas de catalogues. *Nomcatalogue* ne peut pas contenir un modèle de recherche de chaînes.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *nomcatalogue* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *nomcatalogue* est un argument ordinaire ; Il est traité littéralement et sa casse est importante. Pour plus d’informations, consultez [arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Entrée Longueur en caractères de **nomcatalogue*.  
  
 *SchemaName*  
 Entrée Nom de schéma de la table. Si un pilote prend en charge des schémas pour certaines tables, mais pas pour d’autres, par exemple lorsque le pilote récupère des données à partir de différents SGBD, une chaîne vide ("") dénote les tables qui n’ont pas de schémas. *SchemaName* ne peut pas contenir un modèle de recherche de chaînes.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *SchemaName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *SchemaName* est un argument ordinaire ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength2*  
 Entrée Longueur en caractères de **SchemaName*.  
  
 *TableName*  
 Entrée Nom de la table. Cet argument ne peut pas être un pointeur null. *TableName* ne peut pas contenir un modèle de recherche de chaînes.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, *TableName* est traité comme un identificateur et sa casse n’est pas significative. S’il est SQL_FALSE, *TableName* est un argument ordinaire ; Il est traité littéralement et sa casse est importante.  
  
 *NameLength3*  
 Entrée Longueur en caractères de **TableName*.  
  
 *Portée*  
 Entrée Étendue minimale requise de ROWID. Le ROWID retourné peut être de plus grande portée. Doit prendre l'une des valeurs suivantes :  
  
 SQL_SCOPE_CURROW : la validité de ROWID est garantie uniquement lorsqu’elle est positionnée sur cette ligne. Une nouvelle sélection ultérieure à l’aide de ROWID peut ne pas retourner une ligne si la ligne a été mise à jour ou supprimée par une autre transaction.  
  
 SQL_SCOPE_TRANSACTION : la validité de ROWID est garantie pour la durée de la transaction actuelle.  
  
 SQL_SCOPE_SESSION : la validité de ROWID est garantie pour la durée de la session (à travers les limites de transaction).  
  
 *Nullable*  
 Entrée Détermine s’il est nécessaire de retourner des colonnes spéciales qui peuvent avoir une valeur NULL. Doit prendre l'une des valeurs suivantes :  
  
 SQL_NO_NULLS : exclut les colonnes spéciales qui peuvent avoir des valeurs NULL. Certains pilotes ne prennent pas en charge SQL_NO_NULLS, et ces pilotes retournent un jeu de résultats vide si SQL_NO_NULLS a été spécifié. Les applications doivent être préparées pour ce cas et demander SQL_NO_NULLS uniquement si elles sont absolument requises.  
  
 SQL_NULLABLE : retourner des colonnes spéciales, même si elles peuvent avoir des valeurs NULL.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLSpecialColumns** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLSpecialColumns** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|24 000|État de curseur non valide|Un curseur a été ouvert sur *StatementHandle*, et **SQLFetch** ou **SQLFetchScroll** ont été appelés. Cette erreur est retournée par le gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA et est retourné par le pilote si **sqlfetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un curseur a été ouvert sur le *StatementHandle*, mais **SQLFetch** ou **SQLFetchScroll** n’a pas été appelé.|  
|40001|Échec de la sérialisation|La transaction a été restaurée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*. Ensuite, la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|L’argument *TableName* était un pointeur null.<br /><br /> L’attribut de l’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE, l’argument *nomcatalogue* était un pointeur null, et l' *infotype* SQL_CATALOG_NAME retourne que les noms de catalogue sont pris en charge.<br /><br /> (DM) l’attribut d’instruction SQL_ATTR_METADATA_ID a été défini sur SQL_TRUE et l’argument *SchemaName* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction était toujours en cours d’exécution lors de l’appel de **SQLSpecialColumns** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur de l’un des arguments de longueur était inférieure à 0, mais n’est pas égale à SQL_NTS.<br /><br /> La valeur de l’un des arguments de longueur dépasse la valeur de longueur maximale pour le nom correspondant. La longueur maximale de chaque nom peut être obtenue en appelant **SQLGetInfo** avec les valeurs d' *infotype* : SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN ou SQL_MAX_TABLE_NAME_LEN.|  
|HY097|Type de colonne hors limites|(DM) une valeur *IdentifierType* non valide a été spécifiée.|  
|HY098|Type d’étendue hors limites|(DM) une valeur d' *étendue* non valide a été spécifiée.|  
|HY099|Type Nullable hors limites|(DM) une valeur *Nullable* non valide a été spécifiée.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Un catalogue a été spécifié et le pilote ou la source de données ne prend pas en charge les catalogues.<br /><br /> Un schéma a été spécifié, et le pilote ou la source de données ne prend pas en charge les schémas.<br /><br /> La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’a pas été prise en charge par le pilote ou la source de données.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini sur un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai expiré|Le délai d’expiration de la requête a expiré avant que la source de données n’ait retourné le jeu de résultats demandé. Le délai d’attente est défini à l’aide de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 Lorsque l’argument *IdentifierType* est SQL_BEST_ROWID, **SQLSpecialColumns** retourne la ou les colonnes qui identifient de manière unique chaque ligne dans la table. Ces colonnes peuvent toujours être utilisées dans une clause *Select-List* ou **Where** . **SQLColumns**, qui est utilisé pour retourner une variété d’informations sur les colonnes d’une table, ne retourne pas nécessairement les colonnes qui identifient de manière unique chaque ligne, ou les colonnes qui sont mises à jour automatiquement lorsqu’une transaction met à jour une valeur de la ligne. Par exemple, **SQLColumns** peut ne pas retourner le ID de colonne de la pseudo-colonne Oracle. C’est pourquoi **SQLSpecialColumns** est utilisé pour renvoyer ces colonnes. Pour plus d’informations, consultez [utilisation des données de catalogue](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données renvoyées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 S’il n’existe aucune colonne qui identifie de façon unique chaque ligne dans la table, **SQLSpecialColumns** retourne un ensemble de lignes sans ligne ; un appel ultérieur à **SQLFetch** ou **SQLFetchScroll** sur l’instruction retourne SQL_NO_DATA.  
  
 Si les arguments *IdentifierType*, *scope* ou *Nullable* spécifient des caractéristiques qui ne sont pas prises en charge par la source de données, **SQLSpecialColumns** retourne un jeu de résultats vide.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID est défini sur SQL_TRUE, les arguments *nomcatalogue*, *SchemaName* et *TableName* sont traités comme des identificateurs, donc ils ne peuvent pas être définis sur un pointeur null dans certaines situations. (Pour plus d’informations, consultez [arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).)  
  
 **SQLSpecialColumns** retourne les résultats sous la forme d’un jeu de résultats standard, classé par étendue.  
  
 Les colonnes suivantes ont été renommées pour ODBC *3. x*. Les modifications apportées aux noms de colonne n’affectent pas la compatibilité descendante, car les applications sont liées par numéro de colonne.  
  
|Colonne ODBC 2,0|Colonne ODBC *3. x*|  
|---------------------|-----------------------|  
|PRECISION|COLUMN_SIZE|  
|LENGTH|BUFFER_LENGTH|  
|SCALE|DECIMAL_DIGITS|  
  
 Pour déterminer la longueur réelle de la colonne COLUMN_NAME, une application peut appeler **SQLGetInfo** avec l’option SQL_MAX_COLUMN_NAME_LEN.  
  
 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires au-delà de la colonne 8 (PSEUDO_COLUMN) peuvent être définies par le pilote. Une application doit accéder aux colonnes spécifiques aux pilotes en comptant à partir de la fin de l’ensemble de résultats au lieu de spécifier une position ordinale explicite. Pour plus d’informations, consultez [données retournées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nom de la colonne|Numéro de colonne|Type de données|Commentaires|  
|-----------------|-------------------|---------------|--------------|  
|ÉTENDUE (ODBC 1,0)|1|Smallint|Étendue réelle de ROWID. Contient l’une des valeurs suivantes :<br /><br /> SQL_SCOPE_CURROW SQL_SCOPE_TRANSACTION SQL_SCOPE_SESSION<br /><br /> La valeur NULL est retournée lorsque *IdentifierType* est SQL_ROWVER. Pour obtenir une description de chaque valeur, consultez la description de l' *étendue* dans « syntaxe », plus haut dans cette section.|  
|COLUMN_NAME (ODBC 1,0)|2|Varchar non NULL|Nom de la colonne. Le pilote retourne une chaîne vide pour une colonne qui n’a pas de nom.|  
|DATA_TYPE (ODBC 1,0)|3|Smallint non NULL|Type de données SQL. Il peut s’agir d’un type de données SQL ODBC ou d’un type de données SQL spécifique au pilote. Pour obtenir la liste des types de données SQL ODBC valides, consultez [types de données](../../../odbc/reference/appendixes/sql-data-types.md)SQL. Pour plus d’informations sur les types de données SQL spécifiques au pilote, consultez la documentation du pilote.|  
|TYPE_NAME (ODBC 1,0)|4|Varchar non NULL|Nom du type de données dépendant de la source de données ; par exemple, « CHAR », « VARCHAR », « MONEY », « LONG VARBINARY » ou « CHAR () FOR BIT DATA ».|  
|COLUMN_SIZE (ODBC 1,0)|5|Integer|Taille de la colonne sur la source de données. Pour plus d’informations sur la taille des colonnes, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).|  
|BUFFER_LENGTH (ODBC 1,0)|6|Integer|Longueur, en octets, des données transférées sur une opération **SQLGetData** ou **SQLFetch** si SQL_C_DEFAULT est spécifié. Pour les données numériques, cette taille peut être différente de la taille des données stockées dans la source de données. Cette valeur est la même que la colonne COLUMN_SIZE pour les données de type caractère ou binaire. Pour plus d’informations, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).|  
|DECIMAL_DIGITS (ODBC 1,0)|7|Smallint|Chiffres décimaux de la colonne sur la source de données. La valeur NULL est retournée pour les types de données où les chiffres décimaux ne sont pas applicables. Pour plus d’informations sur les chiffres décimaux, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).|  
|PSEUDO_COLUMN (ODBC 2,0)|8|Smallint|Indique si la colonne est une pseudo-colonne, par exemple, ROWID Oracle :<br /><br /> SQL_PC_UNKNOWN SQL_PC_NOT_PSEUDO SQL_PC_PSEUDO **Remarque :**  pour une interopérabilité maximale, les pseudo-colonnes ne doivent pas être placées entre guillemets par le caractère de guillemet d’identificateur retourné par **SQLGetInfo**.|  
  
 Une fois que l’application a extrait des valeurs pour SQL_BEST_ROWID, elle peut utiliser ces valeurs pour resélectionner cette ligne dans l’étendue définie. Il est garanti que l’instruction **Select** ne retourne pas de lignes ou une ligne.  
  
 Si une application resélectionne une ligne en fonction de la ou des colonnes ROWID et que la ligne est introuvable, l’application peut supposer que la ligne a été supprimée ou que les colonnes ROWID ont été modifiées. L’inverse n’est pas vrai : même si ROWID n’a pas changé, les autres colonnes de la ligne peuvent avoir changé.  
  
 Les colonnes retournées pour le type de colonne SQL_BEST_ROWID sont utiles pour les applications qui doivent faire défiler vers l’avant et vers l’arrière dans un jeu de résultats afin de récupérer les données les plus récentes d’un ensemble de lignes. Il est garanti que la ou les colonnes de ROWID ne changent pas lorsqu’elles sont positionnées sur cette ligne.  
  
 La ou les colonnes de ROWID peuvent rester valides même si le curseur n’est pas positionné sur la ligne ; l’application peut déterminer cela en vérifiant la colonne étendue dans le jeu de résultats.  
  
 Les colonnes retournées pour le type de colonne SQL_ROWVER sont utiles pour les applications qui ont besoin de vérifier si des colonnes d’une ligne donnée ont été mises à jour pendant que la ligne a été resélectionnée à l’aide de ROWID. Par exemple, après avoir resélectionné une ligne à l’aide de ROWID, l’application peut comparer les valeurs précédentes dans les colonnes SQL_ROWVER à celles qui viennent d’être extraites. Si la valeur d’une colonne SQL_ROWVER diffère de la valeur précédente, l’application peut avertir l’utilisateur que les données sur l’affichage ont changé.  
  
## <a name="code-example"></a>Exemple de code  
 Pour obtenir un exemple de code d’une fonction similaire, consultez [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retour des colonnes dans une table ou des tables|[Fonction SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)|  
|Extraction d’une seule ligne ou d’un bloc de données dans une direction vers l’avant uniquement|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Retour des colonnes d’une clé primaire|[Fonction SQLPrimaryKeys](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
