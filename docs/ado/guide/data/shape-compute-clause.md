---
description: Clause COMPUTE de la commande SHAPE
title: Shape Compute, clause | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO]
- compute clause [ADO]
- data shaping [ADO], COMPUTE clause
ms.assetid: 3fdfead2-b5ab-4163-9b1d-3d2143a5db8c
author: rothja
ms.author: jroth
ms.openlocfilehash: b246b1e70f18836455008f8d9e3c1b972e71f183
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036999"
---
# <a name="shape-compute-clause"></a>Clause COMPUTE de la commande SHAPE
Une clause COMPUTE Shape génère un **jeu d’enregistrements** parent, dont les colonnes se composent d’une référence à l' **objet Recordset** enfant ; les colonnes facultatives dont le contenu est un chapitre, une nouvelle colonne ou des colonnes calculées, ou le résultat de l’exécution des fonctions d’agrégation sur le **jeu d’enregistrements** enfant ou sur un **jeu d’enregistrements** précédemment mis en forme ; et toutes les colonnes de l' **objet Recordset** enfant qui sont listées dans la clause facultative by.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
SHAPE child-command [AS] child-alias  
   COMPUTE child-alias [[AS] name], [appended-column-list]  
   [BY grp-field-list]  
```  
  
## <a name="description"></a>Description  
 Les parties de cette clause sont les suivantes :  
  
 *commande enfant*  
 Se compose de l’un des éléments suivants :  
  
-   Commande de requête entre accolades (" {} ") qui retourne un objet **Recordset** enfant. La commande est émise pour le fournisseur de données sous-jacent, et sa syntaxe dépend des spécifications de ce fournisseur. Il s’agit généralement du langage SQL, bien qu’ADO ne nécessite pas de langage de requête particulier.  
  
-   Nom d’un **jeu d’enregistrements** mis en forme existant.  
  
-   Autre commande Shape.  
  
-   Le mot clé TABLE, suivi du nom d’une table dans le fournisseur de données.  
  
 *alias-enfant*  
 Alias utilisé pour faire référence au **jeu d’enregistrements** retourné par la *commande enfant.* L' *alias d’enfant* est requis dans la liste de colonnes de la clause COMPUTE et définit la relation entre les objets **Recordset** enfant et parent.  
  
 *appended-column-list*  
 Liste dans laquelle chaque élément définit une colonne dans le parent généré. Chaque élément contient soit une colonne de chapitre, une nouvelle colonne, une colonne calculée, soit une valeur résultant d’une fonction d’agrégation sur le **jeu d’enregistrements** enfant.  
  
 *grp-field-list*  
 Liste de colonnes dans les objets **Recordset** parent et enfant qui spécifient comment les lignes doivent être regroupées dans l’enfant.  
  
 Pour chaque colonne de la *liste grp-field-list* figure une colonne correspondante dans les objets **Recordset** parent et enfant. Pour chaque ligne de l’ensemble d' **enregistrements** parent, les colonnes *grp-field-list* ont des valeurs uniques et le **jeu d’enregistrements** enfant référencé par la ligne parente se compose uniquement de lignes enfants dont les colonnes *grp-field-list* ont les mêmes valeurs que la ligne parente.  
  
 Si la clause BY est incluse, les lignes de l' **objet Recordset** enfant sont regroupées en fonction des colonnes de la clause COMPUTE. Le **jeu d’enregistrements** parent contiendra une ligne pour chaque groupe de lignes dans le **jeu d’enregistrements** enfant.  
  
 Si la clause BY est omise, le **jeu d’enregistrements** enfant entier est traité comme un groupe unique et le **Recordset** parent contient exactement une ligne. Cette ligne fait référence à l’intégralité du **Recordset** enfant. L’omission de la clause BY vous permet de calculer des agrégats « total général » sur l’ensemble du **jeu d’enregistrements** enfant.  
  
 Par exemple :  
  
```  
SHAPE {select * from Orders} AS orders             COMPUTE orders, SUM(orders.OrderAmount) as TotalSales         
```  
  
 Quelle que soit la façon dont le **jeu d’enregistrements** parent est formé (à l’aide de Compute ou d’Append), il contient une colonne de chapitre qui est utilisée pour le lier à un **Recordset** enfant. Si vous le souhaitez, le **jeu d’enregistrements** parent peut également contenir des colonnes qui contiennent des agrégats (somme, min, Max, etc.) sur les lignes enfants. Le parent et le **Recordset** enfant peuvent contenir des colonnes qui contiennent une expression sur la ligne de l’ensemble d' **enregistrements**, ainsi que des colonnes nouvelles et vides initialement.  
  
## <a name="operation"></a>Opération  
 La *commande enfant* est émise au fournisseur, qui retourne un **jeu d’enregistrements** enfant.  
  
 La clause COMPUTE spécifie les colonnes de l' **objet Recordset** parent, qui peut être une référence à l' **objet Recordset** enfant, un ou plusieurs agrégats, une expression calculée ou de nouvelles colonnes. S’il existe une clause BY, les colonnes qu’elle définit sont également ajoutées à l' **objet Recordset** parent. La clause BY spécifie la manière dont les lignes de l’ensemble d' **enregistrements** enfant sont regroupées.  
  
 Par exemple, supposons que vous disposiez d’une table nommée données démographiques, composée de champs État, ville et remplissage. (Les chiffres de remplissage dans le tableau sont fournis uniquement à titre d’exemple).  
  
|State|City|Remplissage|  
|-----------|----------|----------------|  
|WA|Seattle|700 000|  
|OR|Medford|200 000|  
|OR|Portland|400 000|  
|CA|Los Angeles|800 000|  
|CA|San Diego|600 000|  
|WA|Tacoma|500 000|  
|OR|Corvallis|300 000|  
  
 À présent, émettez la commande SHAPE suivante :  
  
```  
rst.Open  "SHAPE {select * from demographics} AS rs "  & _  
          "COMPUTE rs, SUM(rs.population) BY state", _  
           objConnection  
