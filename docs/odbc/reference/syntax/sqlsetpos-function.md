---
description: SQLSetPos, fonction
title: SQLSetPos, fonction | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetPos
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLSetPos
helpviewer_keywords:
- SQLSetPos function [ODBC]
ms.assetid: 80190ee7-ae3b-45e5-92a9-693eb558f322
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4793fe06d25b090dfa6306eb8c4e72d513abc8d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188033"
---
# <a name="sqlsetpos-function"></a>SQLSetPos, fonction
**Conformité**  
 Version introduite : ODBC 1,0 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLSetPos** définit la position du curseur dans un ensemble de lignes et permet à une application d’actualiser des données dans l’ensemble de lignes ou de mettre à jour ou de supprimer des données dans le jeu de résultats.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLSetPos(  
      SQLHSTMT        StatementHandle,  
      SQLSETPOSIROW   RowNumber,  
      SQLUSMALLINT    Operation,  
      SQLUSMALLINT    LockType);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *RowNumber*  
 Entrée Position de la ligne dans l’ensemble de lignes sur laquelle effectuer l’opération spécifiée avec l’argument *operation* . Si *RowNumber* a la valeur 0, l’opération s’applique à chaque ligne de l’ensemble de lignes.  
  
 Pour plus d’informations, consultez la section « commentaires ».  
  
 *opération*  
 Entrée Opération à effectuer :  
  
 SQL_POSITION SQL_REFRESH SQL_UPDATE SQL_DELETE  
  
> [!NOTE]
>  La valeur SQL_ADD de l’argument *operation* est dépréciée pour ODBC *3. x*. Les pilotes ODBC *3. x* doivent prendre en charge SQL_ADD pour assurer la compatibilité descendante. Cette fonctionnalité a été remplacée par un appel à **SQLBulkOperations** avec une *opération* de SQL_ADD. Quand une application ODBC *3. x* fonctionne avec un pilote *ODBC 2. x* , le gestionnaire de pilotes mappe un appel à **SQLBulkOperations** avec une *opération* de SQL_ADD à **SQLSetPos** avec une *opération* de SQL_ADD.  
  
 Pour plus d’informations, consultez la section « commentaires ».  
  
 *Verrou*  
 Entrée Spécifie comment verrouiller la ligne après l’exécution de l’opération spécifiée dans l’argument *operation* .  
  
 SQL_LOCK_NO_CHANGE SQL_LOCK_EXCLUSIVE SQL_LOCK_UNLOCK  
  
 Pour plus d’informations, consultez la section « commentaires ».  
  
