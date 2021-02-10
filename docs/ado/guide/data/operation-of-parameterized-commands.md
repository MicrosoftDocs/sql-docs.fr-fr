---
description: Fonctionnement des commandes paramétrées
title: Fonctionnement des commandes paramétrées | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], parameterized commands
- parameterized commands [ADO]
ms.assetid: 4fae0d54-83b6-4ead-99cc-bcf532daa121
author: rothja
ms.author: jroth
ms.openlocfilehash: 55eaf3798ee8d14a776da14010b4b6617d0e2723
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032651"
---
# <a name="operation-of-parameterized-commands"></a>Fonctionnement des commandes paramétrées
Si vous utilisez un **jeu d’enregistrements** enfant volumineux, en particulier par rapport à la taille du **Recordset** parent, mais que vous n’avez besoin d’accéder qu’à quelques chapitres enfants, il peut s’avérer plus efficace d’utiliser une commande paramétrable.  
  
 Une *commande non paramétrée* récupère l’ensemble des **jeux d’enregistrements** parents et enfants, ajoute une colonne de chapitre au parent, puis assigne une référence au chapitre enfant associé pour chaque ligne parente.  
  
 Une *commande paramétrable* récupère l’ensemble du **Recordset** parent, mais n’extrait que le jeu **d’enregistrements** du chapitre lors de l’accès à la colonne de chapitre. Cette différence de stratégie de récupération peut entraîner des avantages significatifs en matière de performances.  
  
 Par exemple, vous pouvez spécifier les éléments suivants :  
  
```  
SHAPE {SELECT * FROM customer}   
   APPEND ({SELECT * FROM orders WHERE cust_id = ?}   
   RELATE cust_id TO PARAMETER 0)  
```  
  
 Les tables parent et enfant ont un nom de colonne en commun, *cust_id*. La *commande enfant* a un espace réservé «  ? » auquel la clause relate fait référence (autrement dit, «... PARAMÈTRE 0»).  
  
> [!NOTE]
>  La clause PARAMETER se rapporte uniquement à la syntaxe de commande Shape. Elle n’est pas associée à l’objet de [paramètre](../../reference/ado-api/parameter-object.md) ADO ou à la collection [Parameters](../../reference/ado-api/parameters-collection-ado.md) .  
  
 Lorsque la commande de forme paramétrable est exécutée, voici ce qui se produit :  
  
1.  La *commande parent-Command* est exécutée et retourne un **jeu d’enregistrements** parent de la table Customers.  
  
2.  Une colonne de chapitre est ajoutée au **jeu d’enregistrements** parent.  
  
3.  Lorsque vous accédez à la colonne de chapitre d’une ligne parente, la *commande enfant* est exécutée à l’aide de la valeur de la Customer.cust_id comme valeur du paramètre.  
  
4.  Toutes les lignes de l’ensemble de lignes du fournisseur de données créé à l’étape 3 sont utilisées pour remplir le **Recordset** enfant. Dans cet exemple, il s’agit de toutes les lignes de la table Orders dans lesquelles la cust_id est égale à la valeur de customer.cust_id. Par défaut, le **jeu d’enregistrements** enfant est mis en cache sur le client jusqu’à ce que toutes les références au **Recordset** parent soient libérées. Pour modifier ce comportement, affectez la valeur **false** aux **lignes enfants du cache** de [propriétés dynamiques](../../reference/ado-api/ado-dynamic-property-index.md) du **jeu d’enregistrements** .  
  
5.  Une référence aux lignes enfants récupérées (autrement dit, le chapitre du **Recordset** enfant) est placée dans la colonne de chapitre de la ligne actuelle de l' **objet Recordset** parent.  
  
6.  Les étapes 3-5 sont répétées lors de l’accès à la colonne de chapitre d’une autre ligne.  
  
 La propriété dynamique des **lignes enfants du cache** est définie sur **true** par défaut. Le comportement de mise en cache varie en fonction des valeurs de paramètre de la requête. Dans une requête avec un seul paramètre, le **jeu d’enregistrements** enfant pour une valeur de paramètre donnée sera mis en cache entre les demandes d’un enfant avec cette valeur. Le code suivant illustre cela :  
  
```  
SCmd = "SHAPE {select * from customer} " & _  
         "APPEND({select * from orders where cust_id = ?} " & _  
         "RELATE cust_id TO PARAMETER 0) AS chpCustOrder"  
Rst1.Open sCmd, Cnn1  
Set RstChild = Rst1("chpCustOrder").Value  
Rst1.MoveNext      ' Next cust_id passed to Param 0, & new rs fetched   
                   ' into RstChild.  
Rst1.MovePrevious  ' RstChild now holds cached rs, saving round trip.  
```  
  
 Dans une requête avec deux paramètres ou plus, un enfant mis en cache est utilisé uniquement si toutes les valeurs de paramètre correspondent aux valeurs mises en cache.  
  
## <a name="parameterized-commands-and-complex-parent-child-relations"></a>Commandes paramétrées et relations enfants parentes complexes  
 Outre l’utilisation de commandes paramétrables pour améliorer les performances d’une hiérarchie de type d’Equi-Join, les commandes paramétrables peuvent être utilisées pour prendre en charge des relations parent-enfant plus complexes. Par exemple, considérez une petite base de données de la ligue avec deux tables : une comprenant les équipes (team_id, team_name) et l’autre des jeux (date, home_team, visiting_team).  
  
 À l’aide d’une hiérarchie non paramétrée, il n’existe aucun moyen de lier les tables équipes et jeux de telle sorte que le **jeu d’enregistrements** enfant pour chaque équipe contienne son calendrier complet. Vous pouvez créer des chapitres qui contiennent le calendrier d’habitation ou le calendrier routier, mais pas les deux. En effet, la clause RELATe vous limite aux relations parent-enfant de la forme (PC1 = CC1) et (PC2 = PC2). Par conséquent, si votre commande inclut « mettre en relation team_id à home_team, team_id à visiting_team », vous n’obtiendriez que les jeux dans lesquels une équipe s’exécutait elle-même. Ce que vous voulez, c’est « (team_id = home_team) ou (team_id = visiting_team) », mais le fournisseur Shape ne prend pas en charge la clause ou.  
  
 Pour obtenir le résultat souhaité, vous pouvez utiliser une commande paramétrable. Par exemple :  
  
```  
SHAPE {SELECT * FROM teams}   
APPEND ({SELECT * FROM games WHERE home_team = ? OR visiting_team = ?}   
        RELATE team_id TO PARAMETER 0,   
               team_id TO PARAMETER 1)   
```  
  
 Cet exemple exploite la plus grande flexibilité de la clause SQL WHERE pour obtenir le résultat dont vous avez besoin.  
  
> [!NOTE]
>  Lorsque vous utilisez des clauses WHERE, les paramètres ne peuvent pas utiliser les types de données SQL pour Text, ntext et image, sinon une erreur se produit, contenant la description suivante : `Invalid operator for data type` .  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de mise en forme des données](./data-shaping-example.md)   
 [Grammaire de forme formelle](./formal-shape-grammar.md)   
 [Généralités sur les commandes SHAPE](./shape-commands-in-general.md)