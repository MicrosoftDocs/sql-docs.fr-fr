---
description: Fonction SQLFetchScroll
title: SQLFetchScroll, fonction | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFetchScroll
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFetchScroll
helpviewer_keywords:
- SQLFetchScroll function [ODBC]
ms.assetid: c0243667-428c-4dda-ae91-3c307616a1ac
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5cfa442ab1ada0b17e06137257b4d17bf37f854c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198533"
---
# <a name="sqlfetchscroll-function"></a>Fonction SQLFetchScroll
**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLFetchScroll** extrait l’ensemble de lignes spécifié de données du jeu de résultats et retourne des données pour toutes les colonnes liées. Les ensembles de lignes peuvent être spécifiés à une position absolue ou relative ou par signet.  
  
 Lorsque vous utilisez un pilote ODBC 2. x, le gestionnaire de pilotes mappe cette fonction à **SQLExtendedFetch**. Pour plus d’informations, consultez [mappage des fonctions de remplacement pour la compatibilité descendante des applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLFetchScroll(  
      SQLHSTMT      StatementHandle,  
      SQLSMALLINT   FetchOrientation,  
      SQLLEN        FetchOffset);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *FetchOrientation*  
 Entrée  
  
 Type d’extraction :  
  
 SQL_FETCH_NEXT  
  
 SQL_FETCH_PRIOR  
  
 SQL_FETCH_FIRST  
  
 SQL_FETCH_LAST  
  
 SQL_FETCH_ABSOLUTE  
  
 SQL_FETCH_RELATIVE  
  
 SQL_FETCH_BOOKMARK  
  
 Pour plus d’informations, consultez « positionnement du curseur » dans la section « commentaires ».  
  
 *FetchOffset*  
 Entrée  
  
 Numéro de la ligne à extraire. L’interprétation de cet argument dépend de la valeur de l’argument *FetchOrientation* . Pour plus d’informations, consultez « positionnement du curseur » dans la section « commentaires ».  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLFetchScroll** retourne soit SQL_ERROR, soit SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un comme HandleType de SQL_HANDLE_STMT et un handle de StatementHandle. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLFetchScroll** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire. Si une erreur se produit sur une seule colonne, **SQLGetDiagField** peut être appelé avec un DiagIdentifier de SQL_DIAG_COLUMN_NUMBER pour déterminer la colonne sur laquelle l’erreur s’est produite ; et **SQLGetDiagField** peuvent être appelés avec un DiagIdentifier de SQL_DIAG_ROW_NUMBER pour déterminer la ligne contenant cette colonne.  
  
 Pour tous les SQLSTATEs qui peuvent retourner des SQL_SUCCESS_WITH_INFO ou des SQL_ERROR (à l’exception de 01xxx SQLSTATEs), SQL_SUCCESS_WITH_INFO est retourné si une erreur se produit sur une ou plusieurs lignes, mais pas toutes, sur les lignes d’une opération multiligne, et SQL_ERROR est retournée si une erreur se produit sur une opération à une seule ligne.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Les données binaires ou de chaîne retournées pour une colonne ont entraîné la troncation d’un caractère non vide ou de données binaires non NULL. S’il s’agit d’une valeur de chaîne, elle a été tronquée à droite.|  
|01S01|Erreur dans la ligne|Une erreur s’est produite lors de l’extraction d’une ou plusieurs lignes.<br /><br /> (Si ce SQLSTATE est retourné lorsqu’une application ODBC 3 *. x* utilise un pilote ODBC 2 *. x* , elle peut être ignorée.)|  
|01S06|Tentative de récupération avant que le jeu de résultats ait retourné le premier ensemble de lignes|L’ensemble de lignes demandé chevauche le début du jeu de résultats lorsque FetchOrientation a été SQL_FETCH_PRIOR, que la position actuelle se situe au-delà de la première ligne et que le numéro de la ligne actuelle est inférieur ou égal à la taille de l’ensemble de lignes.<br /><br /> L’ensemble de lignes demandé chevauche le début du jeu de résultats lorsque FetchOrientation a été SQL_FETCH_PRIOR, que la position actuelle se situe au-delà de la fin du jeu de résultats et que la taille de l’ensemble de lignes est supérieure à la taille du jeu de résultats.<br /><br /> L’ensemble de lignes demandé chevauche le début du jeu de résultats lorsque FetchOrientation a été SQL_FETCH_RELATIVE, FetchOffset était négatif et la valeur absolue de FetchOffset était inférieure ou égale à la taille de l’ensemble de lignes.<br /><br /> L’ensemble de lignes demandé chevauche le début du jeu de résultats lorsque FetchOrientation a été SQL_FETCH_ABSOLUTE, FetchOffset était négatif et la valeur absolue de FetchOffset était supérieure à la taille du jeu de résultats, mais inférieure ou égale à la taille de l’ensemble de lignes.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S07|Troncation fractionnaire|Les données retournées pour une colonne ont été tronquées. Pour les types de données numériques, la partie fractionnaire du nombre a été tronquée. Pour les types de données time, timestamp et Interval contenant un composant heure, la partie fractionnaire de l’heure a été tronquée.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07006|Violation d’attribut de type de données restreint|La valeur des données d’une colonne du jeu de résultats n’a pas pu être convertie dans le type de données spécifié par *TargetType* dans **SQLBindCol**.<br /><br /> La colonne 0 a été liée avec un type de données de SQL_C_BOOKMARK, et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE.<br /><br /> La colonne 0 a été liée avec un type de données de SQL_C_VARBOOKMARK, et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS n’a pas la valeur SQL_UB_VARIABLE.|  
|07009|Index de descripteur non valide|Le pilote était un pilote ODBC 2 *. x* qui ne prend pas en charge **SQLExtendedFetch** et un numéro de colonne spécifié dans la liaison d’une colonne était 0.<br /><br /> La colonne 0 a été liée et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_OFF.|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|22001|Données de chaîne, tronquées à droite|Un signet de longueur variable retourné pour une colonne a été tronqué.|  
|22002|Variable d’indicateur requise mais non fournie|Les données NULL ont été extraites dans une colonne dont la *StrLen_or_IndPtr* définie par **SQLBindCol** (ou SQL_DESC_INDICATOR_PTR définie par **SQLSetDescField** ou **SQLSetDescRec**) était un pointeur null.|  
|22003|Valeur numérique hors limites|Le retour de la valeur numérique (sous forme de valeur numérique ou de chaîne) pour une ou plusieurs colonnes dépendantes aurait entraîné la troncation de la partie entière (par opposition à la fraction) du nombre à tronquer.<br /><br /> Pour plus d’informations, consultez [conversion de données de SQL en types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) dans l' [annexe D : types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).|  
|22007|Format de date/heure non valide|Une colonne de caractères dans le jeu de résultats a été liée à une structure de date, d’heure ou d’horodatage C, et une valeur dans la colonne était, respectivement, une date, une heure ou un horodateur non valide.|  
|22012|Division par zéro|Une valeur issue d’une expression arithmétique a été retournée, ce qui a entraîné une division par zéro.|  
|22015|Dépassement du champ d’intervalle|L’assignation d’un type SQL exact numérique ou Interval à un type C Interval a provoqué une perte de chiffres significatifs dans le champ de début.<br /><br /> Lors de l’extraction de données vers un type d’intervalle C, il n’existait aucune représentation de la valeur du type SQL dans le type d’intervalle C.|  
|22018|Valeur de caractère non valide pour la spécification de cast|Une colonne de caractères dans le jeu de résultats était liée à une mémoire tampon de caractères C, et la colonne contenait un caractère pour lequel il n’existait aucune représentation dans le jeu de caractères de la mémoire tampon.<br /><br /> Le type C était un type de données numérique exact ou approximatif, DateTime ou Interval ; le type SQL de la colonne était un type de données caractère ; et la valeur dans la colonne n’est pas un littéral valide du type C lié.|  
|24 000|État de curseur non valide|*StatementHandle* était dans un état d’exécution, mais aucun jeu de résultats n’a été associé à *StatementHandle*.|  
|40001|Échec de la sérialisation|La transaction dans laquelle l’extraction a été exécutée a été interrompue pour empêcher un blocage.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*. Ensuite, la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLFetchScroll** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) le *StatementHandle* spécifié n’était pas dans un état d’exécution. La fonction a été appelée sans appeler d’abord **SQLExecDirect**, **SQLExecute** ou une fonction de catalogue.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.<br /><br /> (DM) **SQLFetch** a été appelée pour *StatementHandle* après l’appel de **SQLExtendedFetch** et avant l’appel de **SQLFreeStmt** avec l’option SQL_CLOSE.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|L’attribut d’instruction SQL_ATTR_USE_BOOKMARK a été défini sur SQL_UB_VARIABLE et la colonne 0 était liée à une mémoire tampon dont la longueur n’était pas égale à la longueur maximale du signet pour ce jeu de résultats. (Cette longueur est disponible dans le champ SQL_DESC_OCTET_LENGTH de la IRD et peut être obtenue en appelant **SQLDescribeCol**, **SQLColAttribute** ou **SQLGetDescField**.)|  
|HY106|Type d’extraction hors limites|DM) la valeur spécifiée pour l’argument FetchOrientation n’était pas valide.<br /><br /> (DM) l’argument FetchOrientation a été SQL_FETCH_BOOKMARK et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_OFF.<br /><br /> La valeur de l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été SQL_CURSOR_FORWARD_ONLY et la valeur de l’argument FetchOrientation n’a pas été SQL_FETCH_NEXT.<br /><br /> La valeur de l’attribut d’instruction SQL_ATTR_CURSOR_SCROLLABLE a été SQL_NONSCROLLABLE et la valeur de l’argument FetchOrientation n’a pas été SQL_FETCH_NEXT.|  
|HY107|Valeur de ligne hors limites|La valeur spécifiée avec l’attribut d’instruction SQL_ATTR_CURSOR_TYPE était SQL_CURSOR_KEYSET_DRIVEN, mais la valeur spécifiée avec l’attribut d’instruction SQL_ATTR_KEYSET_SIZE était supérieure à 0 et inférieure à la valeur spécifiée avec l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE.|  
|HY111|Valeur de signet non valide|L’argument FetchOrientation a été SQL_FETCH_BOOKMARK, et le signet vers lequel pointe la valeur dans l’attribut d’instruction SQL_ATTR_FETCH_BOOKMARK_PTR n’était pas valide ou était un pointeur null.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Le pilote ou la source de données ne prend pas en charge la conversion spécifiée par la combinaison du *TargetType* dans **SQLBindCol** et du type de données SQL de la colonne correspondante.|  
|HYT00|Délai expiré|Le délai d’expiration de la requête a expiré avant que la source de données n’ait retourné le jeu de résultats demandé. Le délai d’attente est défini à l’aide de SQLSetStmtAttr, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 **SQLFetchScroll** retourne un ensemble de lignes spécifié à partir du jeu de résultats. Les ensembles de lignes peuvent être spécifiés par position absolue ou relative ou par signet. **SQLFetchScroll** peut être appelé uniquement lorsqu’un jeu de résultats existe, autrement dit, après un appel qui crée un jeu de résultats et avant la fermeture du curseur sur ce jeu de résultats. Si des colonnes sont liées, elles retournent les données dans ces colonnes. Si l’application a spécifié un pointeur vers un tableau d’état de ligne ou une mémoire tampon dans laquelle retourner le nombre de lignes extraites, **SQLFetchScroll** retourne également ces informations. Les appels à **SQLFetchScroll** peuvent être mélangés avec des appels à **SQLFetch** , mais ils ne peuvent pas être mélangés avec des appels à **SQLExtendedFetch**.  
  
 Pour plus d’informations, consultez [utilisation de curseurs de bloc](../../../odbc/reference/develop-app/using-block-cursors.md) et utilisation de [curseurs à défilement](../../../odbc/reference/develop-app/using-scrollable-cursors.md).  
  
