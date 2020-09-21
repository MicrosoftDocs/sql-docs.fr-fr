---
description: Présentation des types de curseurs
title: Présentation des types de curseurs | Microsoft Docs
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 4f4d3db7-4f76-450d-ab63-141237a4f034
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b47569f0580b8e39ce84b9093dc38718845617c1
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88395905"
---
# <a name="understanding-cursor-types"></a>Présentation des types de curseurs
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Les opérations réalisées dans une base de données relationnelle s'exécutent sur un ensemble complet de lignes. L'ensemble de lignes retourné par une instruction SELECT contient toutes les lignes satisfaisant aux conditions de la clause WHERE de l'instruction. Cet ensemble complet de lignes retournées par l'instruction est appelé ensemble de résultats. Les applications peuvent ne pas toujours fonctionner efficacement si le jeu de résultats est traité comme une unité. Ces applications ont besoin d'un mécanisme leur permettant de travailler avec une seule ligne ou avec un petit bloc de lignes à la fois. Les curseurs sont une extension des ensembles de résultats et fournissent ce mécanisme.  
  
 Les curseurs étendent le traitement des jeux de résultats en effectuant les opérations suivantes :  
  
-   Ils permettent de vous positionner sur des lignes spécifiques de l'ensemble de résultats.  
  
-   Ils extraient une ligne ou un bloc de lignes à partir de la position actuelle dans l'ensemble de résultats.  
  
-   Ils prennent en charge les modifications de données apportées à la ligne à la position actuelle dans le jeu de résultats.  
  
-   Ils prennent en charge différents niveaux de visibilité des modifications apportées par d'autres utilisateurs aux données de la base de données qui figurent dans l'ensemble de résultats.  
  
> [!NOTE]  
>  Pour une description complète des types de curseurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez la rubrique « Types de curseurs (Moteur de base de données) » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 La spécification JDBC fournit la prise en charge des curseurs avant uniquement et des curseurs déroulables qui sont sensibles ou insensibles aux modifications apportées par d'autres travaux et qui peuvent être en lecture seule ou mis à jour. Cette fonctionnalité est fournie par la classe [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)][SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="remarks"></a>Notes  
 Le pilote JDBC prend en charge les types de curseurs suivants :  
  