```  
  
 Cette commande ouvre un **Recordset** mis en forme avec deux niveaux. Le niveau parent est un **Recordset** généré avec une colonne d’agrégation ( `SUM(rs.population)` ), une colonne qui fait référence à l' **objet Recordset** enfant ( `rs` ) et une colonne pour le regroupement du **Recordset** enfant ( `state` ). Le niveau enfant est le **jeu d’enregistrements** retourné par la commande de requête ( `select * from demographics` ).  
  
 Les lignes de détail de l' **objet Recordset** enfant sont regroupées par État, mais dans aucun ordre particulier. Autrement dit, les groupes ne seront pas classés par ordre alphabétique ou numérique. Si vous souhaitez que le **jeu d’enregistrements** parent soit ordonné, vous pouvez utiliser la méthode de tri de l’ensemble d' **enregistrements** pour trier le **jeu d'** enregistrements parent.  
  
 Vous pouvez maintenant naviguer dans le **jeu d’enregistrements** parent ouvert et accéder aux objets **Recordset** des détails enfants. Pour plus d’informations, consultez [accès aux lignes d’un jeu d’enregistrements hiérarchique](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md).  
  
## <a name="resultant-parent-and-child-detail-recordsets"></a>Jeux d’enregistrements des détails parents et enfants résultants  
  
### <a name="parent"></a>Parent  
  
|SOMME (RS. Habitants|rs|State|  
|---------------------------|--------|-----------|  
|1,3 million|Référence à child1|CA|  
|1 200 000|Référence à enfant2|WA|  
|1,1 million|Référence à child3|OR|  
  
## <a name="child1"></a>Enfant 1  
  
|State|City|Remplissage|  
|-----------|----------|----------------|  
|CA|Los Angeles|800 000|  
|CA|San Diego|600 000|  
  
## <a name="child2"></a>Enfant 2  
  
|State|City|Remplissage|  
|-----------|----------|----------------|  
|WA|Seattle|700 000|  
|WA|Tacoma|500 000|  
  
## <a name="child3"></a>Enfant 3  
  
|State|City|Remplissage|  
|-----------|----------|----------------|  
|OR|Medford|200 000|  
|OR|Portland|400 000|  
|OR|Corvallis|300 000|  
  
## <a name="see-also"></a>Voir aussi  
 [Accès aux lignes d’un jeu d’enregistrements hiérarchique](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md)   
 [Présentation de la mise en forme des données](../../../ado/guide/data/data-shaping-overview.md)   
 [Field, objet](../../../ado/reference/ado-api/field-object.md)   
 [Grammaire de forme formelle](../../../ado/guide/data/formal-shape-grammar.md)   
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)   
 [Fournisseurs requis pour la mise en forme des données](../../../ado/guide/data/required-providers-for-data-shaping.md)   
 [Clause APPEND de la forme](../../../ado/guide/data/shape-append-clause.md)   
 [Mettre en forme les commandes en général](../../../ado/guide/data/shape-commands-in-general.md)   
 [Value, propriété (ADO)](../../../ado/reference/ado-api/value-property-ado.md)   
 [Fonctions Visual Basic pour Applications](../../../ado/guide/data/visual-basic-for-applications-functions.md)
