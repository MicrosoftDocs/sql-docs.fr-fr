---
description: SQLFetch, fonction
title: SQLFetch, fonction | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFetch
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFetch
helpviewer_keywords:
- SQLFetch function [ODBC]
ms.assetid: 6c6611d2-bc6a-4390-87c9-1c5dd9cfe07c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 095a1ca4a67122344926d9ca363dd9f05205c48b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209798"
---
# <a name="sqlfetch-function"></a>SQLFetch, fonction
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLFetch** extrait l’ensemble de lignes suivant du jeu de résultats et retourne des données pour toutes les colonnes liées.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLFetch(  
     SQLHSTMT     StatementHandle);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLFetch** retourne soit SQL_ERROR, soit SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant la [fonction SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md) avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLFetch** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire. Si une erreur se produit sur une seule colonne, [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md) peut être appelé avec un *DiagIdentifier* de SQL_DIAG_COLUMN_NUMBER pour déterminer la colonne sur laquelle l’erreur s’est produite ; et **SQLGetDiagField** peuvent être appelés avec un *DiagIdentifier* de SQL_DIAG_ROW_NUMBER pour déterminer la ligne qui contient cette colonne.  
  
 Pour tous les SQLSTATEs qui peuvent retourner des SQL_SUCCESS_WITH_INFO ou des SQL_ERROR (à l’exception de 01xxx SQLSTATEs), SQL_SUCCESS_WITH_INFO est retourné si une erreur se produit sur une ou plusieurs lignes, mais pas toutes, sur les lignes d’une opération multiligne, et SQL_ERROR est retournée si une erreur se produit sur une opération à une seule ligne.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Les données binaires ou de chaîne retournées pour une colonne ont entraîné la troncation d’un caractère non vide ou de données binaires non NULL. S’il s’agit d’une valeur de chaîne, elle a été tronquée à droite.|  