|Jeu de résultats<br /><br /> de résultats (curseur)|Type de curseur SQL Server|Caractéristiques|select<br /><br /> Méthode|réponse<br /><br /> des réponses|Description|  
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|-----------------|  
|TYPE_FORWARD_ONLY (CONCUR_READ_ONLY)|N/A|Avant uniquement, lecture seule|directes|complet|L'application doit faire un passage unique (en avant) à travers le jeu de résultats. Il s'agit du comportement par défaut, identique à celui d'un curseur TYPE_SS_DIRECT_FORWARD_ONLY. Le pilote lit l'intégralité du jeu de résultats à partir du serveur dans une mémoire durant l'exécution de l'instruction.|  
|TYPE_FORWARD_ONLY (CONCUR_READ_ONLY)|N/A|Avant uniquement, lecture seule|directes|adaptive|L'application doit faire un passage unique (en avant) à travers le jeu de résultats. Son comportement est identique à celui d'un curseur TYPE_SS_DIRECT_FORWARD_ONLY. Le pilote lit des lignes à partir du serveur à mesure que l'application les demande, ce qui réduit l'utilisation de la mémoire côté client.|  
|TYPE_FORWARD_ONLY (CONCUR_READ_ONLY)|Avance rapide|Avant uniquement, lecture seule|cursor|N/A|L'application doit faire un passage unique (en avant) à travers le jeu de résultats en utilisant un curseur côté serveur. Son comportement est identique à celui d'un curseur TYPE_SS_SERVER_CURSOR_FORWARD_ONLY.<br /><br /> Les lignes sont récupérées à partir du serveur en blocs qui sont spécifiés par la taille de l'extraction.|  
|TYPE_FORWARD_ONLY (CONCUR_UPDATABLE)|Dynamique (avant uniquement)|Avant uniquement, pouvant être mis à jour|N/A|N/A|L'application doit faire un passage unique (en avant) à travers le jeu de résultats pour mettre à jour une ou plusieurs lignes.<br /><br /> Les lignes sont récupérées à partir du serveur en blocs qui sont spécifiés par la taille de l'extraction.<br /><br /> Par défaut, la taille de l’extraction est fixe quand l’application appelle la méthode [setFetchSize](../../connect/jdbc/reference/setfetchsize-method-sqlserverresultset.md) de l’objet [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md).<br /><br /> **Remarque :** Le pilote JDBC propose une fonctionnalité de mise en mémoire tampon adaptative qui vous permet de récupérer les résultats de l’exécution des instructions provenant de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] quand l’application en a besoin, et non pas en une seule fois. Par exemple, si une application doit récupérer des données dont la taille est trop importante pour la mémoire de l'application, une mise en mémoire tampon adaptative permet à l'application cliente de récupérer ces données sous forme de flux. Le comportement par défaut du pilote est « **adaptatif** ». Cependant, pour permettre la mise en mémoire tampon adaptative des jeux de résultats de type avant uniquement pouvant être mis à jour, l’application doit appeler explicitement la méthode [setResponseBuffering](../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) de l’objet [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) en fournissant une valeur **chaîne** « **adaptative** ». Pour un exemple de code, consultez [Exemple de mise à jour de données volumineuses](../../connect/jdbc/updating-large-data-sample.md).|  
|TYPE_SCROLL_INSENSITIVE|statique|Déroulable, ne pouvant pas être mis à jour.<br /><br /> Les mises à jour, insertions et suppressions de lignes externes ne sont pas visibles.|N/A|N/A|L'application requiert un instantané de base de données. Le jeu de résultat ne peut pas être mis à jour. Seul CONCUR_READ_ONLY est pris en charge.  Tous les autres types d'accès simultanés provoqueront une exception en cas d'utilisation avec ce type de curseur.<br /><br /> Les lignes sont récupérées à partir du serveur en blocs qui sont spécifiés par la taille de l'extraction.|  
|TYPE_SCROLL_SENSITIVE<br /><br /> (CONCUR_READ_ONLY)|Keyset|Avec défilement, en lecture seule. Les mises à jour de lignes externes sont visibles et les suppressions apparaissent comme données manquantes.<br /><br /> Les insertions de lignes externes ne sont pas visibles.|N/A|N/A|L’application doit voir les données modifiées seulement pour les lignes existantes.<br /><br /> Les lignes sont récupérées à partir du serveur en blocs qui sont spécifiés par la taille de l'extraction.|  
|TYPE_SCROLL_SENSITIVE<br /><br /> (CONCUR_UPDATABLE, CONCUR_SS_SCROLL_LOCKS, CONCUR_SS_OPTIMISTIC_CC, CONCUR_SS_OPTIMISTIC_CCVAL)|Keyset|Déroulable, pouvant être mis à jour.<br /><br /> Les mises à jour de lignes externes et internes sont visibles et les suppressions apparaissent comme des données manquantes ; les insertions ne sont pas visibles.|N/A|N/A|L’application peut modifier des données dans les lignes existantes en utilisant l’objet ResultSet. L’application doit également être en mesure de voir les modifications apportées aux lignes par d’autres utilisateurs depuis l’extérieur de l’objet ResultSet.<br /><br /> Les lignes sont récupérées à partir du serveur en blocs qui sont spécifiés par la taille de l'extraction.|  
|TYPE_SS_DIRECT_FORWARD_ONLY|N/A|Avant uniquement, lecture seule|N/A|complète ou adaptative|Valeur de type entier = 2003. Fournit un curseur côté client en lecture seule entièrement mis en mémoire tampon. Aucun curseur côté serveur n'est créé.<br /><br /> Seul le type d'accès simultané CONCUR_READ_ONLY est pris en charge. Tous les autres types d'accès simultanés provoquent une exception en cas d'utilisation avec ce type de curseur.|  
|TYPE_SS_SERVER_CURSOR_FORWARD_ONLY|Avance rapide|Curseur avant uniquement|N/A|N/A|Valeur de type entier = 2004. Rapide, accède à toutes les données à l'aide d'un curseur côté serveur. Il peut être mis à jour en cas d'utilisation avec le type d'accès simultané CONCUR_UPDATABLE.<br /><br /> Les lignes sont récupérées à partir du serveur en blocs qui sont spécifiés par la taille de l'extraction.<br /><br /> Pour obtenir la mise en mémoire tampon adaptative pour ce cas, l’application doit appeler explicitement la méthode [setResponseBuffering](../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) de l’objet [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) en fournissant une valeur **chaîne** « **adaptative** ». Pour un exemple de code, consultez [Exemple de mise à jour de données volumineuses](../../connect/jdbc/updating-large-data-sample.md).|  
|TYPE_SS_SCROLL_STATIC|statique|Les mises à jour des autres utilisateurs ne sont pas reflétées.|N/A|N/A|Valeur de type entier = 1004. L'application requiert une capture instantanée de base de données. Il s’agit du synonyme spécifique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le curseur JDBC TYPE_SCROLL_INSENSITIVE, avec le même comportement du paramètre d’accès simultané.<br /><br /> Les lignes sont récupérées à partir du serveur en blocs qui sont spécifiés par la taille de l'extraction.|  
|TYPE_SS_SCROLL_KEYSET<br /><br /> (CONCUR_READ_ONLY)|Keyset|Déroulable, lecture seule. Les mises à jour de lignes externes sont visibles et les suppressions apparaissent comme données manquantes.<br /><br /> Les insertions de lignes externes ne sont pas visibles.|N/A|N/A|Valeur de type entier = 1005. L'application doit voir les données modifiées uniquement pour les lignes existantes. Il s’agit du synonyme spécifique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le curseur JDBC TYPE_SCROLL_SENSITIVE, avec le même comportement du paramètre d’accès simultané.<br /><br /> Les lignes sont récupérées à partir du serveur en blocs qui sont spécifiés par la taille de l'extraction.|  
|TYPE_SS_SCROLL_KEYSET<br /><br /> (CONCUR_UPDATABLE, CONCUR_SS_SCROLL_LOCKS, CONCUR_SS_OPTIMISTIC_CC, CONCUR_SS_OPTIMISTIC_CCVAL)|Keyset|Déroulable, pouvant être mis à jour.<br /><br /> Les mises à jour de lignes externes et internes sont visibles et les suppressions apparaissent comme des données manquantes ; les insertions ne sont pas visibles.|N/A|N/A|Valeur de type entier = 1005. L'application doit modifier des données ou voir les données modifiées pour les lignes existantes. Il s’agit du synonyme spécifique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le curseur JDBC TYPE_SCROLL_SENSITIVE, avec le même comportement du paramètre d’accès simultané.<br /><br /> Les lignes sont récupérées à partir du serveur en blocs qui sont spécifiés par la taille de l'extraction.|  
|TYPE_SS_SCROLL_DYNAMIC<br /><br /> (CONCUR_READ_ONLY)|Dynamique|Avec défilement, en lecture seule.<br /><br /> Les mises à jour et les insertions de lignes externes sont visibles et les suppressions apparaissent comme des données manquantes transitoires dans la mémoire tampon d'extraction actuelle.|N/A|N/A|Valeur de type entier = 1006. L'application doit voir les données modifiées pour les lignes existantes et voir les lignes insérées et les lignes supprimées pendant la durée de vie du curseur.<br /><br /> Les lignes sont récupérées à partir du serveur en blocs qui sont spécifiés par la taille de l'extraction.|  
|TYPE_SS_SCROLL_DYNAMIC<br /><br /> (CONCUR_UPDATABLE, CONCUR_SS_SCROLL_LOCKS, CONCUR_SS_OPTIMISTIC_CC, CONCUR_SS_OPTIMISTIC_CCVAL)|Dynamique|Déroulable, pouvant être mis à jour.<br /><br /> Les mises à jour et les insertions de lignes externes et internes sont visibles et les suppressions apparaissent comme des données manquantes transitoires dans la mémoire tampon d'extraction actuelle.|N/A|N/A|Valeur de type entier = 1006. L’application peut modifier des données pour les lignes existantes, ou insérer ou supprimer des lignes en utilisant l’objet ResultSet. L’application doit également être en mesure de voir les modifications, les insertions et les suppressions apportées par d’autres utilisateurs depuis l’extérieur de l’objet ResultSet.<br /><br /> Les lignes sont récupérées à partir du serveur en blocs qui sont spécifiés par la taille de l'extraction.|  
  