## <a name="positioning-the-cursor"></a>Positionnement du curseur  
 Lorsque le jeu de résultats est créé, le curseur est positionné avant le début du jeu de résultats. **SQLFetchScroll** positionne le curseur de bloc en fonction des valeurs des arguments *FetchOrientation* et *FetchOffset* , comme indiqué dans le tableau suivant. Les règles exactes pour déterminer le début du nouvel ensemble de lignes s’affichent dans la section suivante.  
  
|FetchOrientation|Signification|  
|----------------------|-------------|  
|SQL_FETCH_NEXT|Retourne l’ensemble de lignes suivant. Cela équivaut à appeler **SQLFetch**.<br /><br /> **SQLFetchScroll** ignore la valeur de *FetchOffset*.|  
|SQL_FETCH_PRIOR|Retourne l’ensemble de lignes précédent.<br /><br /> **SQLFetchScroll** ignore la valeur de *FetchOffset*.|  
|SQL_FETCH_RELATIVE|Retourne l’ensemble de lignes *FetchOffset* à partir du début de l’ensemble de lignes actuel.|  
|SQL_FETCH_ABSOLUTE|Retourne l’ensemble de lignes en commençant à la ligne *FetchOffset*.|  
|SQL_FETCH_FIRST|Retourne le premier ensemble de lignes dans le jeu de résultats.<br /><br /> **SQLFetchScroll** ignore la valeur de *FetchOffset*.|  
|SQL_FETCH_LAST|Retourne le dernier ensemble de lignes complet dans le jeu de résultats.<br /><br /> **SQLFetchScroll** ignore la valeur de *FetchOffset*.|  
|SQL_FETCH_BOOKMARK|Retourne les lignes de l’ensemble de lignes FetchOffset à partir du signet spécifié par l’attribut d’instruction SQL_ATTR_FETCH_BOOKMARK_PTR.|  
  
 Les pilotes ne sont pas requis pour prendre en charge toutes les orientations d’extraction ; une application appelle **SQLGetInfo** avec un type d’informations SQL_DYNAMIC_CURSOR_ATTRIBUTES1, SQL_KEYSET_CURSOR_ATTRIBUTES1 ou SQL_STATIC_CURSOR_ATTRIBUTES1 (selon le type du curseur) pour déterminer quelles orientations d’extraction sont prises en charge par le pilote. L’application doit examiner les masques de SQL_CA1_NEXT, SQL_CA1_RELATIVE, SQL_CA1_ABSOLUTE et WQL_CA1_BOOKMARK dans ces types d’informations. En outre, si le curseur est avant uniquement et que FetchOrientation n’est pas SQL_FETCH_NEXT, **SQLFetchScroll** retourne SQLState HY106 (type d’extraction hors limites).  
  
 L’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE spécifie le nombre de lignes dans l’ensemble de lignes. Si l’ensemble de lignes récupéré par **SQLFetchScroll** chevauche la fin du jeu de résultats, **SQLFetchScroll** retourne un ensemble de lignes partiel. Autrement dit, si S + R-1 est supérieur à L, où S est la ligne de départ de l’ensemble de lignes en cours d’extraction, R est la taille de l’ensemble de lignes et L est la dernière ligne du jeu de résultats, alors seules les L-S + 1 premières lignes de l’ensemble de lignes sont valides. Les lignes restantes sont vides et présentent l’État SQL_ROW_NOROW.  
  
 Après le retour de **SQLFetchScroll** , la ligne actuelle est la première ligne de l’ensemble de lignes.  
  
