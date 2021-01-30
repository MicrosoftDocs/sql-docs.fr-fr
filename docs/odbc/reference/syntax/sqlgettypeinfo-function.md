---
description: Fonction SQLGetTypeInfo
title: Fonction SQLGetTypeInfo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetTypeInfo
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetTypeInfo
helpviewer_keywords:
- SQLGetTypeInfo function [ODBC]
ms.assetid: bdedb044-8924-4ca4-85f3-8b37578e0257
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 55de323b6eace8fa2aa8b138f30ee93938122bc0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189719"
---
# <a name="sqlgettypeinfo-function"></a>Fonction SQLGetTypeInfo
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLGetTypeInfo** retourne des informations sur les types de données pris en charge par la source de données. Le pilote retourne les informations sous la forme d’un jeu de résultats SQL. Les types de données sont destinés à être utilisés dans les instructions DDL (Data Definition Language).  
  
> [!IMPORTANT]  
>  Les applications doivent utiliser les noms de type renvoyés dans la colonne TYPE_NAME du jeu de résultats **SQLGetTypeInfo** dans les instructions **ALTER TABLE** et **Create table** . **SQLGetTypeInfo** peut retourner plusieurs lignes ayant la même valeur dans la colonne data_type.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLGetTypeInfo(  
     SQLHSTMT      StatementHandle,  
     SQLSMALLINT   DataType);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction pour le jeu de résultats.  
  
 *DataType*  
 Entrée Type de données SQL. Il doit s’agir de l’une des valeurs de la section [types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md) de l’annexe D : types de données ou d’un type de données SQL spécifique au pilote. SQL_ALL_TYPES spécifie que les informations sur tous les types de données doivent être retournées.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLGetTypeInfo** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLGetTypeInfo** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S02 ne|Valeur d’option modifiée|Un attribut d’instruction spécifié n’était pas valide en raison des conditions de travail de l’implémentation. une valeur similaire a donc été remplacée temporairement. (Appelez **SQLGetStmtAttr** pour déterminer la valeur substituée temporairement.) La valeur de substitution est valide pour *StatementHandle* jusqu’à ce que le curseur soit fermé. Les attributs d’instruction qui peuvent être modifiés sont : SQL_ATTR_CONCURRENCY, SQL_ATTR_CURSOR_TYPE, SQL_ATTR_KEYSET_SIZE, SQL_ATTR_MAX_LENGTH, SQL_ATTR_MAX_ROWS, SQL_ATTR_QUERY_TIMEOUT et SQL_ATTR_SIMULATE_CURSOR. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|24 000|État de curseur non valide|Un curseur a été ouvert sur *StatementHandle,* et **SQLFetch** ou **SQLFetchScroll** ont été appelés. Cette erreur est retournée par le gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA, et est retourné par le pilote si **sqlfetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un jeu de résultats a été ouvert sur le *StatementHandle*, mais **SQLFetch** ou **SQLFetchScroll** n’a pas été appelé.|  
|40001|Échec de la sérialisation|La transaction a été restaurée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY004|Type de données SQL non valide|La valeur spécifiée pour le type de données de l' *argument n’est* ni un identificateur de type de données SQL ODBC valide, ni un identificateur de type de données spécifique au pilote pris en charge par le pilote.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*, la fonction a été appelée et, avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*. Ensuite, la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et, avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLGetTypeInfo** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’a pas été prise en charge par le pilote ou la source de données.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini sur un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai expiré|Le délai d’expiration de la requête a expiré avant que la source de données n’ait retourné le jeu de résultats. Le délai d’attente est défini à l’aide de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote correspondant à *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 **SQLGetTypeInfo** retourne les résultats sous la forme d’un jeu de résultats standard, classé par data_type, puis par la manière dont le type de données est mappé au type de données SQL ODBC correspondant. Les types de données définis par la source de données ont la priorité sur les types de données définis par l’utilisateur. Par conséquent, l’ordre de tri n’est pas nécessairement cohérent, mais peut être généralisé comme DATA_TYPE en premier, suivi de TYPE_NAME, à la fois dans l’ordre croissant. Par exemple, supposons qu’un entier défini dans la source de données et des types de données de compteur, où COUNTER s’auto-incrémente, et qu’un type de données défini par l’utilisateur WHOLENUM a également été défini. Celles-ci sont retournées dans l’ordre entier, WHOLENUM et compteur, car WHOLENUM est étroitement lié au type de données SQL ODBC SQL_INTEGER, alors que le type de données d’incrémentation automatique, bien qu’il soit pris en charge par la source de données, ne correspond pas étroitement à un type de données SQL ODBC. Pour plus d’informations sur la façon dont ces informations peuvent être utilisées, consultez [instructions DDL](../../../odbc/reference/develop-app/ddl-statements.md).  
  
 Si l’argument *DataType* spécifie un type de données valide pour la version de ODBC prise en charge par le pilote, mais n’est pas pris en charge par le pilote, un jeu de résultats vide est retourné.  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données renvoyées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Les colonnes suivantes ont été renommées pour ODBC 3. *x*. Les modifications apportées aux noms de colonne n’affectent pas la compatibilité descendante, car les applications sont liées par numéro de colonne.  
  
