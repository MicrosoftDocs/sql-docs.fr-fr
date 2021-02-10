---
description: Fonctionnement des commandes non paramétrées
title: Fonctionnement des commandes non paramétrées | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- non-parameterized commands [ADO]
- data shaping [ADO], non-parameterized commands
ms.assetid: 9700e50a-9f17-4ba3-8afb-f750741dc6ca
author: rothja
ms.author: jroth
ms.openlocfilehash: e9ee255f895e934c4405d888d72f15b8cee9159d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037199"
---
# <a name="operation-of-non-parameterized-commands"></a>Fonctionnement des commandes non paramétrées
Pour les commandes non paramétrées, toutes les commandes du fournisseur sont exécutées et les **jeux d’enregistrements** sont créés lors de l’exécution de la commande. Si la commande est exécutée de façon synchrone, tous les **jeux d’enregistrements** sont entièrement remplis. Si un mode de remplissage asynchrone a été sélectionné, l’état rempli des **recordsets** dépend du mode de remplissage et de la taille des jeux d' **enregistrements**.  
  
 Par exemple, la *commande parente* peut renvoyer un **jeu d’enregistrements** de clients pour une société à partir d’une table Customers, et la *commande Child-Command* peut retourner un **jeu d’enregistrements** de commandes pour tous les clients d’une table Orders.  
  
```  
SHAPE {SELECT * FROM Customers}   
   APPEND ({SELECT * FROM Orders} AS chapOrders   
   RELATE customerID TO customerID)  
```  
  
 Pour les relations parent-enfant non paramétrées, chaque objet **Recordset** parent et enfant doit avoir une colonne en commun pour les associer. Les colonnes sont nommées dans la clause RELATe, *parent-colonne* en premier, puis dans la *colonne enfant*. Les colonnes peuvent avoir des noms différents dans leurs objets **Recordset** respectifs, mais elles doivent faire référence aux mêmes informations afin de spécifier une relation significative. Par exemple, les objets Recordset **Customers** et **Orders** peuvent avoir tous deux un champ CustomerID. Étant donné que l’appartenance du **Recordset** enfant est déterminée par la commande du fournisseur, le **Recordset** enfant peut contenir des lignes orphelines. Ces lignes orphelines sont inaccessibles sans remodelage supplémentaire.  
  
 La mise en forme des données ajoute une colonne de chapitre au **jeu d’enregistrements** parent. Les valeurs de la colonne chapitre sont des références aux lignes de l' **objet Recordset** enfant, qui répondent à la clause relate. Autrement dit, la même valeur se trouve dans la *colonne parente* d’une ligne parente donnée, comme c’est le cas dans la *colonne enfant* de toutes les lignes de l’enfant du chapitre. Quand plusieurs clauses TO sont utilisées dans la même clause RELATe, elles sont implicitement combinées à l’aide d’un opérateur AND. Si les colonnes parentes de la clause RELATe ne constituent pas une clé de l' **objet Recordset** parent, une seule ligne enfant peut avoir plusieurs lignes parentes.  
  
 Lorsque vous accédez à la référence dans la colonne de chapitre, ADO récupère automatiquement le **Recordset** représenté par la référence. Notez que dans une commande non paramétrable, bien que le **jeu d’enregistrements** enfant entier ait été récupéré, le chapitre ne présente qu’un sous-ensemble de lignes.  
  
 Si la colonne ajoutée n’a pas d' *alias de chapitre*, un nom est généré automatiquement. Un objet de [champ](../../reference/ado-api/field-object.md) pour la colonne est ajouté à la collection de [champs](../../reference/ado-api/fields-collection-ado.md) de l’objet **Recordset** , et son type de données est **adChapter**.  
  
 Pour plus d’informations sur la navigation dans un **jeu d’enregistrements** hiérarchique, consultez [accès aux lignes d’un jeu d’enregistrements hiérarchique](./accessing-rows-in-a-hierarchical-recordset.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de mise en forme des données](./data-shaping-example.md)   
 [Grammaire de forme formelle](./formal-shape-grammar.md)   
 [Généralités sur les commandes SHAPE](./shape-commands-in-general.md)