## <a name="cursor-positioning"></a>Positionnement du curseur  
 Les curseurs TYPE_FORWARD_ONLY, TYPE_SS_DIRECT_FORWARD_ONLY et TYPE_SS_SERVER_CURSOR_FORWARD_ONLY prennent en charge seulement la méthode de positionnement [next](../../connect/jdbc/reference/next-method-sqlserverresultset.md).  
  
 Le curseur TYPE_SS_SCROLL_DYNAMIC ne prend pas en charge les méthodes [absolute](../../connect/jdbc/reference/absolute-method-sqlserverresultset.md) et [getRow](../../connect/jdbc/reference/getrow-method-sqlserverresultset.md). La méthode absolute peut être évaluée approximativement par une combinaison d’appels aux méthodes [first](../../connect/jdbc/reference/first-method-sqlserverresultset.md) et [relative](../../connect/jdbc/reference/relative-method-sqlserverresultset.md) pour les curseurs dynamiques.  
  
 La méthode getRow est prise en charge seulement par les curseurs TYPE_FORWARD_ONLY, TYPE_SS_DIRECT_FORWARD_ONLY, TYPE_SS_SERVER_CURSOR_FORWARD_ONLY, TYPE_SS_SCROLL_KEYSET et TYPE_SS_SCROLL_STATIC. La méthode getRow avec tous les types de curseurs avant uniquement retourne le nombre de lignes lues jusqu’à maintenant via le curseur.  
  