|01S01|Erreur dans la ligne|Une erreur s’est produite lors de l’extraction d’une ou plusieurs lignes.<br /><br /> (Si ce SQLSTATE est retourné lorsqu’une application ODBC 3 *. x* utilise un pilote ODBC 2 *. x* , elle peut être ignorée.)|  
|01S07|Troncation fractionnaire|Les données retournées pour une colonne ont été tronquées. Pour les types de données numériques, la partie fractionnaire du nombre a été tronquée. Pour les types de données time, timestamp et Interval qui contiennent un composant heure, la partie fractionnaire de l’heure a été tronquée.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07006|Violation d’attribut de type de données restreint|La valeur des données d’une colonne du jeu de résultats n’a pas pu être convertie dans le type de données spécifié par *TargetType* dans **SQLBindCol**.<br /><br /> La colonne 0 a été liée avec un type de données de SQL_C_BOOKMARK, et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE.<br /><br /> La colonne 0 a été liée avec un type de données de SQL_C_VARBOOKMARK, et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS n’a pas la valeur SQL_UB_VARIABLE.|  
|07009|Index de descripteur non valide|Le pilote était un pilote ODBC 2 *. x* qui ne prend pas en charge **SQLExtendedFetch** et un numéro de colonne spécifié dans la liaison d’une colonne était 0.<br /><br /> La colonne 0 a été liée et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_OFF.|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|22001|Données de chaîne, tronquées à droite|Un signet de longueur variable retourné pour une colonne a été tronqué.|  
|22002|Variable d’indicateur requise mais non fournie|Les données NULL ont été extraites dans une colonne dont la *StrLen_or_IndPtr* définie par **SQLBindCol** (ou SQL_DESC_INDICATOR_PTR définie par **SQLSetDescField** ou **SQLSetDescRec**) était un pointeur null.|  
|22003|Valeur numérique hors limites|Le retour de la valeur numérique sous forme de valeur numérique ou de chaîne pour une ou plusieurs colonnes dépendantes aurait entraîné la troncation de la partie entière (par opposition à la fraction) du nombre à tronquer.<br /><br /> Pour plus d’informations, consultez [conversion de données de SQL en types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) dans l’annexe D : types de données.|  
|22007|Format de date/heure non valide|Une colonne de caractères dans le jeu de résultats a été liée à une structure de date, d’heure ou d’horodatage C, et une valeur dans la colonne était, respectivement, une date, une heure ou un horodateur non valide.|  
|22012|Division par zéro|Une valeur issue d’une expression arithmétique a été retournée, ce qui a entraîné une division par zéro.|  
|22015|Dépassement du champ d’intervalle|L’assignation d’un type SQL exact numérique ou Interval à un type C Interval a provoqué une perte de chiffres significatifs dans le champ de début.<br /><br /> Lors de l’extraction de données vers un type d’intervalle C, il n’existait aucune représentation de la valeur du type SQL dans le type d’intervalle C.|  
|22018|Valeur de caractère non valide pour la spécification de cast|Une colonne de caractères dans le jeu de résultats était liée à une mémoire tampon de caractères C, et la colonne contenait un caractère pour lequel il n’existait aucune représentation dans le jeu de caractères de la mémoire tampon.<br /><br /> Le type C était un type de données numérique exact ou approximatif, DateTime ou Interval ; le type SQL de la colonne était un type de données caractère ; et la valeur dans la colonne n’est pas un littéral valide du type C lié.|  
|24 000|État de curseur non valide|*StatementHandle* était dans un état d’exécution, mais aucun jeu de résultats n’a été associé à *StatementHandle*.|  
|40001|Échec de la sérialisation|La transaction dans laquelle l’extraction a été exécutée a été interrompue pour empêcher un blocage.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction **SQLFetch** a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*. La fonction **SQLFetch** a ensuite été appelée à nouveau sur le *StatementHandle*.<br /><br /> Ou bien, la fonction  **SQLFetch** a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLFetch** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) le *StatementHandle* spécifié n’était pas dans un état d’exécution. La fonction a été appelée sans appeler d’abord **SQLExecDirect**, **SQLExecute** ou une fonction de catalogue.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.<br /><br /> (DM) **SQLFetch** a été appelée pour *StatementHandle* après l’appel de **SQLExtendedFetch** et avant l’appel de **SQLFreeStmt** avec l’option SQL_CLOSE.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|L’attribut d’instruction SQL_ATTR_USE_BOOKMARK a été défini sur SQL_UB_VARIABLE et la colonne 0 était liée à une mémoire tampon dont la longueur n’était pas égale à la longueur maximale du signet pour ce jeu de résultats. (Cette longueur est disponible dans le champ SQL_DESC_OCTET_LENGTH de la IRD et peut être obtenue en appelant **SQLDescribeCol**, **SQLColAttribute** ou **SQLGetDescField**.)|  
|HY107|Valeur de ligne hors limites|La valeur spécifiée avec l’attribut d’instruction SQL_ATTR_CURSOR_TYPE était SQL_CURSOR_KEYSET_DRIVEN, mais la valeur spécifiée avec l’attribut d’instruction SQL_ATTR_KEYSET_SIZE était supérieure à 0 et inférieure à la valeur spécifiée avec l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Le pilote ou la source de données ne prend pas en charge la conversion spécifiée par la combinaison du *TargetType* dans **SQLBindCol** et du type de données SQL de la colonne correspondante.|  
|HYT00|Délai expiré|Le délai d’expiration de la requête a expiré avant que la source de données n’ait retourné le jeu de résultats demandé. Le délai d’attente est défini à l’aide de SQLSetStmtAttr, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 **SQLFetch** retourne l’ensemble de lignes suivant dans le jeu de résultats. Elle peut être appelée uniquement lorsqu’un jeu de résultats existe : autrement dit, après un appel qui crée un jeu de résultats et avant la fermeture du curseur sur ce jeu de résultats. Si des colonnes sont liées, elles retournent les données dans ces colonnes. Si l’application a spécifié un pointeur vers un tableau d’état de ligne ou une mémoire tampon dans laquelle retourner le nombre de lignes extraites, **SQLFetch** retourne également ces informations. Les appels à **SQLFetch** peuvent être mélangés avec des appels à **SQLFetchScroll** , mais ne peuvent pas être mélangés avec des appels à **SQLExtendedFetch**. Pour plus d’informations, consultez [extraction d’une ligne de données](../../../odbc/reference/develop-app/fetching-a-row-of-data.md).  
  
 Si une application ODBC *3. x* fonctionne avec un pilote ODBC 2 *. x* , le gestionnaire de pilotes mappe les appels **SQLFetch** à **SQLExtendedFetch** pour un pilote ODBC 2 *. x* qui prend en charge **SQLExtendedFetch**. Si le pilote ODBC 2 *. x* ne prend pas en charge **SQLExtendedFetch**, le gestionnaire de pilotes mappe les appels **SQLFetch** à **SQLFetch** dans le pilote ODBC 2 *. x* , qui ne peut extraire qu’une seule ligne.  
  
 Pour plus d’informations, consultez [curseurs de bloc, curseurs avec défilement et compatibilité descendante](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) dans l’annexe G : instructions relatives aux pilotes pour la compatibilité descendante.  
  
