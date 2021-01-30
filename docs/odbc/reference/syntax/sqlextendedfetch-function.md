---
description: SQLExtendedFetch, fonction
title: SQLExtendedFetch, fonction | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLExtendedFetch
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLExtendedFetch
helpviewer_keywords:
- SQLExtendedFetch function [ODBC]
ms.assetid: 940b5cf7-581c-4ede-8533-c67d5e9ef488
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 403b8d426049ba6a333920d472c2e69ed279d761
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179867"
---
# <a name="sqlextendedfetch-function"></a>SQLExtendedFetch, fonction
**Conformité**  
 Version introduite : conformité des normes ODBC 1,0 : déconseillé  
  
 **Résumé**  
 **SQLExtendedFetch** récupère l’ensemble de lignes de données spécifié dans le jeu de résultats et retourne des données pour toutes les colonnes liées. Les ensembles de lignes peuvent être spécifiés à une position absolue ou relative ou par signet.  
  
> [!NOTE]
>  Dans ODBC 3 *. x*, **SQLExtendedFetch** a été remplacé par **SQLFetchScroll**. Les applications ODBC 3 *. x* ne doivent pas appeler **SQLExtendedFetch**; au lieu de cela, ils doivent appeler **SQLFetchScroll**. Le gestionnaire de pilotes mappe **SQLFetchScroll** à **SQLExtendedFetch** quand vous utilisez un pilote ODBC 2 *. x* . Les pilotes ODBC 3 *. x* doivent prendre en charge **SQLExtendedFetch** s’ils souhaitent travailler avec des applications ODBC 2 *. x* qui l’appellent. Pour plus d’informations, consultez « Commentaires » et [curseurs de bloc, curseurs avec défilement et compatibilité descendante](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) dans annexe G : instructions relatives aux pilotes pour la compatibilité descendante.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLExtendedFetch(  
      SQLHSTMT         StatementHandle,  
      SQLUSMALLINT     FetchOrientation,  
      SQLLEN           FetchOffset,  
      SQLULEN *        RowCountPtr,  
      SQLUSMALLINT *   RowStatusArray);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *FetchOrientation*  
 Entrée Type d’extraction. Il s’agit de la même chose que *FetchOrientation* dans **SQLFetchScroll**.  
  
 *FetchOffset*  
 Entrée Numéro de la ligne à extraire. C’est le même que *FetchOffset* dans **SQLFetchScroll**, à une exception près. Quand *FetchOrientation* est SQL_FETCH_BOOKMARK, *FetchOffset* est un signet de longueur fixe, et non un offset d’un signet. En d’autres termes, **SQLExtendedFetch** récupère le signet de cet argument, pas l’attribut d’instruction SQL_ATTR_FETCH_BOOKMARK_PTR. Elle ne prend pas en charge les signets de longueur variable et ne prend pas en charge l’extraction d’un ensemble de lignes à un décalage (autre que 0) à partir d’un signet.  
  
 *RowCountPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre de lignes réellement récupérées. Cette mémoire tampon est utilisée de la même manière que la mémoire tampon spécifiée par l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR. Cette mémoire tampon est utilisée uniquement par **SQLExtendedFetch**. Elle n’est pas utilisée par **SQLFetch** ou **SQLFetchScroll**.  
  
 *RowStatusArray*  
 Sortie Pointeur vers un tableau dans lequel retourner l’état de chaque ligne. Ce tableau est utilisé de la même manière que le tableau spécifié par l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR.  
  
 Toutefois, l’adresse de ce tableau n’est pas stockée dans le champ SQL_DESC_STATUS_ARRAY_PTR dans le IRD. En outre, ce tableau est utilisé uniquement par **SQLExtendedFetch** et **SQLBulkOperations** avec une *opération* de SQL_ADD ou **SQLSetPos** lorsqu’il est appelé après **SQLExtendedFetch**. Il n’est pas utilisé par **SQLFetch** ou **SQLFetchScroll**, et il n’est pas utilisé par **SQLBulkOperations** ou **SQLSetPos** lorsqu’ils sont appelés après **SQLFetch** ou **SQLFetchScroll**. Elle n’est pas non plus utilisée lorsque **SQLBulkOperations** avec une *opération* de SQL_ADD est appelé avant l’appel d’une fonction Fetch. En d’autres termes, il est utilisé uniquement dans l’état de l’instruction S7. Elle n’est pas utilisée dans les États d’instruction S5 ou S6. Pour plus d’informations, consultez [transitions d’instructions](../../../odbc/reference/appendixes/statement-transitions.md) dans annexe B : tables de transition d’État ODBC.  
  
 Les applications doivent fournir un pointeur valide dans l’argument *RowStatusArray* ; Si ce n’est pas le cas, le comportement de **SQLExtendedFetch** et le comportement des appels à **SQLBulkOperations** ou **SQLSetPos** après qu’un curseur a été positionné par **SQLExtendedFetch** ne sont pas définis.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLExtendedFetch** retourne soit SQL_ERROR, soit SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLError**. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLExtendedFetch** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire. Si une erreur se produit sur une seule colonne, **SQLGetDiagField** peut être appelé avec un *DiagIdentifier* de SQL_DIAG_COLUMN_NUMBER pour déterminer la colonne sur laquelle l’erreur s’est produite ; et **SQLGetDiagField** peuvent être appelés avec un *DiagIdentifier* de SQL_DIAG_ROW_NUMBER pour déterminer la ligne contenant cette colonne.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Les données binaires ou de chaîne retournées pour une colonne ont entraîné la troncation d’un caractère non vide ou de données binaires non NULL. S’il s’agit d’une valeur de chaîne, elle a été tronquée à droite. S’il s’agit d’une valeur numérique, la partie fractionnaire du nombre a été tronquée.  (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S01|Erreur dans la ligne|Une erreur s’est produite lors de l’extraction d’une ou plusieurs lignes. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S06|Tentative de récupération avant que le jeu de résultats ait retourné le premier ensemble de lignes|L’ensemble de lignes demandé chevauche le début du jeu de résultats lorsque la position actuelle se situe au-delà de la première ligne, et que *FetchOrientation* a été SQL_PRIOR ou *FetchOrientation* a été SQL_RELATIVE avec un *FetchOffset* négatif dont la valeur absolue était inférieure ou égale à la SQL_ROWSET_SIZE actuelle. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S07|Troncation fractionnaire|Les données retournées pour une colonne ont été tronquées. Pour les types de données numériques, la partie fractionnaire du nombre a été tronquée. Pour les types de données time, timestamp et Interval contenant un composant heure, la partie fractionnaire de l’heure a été tronquée.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07006|Violation d’attribut de type de données restreint|Une valeur de données n’a pas pu être convertie en type de données C spécifié par *TargetType* dans **SQLBindCol**.|  
|07009|Index de descripteur non valide|La colonne 0 était liée à **SQLBindCol** et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_OFF.|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|22002|Variable d’indicateur requise mais non fournie|Les données NULL ont été extraites dans une colonne dont la *StrLen_or_IndPtr* définie par **SQLBindCol** était un pointeur null.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|22003|Valeur numérique hors limites|Le retour de la valeur numérique (sous forme de valeur numérique ou de chaîne) pour une ou plusieurs colonnes aurait entraîné la troncation de la totalité (par opposition à la partie fractionnaire) du nombre à tronquer.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)<br /><br /> Pour plus d’informations, consultez [instructions pour les types de données Interval et numeric](../../../odbc/reference/appendixes/guidelines-for-interval-and-numeric-data-types.md) dans l’annexe D : types de données.|  
|22007|Format de date/heure non valide|Une colonne de caractères dans le jeu de résultats a été liée à une structure de date, d’heure ou d’horodatage C, et une valeur dans la colonne était, respectivement, une date, une heure ou un horodateur non valide.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|22012|Division par zéro|Une valeur issue d’une expression arithmétique a été retournée, ce qui a entraîné une division par zéro.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|22015|Dépassement du champ d’intervalle|L’assignation d’un type SQL exact numérique ou Interval à un type C Interval a provoqué une perte de chiffres significatifs dans le champ de début.<br /><br /> Lors de l’extraction de données vers un type d’intervalle C, il n’existait aucune représentation de la valeur du type SQL dans le type d’intervalle C.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|22018|Valeur de caractère non valide pour la spécification de cast|Le type C était un type de données numérique exact ou approximatif, DateTime ou Interval ; le type SQL de la colonne était un type de données caractère ; et la valeur dans la colonne n’est pas un littéral valide du type C lié.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|24 000|État de curseur non valide|Le *StatementHandle* était dans un état d’exécution, mais aucun jeu de résultats n’a été associé à *StatementHandle*.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLError** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*, puis la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLExtendedFetch** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) le *StatementHandle* spécifié n’était pas dans un état d’exécution. La fonction a été appelée sans appeler d’abord **SQLExecDirect**, **SQLExecute** ou une fonction de catalogue.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.<br /><br /> (DM) **SQLExtendedFetch** a été appelé pour *StatementHandle* après l’appel de **SQLFetch** ou **SQLFetchScroll** et avant l’appel de **SQLFreeStmt** avec l’option SQL_CLOSE.<br /><br /> (DM) **SQLBulkOperations** a été appelé pour une instruction avant que **SQLFetch**, **SQLFetchScroll** ou **SQLExtendedFetch** n’ait été appelé, puis **SQLExtendedFetch** a été appelé avant l’appel de **SQLFreeStmt** avec l’option SQL_CLOSE.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY106|Type d’extraction hors limites|(DM) la valeur spécifiée pour l’argument *FetchOrientation* n’était pas valide. (Voir « commentaires ».)<br /><br /> L’argument *FetchOrientation* a été SQL_FETCH_BOOKMARK et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_OFF.<br /><br /> La valeur de l’option d’instruction SQL_CURSOR_TYPE a été SQL_CURSOR_FORWARD_ONLY et la valeur de l’argument *FetchOrientation* n’a pas été SQL_FETCH_NEXT.<br /><br /> L’argument *FetchOrientation* a été SQL_FETCH_RESUME.|  
|HY107|Valeur de ligne hors limites|La valeur spécifiée avec l’option d’instruction SQL_CURSOR_TYPE a été SQL_CURSOR_KEYSET_DRIVEN, mais la valeur spécifiée avec l’attribut d’instruction SQL_KEYSET_SIZE était supérieure à 0 et inférieure à la valeur spécifiée avec l’attribut d’instruction SQL_ROWSET_SIZE.|  
|HY111|Valeur de signet non valide|L’argument *FetchOrientation* a été SQL_FETCH_BOOKMARK, et le signet spécifié dans l’argument *FetchOffset* n’était pas valide.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Le pilote ou la source de données ne prend pas en charge le type d’extraction spécifié.<br /><br /> Le pilote ou la source de données ne prend pas en charge la conversion spécifiée par la combinaison du *TargetType* dans **SQLBindCol** et du type de données SQL de la colonne correspondante. Cette erreur s’applique uniquement lorsque le type de données SQL de la colonne a été mappé à un type de données SQL spécifique au pilote.|  
|HYT00|Délai expiré|Le délai d’expiration de la requête a expiré avant que la source de données n’ait retourné le jeu de résultats. Le délai d’attente est défini par le biais de **SQLSetStmtOption**, SQL_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Le comportement de **SQLExtendedFetch** est identique à celui de **SQLFetchScroll**, avec les exceptions suivantes :  
  