> [!NOTE]  
>  Quand une application effectue un appel de positionnement de curseur non pris en charge ou un appel non pris en charge à la méthode getRow, une exception est levée avec le message « L’opération demandée n’est pas prise en charge pour ce type de curseur ».  
  
 Seuls le curseur TYPE_SS_SCROLL_KEYSET et le curseur TYPE_SCROLL_SENSITIVE équivalent exposent les lignes supprimées. Si le curseur est positionné sur une ligne supprimée, les valeurs des colonnes ne sont pas disponibles et la méthode [rowDeleted](../../connect/jdbc/reference/rowdeleted-method-sqlserverresultset.md) retourne « true ». Les appels aux méthodes get\<Type> lèvent une exception avec le message « Impossible d’obtenir une valeur à partir d’une ligne supprimée ». Les lignes supprimées ne peuvent pas être mises à jour. Si vous essayez d’appeler une méthode update\<Type> sur une ligne supprimée, une exception est levée avec le message « Une ligne supprimée ne peut pas être mise à jour ». Le curseur TYPE_SS_SCROLL_DYNAMIC a le même comportement jusqu'à ce qu'il soit déplacé hors de la mémoire tampon d'extraction actuelle.  
  
 Les curseurs dynamiques et avant exposent les lignes supprimées de manière semblable, mais uniquement tant qu'ils restent accessibles dans la mémoire tampon d'extraction. Pour les curseurs avant, c'est assez simple. Pour les curseurs dynamiques, c'est plus complexe lorsque la taille de l'extraction est supérieure à 1. Une application peut déplacer le curseur vers l'avant et vers l'arrière dans la fenêtre définie par la mémoire tampon d'extraction, mais la ligne supprimée disparaîtra lorsque la mémoire tampon d'extraction d'origine dans laquelle elle a été mise à jour sera quittée. Si une application ne souhaite pas voir les lignes supprimées temporaires à l'aide de curseurs dynamiques, un relatif d'extraction (0) doit être utilisé.  
  
 Si les valeurs de clés d'une ligne de curseur TYPE_SS_SCROLL_KEYSET ou TYPE_SCROLL_SENSITIVE sont mises à jour avec le curseur, la ligne conserve sa position d'origine dans le jeu de résultats, même si la ligne mise à jour ne satisfait pas aux critères de sélection du curseur. Si la ligne a été mise à jour à l'extérieur du curseur, une ligne supprimée apparaît à la position d'origine de la ligne, mais cette ligne apparaît dans le curseur uniquement si une autre ligne avec les nouvelles valeurs de clés était présente dans le curseur mais a été supprimée depuis.  
  
 Pour les curseurs dynamiques, les lignes mises à jour conservent leur position dans la mémoire tampon d'extraction jusqu'à ce que la fenêtre définie par la mémoire tampon d'extraction soit quittée. Les lignes mises à jour peuvent réapparaître par la suite à des positions différentes dans le jeu de résultats ou peuvent disparaître complètement. Les applications qui doivent éviter les incohérences transitoires dans le jeu de résultats doivent utiliser une taille d'extraction de 1 (la valeur par défaut est 8 lignes avec l'accès simultané CONCUR_SS_SCROLL_LOCKS et 128 lignes avec d'autres accès simultanés).  
  
