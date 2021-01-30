---
description: SQLBulkOperations, fonction
title: SQLBulkOperations fonction) | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLBulkOperations
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBulkOperations
helpviewer_keywords:
- SQLBulkOperations function [ODBC]
ms.assetid: 7029d0da-b0f2-44e6-9114-50bd96f47196
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a95e7701b12f9673452962c005a7bac44701e2d7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181059"
---
# <a name="sqlbulkoperations-function"></a>SQLBulkOperations, fonction
**Conformité**  
 Version introduite : ODBC 3,0 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLBulkOperations** effectue des insertions en bloc et des opérations de signet en bloc, y compris la mise à jour, la suppression et l’extraction par signet.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLBulkOperations(  
     SQLHSTMT       StatementHandle,  
     SQLUSMALLINT   Operation);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *opération*  
 Entrée Opération à effectuer :  
  
 SQL_ADD SQL_UPDATE_BY_BOOKMARK SQL_DELETE_BY_BOOKMARK SQL_FETCH_BY_BOOKMARK  
  
 Pour plus d’informations, consultez la section « commentaires ».  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLBulkOperations** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLBulkOperations** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
 Pour tous les SQLSTATEs qui peuvent retourner des SQL_SUCCESS_WITH_INFO ou des SQL_ERROR (à l’exception de 01xxx SQLSTATEs), SQL_SUCCESS_WITH_INFO est retourné si une erreur se produit sur une ou plusieurs lignes, mais pas toutes, sur les lignes d’une opération multiligne, et SQL_ERROR est retournée si une erreur se produit sur une opération à une seule ligne.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Troncation à droite des données de chaîne|L’argument *operation* était SQL_FETCH_BY_BOOKMARK, et les données de chaîne ou binaires retournées pour une ou plusieurs colonnes avec un type de données SQL_C_CHAR ou SQL_C_BINARY ont entraîné la troncation d’un caractère non vide ou de données binaires non null.|  