## <a name="positioning-the-cursor"></a>Positionnement du curseur  
 Lorsque le jeu de résultats est créé, le curseur est positionné avant le début du jeu de résultats. **SQLFetch** extrait l’ensemble de lignes suivant. Cela équivaut à appeler **SQLFetchScroll** avec *FetchOrientation* défini sur SQL_FETCH_NEXT. Pour plus d’informations sur les curseurs [, consultez](../../../odbc/reference/develop-app/cursors.md) curseurs et [curseurs de bloc](../../../odbc/reference/develop-app/block-cursors.md).  
  
 L’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE spécifie le nombre de lignes dans l’ensemble de lignes. Si l’ensemble de lignes récupéré par **SQLFetch** chevauche la fin du jeu de résultats, **SQLFetch** retourne un ensemble de lignes partiel. Autrement dit, si S + R-1 est supérieur à L, où S est la ligne de départ de l’ensemble de lignes en cours d’extraction, R est la taille de l’ensemble de lignes et L est la dernière ligne du jeu de résultats, alors seules les L-S + 1 premières lignes de l’ensemble de lignes sont valides. Les lignes restantes sont vides et présentent l’État SQL_ROW_NOROW.  
  
 Une fois **SQLFetch** retourné, la ligne actuelle est la première ligne de l’ensemble de lignes.  
  
 Les règles indiquées dans le tableau suivant décrivent le positionnement du curseur après un appel à **SQLFetch**, en fonction des conditions indiquées dans le deuxième tableau de cette section.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|Avant le début|1|  
| \< CurrRowsetStart =  *LastResultRow-RowsetSize*[1]|*CurrRowsetStart*  +  *RowsetSize*[2]|  
|*CurrRowsetStart*  >  *LastResultRow-RowsetSize*[1]|Après la fin|  
|Après la fin|Après la fin|  
  
 [1] si la taille de l’ensemble de lignes est modifiée entre les extractions, il s’agit de la taille de l’ensemble de lignes utilisée avec l’extraction précédente.  
  
 [2] si la taille de l’ensemble de lignes est modifiée entre les extractions, il s’agit de la taille de l’ensemble de lignes utilisée avec la nouvelle extraction.  
  
|Notation|Signification|  
|--------------|-------------|  
|Avant le début|Le curseur de bloc est positionné avant le début du jeu de résultats. Si la première ligne du nouvel ensemble de lignes se trouve avant le début de l’ensemble de résultats, **SQLFetch** retourne SQL_NO_DATA.|  
|Après la fin|Le curseur de bloc est positionné après la fin du jeu de résultats. Si la première ligne du nouvel ensemble de lignes se trouve après la fin de l’ensemble de résultats, **SQLFetch** retourne SQL_NO_DATA.|  
|*CurrRowsetStart*|Numéro de la première ligne dans l’ensemble de lignes actif.|  
|*LastResultRow*|Numéro de la dernière ligne dans le jeu de résultats.|  
|*RowsetSize*|Taille de l’ensemble de lignes.|  
  
 Supposons, par exemple, qu’un jeu de résultats contient 100 lignes et que la taille de l’ensemble de lignes est de 5. Le tableau suivant montre l’ensemble de lignes et le code de retour retournés par **SQLFetch** pour les différentes positions de départ.  
  