## <a name="returns"></a>Retours  
  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLSetPos** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLSetPos** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
 Pour tous les SQLSTATEs qui peuvent retourner des SQL_SUCCESS_WITH_INFO ou des SQL_ERROR (à l’exception de 01xxx SQLSTATEs), SQL_SUCCESS_WITH_INFO est retourné si une erreur se produit sur une ou plusieurs lignes, mais pas toutes, sur les lignes d’une opération multiligne, et SQL_ERROR est retournée si une erreur se produit sur une opération à une seule ligne.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01001|Conflit d’opération de curseur|L’argument *operation* était SQL_DELETE ou SQL_UPDATE et aucune ligne ou plus d’une ligne n’a été supprimée ou mise à jour. (Pour plus d’informations sur les mises à jour de plusieurs lignes, consultez la description de l' *attribut* SQL_ATTR_SIMULATE_CURSOR dans **SQLSetStmtAttr**.) (La fonction retourne SQL_SUCCESS_WITH_INFO.)<br /><br /> L’argument *operation* était SQL_DELETE ou SQL_UPDATE, et l’opération a échoué en raison d’un accès concurrentiel optimiste. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Troncation à droite des données de chaîne|L’argument *operation* était SQL_REFRESH, et les données de chaîne ou binaires retournées pour une ou plusieurs colonnes avec un type de données SQL_C_CHAR ou SQL_C_BINARY ont entraîné la troncation d’un caractère non vide ou de données binaires non null.|  
|01S01|Erreur dans la ligne|L’argument *RowNumber* était 0 et une erreur s’est produite dans une ou plusieurs lignes lors de l’exécution de l’opération spécifiée avec l’argument *operation* .<br /><br /> (SQL_SUCCESS_WITH_INFO est retourné si une erreur se produit sur une ou plusieurs lignes d’une opération multiligne, et SQL_ERROR est retourné si une erreur se produit lors d’une opération sur une seule ligne.)<br /><br /> (Ce SQLSTATE est renvoyé uniquement lorsque **SQLSetPos** est appelé après **SQLExtendedFetch**, si le pilote est un pilote ODBC *2. x* et que la bibliothèque de curseurs n’est pas utilisée.)|  
|01S07|Troncation fractionnaire|L’argument *operation* était SQL_REFRESH, le type de données de la mémoire tampon d’application n’était pas SQL_C_CHAR ou SQL_C_BINARY, et les données retournées aux mémoires tampons d’application pour une ou plusieurs colonnes ont été tronquées. Pour les types de données numériques, la partie fractionnaire du nombre a été tronquée. Pour les types de données time, timestamp et Interval contenant un composant heure, la partie fractionnaire de l’heure a été tronquée.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07006|Violation d’attribut de type de données restreint|La valeur des données d’une colonne du jeu de résultats n’a pas pu être convertie dans le type de données spécifié par *TargetType* dans l’appel à **SQLBindCol**.|  
|07009|Index de descripteur non valide|L' *opération* d’argument a été SQL_REFRESH ou SQL_UPDATE, et une colonne a été liée avec un numéro de colonne supérieur au nombre de colonnes dans le jeu de résultats.|  
|21S02|Le degré de la table dérivée ne correspond pas à la liste des colonnes|L' *opération* d’argument a été SQL_UPDATE et aucune colonne n’a pu être mise à jour parce que toutes les colonnes étaient indépendantes, en lecture seule ou que la valeur de la mémoire tampon de longueur/indicateur liée était SQL_COLUMN_IGNORE.|  
|22001|Chaîne de données, troncation à droite|L’argument *operation* a été SQL_UPDATE, et l’assignation d’une valeur de type caractère ou binaire à une colonne a entraîné la troncation de caractères non vides (pour les caractères) ou non null (pour les caractères binaires) ou d’octets.|  
|22003|Valeur numérique hors limites|L' *opération* d’argument a été SQL_UPDATE, et l’assignation d’une valeur numérique à une colonne du jeu de résultats a entraîné la troncation de la partie entière (par opposition à la fraction) du nombre à tronquer.<br /><br /> L' *opération* d’argument a été SQL_REFRESH et le retour de la valeur numérique pour une ou plusieurs colonnes liées aurait provoqué une perte de chiffres significatifs.|  
|22007|Format de date/heure non valide|L' *opération* d’argument a été SQL_UPDATE, et l’affectation d’une valeur de date ou d’horodateur à une colonne du jeu de résultats a provoqué l’arrêt de la plage du champ année, mois ou jour.<br /><br /> L' *opération* d’argument a été SQL_REFRESH, et le renvoi de la valeur de date ou d’horodatage pour une ou plusieurs colonnes liées aurait provoqué l’arrêt de la plage du champ année, mois ou jour.|  
|22008|Dépassement du champ de date/heure|L’argument *operation* a été SQL_UPDATE, et les performances de l’arithmétique DateTime sur les données envoyées à une colonne dans le jeu de résultats ont généré un champ DateTime (année, mois, jour, heure, minute ou second champ) du résultat en dehors de la plage de valeurs autorisées pour le champ, ou non valide en fonction des règles naturelles du calendrier grégorien pour les valeurs DateTime.<br /><br /> L’argument *operation* a été SQL_REFRESH, et les performances de l’arithmétique DateTime sur les données récupérées à partir du jeu de résultats ont généré un champ DateTime (année, mois, jour, heure, minute ou second champ) du résultat en dehors de la plage autorisée de valeurs pour le champ ou non valide en fonction des règles naturelles du calendrier grégorien pour les valeurs DateTime.|  
|22015|Dépassement du champ d’intervalle|L’argument *operation* était SQL_UPDATE, et l’affectation d’un type exact Numeric ou Interval C à un type de données SQL Interval entraînait une perte de chiffres significatifs.<br /><br /> L’argument *operation* était SQL_UPDATE ; lors de l’affectation à un type SQL Interval, il n’existait aucune représentation de la valeur du type C dans le type SQL Interval.<br /><br /> L’argument *operation* a été SQL_REFRESH, et l’assignation d’un type SQL exact numérique ou Interval à un type d’intervalle C a provoqué une perte de chiffres significatifs dans le champ de début.<br /><br /> L’argument *operation* a été SQL_ Refresh ; lors de l’assignation à un type C Interval, il n’existait aucune représentation de la valeur du type SQL dans le type d’intervalle C.|  
|22018|Valeur de caractère non valide pour la spécification de cast|L’argument *operation* était SQL_REFRESH ; le type C était un type de données numérique exact ou approximatif, DateTime ou Interval ; le type SQL de la colonne était un type de données caractère ; et la valeur dans la colonne n’est pas un littéral valide du type C lié.<br /><br /> L' *opération* d’argument a été SQL_UPDATE ; le type SQL était un type de données numérique exact ou approximatif, DateTime ou Interval ; le type C a été SQL_C_CHAR ; et la valeur dans la colonne n’est pas un littéral valide du type SQL lié.|  
|23000|Violation de contrainte d’intégrité|L' *opération* d’argument a été SQL_DELETE ou SQL_UPDATE, et une contrainte d’intégrité n’a pas été respectée.|  
|24 000|État de curseur non valide|Le *StatementHandle* était dans un état d’exécution, mais aucun jeu de résultats n’a été associé à *StatementHandle*.<br /><br /> (DM) un curseur était ouvert sur le *StatementHandle*, mais **SQLFetch** ou **SQLFetchScroll** n’avait pas été appelé.<br /><br /> Un curseur a été ouvert sur *StatementHandle*, et **SQLFetch** ou **SQLFetchScroll** a été appelé, mais le curseur a été placé avant le début du jeu de résultats ou après la fin du jeu de résultats.<br /><br /> L' *opération* d’argument était SQL_DELETE, SQL_REFRESH ou SQL_UPDATE, et le curseur était positionné avant le début du jeu de résultats ou après la fin du jeu de résultats.|  
|40001|Échec de la sérialisation|La transaction a été restaurée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|42000|Erreur de syntaxe ou violation d’accès|Le pilote n’a pas pu verrouiller la ligne si nécessaire pour effectuer l’opération demandée dans l' *opération* d’argument.<br /><br /> Le pilote n’a pas pu verrouiller la ligne comme demandé dans l’argument *LockType*.|  
|44000|Violation de WITH CHECK OPTION|L’argument *operation* a été SQL_UPDATE, et la mise à jour a été effectuée sur une table affichée ou une table dérivée de la table affichée qui a été créée en spécifiant **with Check option**, de telle sorte qu’une ou plusieurs lignes affectées par la mise à jour ne seront plus présentes dans la table affichée.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*, puis la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction SQLSetPos.<br /><br /> (DM) le *StatementHandle* spécifié n’était pas dans un état d’exécution. La fonction a été appelée sans appeler d’abord **SQLExecDirect**, **SQLExecute** ou une fonction de catalogue.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.<br /><br /> (DM) le pilote était un pilote ODBC *2. x* et **SQLSetPos** a été appelé pour un *StatementHandle* après l’appel de **SQLFetch** .|  
|HY011|L’attribut ne peut pas être défini maintenant|(DM) le pilote était un pilote ODBC *2. x* ; l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR a été défini ; Ensuite, **SQLSetPos** a été appelé avant l’appel de **SQLFetch**, **SQLFetchScroll** ou **SQLExtendedFetch** .|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|L’argument *operation* était SQL_UPDATE, une valeur de données était un pointeur null et la valeur de longueur de colonne était différente de 0, SQL_DATA_AT_EXEC, SQL_COLUMN_IGNORE, SQL_NULL_DATA, ou inférieur ou égal à SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> L’argument *operation* était SQL_UPDATE ; une valeur de données n’était pas un pointeur null ; le type de données C était SQL_C_BINARY ou SQL_C_CHAR ; et la valeur de la longueur de colonne était inférieure à 0, mais n’est pas égale à SQL_DATA_AT_EXEC, SQL_COLUMN_IGNORE, SQL_NTS ou SQL_NULL_DATA, ou inférieure ou égale à SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> La valeur dans une mémoire tampon de longueur/d’indicateur était SQL_DATA_AT_EXEC ; le type SQL était SQL_LONGVARCHAR, SQL_LONGVARBINARY ou un type de données long spécifique à la source de données ; et le type d’information SQL_NEED_LONG_DATA_LEN dans **SQLGetInfo** était « Y ».|  
|HY092|Identificateur d’attribut non valide|(DM) la valeur spécifiée pour l’argument d' *opération* n’est pas valide.<br /><br /> (DM) la valeur spécifiée pour l’argument *LockType* n’était pas valide.<br /><br /> L’argument *operation* était SQL_UPDATE ou SQL_DELETE, et l’attribut d’instruction SQL_ATTR_CONCURRENCY était SQL_ATTR_CONCUR_READ_ONLY.|  
|HY107|Valeur de ligne hors limites|La valeur spécifiée pour l’argument *RowNumber* était supérieure au nombre de lignes dans l’ensemble de lignes.|  
|HY109|Position de curseur non valide|Le curseur associé à *StatementHandle* a été défini comme étant avant uniquement, donc le curseur n’a pas pu être positionné au sein de l’ensemble de lignes. Consultez la description de l’attribut SQL_ATTR_CURSOR_TYPE dans **SQLSetStmtAttr**.<br /><br /> L’argument *operation* était SQL_UPDATE, SQL_DELETE ou SQL_REFRESH, et la ligne identifiée par l’argument *RowNumber* avait été supprimée ou n’a pas été extraite.<br /><br /> (DM) l’argument *RowNumber* était 0 et l’argument *operation* était SQL_POSITION.<br /><br /> **SQLSetPos** a été appelé après l’appel de **SQLBulkOperations** et avant l’appel de **SQLFetchScroll** ou **SQLFetch** .|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Le pilote ou la source de données ne prend pas en charge l’opération demandée dans l’argument *operation* ou l’argument *LockType* .|  
|HYT00|Délai expiré|Le délai d’expiration de la requête a expiré avant que la source de données n’ait retourné le jeu de résultats. Le délai d’attente est défini à l’aide de **SQLSetStmtAttr** avec l' *attribut* SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
  
> [!CAUTION]
>  Pour plus d’informations sur l’instruction stipulant que **SQLSetPos** peut être appelé dans et ce qu’il doit faire pour assurer la compatibilité avec les applications ODBC *2. x* , consultez [curseurs de bloc, curseurs avec défilement et compatibilité descendante](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md).  
  
## <a name="rownumber-argument"></a>RowNumber (argument)  
 L’argument *RowNumber* spécifie le numéro de la ligne dans l’ensemble de lignes sur lequel effectuer l’opération spécifiée par l’argument *operation* . Si *RowNumber* a la valeur 0, l’opération s’applique à chaque ligne de l’ensemble de lignes. *RowNumber* doit être une valeur comprise entre 0 et le nombre de lignes dans l’ensemble de lignes.  
  
> [!NOTE]  
>  Dans le langage C, les tableaux sont de base 0 et l’argument *RowNumber* est de base 1. Par exemple, pour mettre à jour la cinquième ligne de l’ensemble de lignes, une application modifie les tampons de l’ensemble de lignes au niveau de l’index de tableau 4, mais spécifie un *RowNumber* de 5.  
  
 Toutes les opérations déplacent le curseur sur la ligne spécifiée par *RowNumber*. Les opérations suivantes requièrent une position de curseur :  
  
-   Instructions Update et DELETE positionnées.  
  
-   Appels à **SQLGetData**.  
  
-   Appels à **SQLSetPos** avec les options SQL_DELETE, SQL_REFRESH et SQL_UPDATE.  
  
 Par exemple, si *RowNumber* est 2 pour un appel à **SQLSetPos** avec une *opération* de SQL_DELETE, le curseur est positionné sur la deuxième ligne de l’ensemble de lignes et cette ligne est supprimée. L’entrée dans le tableau d’état de la ligne d’implémentation (vers laquelle pointe l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR) pour la deuxième ligne est remplacée par SQL_ROW_DELETED.  
  
 Une application peut spécifier une position de curseur quand elle appelle **SQLSetPos**. En règle générale, il appelle **SQLSetPos** avec l’opération SQL_POSITION ou SQL_REFRESH pour positionner le curseur avant d’exécuter une instruction UPDATE ou DELETE positionnée ou d’appeler **SQLGetData**.  
  
## <a name="operation-argument"></a>Argument de l’opération  
 L’argument *operation* prend en charge les opérations suivantes. Pour déterminer les options prises en charge par une source de données, une application appelle **SQLGetInfo** avec le type d’informations SQL_DYNAMIC_CURSOR_ATTRIBUTES1, SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES1, SQL_KEYSET_CURSOR_ATTRIBUTES1 ou SQL_STATIC_CURSOR_ATTRIBUTES1 (selon le type du curseur).  
  
|*opération*<br /><br /> argument|Opération|  
|------------------------------|---------------|  
|SQL_POSITION|Le pilote positionne le curseur sur la ligne spécifiée par *RowNumber*.<br /><br /> Le contenu du tableau d’état de ligne vers lequel pointe l’attribut d’instruction SQL_ATTR_ROW_OPERATION_PTR est ignoré pour l' *opération* de SQL_POSITION.|  
|SQL_REFRESH|Le pilote positionne le curseur sur la ligne spécifiée par *RowNumber* et actualise les données dans les mémoires tampons de l’ensemble de lignes pour cette ligne. Pour plus d’informations sur la façon dont le pilote retourne des données dans les mémoires tampons d’ensemble de lignes, consultez les descriptions de la liaison de lignes et de colonnes dans **SQLBindCol**.<br /><br /> **SQLSetPos** avec une *opération* de SQL_REFRESH met à jour l’État et le contenu des lignes au sein de l’ensemble de lignes extrait actuel. Cela comprend l’actualisation des signets. Dans la mesure où les données des mémoires tampons sont actualisées mais ne sont pas récupérées, l’appartenance à l’ensemble de lignes est fixe. Cela diffère de l’actualisation effectuée par un appel à **SQLFetchScroll** avec un *FetchOrientation* de SQL_FETCH_RELATIVE et d’un *RowNumber* égal à 0, qui réextrait l’ensemble de lignes du jeu de résultats afin qu’il puisse afficher des données ajoutées et supprimer les données supprimées si ces opérations sont prises en charge par le pilote et le curseur.<br /><br /> Une actualisation réussie avec **SQLSetPos** ne modifie pas l’état de ligne SQL_ROW_DELETED. Les lignes supprimées dans l’ensemble de lignes continueront d’être marquées comme supprimées jusqu’à la prochaine extraction. Les lignes disparaissent à la prochaine extraction si le curseur prend en charge la compression (dans laquelle une instruction **SQLFetch** ou **SQLFetchScroll** suivante ne retourne pas les lignes supprimées).<br /><br /> Les lignes ajoutées n’apparaissent pas lorsqu’une actualisation avec **SQLSetPos** est effectuée. Ce comportement est différent de **SQLFetchScroll** avec un *FetchType* de SQL_FETCH_RELATIVE et d’un *RowNumber* égal à 0, qui actualise également l’ensemble de lignes actuel mais affiche des enregistrements ajoutés ou des enregistrements à en-tête pack supprimés si ces opérations sont prises en charge par le curseur.<br /><br /> Une actualisation réussie avec **SQLSetPos** modifie l’état de ligne SQL_ROW_ADDED en SQL_ROW_SUCCESS (si le tableau d’état de ligne existe).<br /><br /> Une actualisation réussie avec **SQLSetPos** modifie l’état de la ligne SQL_ROW_UPDATED en fonction du nouvel état de la ligne (si le tableau d’état de la ligne existe).<br /><br /> Si une erreur se produit dans une opération **SQLSetPos** sur une ligne, l’état de la ligne est défini sur SQL_ROW_ERROR (si le tableau d’état de ligne existe).<br /><br /> Pour un curseur ouvert avec un SQL_ATTR_CONCURRENCY attribut d’instruction de SQL_CONCUR_ROWVER ou SQL_CONCUR_VALUES, une actualisation avec **SQLSetPos** peut mettre à jour les valeurs d’accès concurrentiel optimiste utilisées par la source de données pour détecter la modification de la ligne. Si cela se produit, les versions de ligne ou les valeurs utilisées pour garantir l’accès concurrentiel au curseur sont mises à jour chaque fois que les tampons d’ensemble de lignes sont actualisés à partir du serveur. Cela se produit pour chaque ligne actualisée.<br /><br /> Le contenu du tableau d’état de ligne vers lequel pointe l’attribut d’instruction SQL_ATTR_ROW_OPERATION_PTR est ignoré pour l' *opération* de SQL_REFRESH.|  
|SQL_UPDATE|Le pilote positionne le curseur sur la ligne spécifiée par *RowNumber* et met à jour la ligne de données sous-jacente avec les valeurs des mémoires tampons de l’ensemble de lignes (argument *TargetValuePtr* dans **SQLBindCol**). Elle récupère les longueurs des données à partir des mémoires tampons de longueur/d’indicateur (l’argument *StrLen_or_IndPtr* dans **SQLBindCol**). Si la longueur d’une colonne est SQL_COLUMN_IGNORE, la colonne n’est pas mise à jour. Après la mise à jour de la ligne, le pilote remplace l’élément correspondant du tableau d’état de ligne par SQL_ROW_UPDATED ou SQL_ROW_SUCCESS_WITH_INFO (si le tableau d’état de ligne existe).<br /><br /> Ce comportement est défini par le pilote si **SQLSetPos** avec un argument *operation* de SQL_UPDATE est appelé sur un curseur qui contient des colonnes dupliquées. Le pilote peut retourner une valeur SQLSTATE définie par le pilote, peut mettre à jour la première colonne qui apparaît dans le jeu de résultats, ou effectuer un autre comportement défini par le pilote.<br /><br /> Le tableau d’opérations de ligne vers lequel pointe l’attribut d’instruction SQL_ATTR_ROW_OPERATION_PTR peut être utilisé pour indiquer qu’une ligne de l’ensemble de lignes actif doit être ignorée lors d’une mise à jour en bloc. Pour plus d’informations, consultez « États et tableaux d’opérations » plus loin dans cette référence de fonction.|  
|SQL_DELETE|Le pilote positionne le curseur sur la ligne spécifiée par *RowNumber* et supprime la ligne de données sous-jacente. Il remplace l’élément correspondant du tableau d’état de ligne par SQL_ROW_DELETED. Une fois que la ligne a été supprimée, les éléments suivants ne sont pas valides pour la ligne : les instructions Update et DELETE positionnées, les appels à **SQLGetData** et les appels à **SQLSetPos** avec *operation* défini sur n’importe quelle valeur, à l’exception de SQL_POSITION. Pour les pilotes qui prennent en charge la compression, la ligne est supprimée du curseur quand de nouvelles données sont récupérées à partir de la source de données.<br /><br /> Le fait que la ligne reste visible dépend du type de curseur. Par exemple, les lignes supprimées sont visibles par les curseurs statiques et pilotés par keyset, mais invisibles aux curseurs dynamiques.<br /><br /> Le tableau d’opérations de ligne vers lequel pointe l’attribut d’instruction SQL_ATTR_ROW_OPERATION_PTR peut être utilisé pour indiquer qu’une ligne de l’ensemble de lignes actif doit être ignorée lors d’une suppression en bloc. Pour plus d’informations, consultez « États et tableaux d’opérations » plus loin dans cette référence de fonction.|  
  
## <a name="locktype-argument"></a>LockType (argument)  
 L’argument *LockType* permet aux applications de contrôler l’accès concurrentiel. Dans la plupart des cas, les sources de données qui prennent en charge les niveaux de concurrence et les transactions ne prennent en charge que la valeur SQL_LOCK_NO_CHANGE de l’argument *LockType* . L’argument *LockType* est généralement utilisé uniquement pour la prise en charge basée sur les fichiers.  
  
 L’argument *LockType* spécifie l’état de verrouillage de la ligne après l’exécution de **SQLSetPos** . Si le pilote ne peut pas verrouiller la ligne pour effectuer l’opération demandée ou pour satisfaire l’argument *LockType* , elle retourne SQL_ERROR et SQLState 42000 (erreur de syntaxe ou violation d’accès).  
  
 Bien que l’argument *LockType* soit spécifié pour une instruction unique, le verrou accorde les mêmes privilèges à toutes les instructions de la connexion. En particulier, un verrou acquis par une instruction sur une connexion peut être déverrouillé par une instruction différente sur la même connexion.  
  
 Une ligne verrouillée via **SQLSetPos** reste verrouillée jusqu’à ce que l’application appelle **SQLSetPos** pour la ligne avec *LockType* défini sur SQL_LOCK_UNLOCK, ou jusqu’à ce que l’application appelle **SQLFreeHandle** pour l’instruction ou **SQLFreeStmt** avec l’option SQL_CLOSE. Pour un pilote qui prend en charge les transactions, une ligne verrouillée via **SQLSetPos** est déverrouillée quand l’application appelle **SQLEndTran** pour valider ou restaurer une transaction sur la connexion (si un curseur est fermé lorsqu’une transaction est validée ou restaurée, comme indiqué par le SQL_CURSOR_COMMIT_BEHAVIOR et SQL_CURSOR_ROLLBACK_BEHAVIOR types d’informations retournés par **SQLGetInfo**).  
  
 L’argument *LockType* prend en charge les types de verrous suivants. Pour déterminer les verrous pris en charge par une source de données, une application appelle **SQLGetInfo** avec le type d’informations SQL_DYNAMIC_CURSOR_ATTRIBUTES1, SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES1, SQL_KEYSET_CURSOR_ATTRIBUTES1 ou SQL_STATIC_CURSOR_ATTRIBUTES1 (selon le type du curseur).  
  
|*LockType* (argument)|Type de verrou|  
|-------------------------|---------------|  
|SQL_LOCK_NO_CHANGE|Le pilote ou la source de données garantit que la ligne est dans le même état verrouillé ou déverrouillé qu’avant l’appel de **SQLSetPos** . Cette valeur de *LockType* permet aux sources de données qui ne prennent pas en charge le verrouillage explicite au niveau des lignes d’utiliser le verrouillage requis par les niveaux d’isolation des transactions et de la concurrence en cours.|  
|SQL_LOCK_EXCLUSIVE|Le pilote ou la source de données verrouille la ligne en mode exclusif. Une instruction sur une connexion différente ou dans une autre application ne peut pas être utilisée pour acquérir des verrous sur la ligne.|  
|SQL_LOCK_UNLOCK|Le pilote ou la source de données déverrouille la ligne.|  
  
 Si un pilote prend en charge SQL_LOCK_EXCLUSIVE mais ne prend pas en charge SQL_LOCK_UNLOCK, une ligne verrouillée restera verrouillée jusqu’à ce que l’un des appels de fonction décrits dans le paragraphe précédent se produise.  
  
 Si un pilote prend en charge SQL_LOCK_EXCLUSIVE mais ne prend pas en charge SQL_LOCK_UNLOCK, une ligne verrouillée restera verrouillée jusqu’à ce que l’application appelle **SQLFreeHandle** pour l’instruction ou **SQLFreeStmt** avec l’option SQL_CLOSE. Si le pilote prend en charge les transactions et ferme le curseur lors de la validation ou de la restauration de la transaction, l’application appelle **SQLEndTran**.  
  
 Pour les opérations de mise à jour et de suppression dans **SQLSetPos**, l’application utilise l’argument *LockType* comme suit :  
  
-   Pour garantir qu’une ligne ne change pas après son extraction, une application appelle **SQLSetPos** avec l' *opération* définie sur SQL_REFRESH et *LockType* défini sur SQL_LOCK_EXCLUSIVE.  
  
-   Si l’application définit *LockType* à SQL_LOCK_NO_CHANGE, le pilote garantit qu’une opération de mise à jour ou de suppression échouera uniquement si l’application spécifiée SQL_CONCUR_LOCK pour l’attribut d’instruction SQL_ATTR_CONCURRENCY.  
  
-   Si l’application spécifie SQL_CONCUR_ROWVER ou SQL_CONCUR_VALUES pour l’attribut d’instruction SQL_ATTR_CONCURRENCY, le pilote compare les versions de ligne ou les valeurs et rejette l’opération si la ligne a changé depuis que l’application a extrait la ligne.  
  
-   Si l’application spécifie SQL_CONCUR_READ_ONLY pour l’attribut d’instruction SQL_ATTR_CONCURRENCY, le pilote rejette toute opération de mise à jour ou de suppression.  
  
 Pour plus d’informations sur l’attribut d’instruction SQL_ATTR_CONCURRENCY, consultez [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md).  
  
## <a name="status-and-operation-arrays"></a>Tableaux des États et des opérations  
 Les tableaux d’État et d’opération suivants sont utilisés lors de l’appel de **SQLSetPos**:  
  
-   Le tableau d’état de ligne (désigné par le champ SQL_DESC_ARRAY_STATUS_PTR dans IRD et l’attribut d’instruction SQL_ATTR_ROW_STATUS_ARRAY) contient des valeurs d’État pour chaque ligne de données de l’ensemble de lignes. Le pilote définit les valeurs d’État dans ce tableau après un appel à **SQLFetch**, **SQLFetchScroll**, **SQLBulkOperations** ou **SQLSetPos**. Ce tableau est désigné par l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR.  
  
-   Le tableau d’opérations de ligne (comme pointé par le champ SQL_DESC_ARRAY_STATUS_PTR dans ARD et l’attribut d’instruction SQL_ATTR_ROW_OPERATION_ARRAY) contient une valeur pour chaque ligne de l’ensemble de lignes qui indique si un appel à **SQLSetPos** pour une opération en bloc est ignoré ou effectué. Chaque élément du tableau a la valeur SQL_ROW_PROCEED (valeur par défaut) ou SQL_ROW_IGNORE. Ce tableau est désigné par l’attribut d’instruction SQL_ATTR_ROW_OPERATION_PTR.  
  
 Le nombre d’éléments dans les tableaux d’État et d’opération doit être égal au nombre de lignes dans l’ensemble de lignes (tel que défini par l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE).  
  
 Pour plus d’informations sur le tableau d’état de ligne, consultez [SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md). Pour plus d’informations sur le tableau d’opérations de ligne, consultez « ignorer une ligne dans une opération en bloc », plus loin dans cette section.  
  
## <a name="using-sqlsetpos"></a>Utilisation de SQLSetPos  
 Avant qu’une application appelle **SQLSetPos**, elle doit exécuter la séquence d’étapes suivante :  
  
1.  Si l’application appelle **SQLSetPos** avec l' *opération* définie sur SQL_UPDATE, appelez **SQLBindCol** (ou **SQLSetDescRec**) pour chaque colonne pour spécifier son type de données et les tampons de liaison pour les données et la longueur de la colonne.  
  
2.  Si l’application appelle **SQLSetPos** avec l' *opération* définie sur SQL_DELETE ou SQL_UPDATE, appelez **SQLColAttribute** pour vous assurer que les colonnes à supprimer ou à mettre à jour peuvent être mises à jour.  
  
3.  Appelez **SQLExecDirect**, **SQLExecute** ou une fonction de catalogue pour créer un jeu de résultats.  
  
4.  Appelez **SQLFetch** ou **SQLFetchScroll** pour récupérer les données.  
  
 Pour plus d’informations sur l’utilisation de **SQLSetPos**, consultez [mise à jour des données avec SQLSetPos](../../../odbc/reference/develop-app/updating-data-with-sqlsetpos.md).  
  
## <a name="deleting-data-using-sqlsetpos"></a>Suppression de données à l’aide de SQLSetPos  
 Pour supprimer des données avec **SQLSetPos**, une application appelle **SQLSetPos** avec *RowNumber* défini sur le numéro de la ligne à supprimer et *operation* a la valeur SQL_DELETE.  
  
 Une fois les données supprimées, le pilote modifie la valeur dans le tableau d’état de la ligne d’implémentation pour la ligne appropriée en SQL_ROW_DELETED (ou SQL_ROW_ERROR).  
  
## <a name="updating-data-using-sqlsetpos"></a>Mise à jour de données à l’aide de SQLSetPos  
 Une application peut passer la valeur d’une colonne dans la mémoire tampon de données liée ou avec un ou plusieurs appels à **SQLPutData**. Les colonnes dont les données sont transmises avec **SQLPutData** sont appelées *colonnes* de *données en* cours d’exécution. Celles-ci sont couramment utilisées pour envoyer des données pour les colonnes SQL_LONGVARBINARY et SQL_LONGVARCHAR et peuvent être mélangées à d’autres colonnes.  
  
#### <a name="to-update-data-with-sqlsetpos-an-application"></a>Pour mettre à jour des données avec SQLSetPos, une application :  
  
1.  Place des valeurs dans les tampons de données et de longueur/indicateur liés par **SQLBindCol**:  
  
    -   Pour les colonnes normales, l’application place la nouvelle valeur de colonne dans la mémoire tampon *\* TargetValuePtr* et la longueur de cette valeur dans la mémoire tampon de *\* StrLen_or_IndPtr* . Si la ligne ne doit pas être mise à jour, l’application place SQL_ROW_IGNORE dans l’élément de cette ligne du tableau d’opérations de ligne.  
  
    -   Pour les colonnes de données en cours d’exécution, l’application place une valeur définie par l’application, telle que le numéro de colonne, dans la mémoire tampon *\* TargetValuePtr* . La valeur peut être utilisée ultérieurement pour identifier la colonne.  
  
         L’application place le résultat de la macro SQL_LEN_DATA_AT_EXEC (*longueur*) dans le tampon **StrLen_or_IndPtr* . Si le type de données SQL de la colonne est SQL_LONGVARBINARY, SQL_LONGVARCHAR ou un type de données long spécifique à la source de données et que le pilote retourne « Y » pour le type d’informations SQL_NEED_LONG_DATA_LEN dans **SQLGetInfo**, *Length* est le nombre d’octets de données à envoyer pour le paramètre. dans le cas contraire, il doit s’agir d’une valeur non négative et est ignorée.  
  
2.  Appelle **SQLSetPos** avec l’argument *Operation* défini sur SQL_UPDATE pour mettre à jour la ligne de données.  
  
    -   S’il n’existe aucune colonne de données en cours d’exécution, le processus est terminé.  
  
    -   S’il existe des colonnes de données en cours d’exécution, la fonction retourne SQL_NEED_DATA et passe à l’étape 3.  
  
3.  Appelle **SQLParamData** pour récupérer l’adresse de la mémoire tampon *\* TargetValuePtr* pour la première colonne de données en cours d’exécution à traiter. **SQLParamData** retourne SQL_NEED_DATA. L’application récupère la valeur définie par l’application à partir de la mémoire tampon *\* TargetValuePtr* .  
  
    > [!NOTE]  
    >  Bien que les paramètres de données en cours d’exécution soient similaires à ceux des colonnes de données en cours d’exécution, la valeur retournée par **SQLParamData** est différente pour chaque.  
  
    > [!NOTE]  
    >  Les paramètres de données en cours d’exécution sont des paramètres dans une instruction SQL pour laquelle des données sont envoyées avec **SQLPutData** lorsque l’instruction est exécutée avec **SQLExecDirect** ou **SQLExecute**. Ils sont liés à **SQLBindParameter** ou en définissant des descripteurs avec **SQLSetDescRec**. La valeur retournée par **SQLParamData** est une valeur 32 bits passée à **SQLBindParameter** dans l’argument *ParameterValuePtr* .  
  
    > [!NOTE]  
    >  Les colonnes de données en cours d’exécution sont des colonnes d’un ensemble de lignes pour lesquelles des données sont envoyées avec **SQLPutData** lorsqu’une ligne est mise à jour avec **SQLSetPos**. Elles sont liées à **SQLBindCol**. La valeur retournée par **SQLParamData** est l’adresse de la ligne dans la mémoire tampon **TargetValuePtr* en cours de traitement.  
  
4.  Appelle **SQLPutData** une ou plusieurs fois pour envoyer des données pour la colonne. Plusieurs appels sont nécessaires si toutes les valeurs de données ne peuvent pas être retournées dans la mémoire tampon *\* TargetValuePtr* spécifiée dans **SQLPutData**; plusieurs appels à **SQLPutData** pour la même colonne sont autorisés uniquement lors de l’envoi de données de caractères c à une colonne avec un type de données caractère, binaire ou spécifique à la source de données, ou lors de l’envoi de données binaires c à une colonne avec un  
  
5.  Appelle à nouveau **SQLParamData** pour signaler que toutes les données ont été envoyées pour la colonne.  
  
    -   S’il y a plus de colonnes de données en cours d’exécution, **SQLParamData** retourne SQL_NEED_DATA et l’adresse de la mémoire tampon *TargetValuePtr* pour la prochaine colonne de données en cours d’exécution à traiter. L’application répète les étapes 4 et 5.  
  
    -   S’il n’y a plus de colonnes de données en cours d’exécution, le processus est terminé. Si l’instruction a été exécutée avec succès, **SQLParamData** retourne SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO ; Si l’exécution a échoué, elle retourne SQL_ERROR. À ce stade, **SQLParamData** peut retourner n’importe quel SQLSTATE pouvant être retourné par **SQLSetPos**.  
  
 Si les données ont été mises à jour, le pilote modifie la valeur dans le tableau d’état de la ligne d’implémentation pour la ligne appropriée en SQL_ROW_UPDATED.  
  
 Si l’opération est annulée ou si une erreur se produit dans **SQLParamData** ou **SQLPutData**, lorsque **SQLSetPos** retourne SQL_NEED_DATA et avant l’envoi des données pour toutes les colonnes de données en cours d’exécution, l’application peut appeler uniquement **SQLCancel**, **SQLGetDiagField**, **SQLGetDiagRec**, **SQLGetFunctions**, **SQLParamData** ou **SQLPutData** pour l’instruction ou la connexion associée à l’instruction. Si elle appelle une autre fonction pour l’instruction ou la connexion associée à l’instruction, la fonction retourne SQL_ERROR et SQLSTATE HY010 (erreur de séquence de fonction).  
  
 Si l’application appelle **SQLCancel** alors que le pilote a encore besoin de données pour les colonnes de données en cours d’exécution, le pilote annule l’opération. L’application peut ensuite appeler **SQLSetPos** à nouveau ; l’annulation n’affecte pas l’état du curseur ou la position actuelle du curseur.  
  
 Lorsque la liste de sélection de la spécification de requête associée au curseur contient plusieurs références à la même colonne, qu’une erreur soit générée ou que le pilote ignore les références dupliquées et effectue les opérations demandées est défini par le pilote.  
  
## <a name="performing-bulk-operations"></a>Exécution d’opérations en bloc  
 Si l’argument *RowNumber* est égal à 0, le pilote effectue l’opération spécifiée dans l’argument *operation* pour chaque ligne de l’ensemble de lignes qui a la valeur SQL_ROW_PROCEED dans son champ du tableau d’opérations de ligne désigné par SQL_ATTR_ROW_OPERATION_PTR attribut d’instruction. Il s’agit d’une valeur valide de l’argument *RowNumber* pour un argument d' *opération* de SQL_DELETE, SQL_REFRESH ou SQL_UPDATE, mais pas SQL_POSITION. **SQLSetPos** avec une *opération* de SQL_POSITION et un *RowNumber* égal à 0 retourne SQLState HY109 (position de curseur non valide).  
  
 Si une erreur se produit dans l’ensemble de lignes, par exemple SQLSTATE HYT00 (délai d’attente expiré), le pilote retourne SQL_ERROR et le SQLSTATE approprié. Le contenu des mémoires tampons d’ensemble de lignes n’est pas défini et la position du curseur est inchangée.  
  
 Si une erreur se produit concernant une seule ligne, le pilote :  
  
-   Définit l’élément pour la ligne dans le tableau d’état de ligne désigné par l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR pour SQL_ROW_ERROR.  
  
-   Publie une ou plusieurs SQLSTATE supplémentaires pour l’erreur dans la file d’attente des erreurs et définit le champ SQL_DIAG_ROW_NUMBER dans la structure de données de diagnostic.  
  
 Après avoir traité l’erreur ou l’avertissement, si le pilote termine l’opération pour les lignes restantes de l’ensemble de lignes, il retourne SQL_SUCCESS_WITH_INFO. Ainsi, pour chaque ligne qui a retourné une erreur, la file d’attente d’erreurs contient zéro ou plusieurs SQLSTATE supplémentaires. Si le pilote arrête l’opération après avoir traité l’erreur ou l’avertissement, il retourne SQL_ERROR.  
  
 Si le pilote retourne des avertissements, tels que SQLSTATE 01004 (données tronquées), il retourne des avertissements qui s’appliquent à l’ensemble de lignes entier ou à des lignes inconnues dans l’ensemble de lignes avant de retourner les informations d’erreur qui s’appliquent à des lignes spécifiques. Elle retourne des avertissements pour des lignes spécifiques, ainsi que d’autres informations sur les erreurs relatives à ces lignes.  
  
 Si *NombLigne* est égal à 0 et que l' *opération* est SQL_UPDATE, SQL_REFRESH ou SQL_DELETE, le nombre de lignes sur lesquelles **SQLSetPos** s’exécute est désigné par l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR.  
  
 Si *NombLigne* est égal à 0 et que l' *opération* est SQL_DELETE, SQL_REFRESH ou SQL_UPDATE, la ligne actuelle après l’opération est la même que la ligne actuelle avant l’opération.  
  
## <a name="ignoring-a-row-in-a-bulk-operation"></a>Une ligne est ignorée dans une opération en bloc  
 Le tableau d’opérations de ligne peut être utilisé pour indiquer qu’une ligne de l’ensemble de lignes actif doit être ignorée pendant une opération en bloc à l’aide de **SQLSetPos**. Pour indiquer au pilote d’ignorer une ou plusieurs lignes lors d’une opération en bloc, une application doit effectuer les étapes suivantes :  
  
1.  Appelez **SQLSetStmtAttr** pour définir l’attribut d’instruction SQL_ATTR_ROW_OPERATION_PTR de façon à ce qu’il pointe vers un tableau de SQLUSMALLINTs. Ce champ peut également être défini en appelant **SQLSetDescField** pour définir le champ d’en-tête SQL_DESC_ARRAY_STATUS_PTR de l’ARD, ce qui nécessite qu’une application obtienne le handle de descripteur.  
  
2.  Affectez à chaque élément du tableau d’opérations de ligne l’une des deux valeurs suivantes :  
  
    -   SQL_ROW_IGNORE, pour indiquer que la ligne est exclue pour l’opération en bloc.  
  
    -   SQL_ROW_PROCEED, pour indiquer que la ligne est incluse dans l’opération en bloc. (Il s'agit de la valeur par défaut.)  
  
3.  Appelez **SQLSetPos** pour effectuer l’opération en bloc.  
  
 Les règles suivantes s’appliquent au tableau d’opérations de ligne :  
  
-   SQL_ROW_IGNORE et SQL_ROW_PROCEED affectent uniquement les opérations en bloc à l’aide de **SQLSetPos** avec une *opération* de SQL_DELETE ou SQL_UPDATE. Ils n’affectent pas les appels à **SQLSetPos** avec une *opération* de SQL_REFRESH ou SQL_POSITION.  
  
-   Par défaut, le pointeur a la valeur null.  
  
-   Si le pointeur a la valeur null, toutes les lignes sont mises à jour comme si tous les éléments avaient la valeur SQL_ROW_PROCEED.  
  
-   La définition d’un élément à SQL_ROW_PROCEED ne garantit pas que l’opération aura lieu sur cette ligne particulière. Par exemple, si une ligne de l’ensemble de lignes a l’État SQL_ROW_ERROR, le pilote peut ne pas être en mesure de mettre à jour cette ligne, que l’application ait spécifié ou non SQL_ROW_PROCEED. Une application doit toujours vérifier le tableau d’état des lignes pour déterminer si l’opération a réussi.  
  
-   SQL_ROW_PROCEED est défini comme 0 dans le fichier d’en-tête. Une application peut initialiser le tableau d’opérations de ligne à 0 pour traiter toutes les lignes.  
  
-   Si le numéro d’élément « n » dans le tableau d’opérations de ligne a la valeur SQL_ROW_IGNORE et que **SQLSetPos** est appelé pour effectuer une opération de suppression ou de mise à jour en bloc, la nième ligne de l’ensemble de lignes reste inchangée après l’appel de **SQLSetPos**.  
  
-   Une application doit automatiquement définir une colonne en lecture seule sur SQL_ROW_IGNORE.  
  
## <a name="ignoring-a-column-in-a-bulk-operation"></a>Ignorer une colonne dans une opération en bloc  
 Pour éviter les diagnostics de traitement inutiles générés par les tentatives de mise à jour vers une ou plusieurs colonnes en lecture seule, une application peut définir la valeur dans la mémoire tampon de longueur/indicateur liée sur SQL_COLUMN_IGNORE. Pour plus d’informations, consultez [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
## <a name="code-example"></a>Exemple de code  
 Dans l’exemple suivant, une application permet à un utilisateur de parcourir la table ORDERs et de mettre à jour l’état de la commande. Le curseur est piloté par jeu de clés avec une taille d’ensemble de lignes de 20 et utilise le contrôle d’accès concurrentiel optimiste comparant les versions de ligne. Une fois chaque ensemble de lignes extrait, l’application l’imprime et permet à l’utilisateur de sélectionner et de mettre à jour l’état d’une commande. L’application utilise **SQLSetPos** pour positionner le curseur sur la ligne sélectionnée et effectue une mise à jour positionnée de la ligne. (La gestion des erreurs est omise par souci de clarté.)  
  
```cpp  
#define ROWS 20  
#define STATUS_LEN 6  
  
SQLCHAR        szStatus[ROWS][STATUS_LEN], szReply[3];  
SQLINTEGER     cbStatus[ROWS], cbOrderID;  
SQLUSMALLINT   rgfRowStatus[ROWS];  
SQLUINTEGER    sOrderID, crow = ROWS, irow;  
SQLHSTMT       hstmtS, hstmtU;  
  
SQLSetStmtAttr(hstmtS, SQL_ATTR_CONCURRENCY, (SQLPOINTER) SQL_CONCUR_ROWVER, 0);  
SQLSetStmtAttr(hstmtS, SQL_ATTR_CURSOR_TYPE, (SQLPOINTER) SQL_CURSOR_KEYSET_DRIVEN, 0);  
SQLSetStmtAttr(hstmtS, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER) ROWS, 0);  
SQLSetStmtAttr(hstmtS, SQL_ATTR_ROW_STATUS_PTR, (SQLPOINTER) rgfRowStatus, 0);  
SQLSetCursorName(hstmtS, "C1", SQL_NTS);  
SQLExecDirect(hstmtS, "SELECT ORDERID, STATUS FROM ORDERS ", SQL_NTS);  
  
SQLBindCol(hstmtS, 1, SQL_C_ULONG, &sOrderID, 0, &cbOrderID);  
SQLBindCol(hstmtS, 2, SQL_C_CHAR, szStatus, STATUS_LEN, &cbStatus);  
  
while ((retcode == SQLFetchScroll(hstmtS, SQL_FETCH_NEXT, 0)) != SQL_ERROR) {  
   if (retcode == SQL_NO_DATA_FOUND)  
      break;  
   for (irow = 0; irow < crow; irow++) {  
      if (rgfRowStatus[irow] != SQL_ROW_DELETED)  
         printf("%2d %5d %*s\n", irow+1, sOrderID, NAME_LEN-1, szStatus[irow]);  
   }  
   while (TRUE) {  
      printf("\nRow number to update?");  
      gets_s(szReply, 3);  
      irow = atoi(szReply);  
      if (irow > 0 && irow <= crow) {  
         printf("\nNew status?");  
         gets_s(szStatus[irow-1], (ROWS * STATUS_LEN));  
         SQLSetPos(hstmtS, irow, SQL_POSITION, SQL_LOCK_NO_CHANGE);  
         SQLPrepare(hstmtU,  
          "UPDATE ORDERS SET STATUS=? WHERE CURRENT OF C1", SQL_NTS);  
         SQLBindParameter(hstmtU, 1, SQL_PARAM_INPUT,  
            SQL_C_CHAR, SQL_CHAR,  
            STATUS_LEN, 0, szStatus[irow], 0, NULL);  
         SQLExecute(hstmtU);  
      } else if (irow == 0) {  
         break;  
      }  
   }  
}  
```  
  
 Pour obtenir plus d’exemples, consultez [instructions Update et DELETE positionnées](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md) et [mise à jour de lignes dans l’ensemble de lignes avec SQLSetPos](../../../odbc/reference/develop-app/updating-rows-in-the-rowset-with-sqlsetpos.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Exécution d’opérations en bloc qui ne sont pas liées à la position du curseur de bloc|[SQLBulkOperations, fonction](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Obtention d’un seul champ d’un descripteur|[Fonction SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|Obtention de plusieurs champs d’un descripteur|[SQLGetDescRec, fonction](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Définition d’un champ unique d’un descripteur|[SQLSetDescField, fonction](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Définition de plusieurs champs d’un descripteur|[SQLSetDescRec, fonction](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
|Définition d’un attribut d’instruction|[Fonction SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