|01S01|Erreur dans la ligne|L’argument *operation* était SQL_ADD, et une erreur s’est produite dans une ou plusieurs lignes lors de l’exécution de l’opération, mais au moins une ligne a été ajoutée avec succès. (La fonction retourne SQL_SUCCESS_WITH_INFO.)<br /><br /> (Cette erreur est déclenchée uniquement lorsqu’une application utilise ODBC 2. pilote *x* .)|  
|01S07|Troncation fractionnaire|L’argument *operation* était SQL_FETCH_BY_BOOKMARK, le type de données de la mémoire tampon d’application n’était pas SQL_C_CHAR ou SQL_C_BINARY, et les données retournées aux mémoires tampons d’application pour une ou plusieurs colonnes ont été tronquées. (Pour les types de données C numériques, la partie fractionnaire du nombre a été tronquée. Pour les types de données time, timestamp et Interval C qui contiennent un composant heure, la partie fractionnaire de l’heure a été tronquée.)<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07006|Violation d’attribut de type de données restreint|L’argument *operation* était SQL_FETCH_BY_BOOKMARK, et la valeur de données d’une colonne dans le jeu de résultats n’a pas pu être convertie dans le type de données spécifié par l’argument *TargetType* dans l’appel à **SQLBindCol**.<br /><br /> L’argument *operation* était SQL_UPDATE_BY_BOOKMARK ou SQL_ADD, et la valeur de données dans les mémoires tampons d’application n’a pas pu être convertie en type de données d’une colonne dans le jeu de résultats.|  
|07009|Index de descripteur non valide|L' *opération* d’argument a été SQL_ADD, et une colonne a été liée avec un numéro de colonne supérieur au nombre de colonnes dans le jeu de résultats.|  
|21S02|Le degré de la table dérivée ne correspond pas à la liste des colonnes|L' *opération* d’argument a été SQL_UPDATE_BY_BOOKMARK ; et aucune colonne ne peut être mise à jour parce que toutes les colonnes étaient indépendantes ou en lecture seule, ou bien la valeur dans la mémoire tampon d’indicateur/longueur liée était SQL_COLUMN_IGNORE.|  
|22001|Troncation à droite des données de chaîne|L’assignation d’une valeur de type caractère ou binaire à une colonne dans le jeu de résultats a entraîné la troncation de caractères non vides (pour les caractères) ou de caractères non null (pour les caractères binaires) ou d’octets.|  
|22003|Valeur numérique hors limites|L’argument *operation* était SQL_ADD ou SQL_UPDATE_BY_BOOKMARK, et l’assignation d’une valeur numérique à une colonne du jeu de résultats a entraîné la troncation de la partie entière (par opposition à la fraction) du nombre à tronquer.<br /><br /> L' *opération* d’argument a été SQL_FETCH_BY_BOOKMARK et le retour de la valeur numérique pour une ou plusieurs colonnes liées aurait provoqué une perte de chiffres significatifs.|  
|22007|Format de date/heure non valide|L’argument *operation* était SQL_ADD ou SQL_UPDATE_BY_BOOKMARK, et l’affectation d’une valeur de date ou d’horodateur à une colonne du jeu de résultats a provoqué l’arrêt de la plage du champ année, mois ou jour.<br /><br /> L' *opération* d’argument a été SQL_FETCH_BY_BOOKMARK, et le renvoi de la valeur de date ou d’horodatage pour une ou plusieurs colonnes liées aurait provoqué l’arrêt de la plage du champ année, mois ou jour.|  
|22008|Dépassement du champ de date/heure|L’argument *operation* était SQL_ADD ou SQL_UPDATE_BY_BOOKMARK, et les performances de l’arithmétique DateTime sur les données envoyées à une colonne dans le jeu de résultats ont généré un champ DateTime (l’année, le mois, le jour, l’heure, la minute ou le deuxième champ) du résultat en dehors de la plage autorisée de valeurs du champ ou non valide en fonction des règles naturelles du calendrier grégorien pour les valeurs DateTime<br /><br /> L’argument *operation* était SQL_FETCH_BY_BOOKMARK, et les performances de l’arithmétique DateTime sur les données récupérées à partir de l’ensemble de résultats entraînaient un champ DateTime (l’année, le mois, le jour, l’heure, la minute ou le deuxième champ) du résultat en dehors de la plage de valeurs autorisées pour le champ ou non valide en fonction des règles naturelles du calendrier grégorien pour les valeurs DateTime.|  
|22015|Dépassement du champ d’intervalle|L’argument *operation* était SQL_ADD ou SQL_UPDATE_BY_BOOKMARK, et l’affectation d’un type exact Numeric ou Interval C à un type de données SQL Interval entraînait une perte de chiffres significatifs.<br /><br /> L’argument *operation* était SQL_ADD ou SQL_UPDATE_BY_BOOKMARK ; lors de l’affectation à un type SQL Interval, il n’existait aucune représentation de la valeur du type C dans le type SQL Interval.<br /><br /> L’argument *operation* a été SQL_FETCH_BY_BOOKMARK, et l’assignation d’un type SQL exact numérique ou Interval à un type d’intervalle C a provoqué une perte de chiffres significatifs dans le champ de début.<br /><br /> L’argument *operation* était SQL_FETCH_BY_BOOKMARK ; lors de l’assignation à un type C Interval, il n’existait aucune représentation de la valeur du type SQL dans le type d’intervalle C.|  
|22018|Valeur de caractère non valide pour la spécification de cast|L’argument *operation* était SQL_FETCH_BY_BOOKMARK ; le type C était un type de données numérique exact ou approximatif, DateTime ou Interval ; le type SQL de la colonne était un type de données caractère ; et la valeur dans la colonne n’est pas un littéral valide du type C lié.<br /><br /> L' *opération* d’argument a été SQL_ADD ou SQL_UPDATE_BY_BOOKMARK ; le type SQL était un type de données numérique exact ou approximatif, DateTime ou Interval ; le type C a été SQL_C_CHAR ; et la valeur dans la colonne n’est pas un littéral valide du type SQL lié.|  
|23000|Violation de contrainte d’intégrité|L’argument *operation* était SQL_ADD, SQL_DELETE_BY_BOOKMARK ou SQL_UPDATE_BY_BOOKMARK, et une contrainte d’intégrité n’a pas été respectée.<br /><br /> L’argument *operation* était SQL_ADD, et une colonne qui n’était pas liée est définie comme NOT NULL et n’a pas de valeur par défaut.<br /><br /> L’argument *operation* était SQL_ADD, la longueur spécifiée dans la mémoire tampon de *StrLen_or_IndPtr* liée était SQL_COLUMN_IGNORE et la colonne n’avait pas de valeur par défaut.|  
|24 000|État de curseur non valide|Le *StatementHandle* était dans un état d’exécution, mais aucun jeu de résultats n’a été associé à *StatementHandle*.|  
|40001|Échec de la sérialisation|La transaction a été restaurée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|42000|Erreur de syntaxe ou violation d’accès|Le pilote n’a pas pu verrouiller la ligne si nécessaire pour effectuer l’opération demandée dans l’argument de l' *opération* .|  
|44000|Violation de WITH CHECK OPTION|L’argument *operation* était SQL_ADD ou SQL_UPDATE_BY_BOOKMARK, et l’insertion ou la mise à jour a été effectuée sur une table affichée (ou une table dérivée de la table affichée) qui a été créée en spécifiant **with Check option**, de telle sorte qu’une ou plusieurs lignes affectées par l’insertion ou la mise à jour ne seront plus présentes dans la table affichée.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*. Ensuite, la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLBulkOperations** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) le *StatementHandle* spécifié n’était pas dans un état d’exécution. La fonction a été appelée sans appeler d’abord **SQLExecDirect**, **SQLExecute** ou une fonction de catalogue.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLSetPos** a été appelé pour le *StatementHandle* et a retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.<br /><br /> (DM) le pilote était un ODBC 2. *x* , et **SQLBulkOperations** a été appelé pour un *StatementHandle* avant l’appel de **SQLFetchScroll** ou **SQLFetch** .<br /><br /> (DM) **SQLBulkOperations** a été appelé après l’appel de **SQLExtendedFetch** sur le *StatementHandle*.|  
|HY011|L’attribut ne peut pas être défini maintenant|(DM) le pilote était un ODBC 2. *x* , et l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR a été défini entre les appels à **SQLFetch** ou **SQLFetchScroll** et **SQLBulkOperations**.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|L’argument *operation* était SQL_ADD ou SQL_UPDATE_BY_BOOKMARK ; une valeur de données n’était pas un pointeur null ; le type de données C était SQL_C_BINARY ou SQL_C_CHAR ; et la valeur de longueur de colonne était inférieure à 0, mais n’est pas égale à SQL_DATA_AT_EXEC, SQL_COLUMN_IGNORE, SQL_NTS ou SQL_NULL_DATA, ou inférieure ou égale à SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> La valeur dans une mémoire tampon de longueur/d’indicateur était SQL_DATA_AT_EXEC ; le type SQL était SQL_LONGVARCHAR, SQL_LONGVARBINARY ou un type de données long spécifique à la source de données ; et le type d’information SQL_NEED_LONG_DATA_LEN dans **SQLGetInfo** était « Y ».<br /><br /> L’argument *operation* était SQL_ADD, l’attribut d’instruction SQL_ATTR_USE_BOOKMARK avait la valeur SQL_UB_VARIABLE et la colonne 0 était liée à une mémoire tampon dont la longueur n’était pas égale à la longueur maximale du signet pour ce jeu de résultats. (Cette longueur est disponible dans le champ SQL_DESC_OCTET_LENGTH de la IRD et peut être obtenue en appelant **SQLDescribeCol**, **SQLColAttribute** ou **SQLGetDescField**.)|  
|HY092|Identificateur d’attribut non valide|(DM) la valeur spécifiée pour l’argument d' *opération* n’est pas valide.<br /><br /> L’argument *operation* était SQL_ADD, SQL_UPDATE_BY_BOOKMARK ou SQL_DELETE_BY_BOOKMARK, et l’attribut d’instruction SQL_ATTR_CONCURRENCY était défini sur SQL_CONCUR_READ_ONLY.<br /><br /> L’argument *operation* était SQL_DELETE_BY_BOOKMARK, SQL_FETCH_BY_BOOKMARK ou SQL_UPDATE_BY_BOOKMARK, et la colonne Bookmark n’était pas liée ou l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS était défini sur SQL_UB_OFF.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Le pilote ou la source de données ne prend pas en charge l’opération demandée dans l’argument d' *opération* .|  
|HYT00|Délai expiré|Le délai d’expiration de la requête a expiré avant que la source de données n’ait retourné le jeu de résultats. Le délai d’attente est défini à l’aide de **SQLSetStmtAttr** avec un argument d' *attribut* de SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
  