|Ensemble de lignes actif|Code de retour|Nouvel ensemble de lignes|nombre de lignes extraites|  
|--------------------|-----------------|----------------|------------------------|  
|Avant le début|SQL_SUCCESS|1 à 5|5|  
|1 à 5|SQL_SUCCESS|6 à 10|5|  
|52 à 56|SQL_SUCCESS|57 à 61|5|  
|91 à 95|SQL_SUCCESS|96 à 100|5|  
|93 à 97|SQL_SUCCESS|de 98 à 100. Les lignes 4 et 5 du tableau d’état de ligne sont définies sur SQL_ROW_NOROW.|3|  
|96 à 100|SQL_NO_DATA|Aucun.|0|  
|99 à 100|SQL_NO_DATA|Aucun.|0|  
|Après la fin|SQL_NO_DATA|Aucun.|0|  
  
## <a name="returning-data-in-bound-columns"></a>Retour de données dans des colonnes dépendantes  
 Comme **SQLFetch** retourne chaque ligne, il place les données pour chaque colonne liée dans la mémoire tampon liée à cette colonne. Si aucune colonne n’est liée, **SQLFetch** ne retourne aucune donnée, mais déplace le curseur de bloc vers l’avant. Les données peuvent toujours être récupérées à l’aide de **SQLGetData**. Si le curseur est un curseur multiligne (autrement dit, si le SQL_ATTR_ROW_ARRAY_SIZE est supérieur à 1), **SQLGetData** peut être appelé uniquement si SQL_GD_BLOCK est retourné lorsque **SQLGetInfo** est appelé avec un *infotype* de SQL_GETDATA_EXTENSIONS. (Pour plus d’informations, consultez [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md).)  
  
 Pour chaque colonne liée d’une ligne, **SQLFetch** effectue les opérations suivantes :  
  