## <a name="cursor-positioning-rules"></a>Règles de positionnement des curseurs  
 Les sections suivantes décrivent les règles exactes pour chaque valeur de FetchOrientation. Ces règles utilisent la notation suivante.  
  
|Notation|Signification|  
|--------------|-------------|  
|*Avant le début*|Le curseur de bloc est positionné avant le début du jeu de résultats. Si la première ligne du nouvel ensemble de lignes se trouve avant le début de l’ensemble de résultats, **SQLFetchScroll** retourne SQL_NO_DATA.|  
|*Après la fin*|Le curseur de bloc est positionné après la fin du jeu de résultats. Si la première ligne du nouvel ensemble de lignes se trouve après la fin de l’ensemble de résultats, **SQLFetchScroll** retourne SQL_NO_DATA.|  
|*CurrRowsetStart*|Numéro de la première ligne dans l’ensemble de lignes actif.|  
|*LastResultRow*|Numéro de la dernière ligne dans le jeu de résultats.|  
|*RowsetSize*|Taille de l’ensemble de lignes.|  
|*FetchOffset*|Valeur de l’argument *FetchOffset* .|  
|*BookmarkRow*|Ligne correspondant au signet spécifié par l’attribut d’instruction SQL_ATTR_FETCH_BOOKMARK_PTR.|  
  