-   **SQLExtendedFetch** et **SQLFetchScroll** utilisent des méthodes différentes pour retourner le nombre de lignes extraites. **SQLExtendedFetch** retourne le nombre de lignes extraites dans *\* RowCountPtr*; **SQLFetchScroll** retourne le nombre de lignes extraites directement dans la mémoire tampon vers laquelle pointe SQL_ATTR_ROWS_FETCHED_PTR. Pour plus d’informations, consultez l’argument *RowCountPtr* .  
  
-   **SQLExtendedFetch** et **SQLFetchScroll** retournent l’état de chaque ligne dans des tableaux différents. Pour plus d’informations, consultez l’argument *RowStatusArray* .  
  
-   **SQLExtendedFetch** et **SQLFetchScroll** utilisent différentes méthodes pour récupérer le signet quand *FetchOrientation* est SQL_FETCH_BOOKMARK. **SQLExtendedFetch** ne prend pas en charge les signets de longueur variable ni la récupération des ensembles de lignes à un décalage autre que 0 d’un signet. Pour plus d’informations, consultez l’argument *FetchOffset* .  
  
-   **SQLExtendedFetch** et **SQLFetchScroll** utilisent des tailles d’ensemble de lignes différentes. **SQLExtendedFetch** utilise la valeur de l’attribut d’instruction SQL_ROWSET_SIZE et **SQLFetchScroll** utilise la valeur de l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE.  
  
