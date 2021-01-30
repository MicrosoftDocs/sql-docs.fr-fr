---
description: SQLExecDirect, fonction
title: Fonction SQLExecDirect | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLExecDirect
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLExecDirect
helpviewer_keywords:
- SQLExecDirect function [ODBC]
ms.assetid: 985fcee1-f204-425c-bdd1-deb0e7d7bbd9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 938d2aa1631041e9dbb7a51ea952ff7d60811fb4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179884"
---
# <a name="sqlexecdirect-function"></a>SQLExecDirect, fonction
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLExecDirect** exécute une instruction préparable, en utilisant les valeurs actuelles des variables de marqueur de paramètre si des paramètres existent dans l’instruction. **SQLExecDirect** est le moyen le plus rapide pour soumettre une instruction SQL pour une exécution unique.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLExecDirect(  
     SQLHSTMT     StatementHandle,  
     SQLCHAR *    StatementText,  
     SQLINTEGER   TextLength);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *StatementText*  
 Entrée Instruction SQL à exécuter.  
  
 *TextLength*  
 Entrée Longueur de **StatementText* en caractères.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_STILL_EXECUTING, SQL_ERROR, SQL_NO_DATA, SQL_INVALID_HANDLE ou SQL_PARAM_DATA_AVAILABLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLExecDirect** retourne soit SQL_ERROR, soit SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLExecDirect** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01001|Conflit d’opération de curseur|\**StatementText* contenait une instruction UPDATE ou DELETE positionnée, et aucune ligne ou plus d’une ligne n’a été mise à jour ou supprimée. (Pour plus d’informations sur les mises à jour de plusieurs lignes, consultez la description de l' *attribut* SQL_ATTR_SIMULATE_CURSOR dans **SQLSetStmtAttr**.)<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01003|Valeur NULL supprimée dans la fonction Set|L’argument *StatementText* contenait une fonction Set (telle que **AVG**, **Max**, **min**, etc.), mais pas la fonction **Count** Set, et les valeurs d’argument null ont été éliminées avant l’application de la fonction. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Les données binaires ou de chaîne retournées pour un paramètre d’entrée/sortie ou de sortie ont entraîné la troncation d’un caractère non vide ou de données binaires non NULL. S’il s’agit d’une valeur de chaîne, elle a été tronquée à droite. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01006|Privilège non révoqué|\**StatementText* contenait une instruction **Revoke** , et l’utilisateur ne disposait pas du privilège spécifié. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01007|Privilège non accordé|*\* StatementText* était une instruction **Grant** , et l’utilisateur n’a pas pu accorder le privilège spécifié.|  
|01S02 ne|Valeur d’option modifiée|Un attribut d’instruction spécifié n’était pas valide en raison des conditions de travail de l’implémentation. une valeur similaire a donc été remplacée temporairement. (**SQLGetStmtAttr** peut être appelé pour déterminer la valeur de substitution temporaire.) La valeur de substitution est valide pour le *StatementHandle* jusqu’à la fermeture du curseur, auquel cas l’attribut d’instruction revient à sa valeur précédente. Les attributs d’instruction qui peuvent être modifiés sont les suivants :<br /><br /> SQL_ ATTR_CONCURRENCY SQL_ ATTR_CURSOR_TYPE SQL_ ATTR_KEYSET_SIZE SQL_ ATTR_MAX_LENGTH SQL_ ATTR_MAX_ROWS SQL_ ATTR_QUERY_TIMEOUT SQL_ ATTR_SIMULATE_CURSOR<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S07|Troncation fractionnaire|Les données retournées pour un paramètre d’entrée/sortie ou de sortie ont été tronquées de telle sorte que la partie fractionnaire d’un type de données numérique était tronquée ou la partie fractionnaire du composant heure d’un type de données time, timestamp ou Interval était tronquée.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07002|Champ de nombre incorrect|Le nombre de paramètres spécifiés dans **SQLBindParameter** était inférieur au nombre de paramètres dans l’instruction SQL contenue dans \* *StatementText*.<br /><br /> **SQLBindParameter** a été appelé avec *ParameterValuePtr* défini sur un pointeur null, *StrLen_or_IndPtr* pas défini sur SQL_NULL_DATA ou SQL_DATA_AT_EXEC, et *InputOutputType* n’a pas la valeur SQL_PARAM_OUTPUT, de sorte que le nombre de paramètres spécifiés dans **SQLBindParameter** soit supérieur au nombre de paramètres dans l’instruction SQL contenue dans **StatementText*.|  
|07006|Violation d’attribut de type de données restreint|La valeur de données identifiée par l’argument *ValueType* dans **SQLBindParameter** pour le paramètre lié n’a pas pu être convertie dans le type de données identifié par l’argument *ParameterType* dans **SQLBindParameter**.<br /><br /> La valeur de données retournée pour un paramètre lié en tant que SQL_PARAM_INPUT_OUTPUT ou SQL_PARAM_OUTPUT n’a pas pu être convertie dans le type de données identifié par l’argument *ValueType* dans **SQLBindParameter**.<br /><br /> (Si les valeurs de données pour une ou plusieurs lignes n’ont pas pu être converties mais qu’une ou plusieurs lignes ont été correctement retournées, cette fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07007|Violation de valeur de paramètre restreinte|Le type de paramètre SQL_PARAM_INPUT_OUTPUT_STREAM est utilisé uniquement pour un paramètre qui envoie et reçoit des données en plusieurs parties. Une mémoire tampon d’entrée n’est pas autorisée pour ce type de paramètre.<br /><br /> Cette erreur se produit lorsque le type de paramètre est SQL_PARAM_INPUT_OUTPUT et lorsque le \* *StrLen_or_IndPtr* spécifié dans **SQLBindParameter** n’est pas égal à SQL_NULL_DATA, SQL_DEFAULT_PARAM, SQL_LEN_DATA_AT_EXEC (Len) ou SQL_DATA_AT_EXEC.|  
|07S01|Utilisation non valide du paramètre par défaut|Une valeur de paramètre, définie avec **SQLBindParameter**, a été SQL_DEFAULT_PARAM, et le paramètre correspondant n’a pas de valeur par défaut.|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|21S01|La liste d’insertion de valeurs ne correspond pas à la liste de colonnes|\**StatementText* contenait une instruction **Insert** , et le nombre de valeurs à insérer ne correspondait pas au degré de la table dérivée.|  
|21S02|Le degré de la table dérivée ne correspond pas à la liste des colonnes|\**StatementText* contenait une instruction **Create View** , et la liste de colonnes non qualifiée (le nombre de colonnes spécifié pour la vue dans les arguments de l' *identificateur de colonne* de l’instruction SQL) contenait plus de noms que le nombre de colonnes dans la table dérivée définie par l’argument de *spécification de requête* de l’instruction SQL.|  
|22001|Chaîne de données, troncation à droite|L’affectation d’une valeur de type caractère ou binaire à une colonne a entraîné la troncation des données de type caractère non vides ou des données binaires non null.|  
|22002|Variable d’indicateur requise mais non fournie|Les données NULL ont été liées à un paramètre de sortie dont la *StrLen_or_IndPtr* définie par **SQLBindParameter** était un pointeur null.|  
|22003|Valeur numérique hors limites|**StatementText* contenait une instruction SQL qui contenait un paramètre numérique ou un littéral lié, et la valeur provoquant la partie entière (par opposition à la fraction) du nombre à tronquer lorsqu’elle est assignée à la colonne de table associée.<br /><br /> Le retour d’une valeur numérique (sous forme de valeur numérique ou de chaîne) pour un ou plusieurs paramètres d’entrée/sortie ou de sortie a entraîné la troncation de la partie entière (par opposition à la fraction) du nombre à tronquer.|  
|22007|Format de date/heure non valide|**StatementText* contenait une instruction SQL qui contenait une structure de date, d’heure ou d’horodatage sous la forme d’un paramètre lié, et le paramètre était, respectivement, une date, une heure ou un horodateur non valide.<br /><br /> Un paramètre d’entrée/sortie ou de sortie a été lié à une structure de date, d’heure ou d’horodatage C, et une valeur dans le paramètre retourné était, respectivement, une date, une heure ou un horodateur non valide. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|22008|Dépassement du champ DateTime|**StatementText* contenait une instruction SQL qui contenait une expression DateTime qui, lorsqu’elle était calculée, entraînait une structure de date, d’heure ou d’horodatage qui était non valide.<br /><br /> Une expression DateTime calculée pour un paramètre d’entrée/sortie ou de sortie a entraîné la non-validité d’une valeur de date, d’heure ou d’horodatage.|  
|22012|Division par zéro|**StatementText* contenait une instruction SQL qui contenait une expression arithmétique qui provoquait une division par zéro.<br /><br /> Une expression arithmétique calculée pour un paramètre d’entrée/sortie ou de sortie a entraîné une division par zéro.|  
|22015|Dépassement du champ d’intervalle|*\* StatementText* contenait un paramètre numérique ou d’intervalle exact qui, lorsqu’il est converti en un type de données SQL Interval, entraînait une perte de chiffres significatifs.<br /><br /> *\* StatementText* contenait un paramètre Interval avec plusieurs champs qui, lorsqu’il est converti en un type de données numérique dans une colonne, n’avait aucune représentation dans le type de données numeric.<br /><br /> *\* StatementText* contenait des données de paramètre qui étaient affectées à un type SQL Interval, et il n’existait aucune représentation de la valeur du type C dans le type SQL Interval.<br /><br /> L’attribution d’un paramètre d’entrée/sortie ou de sortie qui était un type de données numérique ou d’intervalle exact à un type d’intervalle C a provoqué une perte de chiffres significatifs.<br /><br /> Lorsqu’un paramètre d’entrée/sortie ou de sortie a été assigné à une structure d’intervalle C, il n’existait aucune représentation des données dans la structure des données de l’intervalle.|  
|22018|Valeur de caractère non valide pour la spécification de cast|*\* StatementText* contenait un type C qui était un type de données numeric exact ou approximatif, DateTime ou Interval ; le type SQL de la colonne était un type de données character et la valeur de la colonne n’était pas un littéral valide du type C lié.<br /><br /> Lorsqu’un paramètre d’entrée/sortie ou de sortie a été retourné, le type SQL était un type de données numérique exact ou approximatif, DateTime ou Interval ; le type C a été SQL_C_CHAR ; et la valeur dans la colonne n’est pas un littéral valide du type SQL lié.|  
|22019|Caractère d’échappement non valide|\**StatementText* contenait une instruction SQL qui contenait un prédicat **Like** avec un caractère d' **échappement** dans la clause **Where** , et la longueur du caractère d’échappement qui suit **Escape** n’était pas égale à 1.|  
|22025|Séquence d’échappement non valide|\**StatementText* contient une instruction SQL qui contenait «**Like** _caractère_ d’échappement d' **échappement** de _valeur de modèle_ » dans la clause **Where** , et le caractère qui suit le caractère d’échappement dans la valeur de modèle ne faisait pas partie de « % » ou « _ ».|  
|23000|Violation de contrainte d’intégrité|**StatementText* contenait une instruction SQL qui contenait un paramètre ou un littéral. La valeur du paramètre était NULL pour une colonne définie comme NOT NULL dans la colonne de table associée, une valeur dupliquée a été fournie pour une colonne contrainte à contenir uniquement des valeurs uniques, ou une autre contrainte d’intégrité a été violée.|  
|24 000|État de curseur non valide|Un curseur a été placé sur le *StatementHandle* par **SQLFetch** ou **SQLFetchScroll**. Cette erreur est retournée par le gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA, et est retourné par le pilote si **sqlfetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un curseur a été ouvert mais n’est pas positionné sur le *StatementHandle*.<br /><br /> **StatementText* contenait une instruction UPDATE ou DELETE positionnée, et le curseur était positionné avant le début du jeu de résultats ou après la fin du jeu de résultats.|  
|34000|Nom de curseur non valide|**StatementText* contenait une instruction UPDATE ou DELETE positionnée, et le curseur référencé par l’instruction en cours d’exécution n’était pas ouvert.|  
|3D000|Nom de catalogue non valide|Le nom de catalogue spécifié dans *StatementText* n’est pas valide.|  
|3F000|Nom de schéma non valide|Le nom de schéma spécifié dans *StatementText* n’est pas valide.|  
|40001|Échec de la sérialisation|La transaction a été restaurée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|42000|Erreur de syntaxe ou violation d’accès|\**StatementText* contenait une instruction SQL qui n’était pas préparable ou contenait une erreur de syntaxe.<br /><br /> L’utilisateur n’a pas l’autorisation d’exécuter l’instruction SQL contenue dans **StatementText*.|  
|42S01|La table de base ou la vue existe déjà|\**StatementText* contenait une instruction **Create Table** ou **Create View** , et le nom de la table ou le nom de la vue spécifiés existe déjà.|  
|42S02|La table de base ou la vue est introuvable|\**StatementText* contenait une instruction DROP **table** ou **Drop View** , et le nom de la table ou le nom de la vue spécifiés n’existait pas.<br /><br /> \**StatementText* contenait une instruction **ALTER TABLE** , et le nom de table spécifié n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Create View** , et un nom de table ou de vue défini par la spécification de la requête n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Create index** et le nom de la table spécifié n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Grant** ou **Revoke** , et le nom de la table ou de la vue spécifié n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Select** , et un nom de table ou de vue spécifié n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Delete**, **Insert** ou **Update** , et le nom de la table spécifié n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Create table** , et une table spécifiée dans une contrainte (référençant une table autre que celle en cours de création) n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Create Schema** , et un nom de table ou de vue spécifié n’existait pas.|  
|42S11|L’index existe déjà|\**StatementText* contenait une instruction **Create index** et le nom d’index spécifié existait déjà.<br /><br /> \**StatementText* contenait une instruction **Create Schema** et le nom d’index spécifié existait déjà.|  
|42S12|Index introuvable|\**StatementText* contenait une instruction **Drop index** et le nom d’index spécifié n’existait pas.|  
|42S21|La colonne existe déjà|\**StatementText* contenait une instruction **ALTER TABLE** , et la colonne spécifiée dans la clause **Add** n’est pas unique ou identifie une colonne existante dans la table de base.|  
|42S22|Colonne introuvable|\**StatementText* contenait une instruction **Create index** , et un ou plusieurs des noms de colonnes spécifiés dans la liste des colonnes n’existaient pas.<br /><br /> \**StatementText* contenait une instruction **Grant** ou **Revoke** , et un nom de colonne spécifié n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Select**, **Delete**, **Insert** ou **Update** , et un nom de colonne spécifié n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Create table** et une colonne spécifiée dans une contrainte (référençant une table autre que celle en cours de création) n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Create Schema** , et un nom de colonne spécifié n’existait pas.|  
|44000|Violation de WITH CHECK OPTION|L’argument *StatementText* contenait une instruction **Insert** exécutée sur une table affichée ou une table dérivée de la table affichée qui a été créée en spécifiant **with Check option**, de telle sorte qu’une ou plusieurs lignes affectées par l’instruction **Insert** ne seront plus présentes dans la table affichée.<br /><br /> L’argument *StatementText* contenait une instruction **Update** exécutée sur une table affichée ou une table dérivée de la table affichée qui a été créée en spécifiant **with Check option**, de sorte qu’une ou plusieurs lignes affectées par l’instruction **Update** ne soient plus présentes dans la table affichée.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*. Ensuite, la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|(DM) **StatementText* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLExecDirect** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) l’argument *TextLength* était inférieur ou égal à 0, mais n’est pas égal à SQL_NTS.<br /><br /> Une valeur de paramètre, définie avec **SQLBindParameter**, était un pointeur null et la valeur de la longueur du paramètre était différente de 0, SQL_NULL_DATA, SQL_DATA_AT_EXEC, SQL_DEFAULT_PARAM ou inférieure ou égale à SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> Une valeur de paramètre, définie avec **SQLBindParameter**, n’était pas un pointeur null ; le type de données C était SQL_C_BINARY ou SQL_C_CHAR ; et la valeur de la longueur du paramètre est inférieure à 0, mais n’a pas été SQL_NTS, SQL_NULL_DATA, SQL_DATA_AT_EXEC, SQL_DEFAULT_PARAM, ou inférieur ou égal à SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> Une valeur de longueur de paramètre liée par **SQLBindParameter** a été définie sur SQL_DATA_AT_EXEC ; le type SQL était SQL_LONGVARCHAR, SQL_LONGVARBINARY ou un type de données long spécifique à la source de données ; et le type d’information SQL_NEED_LONG_DATA_LEN dans **SQLGetInfo** était « Y ».|  
|HY105|Type de paramètre non valide|La valeur spécifiée pour l’argument *InputOutputType* dans **SQLBindParameter** était SQL_PARAM_OUTPUT, et le paramètre était un paramètre d’entrée.|  
|HY109|Position de curseur non valide|\**StatementText* contenait une instruction UPDATE ou DELETE positionnée, et le curseur était positionné (par **SQLSetPos** ou **SQLFetchScroll**) sur une ligne qui avait été supprimée ou n’a pas pu être extraite.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’a pas été prise en charge par le pilote ou la source de données.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini sur un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai expiré|Le délai d’expiration de la requête a expiré avant que la source de données n’ait retourné le jeu de résultats. Le délai d’attente est défini à l’aide de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 L’application appelle **SQLExecDirect** pour envoyer une instruction SQL à la source de données. Pour plus d’informations sur l’exécution directe, consultez [exécution directe](../../../odbc/reference/develop-app/direct-execution-odbc.md). Le pilote modifie l’instruction pour utiliser la forme SQL utilisée par la source de données, puis l’envoie à la source de données. En particulier, le pilote modifie les séquences d’échappement utilisées pour définir certaines fonctionnalités dans SQL. Pour la syntaxe des séquences d’échappement, consultez [séquences d’échappement dans ODBC](../../../odbc/reference/develop-app/escape-sequences-in-odbc.md).  
  
 L’application peut inclure un ou plusieurs marqueurs de paramètres dans l’instruction SQL. Pour inclure un marqueur de paramètre, l’application incorpore un point d’interrogation ( ?) dans l’instruction SQL à la position appropriée. Pour plus d’informations sur les paramètres, consultez [paramètres d’instruction](../../../odbc/reference/develop-app/statement-parameters.md).  
  
 Si l’instruction SQL est une instruction **Select** et que l’application appelée **SQLSetCursorName** pour associer un curseur à une instruction, le pilote utilise le curseur spécifié. Dans le cas contraire, le pilote génère un nom de curseur.  
  
 Si la source de données est en mode de validation manuelle (nécessitant un lancement de transaction explicite) et qu’une transaction n’a pas déjà été lancée, le pilote lance une transaction avant d’envoyer l’instruction SQL. Pour plus d’informations, consultez [mode de validation manuelle](../../../odbc/reference/develop-app/manual-commit-mode.md).  
  
 Si une application utilise **SQLExecDirect** pour envoyer une instruction **Commit** ou **Rollback** , elle ne sera pas interopérable entre les produits SGBD. Pour valider ou restaurer une transaction, une application appelle **SQLEndTran**.  
  
 Si **SQLExecDirect** rencontre un paramètre de données en cours d’exécution, il retourne SQL_NEED_DATA. L’application envoie les données à l’aide de **SQLParamData** et **SQLPutData**. Consultez [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md), [SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md), [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)et [envoi de données de type long](../../../odbc/reference/develop-app/sending-long-data.md).  
  
 Si **SQLExecDirect** exécute une instruction Update, INSERT ou DELETE recherchée qui n’affecte aucune ligne de la source de données, l’appel à **SQLExecDirect** retourne SQL_NO_DATA.  
  
 Si la valeur de l’attribut d’instruction SQL_ATTR_PARAMSET_SIZE est supérieure à 1 et que l’instruction SQL contient au moins un marqueur de paramètre, **SQLExecDirect** exécute l’instruction SQL une fois pour chaque ensemble de valeurs de paramètre à partir des tableaux vers lesquels pointe l’argument *ParameterValuePointer* dans l’appel de **SQLBindParameter**. Pour plus d’informations, consultez [tableaux de valeurs de paramètre](../../../odbc/reference/develop-app/arrays-of-parameter-values.md).  
  
 Si les signets sont activés et qu’une requête est exécutée et ne peut pas prendre en charge les signets, le pilote doit tenter de forcer l’environnement sur un autre qui prend en charge les signets en modifiant une valeur d’attribut et en renvoyant SQLSTATE 01S02 ne (valeur d’option modifiée). Si l’attribut ne peut pas être modifié, le pilote doit retourner SQLSTATE HY024 (valeur d’attribut non valide).  
  
> [!NOTE]  
>  Lorsque vous utilisez le regroupement de connexions, une application ne doit pas exécuter d’instructions SQL qui modifient la base de données ou le contexte de la base de données, telle que l’instruction **use** _Database_ dans SQL Server, qui modifie le catalogue utilisé par une source de données.  
  
## <a name="code-example"></a>Exemple de code  
 Consultez [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md), [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)et [exemple de programme ODBC](../../../odbc/reference/sample-odbc-program.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Exécution d’une opération de validation ou de restauration|[Fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md)|  
|Exécution d’une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Extraction de plusieurs lignes de données|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Renvoi d’un nom de curseur|[SQLGetCursorName Function](../../../odbc/reference/syntax/sqlgetcursorname-function.md)|  
|Extraction d’une partie ou de la totalité d’une colonne de données|[Fonction SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|Retour du paramètre suivant pour l’envoi de données|[SQLParamData, fonction](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
|Préparation d’une instruction pour l’exécution|[Fonction SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|Envoi de données de paramètre au moment de l’exécution|[Fonction SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|  
|Définition d’un nom de curseur|[SQLSetCursorName, fonction](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
|Définition d’un attribut d’instruction|[Fonction SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