> [!CAUTION]  
>  Pour plus d’informations sur l’instruction stipulant que **SQLBulkOperations** peut être appelé dans et ce qu’il doit faire pour la compatibilité avec ODBC 2. *x* , consultez la section [curseurs de bloc, curseurs avec défilement et compatibilité descendante](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) dans l’annexe G : instructions relatives aux pilotes pour la compatibilité descendante.  
  
 Une application utilise **SQLBulkOperations** pour effectuer les opérations suivantes sur la table ou la vue de base qui correspond à la requête actuelle :  
  
-   Ajoutez de nouvelles lignes.  
  
-   Met à jour un ensemble de lignes où chaque ligne est identifiée par un signet.  
  
-   Supprimer un ensemble de lignes où chaque ligne est identifiée par un signet.  
  
-   Extrait un ensemble de lignes où chaque ligne est identifiée par un signet.  
  
 Après un appel à **SQLBulkOperations**, la position de curseur de bloc n’est pas définie. L’application doit appeler **SQLFetchScroll** pour définir la position du curseur. Une application doit appeler **SQLFetchScroll** uniquement avec un argument *FetchOrientation* de SQL_FETCH_FIRST, SQL_FETCH_LAST, SQL_FETCH_ABSOLUTE ou SQL_FETCH_BOOKMARK. La position du curseur n’est pas définie si l’application appelle **SQLFetch** ou **SQLFetchScroll** avec un argument *FetchOrientation* de SQL_FETCH_PRIOR, SQL_FETCH_NEXT ou SQL_FETCH_RELATIVE.  
  
 Une colonne peut être ignorée dans les opérations en bloc effectuées par un appel à **SQLBulkOperations** en définissant la valeur de la mémoire tampon de l’indicateur/longueur de colonne spécifiée dans l’appel à **SQLBindCol**, sur SQL_COLUMN_IGNORE.  
  
 Il n’est pas nécessaire pour l’application de définir l’attribut d’instruction SQL_ATTR_ROW_OPERATION_PTR lorsqu’il appelle **SQLBulkOperations** , car les lignes ne peuvent pas être ignorées lors de l’exécution d’opérations en bloc avec cette fonction.  
  
 La mémoire tampon vers laquelle pointe l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR contient le nombre de lignes affectées par un appel à **SQLBulkOperations**.  
  
 Lorsque l’argument *operation* est SQL_ADD ou SQL_UPDATE_BY_BOOKMARK et que la liste SELECT de la spécification de requête associée au curseur contient plus d’une référence à la même colonne, elle est définie par le pilote, qu’une erreur soit générée ou que le pilote ignore les références dupliquées et effectue les opérations demandées.  
  
 Pour plus d’informations sur l’utilisation de **SQLBulkOperations**, consultez [mise à jour des données avec SQLBulkOperations](../../../odbc/reference/develop-app/updating-data-with-sqlbulkoperations.md).  
  