1.  Définit le tampon de longueur/d’indicateur à SQL_NULL_DATA et passe à la colonne suivante si les données sont NULL. Si les données sont NULL et qu’aucune mémoire tampon de longueur/d’indicateur n’a été liée, **SQLFetch** retourne SQLState 22002 (variable indicateur requise mais non fournie) pour la ligne et passe à la ligne suivante. Pour plus d’informations sur la façon de déterminer l’adresse de la mémoire tampon de longueur/indicateur, consultez « adresses de tampon » dans [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
     Si les données de la colonne n’ont pas la valeur NULL, **SQLFetch** passe à l’étape 2.  
  
2.  Si l’attribut d’instruction SQL_ATTR_MAX_LENGTH est défini sur une valeur différente de zéro et que la colonne contient des données de type caractère ou binaire, les données sont tronquées à SQL_ATTR_MAX_LENGTH octets.  
  
    > [!NOTE]  
    >  L’attribut d’instruction SQL_ATTR_MAX_LENGTH est destiné à réduire le trafic réseau. Elle est généralement implémentée par la source de données, qui tronque les données avant de les renvoyer sur le réseau. Les pilotes et les sources de données ne sont pas requis pour le prendre en charge. Par conséquent, pour garantir que les données sont tronquées à une taille particulière, une application doit allouer une mémoire tampon de cette taille et spécifier la taille dans l’argument *cbValueMax* dans **SQLBindCol**.  
  
3.  Convertit les données en type spécifié par *TargetType* dans **SQLBindCol**.  
  
4.  Si les données ont été converties en un type de données de longueur variable, tel qu’un caractère ou un binaire, **SQLFetch** vérifie si la longueur des données dépasse la longueur de la mémoire tampon de données. Si la longueur des données caractères (y compris le caractère de fin null) dépasse la longueur de la mémoire tampon de données, **SQLFetch** tronque les données à la longueur de la mémoire tampon de données moins la longueur d’un caractère de fin null. Il termine ensuite les données null. Si la longueur des données binaires dépasse la longueur de la mémoire tampon de données, **SQLFetch** la tronque à la longueur de la mémoire tampon de données. La longueur de la mémoire tampon de données est spécifiée avec *BufferLength* dans **SQLBindCol**.  
  
     **SQLFetch** ne tronque jamais les données converties en types de données de longueur fixe ; elle suppose toujours que la longueur de la mémoire tampon de données correspond à la taille du type de données.  
  
5.  Place les données converties (et éventuellement tronquées) dans le tampon de données. Pour plus d’informations sur la façon de déterminer l’adresse de la mémoire tampon de données, consultez « adresses de tampon » dans [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
6.  Place la longueur des données dans la mémoire tampon de longueur/d’indicateur. Si le pointeur d’indicateur et le pointeur de longueur ont tous deux été définis sur la même mémoire tampon (comme un appel à **SQLBindCol** ), la longueur est écrite dans la mémoire tampon pour les données valides et SQL_NULL_DATA est écrit dans la mémoire tampon pour les données null. Si aucune mémoire tampon de longueur/d’indicateur n’a été liée, **SQLFetch** ne retourne pas la longueur.  
  
    -   Pour les données de type caractère ou binaire, il s’agit de la longueur des données après la conversion et avant la troncation en raison de la taille de la mémoire tampon de données. Si le pilote ne peut pas déterminer la longueur des données après la conversion, comme c’est parfois le cas avec des données longues, il définit la longueur sur SQL_NO_TOTAL. Si les données ont été tronquées en raison de l’attribut d’instruction SQL_ATTR_MAX_LENGTH, la valeur de cet attribut est placée dans la mémoire tampon de longueur/indicateur au lieu de la longueur réelle. Cela est dû au fait que cet attribut est conçu pour tronquer les données sur le serveur avant la conversion, afin que le pilote n’ait aucun moyen de déterminer la longueur réelle.  
  
    -   Pour tous les autres types de données, il s’agit de la longueur des données après la conversion ; autrement dit, il s’agit de la taille du type vers lequel les données ont été converties.  
  
     Pour plus d’informations sur la façon de déterminer l’adresse de la mémoire tampon de longueur/indicateur, consultez « adresses de tampon » dans [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
7.  Si les données sont tronquées au cours de la conversion sans perte de chiffres significatifs (par exemple, le nombre réel 1,234 est tronqué à l’entier 1 lors de la conversion), **SQLFetch** retourne SQLState 01S07 (troncation fractionnaire) et SQL_SUCCESS_WITH_INFO. Si les données sont tronquées parce que la longueur de la mémoire tampon de données est trop petite (par exemple, la chaîne « abcdef » est placée dans une mémoire tampon de 4 octets), **SQLFetch** retourne SQLState 01004 (données tronquées) et SQL_SUCCESS_WITH_INFO. Si les données sont tronquées en raison de l’attribut d’instruction SQL_ATTR_MAX_LENGTH, **SQLFetch** retourne SQL_SUCCESS et ne retourne pas SQLSTATE 01S07 (troncation fractionnaire) ou SQLSTATE 01004 (données tronquées). Si les données sont tronquées pendant la conversion avec une perte de chiffres significatifs (par exemple, si une valeur SQL_INTEGER supérieure à 100 000 ont été converties en SQL_C_TINYINT), **SQLFetch** retourne SQLState 22003 (valeur numérique hors limites) et SQL_ERROR (si la taille de l’ensemble de lignes est égale à 1) ou SQL_SUCCESS_WITH_INFO (si la taille de l’ensemble de lignes est supérieure à 1).  
  
 Le contenu de la mémoire tampon de données liée et de la mémoire tampon de longueur/d’indicateur n’est pas défini si **SQLFetch** ou **SQLFetchScroll** ne retourne pas SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO.  
  
## <a name="row-status-array"></a>Tableau d’état des lignes  
 Le tableau d’état de ligne est utilisé pour retourner l’état de chaque ligne de l’ensemble de lignes. L’adresse de ce tableau est spécifiée avec l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR. Le tableau est alloué par l’application et doit avoir autant d’éléments que spécifiés par l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE. Ses valeurs sont définies par **SQLFetch**, **SQLFetchScroll** et **SQLBulkOperations** ou **SQLSetPos** (sauf si elles ont été appelées après que le curseur a été positionné par **SQLExtendedFetch**). Si la valeur de l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR est un pointeur null, ces fonctions ne retournent pas l’état de la ligne.  
  
 Le contenu de la mémoire tampon du tableau d’état des lignes n’est pas défini si **SQLFetch** ou **SQLFetchScroll** ne retourne pas SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO.  
  
 Les valeurs suivantes sont retournées dans le tableau d’état de ligne.  
  
|Valeur du tableau d’état des lignes|Description|  
|----------------------------|-----------------|  
|SQL_ROW_SUCCESS|La ligne a été extraite avec succès et n’a pas été modifiée depuis son dernier extraction à partir de ce jeu de résultats.|  
|SQL_ROW_SUCCESS_WITH_INFO|La ligne a été extraite avec succès et n’a pas été modifiée depuis son dernier extraction à partir de ce jeu de résultats. Toutefois, un avertissement a été renvoyé à propos de la ligne.|  
|SQL_ROW_ERROR|Une erreur s’est produite lors de l’extraction de la ligne.|  
|SQL_ROW_UPDATED [1], [2] et [3]|La ligne a été récupérée et a été modifiée depuis sa dernière extraction à partir de ce jeu de résultats. Si la ligne est extraite à nouveau à partir de ce jeu de résultats ou qu’elle est actualisée par **SQLSetPos**, l’état passe à l’état nouveau de la ligne.|  
|SQL_ROW_DELETED [3]|La ligne a été supprimée depuis la dernière extraction de ce jeu de résultats.|  
|SQL_ROW_ADDED [4]|La ligne a été insérée par **SQLBulkOperations**. Si la ligne est extraite à nouveau à partir de ce jeu de résultats ou qu’elle est actualisée par **SQLSetPos**, son état est SQL_ROW_SUCCESS.|  
|SQL_ROW_NOROW|L’ensemble de lignes chevauche la fin du jeu de résultats et aucune ligne qui correspond à cet élément du tableau d’état de ligne n’a été retournée.|  
  
 [1] pour les curseurs de jeu de clés, mixtes et dynamiques, si une valeur de clé est mise à jour, la ligne de données est considérée comme supprimée et une nouvelle ligne est ajoutée.  
  
 [2] certains pilotes ne peuvent pas détecter les mises à jour des données et ne peuvent donc pas retourner cette valeur. Pour déterminer si un pilote peut détecter des mises à jour à des lignes récupérées à nouveau, une application appelle **SQLGetInfo** avec l’option SQL_ROW_UPDATES.  
  
 [3]   **SQLFetch** peut retourner cette valeur uniquement lorsqu’elle est mélangée avec des appels à **SQLFetchScroll**. Cela est dû au fait que **SQLFetch** progresse dans le jeu de résultats et lorsqu’il est utilisé exclusivement, ne récupère pas les lignes. Comme aucune ligne n’est réextraite, **SQLFetch** ne détecte pas les modifications apportées aux lignes précédemment récupérées. Toutefois, si **SQLFetchScroll** positionne le curseur avant les lignes précédemment récupérées et que **SQLFetch** est utilisé pour extraire ces lignes, **SQLFetch** peut détecter toute modification apportée à ces lignes.  
  
 [4] retourné par SQLBulkOperations uniquement. Non défini par **SQLFetch** ou **SQLFetchScroll**.  
  
### <a name="rows-fetched-buffer"></a>Tampons extraits de lignes  
 La mémoire tampon extraite de lignes est utilisée pour retourner le nombre de lignes extraites, y compris les lignes pour lesquelles aucune donnée n’a été retournée, car une erreur s’est produite lors de la récupération. En d’autres termes, il s’agit du nombre de lignes pour lesquelles la valeur du tableau d’état de ligne n’est pas SQL_ROW_NOROW. L’adresse de cette mémoire tampon est spécifiée à l’aide de l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR. La mémoire tampon est allouée par l’application. Elle est définie par **SQLFetch** et **SQLFetchScroll**. Si la valeur de l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR est un pointeur null, ces fonctions ne retournent pas le nombre de lignes extraites. Pour déterminer le numéro de la ligne actuelle dans le jeu de résultats, une application peut appeler **SQLGetStmtAttr** avec l’attribut SQL_ATTR_ROW_NUMBER.  
  
 Le contenu de la mémoire tampon extraite de lignes n’est pas défini si **SQLFetch** ou **SQLFetchScroll** ne retourne pas SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO, sauf si SQL_NO_DATA est retourné, auquel cas la valeur de la mémoire tampon de lignes extraites est définie sur 0.  
  
### <a name="error-handling"></a>Gestion des erreurs  
 Les erreurs et les avertissements peuvent s’appliquer à des lignes individuelles ou à la fonction entière. Pour plus d’informations sur les enregistrements de diagnostic, consultez [Diagnostics](../../../odbc/reference/develop-app/diagnostics.md) et [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).  
  
#### <a name="errors-and-warnings-on-the-entire-function"></a>Erreurs et avertissements sur la totalité de la fonction  
 Si une erreur s’applique à la fonction entière, telle que SQLSTATE HYT00 (timeout expiré) ou SQLSTATE 24000 (état de curseur non valide), **SQLFetch** retourne SQL_ERROR et la valeur SQLSTATE applicable. Le contenu des mémoires tampons d’ensemble de lignes n’est pas défini et la position du curseur est inchangée.  
  
 Si un avertissement s’applique à la fonction entière, **SQLFetch** retourne SQL_SUCCESS_WITH_INFO et le SQLSTATE applicable. Les enregistrements d’État pour les avertissements qui s’appliquent à la fonction entière sont retournés avant les enregistrements d’État qui s’appliquent à des lignes individuelles.  
  
#### <a name="errors-and-warnings-in-individual-rows"></a>Erreurs et avertissements dans des lignes individuelles  
 Si une erreur (telle que SQLSTATE 22012 (division par zéro)) ou un avertissement (tel que SQLSTATE 01004 (données tronquées)) s’applique à une seule ligne, **SQLFetch** effectue les opérations suivantes :  
  
-   Définit l’élément correspondant du tableau d’état de ligne à SQL_ROW_ERROR pour les erreurs ou les SQL_ROW_SUCCESS_WITH_INFO pour les avertissements.  
  
-   Ajoute zéro, un ou plusieurs enregistrements d’État qui contiennent des SQLSTATEs pour l’erreur ou l’avertissement.  
  
-   Définit les champs de nombre de lignes et de colonnes dans les enregistrements d’État. Si **SQLFetch** ne peut pas déterminer un numéro de ligne ou de colonne, il définit ce nombre sur SQL_ROW_NUMBER_UNKNOWN ou SQL_COLUMN_NUMBER_UNKNOWN, respectivement. Si l’enregistrement d’État ne s’applique pas à une colonne particulière, **SQLFetch** définit le numéro de colonne sur SQL_NO_COLUMN_NUMBER.  
  
 **SQLFetch** poursuit l’extraction des lignes jusqu’à ce qu’elle ait extrait toutes les lignes de l’ensemble de lignes. Elle retourne SQL_SUCCESS_WITH_INFO, sauf si une erreur se produit dans chaque ligne de l’ensemble de lignes (à l’exclusion des lignes dont l’État est SQL_ROW_NOROW), auquel cas elle retourne SQL_ERROR. En particulier, si la taille de l’ensemble de lignes est 1 et qu’une erreur se produit dans cette ligne, **SQLFetch** retourne SQL_ERROR.  
  
 **SQLFetch** retourne les enregistrements d’État dans l’ordre des numéros de lignes. Autrement dit, elle retourne tous les enregistrements d’État pour les lignes inconnues (le cas échéant); Ensuite, elle retourne tous les enregistrements d’état de la première ligne (le cas échéant), puis retourne tous les enregistrements d’état de la deuxième ligne (le cas échéant), et ainsi de suite. Les enregistrements d’État pour chaque ligne sont triés selon les règles normales de classement des enregistrements d’État. Pour plus d’informations, consultez « séquence des enregistrements d’État » dans [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).  
  
### <a name="descriptors-and-sqlfetch"></a>Descripteurs et SQLFetch  
 Les sections suivantes décrivent comment **SQLFetch** interagit avec les descripteurs.  
  
#### <a name="argument-mappings"></a>Mappages d’arguments  
 Le pilote ne définit aucun champ de descripteur en fonction des arguments de **SQLFetch**.  
  
#### <a name="other-descriptor-fields"></a>Autres champs de descripteur  
 Les champs de descripteur suivants sont utilisés par **SQLFetch**.  
  
|Champ de descripteur|DESC.|Champ dans|Défini par le biais de|  
|----------------------|-----------|--------------|-----------------|  
|SQL_DESC_ARRAY_SIZE|ARD|en-tête|Attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE|  
|SQL_DESC_ARRAY_STATUS_PTR|IRD|en-tête|Attribut d’instruction SQL_ATTR_ROW_STATUS_PTR|  
|SQL_DESC_BIND_OFFSET_PTR|ARD|en-tête|Attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR|  
|SQL_DESC_BIND_TYPE|ARD|en-tête|Attribut d’instruction SQL_ATTR_ROW_BIND_TYPE|  
|SQL_DESC_COUNT|ARD|en-tête|Argument *ColumnNumber* de **SQLBindCol**|  
|SQL_DESC_DATA_PTR|ARD|enregistrements|Argument *TargetValuePtr* de **SQLBindCol**|  
|SQL_DESC_INDICATOR_PTR|ARD|enregistrements|Argument *StrLen_or_IndPtr* dans **SQLBindCol**|  
|SQL_DESC_OCTET_LENGTH|ARD|enregistrements|Argument *BufferLength* dans **SQLBindCol**|  
|SQL_DESC_OCTET_LENGTH_PTR|ARD|enregistrements|Argument *StrLen_or_IndPtr* dans **SQLBindCol**|  
|SQL_DESC_ROWS_PROCESSED_PTR|IRD|en-tête|Attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR|  
|SQL_DESC_TYPE|ARD|enregistrements|Argument *TargetType* dans **SQLBindCol**|  
  
 Tous les champs de descripteur peuvent également être définis via **SQLSetDescField**.  
  
#### <a name="separate-length-and-indicator-buffers"></a>Mémoires tampons d’indicateur et de longueur distinctes  
 Les applications peuvent lier une seule mémoire tampon ou deux mémoires tampons distinctes qui peuvent être utilisées pour contenir des valeurs de longueur et d’indicateur. Quand une application appelle **SQLBindCol**, le pilote définit les champs SQL_DESC_OCTET_LENGTH_PTR et SQL_DESC_INDICATOR_PTR du ARD sur la même adresse, qui est transmise dans l’argument *StrLen_or_IndPtr* . Quand une application appelle **SQLSetDescField** ou **SQLSetDescRec**, elle peut définir ces deux champs sur des adresses différentes.  
  
 **SQLFetch** détermine si l’application a spécifié des tampons de longueur et d’indicateur distincts. Dans ce cas, lorsque les données n’ont pas la valeur NULL, **SQLFetch** définit la mémoire tampon des indicateurs sur 0 et retourne la longueur dans la mémoire tampon de la longueur. Lorsque les données sont NULL, **SQLFetch** définit la mémoire tampon des indicateurs sur SQL_NULL_DATA et ne modifie pas la mémoire tampon de la longueur.  
  
### <a name="code-example"></a>Exemple de code  
 Consultez [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md), [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md), [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)et [SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md).  
  
### <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retour d’informations sur une colonne dans un jeu de résultats|[Fonction SQLDescribeCol](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécution d’une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Fermeture du curseur sur l’instruction|[Fonction SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Extraction d’une partie ou de la totalité d’une colonne de données|[Fonction SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|Retour du nombre de colonnes du jeu de résultats|[Fonction SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Préparation d’une instruction pour l’exécution|[Fonction SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