|Colonne ODBC 2,0|ODBC 3. colonne *x*|  
|---------------------|-----------------------|  
|PRECISION|COLUMN_SIZE|  
|MONEY|FIXED_PREC_SCALE|  
|AUTO_INCREMENT|AUTO_UNIQUE_VALUE|  
  
 Les colonnes suivantes ont été ajoutées au jeu de résultats retourné par **SQLGetTypeInfo** pour ODBC 3. *x*:  
  
-   SQL_DATA_TYPE  
  
-   INTERVAL_PRECISION  
  
-   SQL_DATETIME_SUB  
  
-   NUM_PREC_RADIX  
  
 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires au-delà de la colonne 19 (INTERVAL_PRECISION) peuvent être définies par le pilote. Une application doit accéder aux colonnes spécifiques aux pilotes en comptant à partir de la fin de l’ensemble de résultats au lieu de spécifier une position ordinale explicite. Pour plus d’informations, consultez [données retournées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
> [!NOTE]  
>  **SQLGetTypeInfo** peut ne pas retourner tous les types de données. Par exemple, un pilote peut ne pas retourner de types de données définis par l’utilisateur. Les applications peuvent utiliser n’importe quel type de données valide, qu’elles soient retournées ou non par **SQLGetTypeInfo**. Les types de données retournés par **SQLGetTypeInfo** sont ceux pris en charge par la source de données. Ils sont destinés à être utilisés dans les instructions DDL (Data Definition Language). Les pilotes peuvent retourner des données de jeu de résultats à l’aide de types de données autres que les types retournés par **SQLGetTypeInfo**. Lors de la création du jeu de résultats pour une fonction de catalogue, le pilote peut utiliser un type de données qui n’est pas pris en charge par la source de données.  
  
|Nom de la colonne|Colonne<br /><br /> nombre|Type de données|Commentaires|  
|-----------------|-----------------------|---------------|--------------|  
|TYPE_NAME (ODBC 2,0)|1|Varchar non NULL|Nom du type de données dépendant de la source de données ; par exemple, « CHAR () », « VARCHAR () », « MONEY », « LONG VARBINARY » ou « CHAR () FOR BIT DATA ». Les applications doivent utiliser ce nom dans les instructions **Create table** et **ALTER TABLE** .|  
|DATA_TYPE (ODBC 2,0)|2|Smallint non NULL|Type de données SQL. Il peut s’agir d’un type de données SQL ODBC ou d’un type de données SQL spécifique au pilote. Pour les types de données DateTime ou Interval, cette colonne retourne le type de données concis (par exemple, SQL_TYPE_TIME ou SQL_INTERVAL_YEAR_TO_MONTH). Pour obtenir la liste des types de données SQL ODBC valides, consultez types de données [SQL](../../../odbc/reference/appendixes/sql-data-types.md) dans l’annexe D : types de données. Pour plus d’informations sur les types de données SQL spécifiques au pilote, consultez la documentation du pilote.|  
|COLUMN_SIZE (ODBC 2,0)|3|Integer|Taille de colonne maximale que le serveur prend en charge pour ce type de données. Pour les données numériques, il s’agit de la précision maximale. Pour les données de type chaîne, il s’agit de la longueur en caractères. Pour les types de données DateTime, il s’agit de la longueur en caractères de la représentation sous forme de chaîne (en supposant la précision maximale autorisée du composant fractions de seconde). La valeur NULL est retournée pour les types de données où la taille de colonne n’est pas applicable. Pour les types de données Interval, il s’agit du nombre de caractères dans la représentation sous forme de caractère du littéral d’intervalle (comme défini par la précision d’intervalle). consultez longueur des types de [données d’intervalle](../../../odbc/reference/appendixes/interval-data-type-length.md) dans l’annexe D : types de données.<br /><br /> Pour plus d’informations sur la taille des colonnes, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) dans l’annexe D : types de données.|  
|LITERAL_PREFIX (ODBC 2,0)|4|Varchar|Caractères utilisés pour préfixer un littéral ; par exemple, un guillemet simple (') pour les types de données caractères ou 0x pour les types de données binaires ; La valeur NULL est retournée pour les types de données où un préfixe littéral n’est pas applicable.|  
|LITERAL_SUFFIX (ODBC 2,0)|5|Varchar|Caractère ou caractères utilisés pour terminer un littéral ; par exemple, un guillemet simple (') pour les types de données caractères ; La valeur NULL est retournée pour les types de données où un suffixe littéral n’est pas applicable.|  
|CREATE_PARAMS (ODBC 2,0)|6|Varchar|Liste de mots clés, séparés par des virgules, correspondant à chaque paramètre que l’application peut spécifier entre parenthèses lors de l’utilisation du nom retourné dans le champ TYPE_NAME. Les mots clés de la liste peuvent être de l’une des valeurs suivantes : longueur, précision ou échelle. Elles s’affichent dans l’ordre dans lequel elles doivent être utilisées par la syntaxe. Par exemple, CREATE_PARAMS pour DECIMAL serait « Precision, Scale »; CREATE_PARAMS pour VARCHAR est égal à « Length ». NULL est retourné s’il n’existe aucun paramètre pour la définition du type de données ; par exemple, entier.<br /><br /> Le pilote fournit le texte CREATE_PARAMS dans la langue du pays ou de la région où il est utilisé.|  
|NULLABLE (ODBC 2,0)|7|Smallint non NULL|Indique si le type de données accepte une valeur NULL :<br /><br /> SQL_NO_NULLS si le type de données n’accepte pas les valeurs NULL.<br /><br /> SQL_NULLABLE si le type de données accepte les valeurs NULL.<br /><br /> SQL_NULLABLE_UNKNOWN s’il n’est pas connu que la colonne accepte les valeurs NULL.|  
|CASE_SENSITIVE (ODBC 2,0)|8|Smallint non NULL|Si un type de données caractère respecte la casse dans les classements et les comparaisons :<br /><br /> SQL_TRUE si le type de données est un type de données caractère et respecte la casse.<br /><br /> SQL_FALSE si le type de données n’est pas un type de données caractère ou n’est pas sensible à la casse.|  
|RECHERCHE (ODBC 2,0)|9|Smallint non NULL|Comment le type de données est utilisé dans une clause **Where** :<br /><br /> SQL_PRED_NONE si la colonne ne peut pas être utilisée dans une clause **Where** . (C’est identique à la valeur SQL_UNSEARCHABLE dans ODBC 2. *x*.)<br /><br /> SQL_PRED_CHAR si la colonne peut être utilisée dans une clause **Where** , mais uniquement avec le prédicat **Like** . (C’est identique à la valeur SQL_LIKE_ONLY dans ODBC 2. *x*.)<br /><br /> SQL_PRED_BASIC si la colonne peut être utilisée dans une clause **Where** avec tous les opérateurs de comparaison, à l’exception de **Like** (comparaison, comparaison quantifiée, **entre**, **distinct**, **in**, **match** et **unique**). (C’est identique à la valeur SQL_ALL_EXCEPT_LIKE dans ODBC 2. *x*.)<br /><br /> SQL_SEARCHABLE si la colonne peut être utilisée dans une clause **Where** avec un opérateur de comparaison.|  
|UNSIGNED_ATTRIBUTE (ODBC 2,0)|10|Smallint|Indique si le type de données est non signé :<br /><br /> SQL_TRUE si le type de données n’est pas signé.<br /><br /> SQL_FALSE si le type de données est signé.<br /><br /> La valeur NULL est retournée si l’attribut n’est pas applicable au type de données ou si le type de données n’est pas numérique.|  
|FIXED_PREC_SCALE (ODBC 2,0)|11|Smallint non NULL|Si le type de données a une précision et une échelle fixes prédéfinies (qui sont spécifiques à la source de données), telles qu’un type de données Money :<br /><br /> SQL_TRUE s’il a une précision et une échelle fixes prédéfinies.<br /><br /> SQL_FALSE s’il n’a pas de précision et d’échelle fixes prédéfinies.|  
|AUTO_UNIQUE_VALUE (ODBC 2,0)|12|Smallint|Indique si le type de données est auto-incrémenté :<br /><br /> SQL_TRUE si le type de données est auto-incrémenté.<br /><br /> SQL_FALSE si le type de données ne s’auto-incrémente pas.<br /><br /> La valeur NULL est retournée si l’attribut n’est pas applicable au type de données ou si le type de données n’est pas numérique.<br /><br /> Une application peut insérer des valeurs dans une colonne ayant cet attribut, mais ne peut généralement pas mettre à jour les valeurs de la colonne.<br /><br /> Lorsqu’une insertion est effectuée dans une colonne à incrémentation automatique, une valeur unique est insérée dans la colonne au moment de l’insertion. L’incrément n’est pas défini, mais il est spécifique à la source de données. Une application ne doit pas supposer qu’une colonne à incrémentation automatique commence à un point particulier ou qu’elle incrémente une valeur particulière.|  
|LOCAL_TYPE_NAME (ODBC 2,0)|13|Varchar|Version localisée du nom de type de données dépendant de la source de données. La valeur NULL est retournée si un nom localisé n'est pas pris en charge par la source de données. Ce nom est destiné uniquement à l’affichage, par exemple dans les boîtes de dialogue.|  
|MINIMUM_SCALE (ODBC 2,0)|14|Smallint|Échelle minimale du type de données sur la source de données. Si un type de données possède une échelle fixe, les colonnes MINIMUM_SCALE et MAXIMUM_SCALE contiennent toutes les deux cette valeur. Par exemple, une colonne SQL_TYPE_TIMESTAMP peut avoir une échelle fixe pour les fractions de seconde. La valeur NULL est retournée lorsque l'échelle n'est pas applicable. Pour plus d’informations, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) dans l’annexe D : types de données.|  
|MAXIMUM_SCALE (ODBC 2,0)|15|Smallint|Échelle maximale du type de données sur la source de données. La valeur NULL est retournée lorsque l'échelle n'est pas applicable. Si l’échelle maximale n’est pas définie séparément sur la source de données, mais qu’elle est définie comme étant identique à la précision maximale, cette colonne contient la même valeur que la colonne COLUMN_SIZE. Pour plus d’informations, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) dans l’annexe D : types de données.|  
|SQL_DATA_TYPE (ODBC 3,0)|16|Smallint n’est pas NULL|Valeur du type de données SQL tel qu’il apparaît dans le champ SQL_DESC_TYPE du descripteur. Cette colonne est la même que la colonne DATA_TYPE, à l’exception des types de données Interval et DateTime.<br /><br /> Pour les types de données Interval et DateTime, le champ SQL_DATA_TYPE dans le jeu de résultats retourne SQL_INTERVAL ou SQL_DATETIME, et le champ SQL_DATETIME_SUB retourne le sous-code pour l’intervalle ou le type de données DateTime spécifique. (Voir l' [annexe D : types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).)|  
|SQL_DATETIME_SUB (ODBC 3,0)|17|Smallint|Lorsque la valeur de SQL_DATA_TYPE est SQL_DATETIME ou SQL_INTERVAL, cette colonne contient le sous-code DateTime/Interval. Pour les types de données autres que DateTime et Interval, ce champ a la valeur NULL.<br /><br /> Pour les types de données Interval ou DateTime, le champ SQL_DATA_TYPE dans le jeu de résultats retourne SQL_INTERVAL ou SQL_DATETIME, et le champ SQL_DATETIME_SUB retourne le sous-code pour l’intervalle ou le type de données DateTime spécifique. (Voir l' [annexe D : types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).)|  
|NUM_PREC_RADIX (ODBC 3,0)|18|Integer|Si le type de données est un type numérique approximatif, cette colonne contient la valeur 2 pour indiquer que COLUMN_SIZE spécifie un nombre de bits. Pour les types numériques exacts, cette colonne contient la valeur 10 pour indiquer que COLUMN_SIZE spécifie un nombre de chiffres décimaux. Sinon, cette colonne est NULL.|  
|INTERVAL_PRECISION (ODBC 3,0)|19|Smallint|Si le type de données est un type de données Interval, cette colonne contient la valeur de la précision de début de l’intervalle. (Voir la rubrique [précision du type de données Interval](../../../odbc/reference/appendixes/interval-data-type-precision.md) dans annexe D : types de données.) Dans le cas contraire, cette colonne a la valeur NULL.|  
  
 Les informations d’attribut peuvent s’appliquer à des types de données ou à des colonnes spécifiques dans un jeu de résultats. **SQLGetTypeInfo** retourne des informations sur les attributs associés aux types de données ; **SQLColAttribute** retourne des informations sur les attributs associés aux colonnes d’un jeu de résultats.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retour d’informations sur une colonne dans un jeu de résultats|[Fonction SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Extraction d’une seule ligne ou d’un bloc de données dans une direction vers l’avant uniquement|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Retour d’informations sur un pilote ou une source de données|[Fonction SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
