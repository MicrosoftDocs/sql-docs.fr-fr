---
description: Fonction SQLBindCol
title: SQLBindCol, fonction | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLBindCol
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBindCol
helpviewer_keywords:
- SQLBindCol function [ODBC]
ms.assetid: 41a37655-84cd-423f-9daa-e0b47b88dc54
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d057b3e7b5842cafba2be18afa21f2d528175ffd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181091"
---
# <a name="sqlbindcol-function"></a>Fonction SQLBindCol
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLBindCol** lie les tampons de données d’application aux colonnes du jeu de résultats.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLBindCol(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   ColumnNumber,  
      SQLSMALLINT    TargetType,  
      SQLPOINTER     TargetValuePtr,  
      SQLLEN         BufferLength,  
      SQLLEN *       StrLen_or_IndPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *ColumnNumber*  
 Entrée Numéro de la colonne du jeu de résultats à lier. Les colonnes sont numérotées dans l’ordre des colonnes de plus en plus à partir de 0, où la colonne 0 est la colonne de signets. Si les signets ne sont pas utilisés (autrement dit, l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS est défini sur SQL_UB_OFF), les numéros de colonne commencent à 1.  
  
 *TargetType*  
 Entrée Identificateur du type de données C de la \* mémoire tampon *TargetValuePtr* . Lorsqu’il récupère des données de la source de données avec **SQLFetch**, **SQLFetchScroll**, **SQLBulkOperations** ou **SQLSetPos**, le pilote convertit les données en ce type ; lorsqu’il envoie des données à la source de données avec **SQLBulkOperations** ou **SQLSetPos**, le pilote convertit les données de ce type. Pour obtenir la liste des types de données et des identificateurs de type C valides, consultez la section [types de données c](../../../odbc/reference/appendixes/c-data-types.md) dans l’annexe D : types de données.  
  
 Si l’argument *TargetType* est un type de données Interval, la précision de l’intervalle par défaut (2) et la précision de l’intervalle par défaut (6), telles que définies dans les champs SQL_DESC_DATETIME_INTERVAL_PRECISION et SQL_DESC_PRECISION de l’ARD, respectivement, sont utilisées pour les données. Si l’argument *TargetType* est SQL_C_NUMERIC, la précision par défaut (définie par le pilote) et l’échelle par défaut (0), telles que définies dans les champs SQL_DESC_PRECISION et SQL_DESC_SCALE de ARD, sont utilisées pour les données. Si une précision ou une échelle par défaut n’est pas appropriée, l’application doit définir explicitement le champ de descripteur approprié à l’aide d’un appel à **SQLSetDescField** ou **SQLSetDescRec**.  
  
 Vous pouvez également spécifier un type de données C étendu. Pour plus d’informations, consultez [types de données C dans ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).  
  
 *TargetValuePtr*  
 [Entrée/sortie différée] Pointeur vers la mémoire tampon de données à lier à la colonne. **SQLFetch** et **SQLFetchScroll** retournent des données dans cette mémoire tampon. **SQLBulkOperations** retourne les données de cette mémoire tampon lorsque l' *opération* est SQL_FETCH_BY_BOOKMARK ; elle récupère les données de cette mémoire tampon lorsque l' *opération* est SQL_ADD ou SQL_UPDATE_BY_BOOKMARK. **SQLSetPos** retourne des données dans cette mémoire tampon lorsque l' *opération* est SQL_REFRESH ; elle récupère les données de cette mémoire tampon lorsque l' *opération* est SQL_UPDATE.  
  
 Si *TargetValuePtr* est un pointeur null, le pilote dissocie la mémoire tampon de données pour la colonne. Une application peut dissocier toutes les colonnes en appelant **SQLFreeStmt** à l’aide de l’option SQL_UNBIND. Une application peut dissocier la mémoire tampon de données pour une colonne tout en gardant une limite de mémoire tampon de longueur/d’indicateur pour la colonne, si l’argument *TargetValuePtr* dans l’appel à **SQLBindCol** est un pointeur null, mais que l’argument *StrLen_or_IndPtr* est une valeur valide.  
  
 *BufferLength*  
 Entrée Longueur \* en octets de la mémoire tampon *TargetValuePtr* .  
  
 Le pilote utilise *BufferLength* pour éviter d’écrire au-delà de la fin de la \* mémoire tampon *TargetValuePtr* lorsqu’il retourne des données de longueur variable, telles que des données de type caractère ou binaire. Notez que le pilote compte le caractère de fin null lorsqu’il retourne des données de type caractère à \* *TargetValuePtr*. \**TargetValuePtr* doit donc contenir de l’espace pour le caractère de fin null ou le pilote va tronquer les données.  
  
 Lorsque le pilote retourne des données de longueur fixe, telles qu’un entier ou une structure de date, le pilote ignore *BufferLength* et suppose que la mémoire tampon est suffisamment grande pour contenir les données. Par conséquent, il est important pour l’application d’allouer une mémoire tampon suffisamment importante pour les données de longueur fixe ou le pilote écrit au-delà de la fin de la mémoire tampon.  
  
 **SQLBindCol** retourne SQLState HY090 (longueur de chaîne ou de mémoire tampon non valide) lorsque *BufferLength* est inférieur à 0 mais pas lorsque *BufferLength* est égal à 0. Toutefois, si *TargetType* spécifie un type de caractère, une application ne doit pas affecter la valeur 0 à *BufferLength* , car les pilotes compatibles ISO CLI retournent SQLSTATE HY090 (chaîne non valide ou longueur de la mémoire tampon) dans ce cas.  
  
 *StrLen_or_IndPtr*  
 [Entrée/sortie différée] Pointeur vers la mémoire tampon de longueur/d’indicateur à lier à la colonne. **SQLFetch** et **SQLFetchScroll** retournent une valeur dans cette mémoire tampon. **SQLBulkOperations** récupère une valeur de cette mémoire tampon lorsque l' *opération* est SQL_ADD, SQL_UPDATE_BY_BOOKMARK ou SQL_DELETE_BY_BOOKMARK. **SQLBulkOperations** retourne une valeur dans cette mémoire tampon lorsque l' *opération* est SQL_FETCH_BY_BOOKMARK. **SQLSetPos** retourne une valeur dans cette mémoire tampon lorsque l' *opération* est SQL_REFRESH ; elle récupère une valeur de cette mémoire tampon lorsque l' *opération* est SQL_UPDATE.  
  
 **SQLFetch**, **SQLFetchScroll**, **SQLBulkOperations** et **SQLSetPos** peuvent retourner les valeurs suivantes dans le tampon longueur/indicateur :  
  
-   La longueur des données disponibles pour le retour  
  
-   SQL_NO_TOTAL  
  
-   SQL_NULL_DATA  
  
 L’application peut placer les valeurs suivantes dans la mémoire tampon de longueur/indicateur pour une utilisation avec **SQLBulkOperations** ou **SQLSetPos**:  
  
-   La longueur des données envoyées  
  
-   SQL_NTS  
  
-   SQL_NULL_DATA  
  
-   SQL_DATA_AT_EXEC  
  
-   Résultat de la macro SQL_LEN_DATA_AT_EXEC  
  
-   SQL_COLUMN_IGNORE  
  
 Si la mémoire tampon d’indicateur et la mémoire tampon de longueur sont des mémoires tampons distinctes, le tampon d’indicateur peut retourner uniquement SQL_NULL_DATA, tandis que la mémoire tampon de longueur peut retourner toutes les autres valeurs.  
  
 Pour plus d’informations, consultez [fonction SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md), [fonction SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md), [fonction SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)et [utilisation de valeurs de longueur/indicateur](../../../odbc/reference/develop-app/using-length-and-indicator-values.md).  
  
 Si *StrLen_or_IndPtr* est un pointeur null, aucune valeur de longueur ou d’indicateur n’est utilisée. Il s’agit d’une erreur lors de la récupération de données et les données ont la valeur NULL.  
  
 Consultez [ODBC 64-informations sur ODBC](../../../odbc/reference/odbc-64-bit-information.md), si votre application s’exécute sur un système d’exploitation 64 bits.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLBindCol** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLBindCol** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07006|Violation d’attribut de type de données restreint|(DM) l’argument *ColumnNumber* était 0 et l’argument *TargetType* n’était pas SQL_C_BOOKMARK ou SQL_C_VARBOOKMARK.|  
|07009|Index de descripteur non valide|La valeur spécifiée pour l’argument *ColumnNumber* a dépassé le nombre maximal de colonnes dans le jeu de résultats.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY003|Type de tampon d’application non valide|L’argument *TargetType* n’était ni un type de données valide, ni SQL_C_DEFAULT.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lorsque **SQLBindCol** était appelé.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur spécifiée pour l’argument *BufferLength* est inférieure à 0.<br /><br /> (DM) le pilote était un ODBC 2. *x* , l’argument *ColumnNumber* a été défini sur 0, et la valeur spécifiée pour l’argument *BufferLength* n’était pas égale à 4.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Le pilote ou la source de données ne prend pas en charge la conversion spécifiée par la combinaison de l’argument *TargetType* et du type de données SQL spécifique au pilote de la colonne correspondante.<br /><br /> L’argument *ColumnNumber* était 0 et le pilote ne prend pas en charge les signets.<br /><br /> Le pilote prend en charge uniquement ODBC 2. *x* et l’argument *TargetType* était l’un des éléments suivants :<br /><br /> SQL_C_NUMERIC SQL_C_SBIGINT SQL_C_UBIGINT<br /><br /> et tous les types de données Interval C listés dans les [types de données c](../../../odbc/reference/appendixes/c-data-types.md) de l’annexe D : types de données.<br /><br /> Le pilote ne prend en charge que les versions ODBC antérieures à 3,50, et l’argument *TargetType* était SQL_C_GUID.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 **SQLBindCol** est utilisé pour associer, ou *lier,* les colonnes du jeu de résultats aux tampons de données et aux mémoires tampons de longueur/d’indicateur dans l’application. Lorsque l’application appelle **SQLFetch**, **SQLFetchScroll** ou **SQLSetPos** pour extraire des données, le pilote retourne les données pour les colonnes dépendantes dans les mémoires tampons spécifiées ; Pour plus d’informations, consultez la [fonction SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md). Lorsque l’application appelle **SQLBulkOperations** pour mettre à jour ou insérer une ligne ou **SQLSetPos** pour mettre à jour une ligne, le pilote récupère les données pour les colonnes dépendantes à partir des mémoires tampons spécifiées ; Pour plus d’informations, consultez [fonction SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md) ou [fonction SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md). Pour plus d’informations sur la liaison, consultez [récupération des résultats (de base)](../../../odbc/reference/develop-app/retrieving-results-basic.md).  
  
 Notez que les colonnes n’ont pas besoin d’être liées pour récupérer des données à partir de celles-ci. Une application peut également appeler **SQLGetData** pour récupérer des données à partir de colonnes. Bien qu’il soit possible de lier des colonnes dans une ligne et d’appeler **SQLGetData** pour d’autres, cela peut être soumis à certaines restrictions. Pour plus d’informations, consultez [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md).  
  
## <a name="binding-unbinding-and-rebinding-columns"></a>Liaison, dissociation et reliaison de colonnes  
 Une colonne peut être liée, détachée ou reliée à tout moment, même après que les données ont été extraites du jeu de résultats. La nouvelle liaison prend effet la prochaine fois qu’une fonction qui utilise des liaisons est appelée. Supposons, par exemple, qu’une application lie les colonnes d’un jeu de résultats et appelle **SQLFetch**. Le pilote retourne les données dans les mémoires tampons liées. Supposons à présent que l’application lie les colonnes à un autre ensemble de mémoires tampons. Le pilote ne place pas les données de la ligne juste-à-extraite dans les mémoires tampons nouvellement liées. Au lieu de cela, il attend que **SQLFetch** soit rappelé, puis place les données de la ligne suivante dans les mémoires tampons nouvellement liées.  
  
> [!NOTE]  
>  L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS doit toujours être défini avant la liaison d’une colonne à la colonne 0. Ce n’est pas obligatoire, mais il est fortement recommandé.  
  
## <a name="binding-columns"></a>Liaison de colonnes  
 Pour lier une colonne, une application appelle **SQLBindCol** et transmet le numéro de colonne, le type, l’adresse et la longueur d’une mémoire tampon de données, ainsi que l’adresse d’une mémoire tampon de longueur/d’indicateur. Pour plus d’informations sur l’utilisation de ces adresses, consultez « adresses de tampon » plus loin dans cette section. Pour plus d’informations sur les colonnes de liaison, consultez [utilisation de SQLBindCol](../../../odbc/reference/develop-app/using-sqlbindcol.md).  
  
 L’utilisation de ces mémoires tampons est différée ; autrement dit, l’application les lie à **SQLBindCol** , mais le pilote y accède à partir d’autres fonctions, à savoir **SQLBulkOperations**, **SQLFetch**, **SQLFetchScroll** ou **SQLSetPos**. Il incombe à l’application de s’assurer que les pointeurs spécifiés dans **SQLBindCol** restent valides tant que la liaison reste en vigueur. Si l’application autorise ces pointeurs à devenir non valides, par exemple, elle libère une mémoire tampon, puis appelle une fonction qui s’attend à ce qu’elle soit valide, les conséquences ne sont pas définies. Pour plus d’informations, consultez [mémoires tampons différées](../../../odbc/reference/develop-app/deferred-buffers.md).  
  
 La liaison reste en vigueur jusqu’à ce qu’elle soit remplacée par une nouvelle liaison, que la colonne soit détachée ou que l’instruction soit libérée.  
  
## <a name="unbinding-columns"></a>Dissocier des colonnes  
 Pour annuler la liaison d’une seule colonne, une application appelle **SQLBindCol** avec *ColumnNumber* défini sur le numéro de cette colonne et *TargetValuePtr* défini sur un pointeur null. Si *ColumnNumber* fait référence à une colonne indépendante, **SQLBindCol** retourne toujours SQL_SUCCESS.  
  
 Pour dissocier toutes les colonnes, une application appelle **SQLFreeStmt** avec *fOption* défini sur SQL_UNBIND. Pour ce faire, vous pouvez également définir le champ SQL_DESC_COUNT de ARD sur zéro.  
  
## <a name="rebinding-columns"></a>Reliaison de colonnes  
 Une application peut effectuer l’une des deux opérations suivantes pour modifier une liaison :  
  
-   Appelez **SQLBindCol** pour spécifier une nouvelle liaison pour une colonne qui est déjà liée. Le pilote remplace l’ancienne liaison par la nouvelle.  
  
-   Spécifiez un décalage à ajouter à l’adresse tampon spécifiée par l’appel de liaison à **SQLBindCol**. Pour plus d’informations, consultez la section suivante, « offsets de liaison ».  
  
## <a name="binding-offsets"></a>Décalages de liaison  
 Un décalage de liaison est une valeur qui est ajoutée aux adresses des tampons de données et de longueur/indicateur (comme spécifié dans les arguments *TargetValuePtr* et *StrLen_or_IndPtr* ) avant d’être déréférencés. Lorsque des décalages sont utilisés, les liaisons sont un « modèle » de la façon dont les tampons de l’application sont disposés, et l’application peut déplacer ce « modèle » dans différentes zones de mémoire en modifiant le décalage. Étant donné que le même décalage est ajouté à chaque adresse dans chaque liaison, les décalages relatifs entre les mémoires tampons pour les différentes colonnes doivent être identiques dans chaque ensemble de mémoires tampons. C’est toujours le cas lorsque la liaison selon les lignes est utilisée ; l’application doit disposer soigneusement ses tampons pour que cela soit vrai lorsque la liaison selon les colonnes est utilisée.  
  
 L’utilisation d’un décalage de liaison a fondamentalement le même effet que la reliaison d’une colonne en appelant **SQLBindCol**. La différence est qu’un nouvel appel à **SQLBindCol** spécifie de nouvelles adresses pour la mémoire tampon de données et la mémoire tampon de longueur/d’indicateur, tandis que l’utilisation d’un offset de liaison ne modifie pas les adresses, mais leur ajoute simplement un offset. L’application peut spécifier un nouveau décalage à chaque fois qu’elle le souhaite, et ce décalage est toujours ajouté aux adresses à liaison à l’origine. En particulier, si le décalage est défini sur 0 ou si l’attribut d’instruction est défini sur un pointeur null, le pilote utilise les adresses à liaison à l’origine.  
  
 Pour spécifier un décalage de liaison, l’application définit l’attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR sur l’adresse d’une mémoire tampon SQLINTEGER destinée. Avant que l’application appelle une fonction qui utilise des liaisons, elle place un offset en octets dans cette mémoire tampon. Pour déterminer l’adresse de la mémoire tampon à utiliser, le pilote ajoute le décalage à l’adresse dans la liaison. La somme de l’adresse et du décalage doit être une adresse valide, mais il n’est pas nécessaire que l’adresse à laquelle le décalage est ajouté soit valide. Pour plus d’informations sur l’utilisation des décalages de liaison, consultez « adresses de tampon » plus loin dans cette section.  
  
## <a name="binding-arrays"></a>Tableaux de liaison  
 Si la taille de l’ensemble de lignes (valeur de l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE) est supérieure à 1, l’application lie des tableaux de mémoires tampons au lieu de mémoires tampons uniques. Pour plus d’informations, consultez [bloquer les curseurs](../../../odbc/reference/develop-app/block-cursors.md).  
  
 L’application peut lier des tableaux de deux manières :  
  
-   Liez un tableau à chaque colonne. C’est ce que l’on appelle une liaison selon les *colonnes* , car chaque structure de données (tableau) contient des données pour une colonne unique.  
  
-   Définissez une structure qui contiendra les données pour une ligne entière et liez un tableau de ces structures. C’est ce que l’on appelle une liaison selon les *lignes* , car chaque structure de données contient les données d’une seule ligne.  
  
 Chaque tableau de mémoires tampons doit avoir au moins autant d’éléments que la taille de l’ensemble de lignes.  
  
> [!NOTE]  
>  Une application doit vérifier que l’alignement est valide. Pour plus d’informations sur les considérations d’alignement, consultez [alignment](../../../odbc/reference/develop-app/alignment.md).  
  
## <a name="column-wise-binding"></a>Liaison selon les colonnes  
 Dans une liaison selon les colonnes, l’application lie des données distinctes et des tableaux de longueur/indicateur à chaque colonne.  
  
 Pour utiliser la liaison selon les colonnes, l’application définit d’abord l’attribut d’instruction SQL_ATTR_ROW_BIND_TYPE sur SQL_BIND_BY_COLUMN. (Il s’agit de la valeur par défaut.) Pour chaque colonne à lier, l’application effectue les étapes suivantes :  
  
1.  Alloue un tableau de mémoires tampons de données.  
  
2.  Alloue un tableau de mémoires tampons de longueur/d’indicateur.  
  
    > [!NOTE]  
    >  Si l’application écrit directement dans descripteurs lorsque la liaison selon les colonnes est utilisée, des tableaux distincts peuvent être utilisés pour les données de longueur et d’indicateur.  
  
3.  Appelle **SQLBindCol** avec les arguments suivants :  
  
    -   *TargetType* est le type d’un élément unique dans le tableau de mémoires tampons de données.  
  
    -   *TargetValuePtr* est l’adresse du tableau de tampons de données.  
  
    -   *BufferLength* est la taille d’un élément unique dans le tableau de mémoires tampons de données. L’argument *BufferLength* est ignoré lorsque les données sont de longueur fixe.  
  
    -   *StrLen_or_IndPtr* est l’adresse du tableau longueur/indicateur.  
  
 Pour plus d’informations sur l’utilisation de ces informations, consultez « adresses de tampon » plus loin dans cette section. Pour plus d’informations sur la liaison selon les colonnes, consultez [liaison selon les colonnes](../../../odbc/reference/develop-app/column-wise-binding.md).  
  
## <a name="row-wise-binding"></a>Liaison selon les lignes  
 Dans une liaison selon les lignes, l’application définit une structure qui contient des tampons de données et de longueur/d’indicateur pour chaque colonne à lier.  
  
 Pour utiliser la liaison selon les lignes, l’application effectue les étapes suivantes :  
  
1.  Définit une structure pour contenir une seule ligne de données (y compris les mémoires tampons de longueur/d’indicateur) et alloue un tableau de ces structures.  
  
    > [!NOTE]  
    >  Si l’application écrit directement dans descripteurs lorsque la liaison selon les lignes est utilisée, des champs séparés peuvent être utilisés pour les données de longueur et d’indicateur.  
  
2.  Affecte à l’attribut d’instruction SQL_ATTR_ROW_BIND_TYPE la taille de la structure qui contient une seule ligne de données ou la taille d’une instance d’une mémoire tampon dans laquelle les colonnes de résultats seront liées. La longueur doit inclure de l’espace pour toutes les colonnes liées, ainsi que tout remplissage de la structure ou de la mémoire tampon, pour s’assurer que lorsque l’adresse d’une colonne liée est incrémentée avec la longueur spécifiée, le résultat pointe vers le début de la même colonne dans la ligne suivante. Lorsque vous utilisez l’opérateur *sizeof* en C ANSI, ce comportement est garanti.  
  
3.  Appelle **SQLBindCol** avec les arguments suivants pour chaque colonne à lier :  
  
    -   *TargetType* est le type du membre de la mémoire tampon de données à lier à la colonne.  
  
    -   *TargetValuePtr* est l’adresse du membre de tampon de données dans le premier élément de tableau.  
  
    -   *BufferLength* est la taille du membre de tampon de données.  
  
    -   *StrLen_or_IndPtr* est l’adresse du membre de longueur/indicateur à lier.  
  
 Pour plus d’informations sur l’utilisation de ces informations, consultez « adresses de tampon » plus loin dans cette section. Pour plus d’informations sur la liaison selon les colonnes, consultez [liaison selon les lignes](../../../odbc/reference/develop-app/row-wise-binding.md).  
  
## <a name="buffer-addresses"></a>Adresses de mémoire tampon  
 L' *adresse de la mémoire tampon* correspond à l’adresse réelle de la mémoire tampon de données ou de longueur/indicateur. Le pilote calcule l’adresse de la mémoire tampon juste avant d’écrire dans les tampons (par exemple pendant l’exécution de l’extraction). Elle est calculée à partir de la formule suivante, qui utilise les adresses spécifiées dans les arguments *TargetValuePtr* et *StrLen_or_IndPtr* , le décalage de liaison et le numéro de ligne :  
  
 *Adresse liée*  +  *Décalage de liaison* + ((*numéro de ligne* -1) x taille de l' *élément*)  
  
 où les variables de la formule sont définies comme décrit dans le tableau suivant.  
  
|Variable|Description|  
|--------------|-----------------|  
|*Adresse liée*|Pour les mémoires tampons de données, adresse spécifiée avec l’argument *TargetValuePtr* dans **SQLBindCol**.<br /><br /> Pour les mémoires tampons de longueur/d’indicateur, adresse spécifiée avec l’argument *StrLen_or_IndPtr* dans **SQLBindCol**. Pour plus d’informations, consultez « commentaires supplémentaires » dans la section « descripteurs et SQLBindCol ».<br /><br /> Si l’adresse liée est 0, aucune valeur de données n’est retournée, même si l’adresse telle que calculée par la formule précédente est différente de zéro.|  
|*Décalage de liaison*|Si la liaison selon les lignes est utilisée, il s’agit de la valeur stockée à l’adresse spécifiée avec l’attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR.<br /><br /> Si la liaison selon les colonnes est utilisée ou si la valeur de l’attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR est un pointeur null, l' *offset de liaison* est 0.|  
|*Numéro de ligne*|Numéro de base 1 de la ligne dans l’ensemble de lignes. Pour les extractions sur une seule ligne, il s’agit de la valeur par défaut 1.|  
|*Taille de l’élément*|Taille d’un élément dans le tableau lié.<br /><br /> Si la liaison selon les colonnes est utilisée, il s’agit de **sizeof (SQLINTEGER destinée)** pour les mémoires tampons de longueur/indicateur. Pour les mémoires tampons de données, il s’agit de la valeur de l’argument *BufferLength* dans **SQLBindCol** si le type de données est de longueur variable et de la taille du type de données si la longueur est fixe.<br /><br /> Si la liaison selon les lignes est utilisée, il s’agit de la valeur de l’attribut d’instruction SQL_ATTR_ROW_BIND_TYPE pour les tampons de données et de longueur/d’indicateur.|  
  
## <a name="descriptors-and-sqlbindcol"></a>Descripteurs et SQLBindCol  
 Les sections suivantes décrivent comment **SQLBindCol** interagit avec les descripteurs.  
  
> [!CAUTION]  
>  L’appel de **SQLBindCol** pour une instruction peut affecter d’autres instructions. Cela se produit lorsque le ARD associé à l’instruction est explicitement alloué et est également associé à d’autres instructions. Étant donné que **SQLBindCol** modifie le descripteur, les modifications s’appliquent à toutes les instructions auxquelles ce descripteur est associé. Si ce n’est pas le comportement requis, l’application doit dissocier ce descripteur des autres instructions avant d’appeler **SQLBindCol**.  
  
## <a name="argument-mappings"></a>Mappages d’arguments  
 D’un plan conceptuel, **SQLBindCol** effectue les étapes suivantes dans l’ordre :  
  
1.  Appelle **SQLGetStmtAttr** pour obtenir le handle ARD.  
  
2.  Appelle **SQLGetDescField** pour récupérer le champ SQL_DESC_COUNT de ce descripteur, et si la valeur de l’argument *ColumnNumber* dépasse la valeur de SQL_DESC_COUNT, appelle **SQLSetDescField** pour augmenter la valeur de SQL_DESC_COUNT à *ColumnNumber*.  
  
3.  Appelle **SQLSetDescField** plusieurs fois pour affecter des valeurs aux champs suivants du ARD :  
  
    -   Définit SQL_DESC_TYPE et SQL_DESC_CONCISE_TYPE sur la valeur de *TargetType*, sauf que si *TargetType* est l’un des identificateurs concis d’un sous-type DateTime ou interval, il définit SQL_DESC_TYPE sur SQL_DATETIME ou SQL_INTERVAL, respectivement ; définit SQL_DESC_CONCISE_TYPE sur l’identificateur concis ; et définit SQL_DESC_DATETIME_INTERVAL_CODE sur le sous-code datetime ou Interval correspondant.  
  
    -   Définit un ou plusieurs SQL_DESC_LENGTH, SQL_DESC_PRECISION, SQL_DESC_SCALE et SQL_DESC_DATETIME_INTERVAL_PRECISION, en fonction du *TargetType*.  
  
    -   Définit le champ SQL_DESC_OCTET_LENGTH sur la valeur de *BufferLength*.  
  
    -   Définit le champ SQL_DESC_DATA_PTR sur la valeur de *TargetValuePtr*.  
  
    -   Affecte la valeur de *StrLen_or_IndPtr* au champ SQL_DESC_INDICATOR_PTR. (Voir le paragraphe suivant).  
  
    -   Affecte la valeur de *StrLen_or_IndPtr* au champ SQL_DESC_OCTET_LENGTH_PTR. (Voir le paragraphe suivant).  
  
 La variable à laquelle l’argument *StrLen_or_IndPtr* fait référence est utilisée à la fois pour les informations relatives aux indicateurs et à la longueur. Si une instruction FETCH rencontre une valeur null pour la colonne, elle stocke SQL_NULL_DATA dans cette variable ; dans le cas contraire, elle stocke la longueur des données dans cette variable. Le passage d’un pointeur null en tant que *StrLen_or_IndPtr* empêche l’opération d’extraction de retourner la longueur des données, mais fait échouer l’extraction si elle rencontre une valeur null et n’a aucun moyen de retourner SQL_NULL_DATA.  
  
 Si l’appel à **SQLBindCol** échoue, le contenu des champs de descripteur qu’il aurait définis dans le ARD n’est pas défini et la valeur du champ SQL_DESC_COUNT du ARD est inchangée.  
  
## <a name="implicit-resetting-of-count-field"></a>Réinitialisation implicite du champ de nombre  
 **SQLBindCol** définit SQL_DESC_COUNT à la valeur de l’argument *ColumnNumber* uniquement lorsque cela augmente la valeur de SQL_DESC_COUNT. Si la valeur de l’argument *TargetValuePtr* est un pointeur null et que la valeur de l’argument *ColumnNumber* est égale à SQL_DESC_COUNT (autrement dit, lors de la dissociation de la colonne liée la plus élevée), SQL_DESC_COUNT est défini sur le numéro de la colonne dépendante la plus élevée.  
  
## <a name="cautions-regarding-sql_default"></a>Précautions relatives à SQL_DEFAULT  
 Pour que les données des colonnes soient correctement récupérées, l’application doit déterminer correctement la longueur et le point de départ des données dans la mémoire tampon de l’application. Lorsque l’application spécifie un *TargetType* explicite, les conceptions erronées de l’application sont facilement détectées. Toutefois, lorsque l’application spécifie un *TargetType* de SQL_DEFAULT, **SQLBindCol** peut être appliqué à une colonne d’un type de données différent de celui prévu par l’application, soit à partir de modifications apportées aux métadonnées, soit en appliquant le code à une autre colonne. Dans ce cas, l’application peut ne pas toujours déterminer le début ou la longueur des données de la colonne extraite. Cela peut entraîner des erreurs de données ou des violations de mémoire qui ne sont pas signalées.  
  
## <a name="code-example"></a>Exemple de code  
 Dans l’exemple suivant, une application exécute une instruction **Select** sur la table Customers pour retourner un jeu de résultats des ID de client, des noms et des numéros de téléphone, triés par nom. Il appelle ensuite **SQLBindCol** pour lier les colonnes de données aux mémoires tampons locales. Enfin, l’application extrait chaque ligne de données avec **SQLFetch** et imprime le nom, l’ID et le numéro de téléphone de chaque client.  
  
 Pour obtenir plus d’exemples de code, consultez [fonction SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md), fonction [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md), [fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)et [fonction SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
```cpp  
// SQLBindCol_ref.cpp  
// compile with: odbc32.lib  
#include <windows.h>  
#include <stdio.h>  
  
#define UNICODE  
#include <sqlext.h>  
  
#define NAME_LEN 50  
#define PHONE_LEN 60
  
void show_error() {  
   printf("error\n");  
}  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt = 0;  
   SQLRETURN retcode;  
   SQLWCHAR szName[NAME_LEN], szPhone[PHONE_LEN], sCustID[NAME_LEN];  
   SQLLEN cbName = 0, cbCustID = 0, cbPhone = 0;  
  
   // Allocate environment handle  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set the ODBC version environment attribute  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
      // Allocate connection handle  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
  
         // Set login timeout to 5 seconds  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
            // Connect to data source  
            retcode = SQLConnect(hdbc, (SQLWCHAR*) L"NorthWind", SQL_NTS, (SQLWCHAR*) NULL, 0, NULL, 0);  
  
            // Allocate statement handle  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {   
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);   
  
               retcode = SQLExecDirect(hstmt, (SQLWCHAR *) L"SELECT CustomerID, ContactName, Phone FROM CUSTOMERS ORDER BY 2, 1, 3", SQL_NTS);  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
                  // Bind columns 1, 2, and 3  
                  retcode = SQLBindCol(hstmt, 1, SQL_C_WCHAR, &sCustID, 100, &cbCustID);  
                  retcode = SQLBindCol(hstmt, 2, SQL_C_WCHAR, szName, NAME_LEN, &cbName);  
                  retcode = SQLBindCol(hstmt, 3, SQL_C_WCHAR, szPhone, PHONE_LEN, &cbPhone);   
  
                  // Fetch and print each row of data. On an error, display a message and exit.  
                  for (int i=0 ; ; i++) {  
                     retcode = SQLFetch(hstmt);  
                     if (retcode == SQL_ERROR || retcode == SQL_SUCCESS_WITH_INFO)  
                        show_error();  
                     if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
                     {
                        //replace wprintf with printf
                        //%S with %ls
                        //warning C4477: 'wprintf' : format string '%S' requires an argument of type 'char *'
                        //but variadic argument 2 has type 'SQLWCHAR *'
                        //wprintf(L"%d: %S %S %S\n", i + 1, sCustID, szName, szPhone);  
                        printf("%d: %ls %ls %ls\n", i + 1, sCustID, szName, szPhone);  
                    }    
                     else  
                        break;  
                  }  
               }  
  
               // Process data  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
                  SQLCancel(hstmt);  
                  SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
               }  
  
               SQLDisconnect(hdbc);  
            }  
  
            SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
         }  
      }  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
   }  
}  
```  
  
 Consultez également [exemple de programme ODBC](../../../odbc/reference/sample-odbc-program.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Retour d’informations sur une colonne dans un jeu de résultats|[Fonction SQLDescribeCol](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Extraction de plusieurs lignes de données|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Libération de mémoires tampons de colonne sur l’instruction|[Fonction SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Extraction d’une partie ou de la totalité d’une colonne de données|[Fonction SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|Retour du nombre de colonnes du jeu de résultats|[Fonction SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