## <a name="sql_fetch_next"></a>SQL_FETCH_NEXT  
 Les règles suivantes s’appliquent.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|*Avant le début*|1|  
|*CurrRowsetStart + RowsetSize*[1] *\< = LastResultRow*|*CurrRowsetStart + RowsetSize*[1]|  
|*CurrRowsetStart + RowsetSize*[1]*> LastResultRow*|*Après la fin*|  
|*Après la fin*|*Après la fin*|  
  
 [1] si la taille de l’ensemble de lignes a été modifiée depuis l’appel précédent à FETCH Rows, il s’agit de la taille de l’ensemble de lignes utilisée avec l’appel précédent.  
  
## <a name="sql_fetch_prior"></a>SQL_FETCH_PRIOR  
 Les règles suivantes s’appliquent.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|*Avant le début*|*Avant le début*|  
|*CurrRowsetStart = 1*|*Avant le début*|  
|*1 < CurrRowsetStart <= RowsetSize* <sup>[2]</sup>|*1* <sup>[1]</sup>|  
|*CurrRowsetStart > RowsetSize* <sup>[2]</sup>|*CurrRowsetStart-RowsetSize* <sup>[2]</sup>|  
|*Après end et LastResultRow < RowsetSize* <sup>[2]</sup>|*1* <sup>[1]</sup>|  
|*Après end et LastResultRow >= RowsetSize* <sup>[2]</sup>|*LastResultRow-RowsetSize + 1* <sup>[2]</sup>|  
  
 [1]   **SQLFetchScroll** retourne SQLState 01S06 (tentative d’extraction avant que le jeu de résultats ait retourné le premier ensemble de lignes) et SQL_SUCCESS_WITH_INFO.  
  
 [2] si la taille de l’ensemble de lignes a été modifiée depuis l’appel précédent à FETCH Rows, il s’agit de la nouvelle taille de l’ensemble de lignes.  
  
## <a name="sql_fetch_relative"></a>SQL_FETCH_RELATIVE  
 Les règles suivantes s’appliquent.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|*(Avant le début et la FetchOffset > 0) OU (après end et FetchOffset < 0)*|*--*<sup>[1]</sup>|  