## <a name="performing-bulk-inserts"></a>Exécution d’insertions en bloc  
 Pour insérer des données avec **SQLBulkOperations**, une application exécute la séquence d’étapes suivante :  
  
1.  Exécute une requête qui retourne un jeu de résultats.  
  
2.  Définit l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE sur le nombre de lignes qu’il souhaite insérer.  
  
3.  Appelle **SQLBindCol** pour lier les données qu’il souhaite insérer. Les données sont liées à un tableau dont la taille est égale à la valeur de SQL_ATTR_ROW_ARRAY_SIZE.  
  
    > [!NOTE]  
    >  La taille du tableau vers lequel pointe l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR doit être égale à SQL_ATTR_ROW_ARRAY_SIZE ou SQL_ATTR_ROW_STATUS_PTR doit être un pointeur null.  
  
4.  Appelle **SQLBulkOperations**(*StatementHandle,* SQL_ADD) pour effectuer l’insertion.  
  
5.  Si l’application a défini l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR, elle peut inspecter ce tableau pour voir le résultat de l’opération.  
  
 Si une application lie la colonne 0 avant d’appeler **SQLBulkOperations** avec un argument *operation* de SQL_ADD, le pilote met à jour les mémoires tampons de la colonne liée 0 avec les valeurs de signet pour la ligne qui vient d’être insérée. Pour que cela se produise, l’application doit avoir défini l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS sur SQL_UB_VARIABLE avant d’exécuter l’instruction. (Cela ne fonctionne pas avec ODBC 2. pilote *x* .)  
  
 Les données longues peuvent être ajoutées dans des parties par SQLBulkOperations, en utilisant des appels à SQLParamData et SQLPutData. Pour plus d’informations, consultez « fourniture de données de type long pour les insertions et les mises à jour en bloc » plus loin dans cette référence de fonction.  
  
 Il n’est pas nécessaire que l’application appelle **SQLFetch** ou **SQLFetchScroll** avant d’appeler **SQLBulkOperations** (sauf en cas de passage à un ODBC 2.*pilote x* ; Voir [compatibilité descendante et conformité aux normes](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md)).  
  
 Le comportement est défini par le pilote si **SQLBulkOperations**, avec un argument *operation* de SQL_ADD, est appelé sur un curseur qui contient des colonnes dupliquées. Le pilote peut retourner une valeur SQLSTATE définie par le pilote, ajouter les données à la première colonne qui apparaît dans le jeu de résultats, ou effectuer d’autres comportements définis par le pilote.  
  
## <a name="performing-bulk-updates-by-using-bookmarks"></a>Réalisation de mises à jour en bloc à l’aide de signets  
 Pour effectuer des mises à jour en bloc à l’aide de signets avec **SQLBulkOperations**, une application effectue les étapes suivantes dans l’ordre :  
  
1.  Définit l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS sur SQL_UB_VARIABLE.  
  
2.  Exécute une requête qui retourne un jeu de résultats.  
  
3.  Définit l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE sur le nombre de lignes qu’il souhaite mettre à jour.  
  
4.  Appelle **SQLBindCol** pour lier les données qu’il souhaite mettre à jour. Les données sont liées à un tableau dont la taille est égale à la valeur de SQL_ATTR_ROW_ARRAY_SIZE. Il appelle également **SQLBindCol** pour lier la colonne 0 (la colonne de signets).  
  
5.  Copie les signets pour les lignes qu’il souhaite mettre à jour dans le tableau lié à la colonne 0.  
  
6.  Met à jour les données dans les mémoires tampons liées.  
  
    > [!NOTE]  
    >  La taille du tableau vers lequel pointe l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR doit être égale à SQL_ATTR_ROW_ARRAY_SIZE ou SQL_ATTR_ROW_STATUS_PTR doit être un pointeur null.  
  
7.  Appelle **SQLBulkOperations**(*StatementHandle,* SQL_UPDATE_BY_BOOKMARK).  
  
    > [!NOTE]  
    >  Si l’application a défini l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR, elle peut inspecter ce tableau pour voir le résultat de l’opération.  
  
8.  Appelle éventuellement **SQLBulkOperations**(*StatementHandle*, SQL_FETCH_BY_BOOKMARK) pour extraire des données dans les mémoires tampons d’application liées afin de vérifier que la mise à jour s’est produite.  
  
