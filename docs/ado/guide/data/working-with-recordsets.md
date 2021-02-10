---
description: Utilisation des recordsets
title: Utilisation des recordsets | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset object [ADO]
ms.assetid: bdf9a56a-de4a-44de-9111-2f11ab7b16ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 4e64608adc461534a30630a6b8b8d29d0478544a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032251"
---
# <a name="working-with-recordsets"></a>Utilisation des recordsets
L’objet **Recordset** offre des fonctionnalités intégrées qui vous permettent de réorganiser l’ordre des données dans le jeu de résultats, de rechercher un enregistrement spécifique en fonction des critères que vous fournissez, et même d’optimiser ces opérations de recherche à l’aide d’index. Le fait que ces fonctionnalités soient disponibles dépend du fournisseur et, dans certains cas, de la propriété [index](../../../ado/reference/ado-api/index-property.md) (la structure de la source de données elle-même).  
  
## <a name="arranging-data"></a>Organisation des données  
 Souvent, la méthode la plus efficace pour trier les données dans votre **jeu d’enregistrements** consiste à spécifier une clause ORDER BY dans la commande SQL utilisée pour y retourner les résultats. Toutefois, vous devrez peut-être modifier l’ordre des données dans un **jeu d’enregistrements** qui a déjà été créé. Vous pouvez utiliser la propriété **sort** pour déterminer l’ordre dans lequel les lignes d’un **jeu d’enregistrements** sont parcourues. En outre, la propriété **Filter** détermine les lignes accessibles lors du parcours de lignes.  
  
 La propriété de **Tri** définit ou retourne une valeur de **chaîne** qui indique les noms de champs dans le **jeu d’enregistrements** sur lequel effectuer le tri. Chaque nom est séparé par une virgule et est éventuellement suivi d’un espace et du mot clé **ASC** (qui trie le champ par ordre croissant) ou **desc** (qui trie le champ dans l’ordre décroissant). Par défaut, si aucun mot clé n’est spécifié, le champ est trié dans l’ordre croissant.  
  
 L’opération de tri est efficace, car les données ne sont pas réorganisées physiquement, mais sont accessibles dans l’ordre spécifié par un index.  
  
 La propriété **sort** requiert la définition de la propriété [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) sur **adUseClient**. Un index temporaire sera créé pour chaque champ spécifié dans la propriété de **Tri** si un index n’existe pas déjà.  
  
 Si vous affectez une chaîne vide à la propriété de **Tri** , les lignes sont réinitialisées dans leur ordre d’origine et les index temporaires sont supprimés. Les index existants ne seront pas supprimés.  
  
 Supposons qu’un **Recordset** contienne trois champs nommés *FirstName*, *MiddleInitial* et *LastName*. Définissez la propriété de **Tri** sur la chaîne « `lastName DESC, firstName ASC` », ce qui permet d’ordonner le **jeu d’enregistrements** par nom de famille dans l’ordre décroissant, puis par le prénom dans l’ordre croissant. L’initiale du deuxième prénom est ignorée.  
  
 Aucun champ référencé dans une chaîne de critères de tri ne peut être nommé « ASC » ou « DESC », car ces noms sont en conflit avec les mots clés **ASC** et **desc**. Donnez à un champ qui a un nom en conflit un alias à l’aide du mot clé **As** dans la requête qui retourne le **Recordset**.  
  
 Pour plus d’informations sur le filtrage du **Recordset** , consultez « Filtrage des résultats » plus loin dans cette rubrique.  
  
## <a name="finding-a-specific-record"></a>Recherche d’un enregistrement spécifique  
 ADO fournit les méthodes de [recherche](../../../ado/reference/ado-api/find-method-ado.md) et [de recherche pour](../../../ado/reference/ado-api/seek-method.md) localiser un enregistrement particulier dans un **jeu d’enregistrements**. La méthode **Find** est prise en charge par divers fournisseurs, mais est limitée à un seul critère de recherche. La méthode **Seek** prend en charge la recherche sur plusieurs critères, mais n’est pas prise en charge par de nombreux fournisseurs.  
  
 Les index sur les champs peuvent améliorer les performances de la méthode **Find** et des propriétés de **Tri** et de **filtre** de l’objet **Recordset** . Vous pouvez créer un index interne pour un objet de **champ** en définissant sa propriété [optimisation](../../../ado/reference/ado-api/optimize-property-dynamic-ado.md) dynamique. Cette propriété dynamique est ajoutée à la collection **Properties** de l’objet **Field** lorsque vous affectez à la propriété [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) la valeur **adUseClient**. N’oubliez pas que cet index est interne à ADO. vous ne pouvez pas y accéder ou l’utiliser à d’autres fins. En outre, cet index est différent de la propriété [index](../../../ado/reference/ado-api/index-property.md) de l’objet **Recordset** .  
  
 La méthode **Find** localise rapidement une valeur dans une colonne (champ) d’un **jeu d’enregistrements**. Vous pouvez souvent améliorer la vitesse de la méthode **Find** sur une colonne en utilisant la propriété **optimize** pour créer un index sur celle-ci.  
  
 La méthode **Find** limite votre recherche au contenu d’un champ. La méthode **Seek** exige que vous disposiez d’un index et que vous ayez également d’autres limitations. Si vous devez effectuer une recherche sur plusieurs champs qui ne sont pas la base d’un index ou si votre fournisseur ne prend pas en charge les index, vous pouvez limiter vos résultats à l’aide de la propriété **Filter** de l’objet **Recordset** .  
  