|*BeforeStart et FetchOffset <= 0*|*Avant le début*|  
|*CurrRowsetStart = 1 et FetchOffset < 0*|*Avant le début*|  
|*CurrRowsetStart > 1 et CurrRowsetStart + FetchOffset < 1 et &#124; FetchOffset &#124; > RowsetSize* <sup>[3]</sup>|*Avant le début*|  
|*CurrRowsetStart > 1 et CurrRowsetStart + FetchOffset < 1 et &#124; FetchOffset &#124; <= RowsetSize* <sup>[3]</sup>|*1* <sup>[2]</sup>|  
|*1 <= CurrRowsetStart + FetchOffset \< = LastResultRow*|*CurrRowsetStart + FetchOffset*|  
|*CurrRowsetStart + FetchOffset > LastResultRow*|*Après la fin*|  
|*Après end et FetchOffset >= 0*|*Après la fin*|  
  
 [1] ***SQLFetchScroll** _ retourne le même ensemble de lignes que s’il a été appelé avec FetchOrientation défini sur SQL_FETCH_ABSOLUTE. Pour plus d’informations, consultez la section « SQL_FETCH_ABSOLUTE ».  
  
 [2] _ *SQLFetchScroll** retourne SQLState 01S06 (tentative d’extraction avant que le jeu de résultats ait retourné le premier ensemble de lignes) et SQL_SUCCESS_WITH_INFO.  
  
 [3] si la taille de l’ensemble de lignes a été modifiée depuis l’appel précédent à FETCH Rows, il s’agit de la nouvelle taille de l’ensemble de lignes.  
  
## <a name="sql_fetch_absolute"></a>SQL_FETCH_ABSOLUTE  
 Les règles suivantes s’appliquent.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|*FetchOffset < 0 et &#124; FetchOffset &#124; <= LastResultRow*|*LastResultRow + FetchOffset + 1*|  
|*FetchOffset < 0 et &#124; FetchOffset &#124; > LASTRESULTROW et &#124; FetchOffset &#124; > RowsetSize* <sup>[2]</sup>|*Avant le début*|  
|*FetchOffset < 0 et &#124; FetchOffset &#124; > LASTRESULTROW et &#124; FetchOffset &#124; <= RowsetSize* <sup>[2]</sup>|*1* <sup>[1]</sup>|  
|*FetchOffset = 0*|*Avant le début*|  
|*1 <= FetchOffset \< = LastResultRow*|*FetchOffset*|  
|*FetchOffset > LastResultRow*|*Après la fin*|  
  
 [1]   **SQLFetchScroll** retourne SQLState 01S06 (tentative d’extraction avant que le jeu de résultats ait retourné le premier ensemble de lignes) et SQL_SUCCESS_WITH_INFO.  
  
 [2] si la taille de l’ensemble de lignes a été modifiée depuis l’appel précédent à FETCH Rows, il s’agit de la nouvelle taille de l’ensemble de lignes.  
  
 Une extraction absolue effectuée sur un curseur dynamique ne peut pas fournir le résultat requis, car les positions de ligne dans un curseur dynamique ne sont pas déterministes. Une telle opération est équivalente à une instruction FETCH First suivie d’une instruction FETCH relative ; il ne s’agit pas d’une opération atomique, comme c’est le cas d’une extraction absolue sur un curseur statique.  
  
## <a name="sql_fetch_first"></a>SQL_FETCH_FIRST  
 Les règles suivantes s’appliquent.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|*Any*|*1*|  
  
## <a name="sql_fetch_last"></a>SQL_FETCH_LAST  
 Les règles suivantes s’appliquent.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|*RowsetSize* <sup>[1]</sup> <= LastResultRow|*LastResultRow-RowsetSize + 1* <sup>[1]</sup>|  
|*RowsetSize* <sup>[1]</sup> > LastResultRow|*1*|  
  
 [1] si la taille de l’ensemble de lignes a été modifiée depuis l’appel précédent à FETCH Rows, il s’agit de la nouvelle taille de l’ensemble de lignes.  
  
## <a name="sql_fetch_bookmark"></a>SQL_FETCH_BOOKMARK  
 Les règles suivantes s’appliquent.  
  
|Condition|Première ligne du nouvel ensemble de lignes|  
|---------------|-----------------------------|  
|*BookmarkRow + FetchOffset < 1*|*Avant le début*|  
|*1 <= BookmarkRow + FetchOffset \< = LastResultRow*|*BookmarkRow + FetchOffset*|  
|*BookmarkRow + FetchOffset > LastResultRow*|*Après la fin*|  
  
 Pour plus d’informations sur les signets, consultez [signets (ODBC)](../../../odbc/reference/develop-app/bookmarks-odbc.md).  
  