9. Si les données ont été mises à jour, le pilote modifie la valeur dans le tableau d’état des lignes pour que les lignes appropriées soient SQL_ROW_UPDATED.  
  
 Les mises à jour en bloc effectuées par **SQLBulkOperations** peuvent inclure des données de type long à l’aide d’appels à **SQLParamData** et **SQLPutData**. Pour plus d’informations, consultez « fourniture de données de type long pour les insertions et les mises à jour en bloc » plus loin dans cette référence de fonction.  
  
 Si les signets sont conservés sur les curseurs, l’application n’a pas besoin d’appeler **SQLFetch** ou **SQLFetchScroll** avant d’effectuer la mise à jour par signet. Il peut utiliser des signets qu’il a stocké à partir d’un curseur précédent. Si les signets ne sont pas conservés sur les curseurs, l’application doit appeler **SQLFetch** ou **SQLFetchScroll** pour récupérer les signets.  
  
 Le comportement est défini par le pilote si **SQLBulkOperations**, avec un argument *operation* de SQL_UPDATE_BY_BOOKMARK, est appelé sur un curseur qui contient des colonnes dupliquées. Le pilote peut retourner une valeur SQLSTATE définie par le pilote, mettre à jour la première colonne qui apparaît dans le jeu de résultats, ou effectuer un autre comportement défini par le pilote.  
  
## <a name="performing-bulk-fetches-using-bookmarks"></a>Exécution d’extractions en bloc à l’aide de signets  
 Pour effectuer des extractions en bloc à l’aide de signets avec **SQLBulkOperations**, une application effectue les étapes suivantes dans l’ordre :  
  
1.  Définit l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS sur SQL_UB_VARIABLE.  
  
2.  Exécute une requête qui retourne un jeu de résultats.  
  
3.  Définit l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE sur le nombre de lignes qu’il souhaite extraire.  
  
4.  Appelle **SQLBindCol** pour lier les données qu’il souhaite extraire. Les données sont liées à un tableau dont la taille est égale à la valeur de SQL_ATTR_ROW_ARRAY_SIZE. Il appelle également **SQLBindCol** pour lier la colonne 0 (la colonne de signets).  
  
5.  Copie les signets pour les lignes qui intéressent l’extraction dans le tableau lié à la colonne 0. (Cela suppose que l’application a déjà obtenu les signets séparément.)  
  
    > [!NOTE]  
    >  La taille du tableau vers lequel pointe l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR doit être égale à SQL_ATTR_ROW_ARRAY_SIZE ou SQL_ATTR_ROW_STATUS_PTR doit être un pointeur null.  
  
6.  Appelle **SQLBulkOperations**(*StatementHandle,* SQL_FETCH_BY_BOOKMARK).  
  
7.  Si l’application a défini l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR, elle peut inspecter ce tableau pour voir le résultat de l’opération.  
  
 Si les signets sont rendus persistants sur les curseurs, l’application n’a pas besoin d’appeler **SQLFetch** ou **SQLFetchScroll** avant l’extraction par signet. Il peut utiliser des signets qu’il a stocké à partir d’un curseur précédent. Si les signets ne sont pas conservés sur les curseurs, l’application doit appeler **SQLFetch** ou **SQLFetchScroll** une fois pour récupérer les signets.  
  
## <a name="performing-bulk-deletes-using-bookmarks"></a>Exécution de suppressions en bloc à l’aide de signets  
 Pour effectuer des suppressions en bloc à l’aide de signets avec **SQLBulkOperations**, une application effectue les étapes suivantes dans l’ordre :  
  
1.  Définit l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS sur SQL_UB_VARIABLE.  
  
2.  Exécute une requête qui retourne un jeu de résultats.  
  
3.  Définit l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE sur le nombre de lignes qu’il souhaite supprimer.  
  
4.  Appelle **SQLBindCol** pour lier la colonne 0 (la colonne de signets).  
  
5.  Copie les signets pour les lignes qu’il souhaite supprimer dans le tableau lié à la colonne 0.  
  
    > [!NOTE]  
    >  La taille du tableau vers lequel pointe l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR doit être égale à SQL_ATTR_ROW_ARRAY_SIZE ou SQL_ATTR_ROW_STATUS_PTR doit être un pointeur null.  
  
6.  Appelle **SQLBulkOperations**(*StatementHandle,* SQL_DELETE_BY_BOOKMARK).  
  
7.  Si l’application a défini l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR, elle peut inspecter ce tableau pour voir le résultat de l’opération.  
  
 Si les signets sont conservés sur les curseurs, l’application n’a pas besoin d’appeler **SQLFetch** ou **SQLFetchScroll** avant de les supprimer par des signets. Il peut utiliser des signets qu’il a stocké à partir d’un curseur précédent. Si les signets ne sont pas conservés sur les curseurs, l’application doit appeler **SQLFetch** ou **SQLFetchScroll** une fois pour récupérer les signets.  
  
## <a name="providing-long-data-for-bulk-inserts-and-updates"></a>Fourniture de données longues pour les insertions et les mises à jour en bloc  
 Des données longues peuvent être fournies pour les insertions et mises à jour en bloc effectuées par les appels à **SQLBulkOperations**. Pour insérer ou mettre à jour des données de type long, une application effectue les étapes suivantes en plus des étapes décrites dans les sections « exécution d’insertions en bloc » et « exécution de mises à jour en bloc à l’aide de signets » plus haut dans cette rubrique.  
  