-   **SQLExtendedFetch** a une sémantique de gestion des erreurs légèrement différente de **SQLFetchScroll**. Pour plus d’informations, consultez « Gestion des erreurs » dans la section « commentaires » de [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md).  
  
-   **SQLExtendedFetch** ne prend pas en charge les décalages de liaison (l’attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR).  
  
-   Les appels à **SQLExtendedFetch** ne peuvent pas être mélangés avec des appels à **SQLFetch** ou **SQLFetchScroll**, et si **SQLBulkOperations** est appelé avant que toute fonction FETCH ne soit appelée, **SQLExtendedFetch** ne peut pas être appelé tant que le curseur n’est pas fermé puis rouvert. Autrement dit, **SQLExtendedFetch** ne peut être appelé que dans l’état de l’instruction S7. Pour plus d’informations, consultez [transitions d’instructions](../../../odbc/reference/appendixes/statement-transitions.md) dans annexe B : tables de transition d’État ODBC.  
  
 Quand une application appelle **SQLFetchScroll** lors de l’utilisation d’un pilote ODBC 2 *. x* , le gestionnaire de pilotes mappe cet appel à **SQLExtendedFetch**. Pour plus d’informations, consultez « pilotes SQLFetchScroll et ODBC 2 *. x* » dans [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md).  
  
 Dans ODBC 2 *. x*, **SQLExtendedFetch** a été appelé pour extraire plusieurs lignes et **SQLFetch** a été appelé pour extraire une seule ligne. Dans ODBC 3 *. x*, en revanche, **SQLFetch** peut être appelée pour extraire plusieurs lignes.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Exécution d’opérations d’insertion, de mise à jour ou de suppression en bloc|[SQLBulkOperations, fonction](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retour d’informations sur une colonne dans un jeu de résultats|[Fonction SQLDescribeCol](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécution d’une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Retour du nombre de colonnes du jeu de résultats|[Fonction SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Positionnement du curseur, actualisation des données dans l’ensemble de lignes ou mise à jour ou suppression de données dans le jeu de résultats|[SQLSetPos, fonction](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
|Définition d’un attribut d’instruction|[Fonction SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