## <a name="effect-of-deleted-added-and-error-rows-on-cursor-movement"></a>Effet des lignes supprimées, ajoutées et d’erreur sur le déplacement du curseur  
 Les curseurs statiques et pilotés par keyset détectent parfois les lignes ajoutées au jeu de résultats et suppriment les lignes supprimées du jeu de résultats. En appelant **SQLGetInfo** avec les options SQL_STATIC_CURSOR_ATTRIBUTES2 et SQL_KEYSET_CURSOR_ATTRIBUTES2 et en observant les masques de SQL_CA2_SENSITIVITY_ADDITIONS, SQL_CA2_SENSITIVITY_DELETIONS et SQL_CA2_SENSITIVITY_UPDATES, une application détermine si les curseurs implémentés par un pilote particulier le font. Pour les pilotes capables de détecter les lignes supprimées et de les supprimer, les paragraphes suivants décrivent les effets de ce comportement. Pour les pilotes qui peuvent détecter les lignes supprimées, mais ne peuvent pas les supprimer, les suppressions n’ont aucun effet sur les mouvements de curseur, et les paragraphes suivants ne s’appliquent pas.  
  
 Si le curseur détecte des lignes ajoutées au jeu de résultats ou supprime des lignes supprimées du jeu de résultats, il apparaît comme s’il détecte ces modifications uniquement lorsqu’il extrait des données. Cela inclut le cas où **SQLFetchScroll** est appelé avec FetchOrientation défini sur SQL_FETCH_RELATIVE et FetchOffset défini sur 0 pour réextraire le même ensemble de lignes, mais n’inclut pas le cas où SQLSetPos est appelé avec fOption défini sur SQL_REFRESH. Dans ce dernier cas, les données dans les mémoires tampons de l’ensemble de lignes sont actualisées, mais ne sont pas récupérées, et les lignes supprimées ne sont pas supprimées du jeu de résultats. Ainsi, lorsqu’une ligne est supprimée ou insérée dans l’ensemble de lignes actuel, le curseur ne modifie pas les tampons d’ensemble de lignes. Au lieu de cela, il détecte la modification lors de l’extraction d’un ensemble de lignes qui comprenait précédemment la ligne supprimée ou inclut maintenant la ligne insérée.  
  
 Par exemple :  
  
```cpp  
// Fetch the next rowset.  
SQLFetchScroll(hstmt, SQL_FETCH_NEXT, 0);  
// Delete third row of the rowset. Does not modify the rowset buffers.  
SQLSetPos(hstmt, 3, SQL_DELETE, SQL_LOCK_NO_CHANGE);  
// The third row has a status of SQL_ROW_DELETED after this call.  
SQLSetPos(hstmt, 3, SQL_REFRESH, SQL_LOCK_NO_CHANGE);  
// Refetch the same rowset. The third row is removed, replaced by what  
// was previously the fourth row.  
SQLFetchScroll(hstmt, SQL_FETCH_RELATIVE, 0);  
```  
  
 Lorsque **SQLFetchScroll** retourne un nouvel ensemble de lignes qui a une position par rapport à l’ensemble de lignes actuel, autrement dit, FetchOrientation est SQL_FETCH_NEXT, SQL_FETCH_PRIOR ou SQL_FETCH_RELATIVE-il n’inclut pas les modifications apportées à l’ensemble de lignes actuel lors du calcul de la position de départ du nouvel ensemble de lignes. Toutefois, elle inclut des modifications en dehors de l’ensemble de lignes actuel si elle est en capacité de les détecter. En outre, lorsque **SQLFetchScroll** retourne un nouvel ensemble de lignes qui a une position indépendante de l’ensemble de lignes actuel, autrement dit, FetchOrientation est SQL_FETCH_FIRST, SQL_FETCH_LAST, SQL_FETCH_ABSOLUTE ou SQL_FETCH_BOOKMARK-il inclut toutes les modifications qu’il est capable de détecter, même s’il se trouve dans l’ensemble de lignes actif.  
  
 Pour déterminer si des lignes nouvellement ajoutées se trouvent à l’intérieur ou à l’extérieur de l’ensemble de lignes actuel, un ensemble de lignes partiel est considéré comme se terminant à la dernière ligne valide ; autrement dit, la dernière ligne pour laquelle l’état de la ligne n’est pas SQL_ROW_NOROW. Par exemple, supposons que le curseur puisse détecter les nouvelles lignes ajoutées, que l’ensemble de lignes actuel est un ensemble de lignes partiel, que l’application ajoute de nouvelles lignes et que le curseur ajoute ces lignes à la fin du jeu de résultats. Si l’application appelle **SQLFetchScroll** avec FetchOrientation défini sur SQL_FETCH_NEXT, **SQLFetchScroll** retourne l’ensemble de lignes en commençant par la première ligne qui vient d’être ajoutée.  
  
 Par exemple, supposons que l’ensemble de lignes actuel comprend les lignes 21 à 30, la taille de l’ensemble de lignes est 10, le curseur supprime les lignes supprimées du jeu de résultats et le curseur détecte les lignes ajoutées au jeu de résultats. Le tableau suivant montre les lignes **retournées** dans différentes situations.  
  
