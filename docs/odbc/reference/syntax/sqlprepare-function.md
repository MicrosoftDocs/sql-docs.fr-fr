---
description: Fonction SQLPrepare
title: Fonction SQLPrepare | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLPrepare
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLPrepare
helpviewer_keywords:
- SQLPrepare function [ODBC]
ms.assetid: 332e1b4b-b0ed-4e7a-aa4d-4f35f4f4476b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ce0348f2161def7dabc4941f088b883b286d2969
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192595"
---
# <a name="sqlprepare-function"></a>Fonction SQLPrepare
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLPrepare** prépare une chaîne SQL pour l’exécution.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLPrepare(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     StatementText,  
     SQLINTEGER    TextLength);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *StatementText*  
 Entrée Chaîne de texte SQL.  
  
 *TextLength*  
 Entrée Longueur de **StatementText* en caractères.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLPrepare** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLPrepare** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S02 ne|Valeur d’option modifiée|Un attribut d’instruction spécifié n’était pas valide en raison des conditions de travail de l’implémentation. une valeur similaire a donc été remplacée temporairement. (**SQLGetStmtAttr** peut être appelé pour déterminer la valeur de substitution temporaire.) La valeur de substitution est valide pour *StatementHandle* jusqu’à ce que le curseur soit fermé. Les attributs de l’instruction qui peuvent être modifiés sont : SQL_ATTR_CONCURRENCY SQL_ATTR_CURSOR_TYPE SQL_ATTR_KEYSET_SIZE SQL_ATTR_MAX_LENGTH SQL_ATTR_MAX_ROWS SQL_ATTR_QUERY_TIMEOUT SQL_ATTR_SIMULATE_CURSOR<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|21S01|La liste d’insertion de valeurs ne correspond pas à la liste de colonnes|\**StatementText* contenait une instruction **Insert** , et le nombre de valeurs à insérer ne correspondait pas au degré de la table dérivée.|  
|21S02|Le degré de la table dérivée ne correspond pas à la liste des colonnes|\**StatementText* contenait une instruction **Create View** , et le nombre de noms spécifiés n’est pas le même que celui de la table dérivée définie par la spécification de la requête.|  
|22018|Valeur de caractère non valide pour la spécification de cast|**StatementText* contenait une instruction SQL qui contenait un littéral ou un paramètre, et la valeur était incompatible avec le type de données de la colonne de table associée.|  
|22019|Caractère d’échappement non valide|L’argument *StatementText* contenait un prédicat **Like** avec un caractère d' **échappement** dans la clause **Where** , et la longueur du caractère d’échappement qui suit **Escape** n’était pas égale à 1.|  
|22025|Séquence d’échappement non valide|L’argument *StatementText* contient «**Like** _valeur du modèle_ **Escape** _caractère_ d’échappement » dans la clause **Where** , et le caractère qui suit le caractère d’échappement dans la valeur de modèle n’était ni « % », ni « _ ».|  
|24 000|État de curseur non valide|(DM) un curseur a été ouvert sur le *StatementHandle*, et **SQLFetch** ou **SQLFetchScroll** ont été appelés.<br /><br /> Un curseur a été ouvert sur le *StatementHandle*, mais **SQLFetch** ou **SQLFetchScroll** n’a pas été appelé.|  
|34000|Nom de curseur non valide|\**StatementText* contenait une **suppression** positionnée ou une **mise à jour** positionnée, et le curseur référencé par l’instruction en cours de préparation n’était pas ouvert.|  
|3D000|Nom de catalogue non valide|Le nom de catalogue spécifié dans *StatementText* n’est pas valide.|  
|3F000|Nom de schéma non valide|Le nom de schéma spécifié dans *StatementText* n’est pas valide.|  
|42000|Erreur de syntaxe ou violation d’accès|\**StatementText* contenait une instruction SQL qui n’était pas préparable ou contenait une erreur de syntaxe.<br /><br /> **StatementText* contenait une instruction pour laquelle l’utilisateur ne disposait pas des privilèges requis.|  
|42S01|La table de base ou la vue existe déjà|\**StatementText* contenait une instruction **Create Table** ou **Create View** , et le nom de la table ou le nom de la vue spécifiés existe déjà.|  
|42S02|La table de base ou la vue est introuvable|\**StatementText* contenait une instruction DROP **table** ou **Drop View** , et le nom de la table ou le nom de la vue spécifiés n’existait pas.<br /><br /> \**StatementText* contenait une instruction **ALTER TABLE** , et le nom de table spécifié n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Create View** , et un nom de table ou de vue défini par la spécification de la requête n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Create index** et le nom de la table spécifié n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Grant** ou **Revoke** , et le nom de la table ou de la vue spécifié n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Select** , et un nom de table ou de vue spécifié n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Delete**, **Insert** ou **Update** , et le nom de la table spécifié n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Create table** , et une table spécifiée dans une contrainte (référençant une table autre que celle en cours de création) n’existait pas.|  
|42S11|L’index existe déjà|\**StatementText* contenait une instruction **Create index** et le nom d’index spécifié existait déjà.|  
|42S12|Index introuvable|\**StatementText* contenait une instruction **Drop index** et le nom d’index spécifié n’existait pas.|  
|42S21|La colonne existe déjà|\**StatementText* contenait une instruction **ALTER TABLE** , et la colonne spécifiée dans la clause **Add** n’est pas unique ou identifie une colonne existante dans la table de base.|  
|42S22|Colonne introuvable|\**StatementText* contenait une instruction **Create index** , et un ou plusieurs des noms de colonnes spécifiés dans la liste des colonnes n’existaient pas.<br /><br /> \**StatementText* contenait une instruction **Grant** ou **Revoke** , et un nom de colonne spécifié n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Select**, **Delete**, **Insert** ou **Update** , et un nom de colonne spécifié n’existait pas.<br /><br /> \**StatementText* contenait une instruction **Create table** et une colonne spécifiée dans une contrainte (référençant une table autre que celle en cours de création) n’existait pas.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*, puis la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|(DM) *StatementText* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLPrepare** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) l’argument *TextLength* était inférieur ou égal à 0, mais n’est pas égal à SQL_NTS.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Le paramètre d’accès concurrentiel n’est pas valide pour le type de curseur défini.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini sur un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai expiré|Le délai d’attente a expiré avant que la source de données n’ait retourné le jeu de résultats. Le délai d’attente est défini à l’aide de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 L’application appelle **SQLPrepare** pour envoyer une instruction SQL à la source de données en vue de sa préparation. Pour plus d’informations sur l’exécution préparée, consultez [Exécution préparée](../../../odbc/reference/develop-app/prepared-execution-odbc.md). L’application peut inclure un ou plusieurs marqueurs de paramètres dans l’instruction SQL. Pour inclure un marqueur de paramètre, l’application incorpore un point d’interrogation ( ?) dans la chaîne SQL à la position appropriée. Pour plus d’informations sur les paramètres, consultez [paramètres d’instruction](../../../odbc/reference/develop-app/statement-parameters.md).  
  
