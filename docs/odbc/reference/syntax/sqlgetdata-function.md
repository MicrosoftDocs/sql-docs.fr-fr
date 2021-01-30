---
description: Fonction SQLGetData
title: Fonction SQLGetData | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetData
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetData
helpviewer_keywords:
- SQLGetData function [ODBC]
ms.assetid: e3c1356a-5db7-4186-85fd-8b74633317e8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42b81d50a83fef0be7c13604902cfe67b2d2dd89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201963"
---
# <a name="sqlgetdata-function"></a>Fonction SQLGetData
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLGetData** récupère les données pour une colonne unique dans le jeu de résultats ou pour un paramètre unique après **SQLParamData** , retourne SQL_PARAM_DATA_AVAILABLE. Il peut être appelé plusieurs fois pour extraire des données de longueur variable en parties.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLGetData(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   Col_or_Param_Num,  
      SQLSMALLINT    TargetType,  
      SQLPOINTER     TargetValuePtr,  
      SQLLEN         BufferLength,  
      SQLLEN *       StrLen_or_IndPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *Col_or_Param_Num*  
 Entrée Pour récupérer des données de colonne, il s’agit du numéro de la colonne pour laquelle les données doivent être retournées. Les colonnes de l’ensemble de résultats sont numérotées dans l’ordre de colonne plus élevé, à partir de 1. La colonne de signet est le numéro de colonne 0 ; Cela ne peut être spécifié que si les signets sont activés.  
  
 Pour récupérer des données de paramètre, il s’agit de l’ordinal du paramètre, qui commence à 1.  
  
 *TargetType*  
 Entrée Identificateur de type du type de données C de la mémoire tampon **TargetValuePtr* . Pour obtenir la liste des types de données et des identificateurs de type C valides, consultez la section [types de données c](../../../odbc/reference/appendixes/c-data-types.md) dans l’annexe D : types de données.  
  
 Si *TargetType* est SQL_ARD_TYPE, le pilote utilise l’identificateur de type spécifié dans le champ SQL_DESC_CONCISE_TYPE de ARD. Si *TargetType* est SQL_APD_TYPE, **SQLGetData** utilise le même type de données C que celui qui a été spécifié dans **SQLBindParameter**. Dans le cas contraire, le type de données C spécifié dans **SQLGetData** remplace le type de données c spécifié dans **SQLBindParameter**. S’il est SQL_C_DEFAULT, le pilote sélectionne le type de données C par défaut en fonction du type de données SQL de la source.  
  
 Vous pouvez également spécifier un type de données C étendu. Pour plus d’informations, consultez [types de données C dans ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).  
  
 *TargetValuePtr*  
 Sortie Pointeur vers la mémoire tampon dans laquelle les données doivent être retournées.  
  
 *TargetValuePtr* ne peut pas être null.  
  
 *BufferLength*  
 Entrée Longueur de la mémoire tampon **TargetValuePtr* en octets.  
  
 Le pilote utilise *BufferLength* pour éviter d’écrire au-delà de la fin de la \* mémoire tampon *TargetValuePtr* lors du retour de données de longueur variable, telles que des données de type caractère ou binaire. Notez que le pilote compte le caractère de fin null lorsqu’il retourne des données de type caractère à \* *TargetValuePtr*. **TargetValuePtr* doit donc contenir de l’espace pour le caractère de fin null ou le pilote va tronquer les données.  
  
 Lorsque le pilote retourne des données de longueur fixe, telles qu’un entier ou une structure de date, le pilote ignore *BufferLength* et suppose que la mémoire tampon est suffisamment grande pour contenir les données. Il est donc important pour l’application d’allouer une mémoire tampon suffisamment importante pour les données de longueur fixe ou le pilote écrira au-delà de la fin de la mémoire tampon.  
  
 **SQLGetData** retourne SQLState HY090 (longueur de chaîne ou de mémoire tampon non valide) lorsque *BufferLength* est inférieur à 0 mais pas lorsque *BufferLength* est égal à 0.  
  
 *StrLen_or_IndPtr*  
 Sortie Pointeur vers la mémoire tampon dans laquelle retourner la valeur de longueur ou d’indicateur. S’il s’agit d’un pointeur null, aucune valeur de longueur ou d’indicateur n’est retournée. Cela renvoie une erreur lorsque les données extraites ont la valeur NULL.  
  
 **SQLGetData** peut retourner les valeurs suivantes dans le tampon longueur/indicateur :  
  
-   La longueur des données disponibles pour le retour  
  
-   SQL_NO_TOTAL  
  
-   SQL_NULL_DATA  
  
 Pour plus d’informations, consultez [utilisation des valeurs de longueur/indicateur](../../../odbc/reference/develop-app/using-length-and-indicator-values.md) et des « commentaires » dans cette rubrique.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLGetData** retourne soit SQL_ERROR, soit SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLGetData** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Toutes les données de la colonne spécifiée, *Col_or_Param_Num*, peuvent être récupérées dans un seul appel à la fonction. SQL_NO_TOTAL ou la longueur des données restantes dans la colonne spécifiée avant l’appel en cours à **SQLGetData** est retournée dans \* *StrLen_or_IndPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO.)<br /><br /> Pour plus d’informations sur l’utilisation de plusieurs appels à **SQLGetData** pour une seule colonne, consultez « comments ».|  