|Modifier|Type d’extraction|FetchOffset|Nouvel ensemble de lignes [1]|  
|------------|----------------|-----------------|---------------------|  
|Supprimer la ligne 21|NEXT|0|31 à 40|  
|Supprimer la ligne 31|NEXT|0|32 à 41|  
|Insérer une ligne entre les lignes 21 et 22|NEXT|0|31 à 40|  
|Insérer une ligne entre les lignes 30 et 31|NEXT|0|Ligne insérée, 31 à 39|  
|Supprimer la ligne 21|PRIOR|0|11 à 20|  
|Supprimer la ligne 20|PRIOR|0|10 à 19|  
|Insérer une ligne entre les lignes 21 et 22|PRIOR|0|11 à 20|  
|Insérer une ligne entre les lignes 20 et 21|PRIOR|0|12 à 20, ligne insérée|  
|Supprimer la ligne 21|RELATIVE|0|22 à 31<sup>[2]</sup>|  
|Supprimer la ligne 21|RELATIVE|1|de 22 à 31|  
|Insérer une ligne entre les lignes 21 et 22|RELATIVE|0|21, ligne insérée, 22 à 29|  
|Insérer une ligne entre les lignes 21 et 22|RELATIVE|1|de 22 à 31|  
|Supprimer la ligne 21|ABSOLUTE|21|22 à 31<sup>[2]</sup>|  
|Supprimer la ligne 22|ABSOLUTE|21|21, 23 à 31|  
|Insérer une ligne entre les lignes 21 et 22|ABSOLUTE|22|Ligne insérée, entre 22 et 29|  
  
 [1] cette colonne utilise les numéros de ligne avant l’insertion ou la suppression de lignes.  
  
 [2] dans ce cas, le curseur tente de renvoyer des lignes commençant par la ligne 21. Étant donné que la ligne 21 a été supprimée, la première ligne renvoyée est la ligne 22.  
  
 Les lignes d’erreur (autrement dit, les lignes dont l’État est SQL_ROW_ERROR) n’affectent pas le déplacement du curseur. Par exemple, si l’ensemble de lignes actif commence par la ligne 11 et que l’état de la ligne 11 est SQL_ROW_ERROR, l’appel de **SQLFetchScroll** avec FetchOrientation défini sur SQL_FETCH_RELATIVE et FetchOffset défini à 5 retourne l’ensemble de lignes à partir de la ligne 16, exactement comme si l’état de la ligne 11 était SQL_SUCCESS.  
  