### <a name="find"></a>Rechercher  
 La méthode **Find** recherche un **jeu d’enregistrements** pour la ligne qui répond à un critère spécifié. Éventuellement, la direction de la recherche, la ligne de départ et le décalage à partir de la ligne de départ peuvent être spécifiés. Si le critère est respecté, la position de ligne actuelle est définie sur l’enregistrement trouvé ; dans le cas contraire, la position est définie à la fin (ou au début) du **Recordset**, selon le sens de la recherche.  
  
 Seul un nom de colonne unique peut être spécifié pour le critère. En d’autres termes, cette méthode ne prend pas en charge les recherches multicolonnes.  
  
 L’opérateur de comparaison pour le critère peut être " **>** " (supérieur à), "* * \<**" (less than), "=" (equal), "> =" (supérieur ou égal à), "<=" (inférieur ou égal à), "<>" (différent de) ou "like" (critères spéciaux).  
  
 La valeur de critère peut être une chaîne, un nombre à virgule flottante ou une date. Les valeurs de chaîne sont délimitées par des guillemets simples ou par des signes dièse (#) (par exemple, "State = 'WA" "ou" state = #WA # "). Les valeurs de date sont délimitées par des signes « # » (dièse) (par exemple, « start_date > #7/22/97 # »).  
  
 Si l’opérateur de comparaison est « like », la valeur de chaîne peut contenir un astérisque (*) pour rechercher une ou plusieurs occurrences de n’importe quel caractère ou sous-chaîne. Par exemple, « État comme «m » \* » correspond à Maine et Massachusetts. Vous pouvez également utiliser des astérisques de début et de fin pour rechercher une sous-chaîne contenue dans les valeurs. Par exemple, « State like » \* \* correspond à Alaska, Arkansas et Massachusetts.  
  
 Les astérisques peuvent être utilisés uniquement à la fin d’une chaîne de critères, ou au début et à la fin d’une chaîne de critères, comme indiqué plus haut. Vous ne pouvez pas utiliser l’astérisque comme caractère générique de début (« * STR ») ou un caractère générique incorporé ( \* r). Cela génère une erreur.  
  
### <a name="seek-and-index"></a>Recherche et index  
 Utilisez la méthode **Seek** avec la propriété **index** si le fournisseur sous-jacent prend en charge les index sur l’objet **Recordset** . Utilisez la méthode [supports](../../../ado/reference/ado-api/supports-method.md)**(adSeek)** pour déterminer si le fournisseur sous-jacent prend en charge la **recherche**, et la méthode **prend en charge (adIndex)** pour déterminer si le fournisseur prend en charge les index. (Par exemple, le [fournisseur de OLE DB pour Microsoft Jet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-microsoft-jet.md) prend en charge la **recherche** et l' **indexation**.)  
  
 Si la **recherche** ne trouve pas la ligne souhaitée, aucune erreur ne se produit et la ligne est positionnée à la fin de l’ensemble d' **enregistrements**. Définissez la propriété **index** sur l’index souhaité avant d’exécuter cette méthode.  
  
 Cette méthode est prise en charge uniquement avec les curseurs côté serveur. Seek n’est pas pris en charge lorsque la valeur de la propriété [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) de l’objet **Recordset** est **adUseClient**.  
  
 Cette méthode peut être utilisée uniquement lorsque l’objet **Recordset** a été ouvert avec une valeur [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md) de **adCmdTableDirect**.  
  
## <a name="filtering-the-results"></a>Filtrage des résultats  
 La méthode **Find** limite votre recherche au contenu d’un champ. La méthode **Seek** exige que vous disposiez d’un index et que vous ayez également d’autres limitations. Si vous devez effectuer une recherche sur plusieurs champs qui ne sont pas la base d’un index ou si votre fournisseur ne prend pas en charge les index, vous pouvez limiter vos résultats à l’aide de la propriété **Filter** de l’objet **Recordset** .  
  
 Utilisez la propriété **Filter pour filtrer** de manière sélective les enregistrements d’un objet **Recordset** . Le **Recordset** filtré devient le curseur actuel, ce qui signifie que les enregistrements qui ne répondent pas aux critères de **filtre** ne sont pas disponibles dans le **Recordset** tant que le **filtre** n’est pas supprimé. Les autres propriétés qui retournent des valeurs basées sur le curseur actuel sont affectées, telles que **AbsolutePosition**, **AbsolutePage**, **RecordCount** et **PageCount**. Cela est dû au fait que la définition de la propriété **Filter** sur une valeur spécifique déplace l’enregistrement en cours vers le premier enregistrement qui satisfait la nouvelle valeur.  
  
 La propriété **Filter** accepte un argument variant. Cette valeur représente l’une des trois méthodes d’utilisation de la propriété **Filter** : une chaîne de critères, une constante **FilterGroupEnum** ou un tableau de signets. Pour plus d’informations, consultez filtrage à l’aide d’une chaîne de critères, filtrage avec une constante et filtrage avec des signets plus loin dans cette rubrique.  
  
> [!NOTE]
>  Lorsque vous connaissez les données que vous souhaitez sélectionner, il est généralement plus efficace d’ouvrir un **jeu d’enregistrements** avec une instruction SQL qui filtre efficacement le jeu de résultats, au lieu de s’appuyer sur la propriété **Filter** .  
  
 Pour supprimer un filtre d’un **jeu d’enregistrements**, utilisez la constante **adFilterNone** . La définition de la propriété **Filter** sur une chaîne de longueur nulle ("") a le même effet que l’utilisation de la constante **adFilterNone** .  
  
### <a name="filtering-with-a-criteria-string"></a>Filtrage à l’aide d’une chaîne de critères  
 La chaîne de critères est constituée de clauses au format *nom_champ opérateur valeur* (par exemple, `"LastName = 'Smith'"` ). Vous pouvez créer des clauses composées en concaténant des clauses individuelles avec **et** (par exemple, `"LastName = 'Smith' AND FirstName = 'John'"` ) et **ou** (par exemple, `"LastName = 'Smith' OR LastName = 'Jones'"` ). Utilisez les instructions suivantes pour les chaînes de critères :  
  
-   *FieldName* doit être un nom de champ valide du **Recordset**. Si le nom du champ contient des espaces, vous devez placer le nom entre crochets.  
  
-   L' *opérateur* doit être l’un des suivants : **\<**, **>** ,,, **\<=**, **>=** **<>** **=** ou **Like**.  
  
-   La *valeur* est la valeur avec laquelle vous allez comparer les valeurs de champ (par exemple,,, `'Smith'` `#8/24/95#` `12.345` ou `$50.00` ). Utilisez des guillemets simples (') avec des chaînes et des signes dièse ( `#` ) avec des dates. Pour les nombres, vous pouvez utiliser des points décimaux, des signes dollar et une notation scientifique. Si l' *opérateur* est **comme**, la *valeur* peut utiliser des caractères génériques. Uniquement l’astérisque ( \* ) et le signe de pourcentage (%) les caractères génériques sont autorisés et doivent être le dernier caractère de la chaîne. La *valeur* ne peut pas être null.  
  
    > [!NOTE]
    >  Pour inclure des guillemets simples (') dans la *valeur* de filtre, utilisez deux guillemets simples pour en représenter un. Par exemple, pour filtrer sur *O’Malley*, la chaîne de critères doit être `"col1 = 'O''Malley'"` . Pour inclure des guillemets simples à la fois au début et à la fin de la valeur de filtre, mettez la chaîne entre signes dièse (#). Par exemple, pour filtrer sur *« 1 »*, la chaîne de critères doit être `"col1 = #'1'#"` .  
  
 Il n’existe aucune priorité entre **and** et **or**. Les clauses peuvent être regroupées entre parenthèses. Toutefois, vous ne pouvez pas regrouper les clauses jointes par **ou** , puis joindre le groupe à une autre clause avec un et, comme suit.  
  
```  
(LastName = 'Smith' OR LastName = 'Jones') AND FirstName = 'John'  
```  
  
 Au lieu de cela, vous devez construire ce filtre comme suit.  
  
```  
(LastName = 'Smith' AND FirstName = 'John') OR (LastName = 'Jones' AND FirstName = 'John')  
```  
  
 Dans une clause **Like** , vous pouvez utiliser un caractère générique au début et à la fin du modèle (par exemple, `LastName Like '*mit*'` ) ou uniquement à la fin du modèle (par exemple, `LastName Like 'Smit*'` ).  
  
### <a name="filtering-with-a-constant"></a>Filtrage avec une constante  
 Les constantes suivantes sont disponibles pour filtrer les **recordsets**.  
  
|Constante|Description|  
|--------------|-----------------|  
|**adFilterAffectedRecords**|Filtres permettant d’afficher uniquement les enregistrements affectés par le dernier appel **Delete**, **Resync**, **UpdateBatch** ou **CancelBatch** .|  
|**adFilterConflictingRecords**|Filtres permettant d’afficher les enregistrements qui ont échoué lors de la dernière mise à jour par lot.|  
|**adFilterFetchedRecords**|Filtres permettant d’afficher les enregistrements dans le cache actuel, autrement dit, les résultats du dernier appel pour récupérer des enregistrements de la base de données.|  
|**adFilterNone**|Supprime le filtre en cours et restaure tous les enregistrements à afficher.|  
|**adFilterPendingRecords**|Filtres permettant d’afficher uniquement les enregistrements qui ont été modifiés mais qui n’ont pas encore été envoyés au serveur. S’applique uniquement au mode de mise à jour par lot.|  
  
 Les constantes de filtre facilitent la résolution des conflits d’enregistrements individuels pendant le mode de mise à jour par lot en vous permettant d’afficher, par exemple, uniquement les enregistrements qui ont été affectés lors du dernier appel de la méthode **UpdateBatch** , comme indiqué dans l’exemple suivant.  
  
 `Attribute VB_Name = "modExaminingData"`  
  
### <a name="filtering-with-bookmarks"></a>Filtrage avec des signets  
 Enfin, vous pouvez passer un tableau de signets de type Variant à la propriété **Filter** . Le curseur résultant contient uniquement les enregistrements dont le signet a été passé à la propriété. L’exemple de code suivant crée un tableau de signets à partir des enregistrements d’un **jeu d’enregistrements** qui ont un « B » dans le champ *ProductName* . Il passe ensuite le tableau à la propriété **Filter** et affiche des informations sur le **Recordset** filtré résultant.  
  
```  
'BeginFilterBkmk  
Dim vBkmkArray() As Variant  
Dim i As Integer  
  
'Recordset created using "SELECT * FROM Products" as command.  
'So, we will check to see if ProductName has a capital B, and  
'if so, add to the array.  
i = 0  
Do While Not objRs.EOF  
    If InStr(1, objRs("ProductName"), "B") Then  
        ReDim Preserve vBkmkArray(i)  
        vBkmkArray(i) = objRs.Bookmark  
        i = i + 1  
        Debug.Print objRs("ProductName")  
    End If  
    objRs.MoveNext  
Loop  
  
'Filter using the array of bookmarks.  
objRs.Filter = vBkmkArray  
  
objRs.MoveFirst  
Do While Not objRs.EOF  
    Debug.Print objRs("ProductName")  
    objRs.MoveNext  
Loop  
'EndFilterBkmk  
```  
  
## <a name="creating-a-clone-of-a-recordset"></a>Création d’un clone d’un jeu d’enregistrements  
 Utilisez la méthode **clone** pour créer plusieurs objets **Recordset** dupliqués, en particulier si vous souhaitez conserver plusieurs enregistrements actifs dans un ensemble donné d’enregistrements. L’utilisation de la méthode **clone** est plus efficace que la création et l’ouverture d’un nouvel objet **Recordset** avec la même définition que l’original.  
  
 L’enregistrement actuel d’un clone nouvellement créé est initialement défini sur le premier enregistrement. Le pointeur d’enregistrement actif dans un **jeu d’enregistrements** cloné n’est pas synchronisé avec l’original ou vice versa. Vous pouvez naviguer indépendamment dans chaque **Recordset**.  
  
 Les modifications que vous apportez à un objet **Recordset** sont visibles dans tous ses clones, quel que soit le type de curseur. Toutefois, après l’exécution de [Requery](../../../ado/reference/ado-api/requery-method.md) sur le **jeu d’enregistrements** d’origine, les clones ne sont plus synchronisés avec l’original.  
  
 La fermeture de l' **objet Recordset** d’origine ne ferme pas ses copies et ne ferme pas non plus la copie d’origine ou de l’une des autres copies.  
  
 Vous pouvez cloner un objet **Recordset** uniquement s’il prend en charge les signets. Les valeurs de signet sont interchangeables ; autrement dit, une référence de signet d’un objet **Recordset** fait référence au même enregistrement dans l’un de ses clones.