|01S07|Troncation fractionnaire|Les données retournées pour une ou plusieurs colonnes ont été tronquées. Pour les types de données numériques, la partie fractionnaire du nombre a été tronquée. Pour les types de données time, timestamp et Interval contenant un composant heure, la partie fractionnaire de l’heure a été tronquée.<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07006|Violation d’attribut de type de données restreint|La valeur des données d’une colonne du jeu de résultats ne peut pas être convertie dans le type de données C spécifié par l’argument *TargetType*.|  
|07009|Index de descripteur non valide|La valeur spécifiée pour l’argument *Col_or_Param_Num* était 0 et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_OFF.<br /><br /> La valeur spécifiée pour l’argument *Col_or_Param_Num* est supérieure au nombre de colonnes dans le jeu de résultats.<br /><br /> La valeur de *Col_or_Param_Num* n’est pas égale à l’ordinal du paramètre disponible.<br /><br /> (DM) la colonne spécifiée a été liée. Cette description ne s’applique pas aux pilotes qui renvoient le masque de SQL_GD_BOUND de l’option SQL_GETDATA_EXTENSIONS dans **SQLGetInfo**.<br /><br /> (DM) le numéro de la colonne spécifiée est inférieur ou égal au nombre de la colonne liée la plus élevée. Cette description ne s’applique pas aux pilotes qui renvoient le masque de SQL_GD_ANY_COLUMN de l’option SQL_GETDATA_EXTENSIONS dans **SQLGetInfo**.<br /><br /> (DM) l’application a déjà appelé **SQLGetData** pour la ligne actuelle ; le numéro de la colonne spécifiée dans l’appel actuel est inférieur au numéro de la colonne spécifiée dans l’appel précédent ; et le pilote ne retourne pas le masque de SQL_GD_ANY_ORDER de l’option SQL_GETDATA_EXTENSIONS dans **SQLGetInfo**.<br /><br /> (DM) l’argument *TargetType* était SQL_ARD_TYPE, et l’enregistrement de descripteur de *COL_OR_PARAM_NUM* dans le ARD n’a pas réussi la vérification de cohérence.<br /><br /> (DM) l’argument *TargetType* était SQL_ARD_TYPE, et la valeur dans le champ SQL_DESC_COUNT du ARD était inférieure à l’argument *Col_or_Param_Num* .|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|22002|Variable d’indicateur requise mais non fournie|*StrLen_or_IndPtr* était un pointeur null et les données null ont été récupérées.|  
|22003|Valeur numérique hors limites|Le retour de la valeur numérique (sous forme de valeur numérique ou de chaîne) pour la colonne aurait entraîné la troncation de la totalité (par opposition à la partie fractionnaire) du nombre à tronquer.<br /><br /> Pour plus d’informations, consultez l' [annexe D : types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).|  
|22007|Format de date/heure non valide|La colonne de caractères dans le jeu de résultats a été liée à une structure de date, d’heure ou d’horodatage C, et la valeur de la colonne était une date, une heure ou un horodateur non valide, respectivement. Pour plus d’informations, consultez l' [annexe D : types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).|  
|22012|Division par zéro|Une valeur issue d’une expression arithmétique ayant généré une division par zéro a été retournée.|  
|22015|Dépassement du champ d’intervalle|L’assignation d’un type SQL exact numérique ou Interval à un type C Interval a provoqué une perte de chiffres significatifs dans le champ de début.<br /><br /> Lors du retour de données à un type d’intervalle C, il n’existait aucune représentation de la valeur du type SQL dans le type d’intervalle C.|  
|22018|Valeur de caractère non valide pour la spécification de cast|Une colonne de caractères dans le jeu de résultats a été retournée dans une mémoire tampon de caractères C, et la colonne contenait un caractère pour lequel il n’existait aucune représentation dans le jeu de caractères de la mémoire tampon.<br /><br /> Le type C était un type de données numérique exact ou approximatif, DateTime ou Interval ; le type SQL de la colonne était un type de données caractère ; et la valeur dans la colonne n’est pas un littéral valide du type C lié.|  
|24 000|État de curseur non valide|(DM) la fonction a été appelée sans appeler d’abord **SQLFetch** ou **SQLFetchScroll** pour positionner le curseur sur la ligne de données requise.<br /><br /> (DM) le *StatementHandle* était dans un état d’exécution, mais aucun jeu de résultats n’était associé à *StatementHandle*.<br /><br /> Un curseur a été ouvert sur *StatementHandle* et **SQLFetch** ou **SQLFetchScroll** a été appelé, mais le curseur a été placé avant le début du jeu de résultats ou après la fin du jeu de résultats.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY003|Type de programme hors limites|(DM) l’argument *TargetType* n’était pas un type de données valide, SQL_C_DEFAULT, SQL_ARD_TYPE (dans le cas d’une récupération de données de colonne) ou SQL_APD_TYPE (en cas d’extraction des données de paramètre).<br /><br /> (DM) l’argument *Col_or_Param_Num* était 0 et l’argument *TargetType* n’était pas SQL_C_BOOKMARK pour un signet de longueur fixe ou SQL_C_VARBOOKMARK pour un signet de longueur variable.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*, puis la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread, puis la fonction a été appelée à nouveau sur le *StatementHandle*.|  
|HY009|Utilisation non valide d’un pointeur null|(DM) l’argument *TargetValuePtr* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) le *StatementHandle* spécifié n’était pas dans un état d’exécution. La fonction a été appelée sans appeler d’abord **SQLExecDirect**, **SQLExecute** ou une fonction de catalogue.<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLGetData** .<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.<br /><br /> (DM) le *StatementHandle* était dans un état d’exécution, mais aucun jeu de résultats n’était associé à *StatementHandle*.<br /><br /> Un appel à **SQLExeceute**, **SQLExecDirect** ou **SQLMoreResults** a retourné SQL_PARAM_DATA_AVAILABLE, mais **SQLGetData** a été appelé, au lieu de **SQLParamData**.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur spécifiée pour l’argument *BufferLength* est inférieure à 0.<br /><br /> La valeur spécifiée pour l’argument *BufferLength* est inférieure à 4, l’argument *Col_or_Param_Num* a été défini sur 0 et le pilote est un pilote ODBC 2 *. x* .|  
|HY109|Position de curseur non valide|Le curseur a été positionné (par **SQLSetPos**, **SQLFetch**, **SQLFetchScroll** ou **SQLBulkOperations**) sur une ligne qui avait été supprimée ou n’a pas pu être extraite.<br /><br /> Le curseur était un curseur avant uniquement et la taille de l’ensemble de lignes était supérieure à un.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Le pilote ou la source de données ne prend pas en charge l’utilisation de **SQLGetData** avec plusieurs lignes dans **SQLFetchScroll**. Cette description ne s’applique pas aux pilotes qui renvoient le masque de SQL_GD_BLOCK de l’option SQL_GETDATA_EXTENSIONS dans **SQLGetInfo**.<br /><br /> Le pilote ou la source de données ne prend pas en charge la conversion spécifiée par la combinaison de l’argument *TargetType* et du type de données SQL de la colonne correspondante. Cette erreur s’applique uniquement lorsque le type de données SQL de la colonne a été mappé à un type de données SQL spécifique au pilote.<br /><br /> Le pilote prend en charge uniquement ODBC 2 *. x*, et l’argument *TargetType* était l’un des éléments suivants :<br /><br /> SQL_C_NUMERIC SQL_C_SBIGINT SQL_C_UBIGINT<br /><br /> et tous les types de données Interval C listés dans les [types de données c](../../../odbc/reference/appendixes/c-data-types.md) de l’annexe D : types de données.<br /><br /> Le pilote ne prend en charge que les versions ODBC antérieures à 3,50, et l’argument *TargetType* était SQL_C_GUID.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote correspondant à *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 **SQLGetData** retourne les données dans une colonne spécifiée. **SQLGetData** ne peut être appelé qu’après qu’une ou plusieurs lignes ont été extraites de l’ensemble de résultats par **SQLFetch**, **SQLFetchScroll** ou **SQLExtendedFetch**. Si les données de longueur variable sont trop volumineuses pour être retournées dans un seul appel à **SQLGetData** (en raison d’une limitation dans l’application), **SQLGetData** peut les récupérer dans des parties. Il est possible de lier certaines colonnes dans une ligne et d’appeler **SQLGetData** pour d’autres, bien que cela soit soumis à certaines restrictions. Pour plus d’informations, consultez [obtention de données de type long](../../../odbc/reference/develop-app/getting-long-data.md).  
  
 Pour plus d’informations sur l’utilisation de **SQLGetData** avec les paramètres de sortie diffusés en continu, consultez [récupération des paramètres de sortie à l’aide de SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="using-sqlgetdata"></a>Utilisation de SQLGetData  
 Si le pilote ne prend pas en charge les extensions de **SQLGetData**, la fonction peut retourner des données uniquement pour les colonnes indépendantes dont le nombre est supérieur à celui de la dernière colonne liée. En outre, dans une ligne de données, la valeur de l’argument *Col_or_Param_Num* dans chaque appel à **SQLGetData** doit être supérieure ou égale à la valeur de *Col_or_Param_Num* dans l’appel précédent ; autrement dit, les données doivent être récupérées dans l’ordre d’incrémentation des numéros de colonne. Enfin, si aucune extension n’est prise en charge, **SQLGetData** ne peut pas être appelé si la taille de l’ensemble de lignes est supérieure à 1.  
  
 Les pilotes peuvent assouplir ces restrictions. Pour déterminer quelles restrictions un pilote assouplit, une application appelle **SQLGetInfo** avec l’une des options de SQL_GETDATA_EXTENSIONS suivantes :  
  
-   SQL_GD_OUTPUT_PARAMS = **SQLGetData** peut être appelé pour retourner les valeurs des paramètres de sortie. Pour plus d’informations, consultez [récupération des paramètres de sortie à l’aide de SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
-   SQL_GD_ANY_COLUMN. Si cette option est retournée, **SQLGetData** peut être appelé pour toute colonne non liée, y compris celles antérieures à la dernière colonne liée.  
  
-   SQL_GD_ANY_ORDER. Si cette option est retournée, **SQLGetData** peut être appelé pour les colonnes indépendantes dans n’importe quel ordre.  
  
-   SQL_GD_BLOCK. Si cette option est retournée par **SQLGetInfo** pour l’infotype SQL_GETDATA_EXTENSIONS, le pilote prend en charge les appels à **SQLGetData** lorsque la taille de l’ensemble de lignes est supérieure à 1 et que l’application peut appeler **SQLSetPos** avec l’option SQL_POSITION pour positionner le curseur sur la ligne correcte avant d’appeler **SQLGetData.**  
  
-   SQL_GD_BOUND. Si cette option est retournée, **SQLGetData** peut être appelé pour les colonnes dépendantes, ainsi que pour les colonnes indépendantes.  
  
 Il existe deux exceptions à ces restrictions et la capacité d’un pilote à les assouplir. Tout d’abord, **SQLGetData** ne doit jamais être appelé pour un curseur avant uniquement lorsque la taille de l’ensemble de lignes est supérieure à 1. Deuxièmement, si un pilote prend en charge les signets, il doit toujours prendre en charge la possibilité d’appeler **SQLGetData** pour la colonne 0, même s’il ne permet pas aux applications d’appeler **SQLGetData** pour d’autres colonnes avant la dernière colonne liée. (Lorsqu’une application utilise ODBC 2 *. x* , **SQLGetData** retourne avec succès un signet lorsqu’il est appelé avec *Col_or_Param_Num* égal à 0 après un appel à **SQLFetch**, car **SQLFetch** est mappée par le gestionnaire de pilotes odbc 3 *. x* à **SQLExtendedFetch** avec un *FetchOrientation* de SQL_FETCH_NEXT, et **SQLGetData** avec un *Col_or_Param_Num* de 0 est mappé par le gestionnaire de pilotes ODBC 3 *. x* à **SQLGetStmtOption** avec un *fOption* de SQL_GET_BOOKMARK.)  
  
 **SQLGetData** ne peut pas être utilisé pour récupérer le signet d’une ligne qui vient d’être insérée en appelant **SQLBulkOperations** avec l’option SQL_ADD, car le curseur n’est pas positionné sur la ligne. Une application peut récupérer le signet pour une telle ligne en liant la colonne 0 avant d’appeler **SQLBulkOperations** avec SQL_ADD, auquel cas **SQLBulkOperations** retourne le signet dans la mémoire tampon liée. **SQLFetchScroll** peut ensuite être appelé avec SQL_FETCH_BOOKMARK pour repositionner le curseur sur cette ligne.  
  
 Si l’argument *TargetType* est un type de données Interval, la précision de l’intervalle par défaut (2) et la précision de l’intervalle par défaut (6), telles que définies dans les champs SQL_DESC_DATETIME_INTERVAL_PRECISION et SQL_DESC_PRECISION de l’ARD, respectivement, sont utilisées pour les données. Si l’argument *TargetType* est un type de données SQL_C_NUMERIC, la précision par défaut (définie par le pilote) et l’échelle par défaut (0), telles que définies dans les champs SQL_DESC_PRECISION et SQL_DESC_SCALE de ARD, sont utilisées pour les données. Si une précision ou une échelle par défaut n’est pas appropriée, l’application doit définir explicitement le champ de descripteur approprié à l’aide d’un appel à **SQLSetDescField** ou **SQLSetDescRec**. Elle peut définir le champ SQL_DESC_CONCISE_TYPE sur SQL_C_NUMERIC et appeler **SQLGetData** avec un argument *TargetType* de SQL_ARD_TYPE, ce qui entraîne l’utilisation des valeurs de précision et d’échelle dans les champs de descripteur.  
  
> [!NOTE]
>  Dans ODBC 2 *. x*, les applications définissent *TargetType* à SQL_C_DATE, SQL_C_TIME ou SQL_C_TIMESTAMP pour indiquer que \* *TargetValuePtr* est une structure de date, d’heure ou d’horodatage. Dans ODBC 3 *. x*, les applications définissent *TargetType* sur SQL_C_TYPE_DATE, SQL_C_TYPE_TIME ou SQL_C_TYPE_TIMESTAMP. Le gestionnaire de pilotes effectue les mappages appropriés, si nécessaire, en fonction de la version de l’application et du pilote.  
  
## <a name="retrieving-variable-length-data-in-parts"></a>Récupération de données Variable-Lengthes dans des parties  
 **SQLGetData** peut être utilisé pour extraire des données d’une colonne qui contient des données de longueur variable en parties, c’est-à-dire lorsque l’identificateur du type de données SQL de la colonne est SQL_CHAR, SQL_VARCHAR, SQL_LONGVARCHAR, SQL_WCHAR, SQL_WVARCHAR, SQL_WLONGVARCHAR, SQL_BINARY, SQL_VARBINARY, SQL_LONGVARBINARY ou un identificateur spécifique au pilote pour un type de longueur variable.  
  
 Pour extraire des données d’une colonne en parties, l’application appelle **SQLGetData** plusieurs fois de suite pour la même colonne. À chaque appel, **SQLGetData** retourne la partie suivante des données. Il revient à l’application de réassembler les parties, en veillant à supprimer le caractère de fin null des parties intermédiaires des données de type caractère. S’il y a plus de données à retourner ou si la mémoire tampon n’est pas suffisante pour le caractère de fin, **SQLGetData** retourne SQL_SUCCESS_WITH_INFO et SQLSTATE 01004 (données tronquées). Lorsqu’il retourne la dernière partie des données, **SQLGetData** retourne SQL_SUCCESS. Ni SQL_NO_TOTAL ni zéro ne peuvent être retournés sur le dernier appel valide pour récupérer des données d’une colonne, car l’application n’a alors aucun moyen de savoir quelle quantité de données dans la mémoire tampon de l’application est valide. Si **SQLGetData** est appelé après ce, il retourne SQL_NO_DATA. Pour plus d’informations, consultez la section suivante, « extraction de données avec SQLGetData ».  
  
 Les signets de longueur variable peuvent être retournés en parties par **SQLGetData**. Comme pour les autres données, un appel à **SQLGetData** pour retourner des signets de longueur variable dans des parties retourne SQLState 01004 (String Data, Right TRUNCATE) et SQL_SUCCESS_WITH_INFO lorsqu’il y a plus de données à retourner. Cela est différent du cas où un signet de longueur variable est tronqué par un appel à **SQLFetch** ou **SQLFetchScroll**, qui retourne SQL_ERROR et SQLSTATE 22001 (données de chaîne tronquées à droite).  
  
 **SQLGetData** ne peut pas être utilisé pour retourner des données de longueur fixe dans des parties. Si **SQLGetData** est appelé plus d’une fois dans une ligne pour une colonne contenant des données de longueur fixe, il retourne SQL_NO_DATA pour tous les appels après le premier.  
  
## <a name="retrieving-streamed-output-parameters"></a>Récupération des paramètres de sortie diffusés en continu  
 Si un pilote prend en charge les paramètres de sortie diffusés en continu, une application peut appeler **SQLGetData** avec une petite mémoire tampon plusieurs fois pour récupérer une valeur de paramètre élevée. Pour plus d’informations sur les paramètres de sortie diffusés en continu, consultez [récupération des paramètres de sortie à l’aide de SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="retrieving-data-with-sqlgetdata"></a>Récupération de données avec SQLGetData  
 Pour retourner les données de la colonne spécifiée, **SQLGetData** effectue la séquence d’étapes suivante :  
  
1.  Retourne SQL_NO_DATA s’il a déjà retourné toutes les données de la colonne.  
  
2.  Définit \* *StrLen_or_IndPtr* sur SQL_NULL_DATA si les données sont null. Si les données ont la valeur NULL et que *StrLen_or_IndPtr* était un pointeur null, **SQLGetData** retourne SQLState 22002 (variable indicateur requise mais non fournie).  
  
     Si les données de la colonne ne sont pas NULL, **SQLGetData** passe à l’étape 3.  
  
3.  Si l’attribut d’instruction SQL_ATTR_MAX_LENGTH est défini sur une valeur différente de zéro, si la colonne contient des données de type caractère ou binaire, et si **SQLGetData** n’a pas encore été appelé pour la colonne, les données sont tronquées à SQL_ATTR_MAX_LENGTH octets.  
  
    > [!NOTE]  
    >  L’attribut d’instruction SQL_ATTR_MAX_LENGTH est destiné à réduire le trafic réseau. Elle est généralement implémentée par la source de données, qui tronque les données avant de les renvoyer sur le réseau. Les pilotes et les sources de données ne sont pas requis pour le prendre en charge. Par conséquent, pour garantir que les données sont tronquées à une taille particulière, une application doit allouer une mémoire tampon de cette taille et spécifier la taille dans l’argument *BufferLength* .  
  
4.  Convertit les données vers le type spécifié dans *TargetType*. La précision et l’échelle par défaut de ce type de données sont attribuées aux données. Si *TargetType* est SQL_ARD_TYPE, le type de données dans le champ SQL_DESC_CONCISE_TYPE de l’ARD est utilisé. Si *TargetType* est SQL_ARD_TYPE, les données reçoivent la précision et l’échelle dans les champs SQL_DESC_DATETIME_INTERVAL_PRECISION, SQL_DESC_PRECISION et SQL_DESC_SCALE de ARD, selon le type de données du champ SQL_DESC_CONCISE_TYPE. Si une précision ou une échelle par défaut n’est pas appropriée, l’application doit définir explicitement le champ de descripteur approprié à l’aide d’un appel à **SQLSetDescField** ou **SQLSetDescRec**.  
  
5.  Si les données ont été converties en un type de données de longueur variable, tel qu’un caractère ou un binaire, **SQLGetData** vérifie si la longueur des données dépasse *BufferLength*. Si la longueur des données de caractères (y compris le caractère de fin null) dépasse *BufferLength*, **SQLGetData** tronque les données à *BufferLength* moins la longueur d’un caractère de fin null. Il termine ensuite les données null. Si la longueur des données binaires dépasse la longueur de la mémoire tampon de données, **SQLGetData** la tronque à *BufferLength* octets.  
  
     Si le tampon de données fourni est trop petit pour contenir le caractère de fin null, **SQLGetData** retourne SQL_SUCCESS_WITH_INFO et SQLSTATE 01004.  
  
     **SQLGetData** ne tronque jamais les données converties en types de données de longueur fixe ; elle suppose toujours que la longueur de **TargetValuePtr* est la taille du type de données.  
  
6.  Place les données converties (et éventuellement tronquées) dans \* *TargetValuePtr*. Notez que **SQLGetData** ne peut pas retourner de données hors ligne.  
  
7.  Place la longueur des données dans \* *StrLen_or_IndPtr*. Si *StrLen_or_IndPtr* était un pointeur null, **SQLGetData** ne retourne pas la longueur.  
  
    -   Pour les données de type caractère ou binaire, il s’agit de la longueur des données après la conversion et avant la troncation en raison de *BufferLength*. Si le pilote ne peut pas déterminer la longueur des données après la conversion, comme c’est parfois le cas avec des données longues, il retourne SQL_SUCCESS_WITH_INFO et définit la longueur sur SQL_NO_TOTAL. (Le dernier appel à **SQLGetData** doit toujours retourner la longueur des données, et non zéro ou SQL_NO_TOTAL.) Si les données ont été tronquées en raison de l’attribut d’instruction SQL_ATTR_MAX_LENGTH, la valeur de cet attribut, par opposition à la longueur réelle, est placée dans \* *StrLen_or_IndPtr*. Cela est dû au fait que cet attribut est conçu pour tronquer les données sur le serveur avant la conversion, de sorte que le pilote n’a aucun moyen de déterminer la longueur réelle. Lorsque **SQLGetData** est appelé plusieurs fois de suite pour la même colonne, il s’agit de la longueur des données disponibles au début de l’appel en cours. autrement dit, la longueur diminue avec chaque appel suivant.  
  
    -   Pour tous les autres types de données, il s’agit de la longueur des données après la conversion ; autrement dit, il s’agit de la taille du type vers lequel les données ont été converties.  
  
8.  Si les données sont tronquées sans perte de précision pendant la conversion (par exemple, le nombre réel 1,234 est tronqué lorsqu’il est converti en entier 1) ou parce que *BufferLength* est trop petit (par exemple, la chaîne « abcdef » est placée dans une mémoire tampon de 4 octets), **SQLGetData** retourne SQLState 01004 (données tronquées) et SQL_SUCCESS_WITH_INFO. Si les données sont tronquées sans perte de précision en raison de l’attribut d’instruction SQL_ATTR_MAX_LENGTH, **SQLGetData** retourne SQL_SUCCESS et ne retourne pas SQLSTATE 01004 (données tronquées).  
  
 Le contenu de la mémoire tampon de données liée (si **SQLGetData** est appelé sur une colonne liée) et que la mémoire tampon de longueur/d’indicateur n’est pas définie si **SQLGetData** ne retourne pas SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO.  
  
 Les appels successifs à **SQLGetData** récupèrent les données de la dernière colonne demandée. les décalages précédents deviennent non valides. Par exemple, lors de l’exécution de la séquence suivante :  
  
```cpp  
SQLGetData(icol=n), SQLGetData(icol=m), SQLGetData(icol=n)  
```  
  
 le deuxième appel à SQLGetData (Icol = n) récupère les données à partir du début de la colonne n. Tout décalage dans les données dues à des appels antérieurs à **SQLGetData** pour la colonne n’est plus valide.  
  
## <a name="descriptors-and-sqlgetdata"></a>Descripteurs et SQLGetData  
 **SQLGetData** n’interagit pas directement avec les champs de descripteur.  
  
 Si *TargetType* est SQL_ARD_TYPE, le type de données dans le champ SQL_DESC_CONCISE_TYPE de l’ARD est utilisé. Si *TargetType* a la valeur SQL_ARD_TYPE ou SQL_C_DEFAULT, les données se composent de la précision et de l’échelle dans les champs SQL_DESC_DATETIME_INTERVAL_PRECISION, SQL_DESC_PRECISION et SQL_DESC_SCALE du ARD, selon le type de données du champ SQL_DESC_CONCISE_TYPE.  
  
## <a name="code-example"></a>Exemple de code  
 Dans l’exemple suivant, une application exécute une instruction **Select** pour retourner un jeu de résultats des ID de client, des noms et des numéros de téléphone triés par nom, ID et numéro de téléphone. Pour chaque ligne de données, il appelle **SQLFetch** pour positionner le curseur sur la ligne suivante. Il appelle **SQLGetData** pour récupérer les données extraites ; les mémoires tampons pour les données et le nombre d’octets retournés sont spécifiés dans l’appel à **SQLGetData**. Enfin, il affiche le nom, l’ID et le numéro de téléphone de chaque employé.  
  
```cpp  
#define NAME_LEN 50  
#define PHONE_LEN 50  
  
SQLCHAR      szName[NAME_LEN], szPhone[PHONE_LEN];  
SQLINTEGER   sCustID, cbName, cbAge, cbBirthday;  
SQLRETURN    retcode;  
SQLHSTMT     hstmt;  
  
retcode = SQLExecDirect(hstmt,  
   "SELECT CUSTID, NAME, PHONE FROM CUSTOMERS ORDER BY 2, 1, 3",  
   SQL_NTS);  
  
if (retcode == SQL_SUCCESS) {  
   while (TRUE) {  
      retcode = SQLFetch(hstmt);  
      if (retcode == SQL_ERROR || retcode == SQL_SUCCESS_WITH_INFO) {  
         show_error();  
      }  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO){  
  
         /* Get data for columns 1, 2, and 3 */  
  
         SQLGetData(hstmt, 1, SQL_C_ULONG, &sCustID, 0, &cbCustID);  
         SQLGetData(hstmt, 2, SQL_C_CHAR, szName, NAME_LEN, &cbName);  
         SQLGetData(hstmt, 3, SQL_C_CHAR, szPhone, PHONE_LEN,  
            &cbPhone);  
  
         /* Print the row of data */  
  
         fprintf(out, "%-5d %-*s %*s", sCustID, NAME_LEN-1, szName,   
            PHONE_LEN-1, szPhone);  
      } else {  
         break;  
      }  
   }  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Affectation du stockage pour une colonne dans un jeu de résultats|[SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Exécution d’opérations en bloc qui ne sont pas liées à la position du curseur de bloc|[SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Annulation du traitement des instructions|[SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Exécution d’une instruction SQL|[SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécution d’une instruction SQL préparée|[SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Extraction d’une seule ligne de données ou d’un bloc de données dans une direction vers l’avant uniquement|[SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Envoi de données de paramètre au moment de l’exécution|[SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|  
|Positionnement du curseur, actualisation des données dans l’ensemble de lignes ou mise à jour ou suppression de données dans l’ensemble de lignes|[SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Récupération des paramètres de sortie à l’aide de SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