## <a name="returning-data-in-bound-columns"></a>Retour de données dans des colonnes dépendantes  
 **SQLFetchScroll** retourne des données dans des colonnes dépendantes de la même façon que **SQLFetch**. Pour plus d’informations, consultez « retour de données dans des colonnes dépendantes » dans la [fonction SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
 Si aucune colonne n’est liée, **SQLFetchScroll** ne retourne pas de données, mais déplace le curseur de bloc à la position spécifiée. Indique si les données peuvent être récupérées à partir de colonnes indépendantes d’un curseur de bloc avec **SQLGetData** dépend du pilote. Cette fonctionnalité est prise en charge si un appel à **SQLGetInfo** retourne le bit SQL_GD_BLOCK pour le type d’informations SQL_GETDATA_EXTENSIONS.  
  
## <a name="buffer-addresses"></a>Adresses de mémoire tampon  
 **SQLFetchScroll** utilise la même formule pour déterminer l’adresse des tampons de données et de longueur/d’indicateur en tant que **SQLFetch**. Pour plus d’informations, consultez « adresses de tampon » dans la [fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
## <a name="row-status-array"></a>Tableau d’état des lignes  
 **SQLFetchScroll** définit les valeurs dans le tableau d’état de ligne de la même manière que SQLFetch. Pour plus d’informations, consultez « tableau d’état des lignes » dans la [fonction SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
## <a name="rows-fetched-buffer"></a>Tampons extraits de lignes  
 **SQLFetchScroll** retourne le nombre de lignes extraites dans le tampon de lignes extraites de la même façon que **SQLFetch**. Pour plus d’informations, consultez « tampons extraits de lignes » dans la [fonction SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Quand une application appelle **SQLFetchScroll** dans un pilote ODBC 3. x, le gestionnaire de pilotes appelle **SQLFetchScroll** dans le pilote. Quand une application appelle **SQLFetchScroll** dans un pilote ODBC 2. x, le gestionnaire de pilotes appelle SQLExtendedFetch dans le pilote. Étant donné que **SQLFetchScroll** et SQLExtendedFetch gèrent les erreurs d’une manière légèrement différente, l’application voit un comportement d’erreur légèrement différent lorsqu’il appelle **SQLFetchScroll** dans les pilotes ODBC 2. x et ODBC 3. x.  
  
 **SQLFetchScroll** retourne des erreurs et des avertissements de la même manière que **SQLFetch**; Pour plus d’informations, consultez « Gestion des erreurs » dans **SQLFetch**. **SQLExtendedFetch** retourne les erreurs de la même manière que **SQLFetch**, avec les exceptions suivantes :  
  
 Lorsqu’un avertissement s’applique à une ligne particulière de l’ensemble de lignes, SQLExtendedFetch définit l’entrée correspondante dans le tableau d’état de ligne sur SQL_ROW_SUCCESS, et non SQL_ROW_SUCCESS_WITH_INFO.  
  
 Si des erreurs se produisent dans chaque ligne de l’ensemble de lignes, SQLExtendedFetch retourne SQL_SUCCESS_WITH_INFO, pas SQL_ERROR.  
  
 Dans chaque groupe d’enregistrements d’État qui s’applique à une ligne individuelle, le premier enregistrement d’état retourné par SQLExtendedFetch doit contenir SQLSTATE 01S01 (erreur dans la ligne); **SQLFetchScroll** ne retourne pas cette SQLSTATE. Si SQLExtendedFetch n’est pas en mesure de renvoyer des SQLSTATE supplémentaires, il doit toujours renvoyer ce SQLSTATE.  
  
## <a name="sqlfetchscroll-and-optimistic-concurrency"></a>SQLFetchScroll et accès concurrentiel optimiste  
 Si un curseur utilise l’accès concurrentiel optimiste (autrement dit, l’attribut d’instruction SQL_ATTR_CONCURRENCY a la valeur SQL_CONCUR_VALUES ou SQL_CONCUR_ROWVER- **SQLFetchScroll** met à jour les valeurs d’accès concurrentiel optimiste utilisées par la source de données pour détecter si une ligne a changé. Cela se produit chaque fois que **SQLFetchScroll** extrait un nouvel ensemble de lignes, y compris lorsqu’il récupère à nouveau l’ensemble de lignes actuel. (Elle est appelée avec FetchOrientation défini sur SQL_FETCH_RELATIVE et FetchOffset défini sur 0.)  
  
## <a name="sqlfetchscroll-and-odbc-2x-drivers"></a>Pilotes SQLFetchScroll et ODBC 2. x  
 Quand une application appelle **SQLFetchScroll** dans un pilote ODBC 2. x, le gestionnaire de pilotes mappe cet appel à **SQLExtendedFetch**. Il passe les valeurs suivantes pour les arguments de **SQLExtendedFetch**.  
  
|SQLExtendedFetch, argument|Valeur|  
|-------------------------------|-----------|  
|StatementHandle|StatementHandle dans **SQLFetchScroll**.|  
|FetchOrientation|FetchOrientation dans **SQLFetchScroll**.|  
|FetchOffset|Si FetchOrientation n’est pas SQL_FETCH_BOOKMARK, la valeur de l’argument FetchOffset dans **SQLFetchScroll** est utilisée.<br /><br /> Si FetchOrientation est SQL_FETCH_BOOKMARK, la valeur stockée à l’adresse spécifiée par l’attribut d’instruction SQL_ATTR_FETCH_BOOKMARK_PTR est utilisée.|  
|RowCountPtr|Adresse spécifiée par l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR.|  
|RowStatusArray|Adresse spécifiée par l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR.|  
  
 Pour plus d’informations, consultez [curseurs de bloc, curseurs avec défilement et compatibilité descendante](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) dans l’annexe G : instructions relatives aux pilotes pour la compatibilité descendante.  
  
## <a name="descriptors-and-sqlfetchscroll"></a>Descripteurs et SQLFetchScroll  
 **SQLFetchScroll** interagit avec les descripteurs de la même manière que **SQLFetch**. Pour plus d’informations, consultez la section « descripteurs and SQLFetchScroll » de la [fonction SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
## <a name="code-example"></a>Exemple de code  
 Consultez [liaison](../../../odbc/reference/develop-app/column-wise-binding.md)selon les colonnes, [liaison](../../../odbc/reference/develop-app/row-wise-binding.md)selon les lignes, [instructions Update et DELETE positionnées](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md)et [mise à jour de lignes dans l’ensemble de lignes avec SQLSetPos](../../../odbc/reference/develop-app/updating-rows-in-the-rowset-with-sqlsetpos.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Exécution d’opérations d’insertion, de mise à jour ou de suppression en bloc|[SQLBulkOperations, fonction](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retour d’informations sur une colonne dans un jeu de résultats|[Fonction SQLDescribeCol](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécution d’une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Extraction d’une seule ligne ou d’un bloc de données dans une direction vers l’avant uniquement|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Fermeture du curseur sur l’instruction|[Fonction SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Retour du nombre de colonnes du jeu de résultats|[Fonction SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Positionnement du curseur, actualisation des données dans l’ensemble de lignes ou mise à jour ou suppression de données dans le jeu de résultats|[SQLSetPos, fonction](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
|Définition d’un attribut d’instruction|[Fonction SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