1.  Quand il lie les données à l’aide de **SQLBindCol**, l’application place une valeur définie par l’application, telle que le numéro de colonne, dans la mémoire tampon *\* TargetValuePtr* pour les colonnes de données en cours d’exécution. La valeur peut être utilisée ultérieurement pour identifier la colonne.  
  
     L’application place le résultat de la macro SQL_LEN_DATA_AT_EXEC (*longueur*) dans la mémoire tampon de *\* StrLen_or_IndPtr* . Si le type de données SQL de la colonne est SQL_LONGVARBINARY, SQL_LONGVARCHAR ou un type de données long spécifique à la source de données et que le pilote retourne « Y » pour le type d’informations SQL_NEED_LONG_DATA_LEN dans **SQLGetInfo**, *Length* est le nombre d’octets de données à envoyer pour le paramètre. dans le cas contraire, il doit s’agir d’une valeur non négative et est ignorée.  
  
2.  Quand **SQLBulkOperations** est appelé, s’il existe des colonnes de données en cours d’exécution, la fonction retourne SQL_NEED_DATA et passe à l’étape 3, qui suit. (S’il n’existe aucune colonne de données en cours d’exécution, le processus est terminé.)  
  
3.  L’application appelle **SQLParamData** pour récupérer l’adresse de la mémoire tampon *\* TargetValuePtr* pour la première colonne de données en cours d’exécution à traiter. **SQLParamData** retourne SQL_NEED_DATA. L’application récupère la valeur définie par l’application à partir de la mémoire tampon *\* TargetValuePtr* .  
  
    > [!NOTE]  
    >  Bien que les paramètres de données en cours d’exécution ressemblent aux colonnes de données en cours d’exécution, la valeur retournée par **SQLParamData** est différente pour chaque.  
  
     Les colonnes de données en cours d’exécution sont des colonnes d’un ensemble de lignes pour lesquelles des données sont envoyées avec **SQLPutData** lorsqu’une ligne est mise à jour ou insérée avec **SQLBulkOperations**. Elles sont liées à **SQLBindCol**. La valeur retournée par **SQLParamData** est l’adresse de la ligne dans la mémoire tampon **TargetValuePtr* en cours de traitement.  
  
4.  L’application appelle **SQLPutData** une ou plusieurs fois pour envoyer des données pour la colonne. Plusieurs appels sont nécessaires si toutes les valeurs de données ne peuvent pas être retournées dans la mémoire tampon *\* TargetValuePtr* spécifiée dans **SQLPutData**; plusieurs appels à **SQLPutData** pour la même colonne sont autorisés uniquement lors de l’envoi de données de caractères c à une colonne avec un type de données caractère, binaire ou spécifique à la source de données, ou lors de l’envoi de données binaires c à une colonne avec un  
  
5.  L’application appelle **SQLParamData** à nouveau pour signaler que toutes les données ont été envoyées pour la colonne.  
  
    -   S’il y a plus de colonnes de données en cours d’exécution, **SQLParamData** retourne SQL_NEED_DATA et l’adresse de la mémoire tampon *TargetValuePtr* pour la prochaine colonne de données en cours d’exécution à traiter. L’application répète les étapes 4 et 5.  
  
    -   S’il n’y a plus de colonnes de données en cours d’exécution, le processus est terminé. Si l’instruction a été exécutée avec succès, **SQLParamData** retourne SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO ; Si l’exécution a échoué, elle retourne SQL_ERROR. À ce stade, **SQLParamData** peut retourner n’importe quel SQLSTATE pouvant être retourné par **SQLBulkOperations**.  
  
 Si l’opération est annulée ou si une erreur se produit dans **SQLParamData** ou **SQLPutData** après que **SQLBulkOperations** a retourné SQL_NEED_DATA et avant que les données ne soient envoyées pour toutes les colonnes de données en cours d’exécution, l’application peut appeler uniquement **SQLCancel**, **SQLGetDiagField**, **SQLGetDiagRec**, **SQLGetFunctions**, **SQLParamData** ou **SQLPutData** pour l’instruction ou la connexion associée à l’instruction. Si elle appelle une autre fonction pour l’instruction ou la connexion associée à l’instruction, la fonction retourne SQL_ERROR et SQLSTATE HY010 (erreur de séquence de fonction).  
  
 Si l’application appelle **SQLCancel** alors que le pilote a encore besoin de données pour les colonnes de données en cours d’exécution, le pilote annule l’opération. L’application peut ensuite appeler **SQLBulkOperations** à nouveau ; l’annulation n’affecte pas l’état du curseur ou la position actuelle du curseur.  
  