## <a name="cursor-conversion"></a>Conversion de curseur  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut parfois choisir d’implémenter un type de curseur autre que celui demandé ; ceci s’appelle une conversion de curseur implicite (ou dégradation de curseur). Pour plus d’informations sur la conversion de curseur implicite, consultez la rubrique « Utilisation des conversions implicites de curseurs » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Avec [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)], lorsque vous mettez à jour les données par le biais des jeux de résultats ResultSet.TYPE_SCROLL_SENSITIVE et ResultSet.CONCUR_UPDATABLE, une exception est levée avec un message « Le curseur est READ ONLY ». Cette exception se produit car [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] a effectué une conversion de curseur implicite pour ce jeu de résultats et n’a pas retourné le curseur pouvant être mis à jour qui a été demandé.  
  
 Pour contourner ce problème, vous pouvez choisir l'une des deux solutions suivantes :  
  
-   Assurez-vous que la table sous-jacente a une clé primaire  
  
-   Utilisez [ SQLServerResultSet.TYPE_SS_SCROLL_DYNAMIC](../../connect/jdbc/reference/type-ss-scroll-dynamic-field-sqlserverresultset.md) au lieu de ResultSet.TYPE_SCROLL_SENSITIVE lors de la création d'une instruction.  
  
## <a name="cursor-updating"></a>Mise à jour du curseur  
 Les mises à jour sur place sont prises en charge pour les curseurs où le type de curseur et l'accès simultané prennent en charge les mises à jour. Si le curseur n’est pas positionné sur une ligne pouvant être mise à jour dans le jeu de résultats (aucun appel de méthode get\<Type> n’a réussi), un appel à une méthode update\<Type> lève une exception avec le message « Le jeu de résultat n’a pas de ligne actuelle ». La spécification JDBC stipule qu'une exception survient lorsqu'une méthode de mise à jour est appelée pour une colonne d'un curseur qui est CONCUR_READ_ONLY. Dans les situations où la ligne ne peut pas être mise à jour, par exemple à cause d’un conflit d’accès simultané optimiste comme une mise à jour ou une suppression en concurrence, l’exception peut ne pas survenir avant un appel à [insertRow](../../connect/jdbc/reference/insertrow-method-sqlserverresultset.md), [updateRow](../../connect/jdbc/reference/updaterow-method-sqlserverresultset.md) ou [deleteRow](../../connect/jdbc/reference/deleterow-method-sqlserverresultset.md).  
  
 Après un appel à update\<Type>, la colonne affectée est accessible pour get\<Type> uniquement une fois que updateRow ou [cancelRowUpdates](../../connect/jdbc/reference/cancelrowupdates-method-sqlserverresultset.md) a été appelé. Cela évite les problèmes selon lesquels une colonne est mise à jour en utilisant un type différent du type retourné par le serveur, et les appels d'accesseur Get suivants pourraient appeler des conversions de type côté client donnant des résultats inexacts. Les appels à get\<Type> lèvent alors une exception avec le message « Impossible d’accéder aux colonnes mises à jour tant que updateRow() ou cancelRowUpdates() n’a pas été appelé ».  
  