> [!NOTE]  
>  Si une application utilise **SQLPrepare** pour préparer et **SQLExecute** pour soumettre une instruction **Commit** ou **Rollback** , elle ne sera pas interopérable entre les produits SGBD. Pour valider ou restaurer une transaction, appelez **SQLEndTran**.  
  
 Le pilote peut modifier l’instruction pour utiliser la forme SQL utilisée par la source de données, puis l’envoyer à la source de données pour la préparation. En particulier, le pilote modifie les séquences d’échappement utilisées pour définir la syntaxe SQL de certaines fonctionnalités. (Pour obtenir une description de la syntaxe de l’instruction SQL, consultez [séquences d’échappement dans ODBC](../../../odbc/reference/develop-app/escape-sequences-in-odbc.md) et [annexe C : grammaire SQL](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md).) Pour le pilote, un descripteur d’instruction est semblable à un identificateur d’instruction dans le code SQL incorporé. Si la source de données prend en charge les identificateurs d’instruction, le pilote peut envoyer un identificateur d’instruction et des valeurs de paramètre à la source de données.  
  
 Après la préparation d’une instruction, l’application utilise le descripteur d’instruction pour faire référence à l’instruction dans les appels de fonction ultérieurs. L’instruction préparée associée au descripteur d’instruction peut être réexécutée en appelant **SQLExecute** jusqu’à ce que l’application libère l’instruction avec un appel à **SQLFreeStmt** avec l’option SQL_DROP ou jusqu’à ce que le descripteur d’instruction soit utilisé dans un appel à **SQLPrepare**, **SQLExecDirect** ou à l’une des fonctions de catalogue (**SQLColumns**, **SQLTables**, etc.). Une fois que l’application a préparé une instruction, elle peut demander des informations sur le format du jeu de résultats. Pour certaines implémentations, l’appel de **SQLDescribeCol** ou **SQLDescribeParam** après **SQLPrepare** peut ne pas être aussi efficace que l’appel de la fonction après **SQLExecute** ou **SQLExecDirect**.  
  
 Certains pilotes ne peuvent pas retourner d’erreurs de syntaxe ou de violations d’accès lorsque l’application appelle **SQLPrepare**. Un pilote peut gérer les erreurs de syntaxe et les violations d’accès, uniquement les erreurs de syntaxe ou aucune erreur de syntaxe ou violation d’accès. Par conséquent, une application doit être en mesure de gérer ces conditions lors de l’appel de fonctions connexes ultérieures telles que **SQLNumResultCols**, **SQLDescribeCol**, **SQLColAttribute** et **SQLExecute**.  
  
 En fonction des fonctionnalités du pilote et de la source de données, les informations sur les paramètres (telles que les types de données) peuvent être vérifiées lorsque l’instruction est préparée (si tous les paramètres ont été liés) ou lorsqu’elle est exécutée (si tous les paramètres n’ont pas été liés). Pour une interopérabilité maximale, une application doit dissocier tous les paramètres qui ont été appliqués à une ancienne instruction SQL avant de préparer une nouvelle instruction SQL sur la même instruction. Cela permet d’éviter les erreurs dues à l’application d’anciennes informations sur les paramètres à la nouvelle instruction.  
  
> [!IMPORTANT]  
>  La validation d’une transaction, soit en appelant explicitement **SQLEndTran** ou en mode de validation automatique, peut entraîner la suppression par la source de données des plans d’accès pour toutes les instructions d’une connexion. Pour plus d’informations, consultez les types d’informations SQL_CURSOR_COMMIT_BEHAVIOR et SQL_CURSOR_ROLLBACK_BEHAVIOR dans [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) et [effet des transactions sur les curseurs et les instructions préparées](../../../odbc/reference/develop-app/effect-of-transactions-on-cursors-and-prepared-statements.md).  
  
## <a name="code-example"></a>Exemple de code  
 Consultez [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md), [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)et [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Allocation d’un descripteur d’instruction|[SQLAllocHandle, fonction](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Liaison d’une mémoire tampon à un paramètre|[Fonction SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Exécution d’une opération de validation ou de restauration|[Fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md)|  
|Exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécution d’une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Retour du nombre de lignes affectées par une instruction|[SQLRowCount (fonction)](../../../odbc/reference/syntax/sqlrowcount-function.md)|  
|Définition d’un nom de curseur|[SQLSetCursorName, fonction](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