## <a name="row-status-array"></a>Tableau d’état des lignes  
 Le tableau d’état de ligne contient des valeurs d’État pour chaque ligne de données de l’ensemble de lignes après un appel à **SQLBulkOperations**. Le pilote définit les valeurs d’État dans ce tableau après un appel à **SQLFetch**, **SQLFetchScroll**, **SQLSetPos** ou **SQLBulkOperations**. Ce tableau est initialement rempli par un appel à **SQLBulkOperations** si **SQLFetch** ou **SQLFetchScroll** n’a pas été appelé avant **SQLBulkOperations**. Ce tableau est désigné par l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR. Le nombre d’éléments dans les tableaux d’état de ligne doit être égal au nombre de lignes dans l’ensemble de lignes (tel que défini par l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE). Pour plus d’informations sur ce tableau d’état de ligne, consultez [SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
## <a name="code-example"></a>Exemple de code  
 L’exemple suivant extrait 10 lignes de données à la fois à partir de la table Customers. Il invite ensuite l’utilisateur à entrer une action à effectuer. Pour réduire le trafic réseau, l’exemple de mémoire tampon met à jour, supprime et insère localement dans les tableaux liés, mais à des décalages au-delà des données de l’ensemble de lignes. Lorsque l’utilisateur choisit d’envoyer des mises à jour, des suppressions et des insertions à la source de données, le code définit le décalage de liaison correctement et appelle **SQLBulkOperations**. Par souci de simplicité, l’utilisateur ne peut pas mettre en mémoire tampon plus de 10 mises à jour, suppressions ou insertions.  
  
```cpp  
// SQLBulkOperations_Function.cpp  
// compile with: ODBC32.lib  
#include <windows.h>  
#include <sqlext.h>  
#include "stdio.h"  
  
#define UPDATE_ROW 100  
#define DELETE_ROW 101  
#define ADD_ROW 102  
#define SEND_TO_DATA_SOURCE 103  
#define UPDATE_OFFSET 10  
#define INSERT_OFFSET 20  
#define DELETE_OFFSET 30  
  
// Define structure for customer data (assume 10 byte maximum bookmark size).  
typedef struct tagCustStruct {  
   SQLCHAR Bookmark[10];  
   SQLINTEGER BookmarkLen;  
   SQLUINTEGER CustomerID;  
   SQLINTEGER CustIDInd;  
   SQLCHAR CompanyName[51];  
   SQLINTEGER NameLenOrInd;  
   SQLCHAR Address[51];  
   SQLINTEGER AddressLenOrInd;  
   SQLCHAR Phone[11];  
   SQLINTEGER PhoneLenOrInd;  
} CustStruct;  
  
// Allocate 40 of these structures. Elements 0-9 are for the current rowset,  
// elements 10-19 are for the buffered updates, elements 20-29 are for  
// the buffered inserts, and elements 30-39 are for the buffered deletes.  
CustStruct CustArray[40];  
SQLUSMALLINT RowStatusArray[10], Action, RowNum, NumUpdates = 0, NumInserts = 0,  
NumDeletes = 0;  
SQLLEN BindOffset = 0;  
SQLRETURN retcode;  
SQLHENV henv = NULL;  
SQLHDBC hdbc = NULL;  
SQLHSTMT hstmt = NULL;  
  
int main() {  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   // Set the following statement attributes:  
   // SQL_ATTR_CURSOR_TYPE:           Keyset-driven  
   // SQL_ATTR_ROW_BIND_TYPE:         Row-wise  
   // SQL_ATTR_ROW_ARRAY_SIZE:        10  
   // SQL_ATTR_USE_BOOKMARKS:         Use variable-length bookmarks  
   // SQL_ATTR_ROW_STATUS_PTR:        Points to RowStatusArray  
   // SQL_ATTR_ROW_BIND_OFFSET_PTR:   Points to BindOffset  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_CURSOR_TYPE, (SQLPOINTER)SQL_CURSOR_KEYSET_DRIVEN, 0);  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_BIND_TYPE, (SQLPOINTER)sizeof(CustStruct), 0);  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)10, 0);  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_USE_BOOKMARKS, (SQLPOINTER)SQL_UB_VARIABLE, 0);  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_STATUS_PTR, RowStatusArray, 0);  
   retcode = SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_BIND_OFFSET_PTR, &BindOffset, 0);  
  
   // Bind arrays to the bookmark, CustomerID, CompanyName, Address, and Phone columns.  
   retcode = SQLBindCol(hstmt, 0, SQL_C_VARBOOKMARK, CustArray[0].Bookmark, sizeof(CustArray[0].Bookmark), &CustArray[0].BookmarkLen);  
   retcode = SQLBindCol(hstmt, 1, SQL_C_ULONG, &CustArray[0].CustomerID, 0, &CustArray[0].CustIDInd);  
   retcode = SQLBindCol(hstmt, 2, SQL_C_CHAR, CustArray[0].CompanyName, sizeof(CustArray[0].CompanyName), &CustArray[0].NameLenOrInd);  
   retcode = SQLBindCol(hstmt, 3, SQL_C_CHAR, CustArray[0].Address, sizeof(CustArray[0].Address), &CustArray[0].AddressLenOrInd);  
   retcode = SQLBindCol(hstmt, 4, SQL_C_CHAR, CustArray[0].Phone, sizeof(CustArray[0].Phone), &CustArray[0].PhoneLenOrInd);  
  
   // Execute a statement to retrieve rows from the Customers table.  
   retcode = SQLExecDirect(hstmt, (SQLCHAR*)"SELECT CustomerID, CompanyName, Address, Phone FROM Customers", SQL_NTS);  
  
   // Fetch and display the first 10 rows.  
   retcode = SQLFetchScroll(hstmt, SQL_FETCH_NEXT, 0);  
   // DisplayCustData(CustArray, 10);  
  
   // Call GetAction to get an action and a row number from the user.  
   // while (GetAction(&Action, &RowNum)) {  
   Action = SQL_FETCH_NEXT;  
   RowNum = 2;  
   switch (Action) {  
      case SQL_FETCH_NEXT:  
      case SQL_FETCH_PRIOR:  
      case SQL_FETCH_FIRST:  
      case SQL_FETCH_LAST:  
      case SQL_FETCH_ABSOLUTE:  
      case SQL_FETCH_RELATIVE:  
         // Fetch and display the requested data.  
         SQLFetchScroll(hstmt, Action, RowNum);  
         // DisplayCustData(CustArray, 10);  
         break;  
  
      case UPDATE_ROW:  
         // Check if we have reached the maximum number of buffered updates.  
         if (NumUpdates < 10) {  
            // Get the new customer data and place it in the next available element of  
            // the buffered updates section of CustArray, copy the bookmark of the row  
            // being updated to the same element, and increment the update counter.  
            // Checking to see we have not already buffered an update for this  
            // row not shown.  
            // GetNewCustData(CustArray, UPDATE_OFFSET + NumUpdates);  
            memcpy(CustArray[UPDATE_OFFSET + NumUpdates].Bookmark,  
               CustArray[RowNum - 1].Bookmark,  
               CustArray[RowNum - 1].BookmarkLen);  
            CustArray[UPDATE_OFFSET + NumUpdates].BookmarkLen =  
               CustArray[RowNum - 1].BookmarkLen;  
            NumUpdates++;  
         } else {  
            printf("Buffers full. Send buffered changes to the data source.");  
         }  
         break;  
      case DELETE_ROW:  
         // Check if we have reached the maximum number of buffered deletes.  
         if (NumDeletes < 10) {  
            // Copy the bookmark of the row being deleted to the next available element  
            // of the buffered deletes section of CustArray and increment the delete  
            // counter. Checking to see we have not already buffered an update for  
            // this row not shown.  
            memcpy(CustArray[DELETE_OFFSET + NumDeletes].Bookmark,  
               CustArray[RowNum - 1].Bookmark,  
               CustArray[RowNum - 1].BookmarkLen);  
  
            CustArray[DELETE_OFFSET + NumDeletes].BookmarkLen =  
               CustArray[RowNum - 1].BookmarkLen;  
  
            NumDeletes++;  
         } else  
            printf("Buffers full. Send buffered changes to the data source.");  
         break;  
  
      case ADD_ROW:  
         // reached maximum number of buffered inserts?  
         if (NumInserts < 10) {  
            // Get the new customer data and place it in the next available element of  
            // the buffered inserts section of CustArray and increment insert counter.  
            // GetNewCustData(CustArray, INSERT_OFFSET + NumInserts);  
            NumInserts++;  
         } else  
            printf("Buffers full. Send buffered changes to the data source.");  
         break;  
  
      case SEND_TO_DATA_SOURCE:  
         // If there are any buffered updates, inserts, or deletes, set the array size  
         // to that number, set the binding offset to use the data in the buffered  
         // update, insert, or delete part of CustArray, and call SQLBulkOperations to  
         // do the updates, inserts, or deletes. Because we will never have more than  
         // 10 updates, inserts, or deletes, we can use the same row status array.  
         if (NumUpdates) {  
            SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)NumUpdates, 0);  
            BindOffset = UPDATE_OFFSET * sizeof(CustStruct);  
            SQLBulkOperations(hstmt, SQL_UPDATE_BY_BOOKMARK);  
            NumUpdates = 0;  
         }  
  
         if (NumInserts) {  
            SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)NumInserts, 0);  
            BindOffset = INSERT_OFFSET * sizeof(CustStruct);  
            SQLBulkOperations(hstmt, SQL_ADD);  
            NumInserts = 0;  
         }  
  
         if (NumDeletes) {  
            SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)NumDeletes, 0);  
            BindOffset = DELETE_OFFSET * sizeof(CustStruct);  
            SQLBulkOperations(hstmt, SQL_DELETE_BY_BOOKMARK);  
            NumDeletes = 0;  
         }  
  
         // If there were any updates, inserts, or deletes, reset the binding offset  
         // and array size to their original values.  
         if (NumUpdates || NumInserts || NumDeletes) {  
            SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)10, 0);  
            BindOffset = 0;  
         }  
         break;  
   }  
   // }  
  
   // Close the cursor.  
   SQLFreeStmt(hstmt, SQL_CLOSE);  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Obtention d’un seul champ d’un descripteur|[Fonction SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|Obtention de plusieurs champs d’un descripteur|[SQLGetDescRec, fonction](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Définition d’un champ unique d’un descripteur|[SQLSetDescField, fonction](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Définition de plusieurs champs d’un descripteur|[SQLSetDescRec, fonction](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
|Positionnement du curseur, actualisation des données dans l’ensemble de lignes ou mise à jour ou suppression de données dans l’ensemble de lignes|[SQLSetPos, fonction](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
|Définition d’un attribut d’instruction|[Fonction SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