> [!NOTE]  
>  Si la méthode updateRow est appelée quand aucune colonne n’a été mise à jour, le pilote JDBC lève une exception avec un message indiquant que updateRow() a été appelée alors qu’aucune colonne n’a été mise à jour.  
  
 Après l’appel de [moveToInsertRow](../../connect/jdbc/reference/movetoinsertrow-method-sqlserverresultset.md), une exception est levée si une méthode autre que get\<Type>, update\<Type>, insertRow et des méthodes de positionnement de curseur (y compris [moveToCurrentRow](../../connect/jdbc/reference/movetocurrentrow-method-sqlserverresultset.md)) sont appelées sur le jeu de résultats. La méthode moveToInsertRow place le jeu de résultats en mode insertion et les méthodes de positionnement de curseur mettent fin au mode insertion. Les appels de positionnement de curseur relatif déplacent le curseur relativement à la position où il était avant l’appel de moveToInsertRow. Après les appels de positionnement de curseur, la position de curseur de destination finale devient la nouvelle position de curseur.  
  
 Si l’appel de positionnement de curseur effectué lors du mode insertion ne réussit pas, la position du curseur après l’appel ayant échoué est la position de curseur d’origine avant l’appel de ToInsertRow. Si insertRow échoue, le curseur reste sur la ligne d’insertion et il reste en mode insertion.  
  
 Les colonnes dans la ligne d'insertion sont initialement dans un état non initialisé. Les appels à la méthode update\<Type> définissent l’état de colonne sur « initialisé ». Un appel à la méthode get\<Type> pour une colonne non initialisée lève une exception. Un appel à la méthode insertRow retourne toutes les colonnes dans la ligne d’insertion à un état non initialisé.  
  
 Si des colonnes ne sont pas initialisées lorsque la méthode insertRow est appelée, la valeur par défaut pour la colonne est insérée. S'il n'y a aucune valeur par défaut mais que la colonne est nullable, la valeur NULL est insérée. S'il n'y a aucune valeur par défaut et que la colonne n'est pas nullable, le serveur retourne une erreur et une exception est levée.  
  
> [!NOTE]  
>  Les appels à la méthode getRow retournent 0 en mode insertion.  
>   
>  Le pilote JDBC ne prend pas en charge les mises à jour ou les suppressions positionnées. D’après la spécification JDBC, la méthode [setCursorName](../../connect/jdbc/reference/setcursorname-method-sqlserverstatement.md) n’a aucun effet et la méthode [getCursorName](../../connect/jdbc/reference/getcursorname-method-sqlserverresultset.md) lève une exception si elle est appelée.  
>   
>  Les curseurs en lecture seule et statiques ne peuvent jamais être mis à jour.  
>   
>  SQL Server restreint les curseurs côté serveur à un seul jeu de résultats. Si une procédure de lot ou une procédure stockée contient plusieurs instructions, un curseur client en lecture seule avant uniquement doit être utilisé.  
  
## <a name="see-also"></a>Voir aussi  
 [Gestion des jeux de résultats avec le pilote JDBC](../../connect/jdbc/managing-result-sets-with-the-jdbc-driver.md)  
  
  
