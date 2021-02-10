---
description: Remise en forme
title: Mise en forme | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- reshaping previously shaped Recordset [ADO]
- data shaping [ADO], reshaping
ms.assetid: b1c965b7-3dad-4de6-9e0e-502ca8785be3
author: rothja
ms.author: jroth
ms.openlocfilehash: c4604bcb4713f02ffb3f804f86b429d1db3a6aab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037079"
---
# <a name="reshaping"></a>Remise en forme
Un **jeu d’enregistrements** créé par une clause d’une commande SHAPE peut se voir attribuer un nom d' *alias* (généralement avec le mot clé As). L’alias d’un **Recordset** mis en forme peut être référencé dans une commande complètement différente. Autrement dit, vous pouvez réutiliser, ou *reformer*, un **jeu d’enregistrements** précédemment mis en forme dans une nouvelle commande de forme. Pour prendre en charge cette fonctionnalité, ADO fournit une propriété, [Remodel Name](../../../ado/reference/ado-api/reshape-name-property-dynamic-ado.md).  
  
 La remodelage a deux fonctions principales. La première consiste à associer un **jeu d’enregistrements** existant à un nouveau **jeu d’enregistrements** parent.  
  
## <a name="example"></a>Exemple  
  
```  
rs1.Open "SHAPE {select * from Customers} " & _  
         "APPEND ({select * from Orders} AS chapOrders " & _  
         "RELATE CustomerID to CustomerID)", cn  
  
rs2.Open "SHAPE {select * from Employees} " & _  
         "APPEND (chapOrders RELATE EmployeeID to EmployeeID)", cn  
```  
  
 La deuxième fonction consiste à activer l’accès non chapitre aux objets **Recordset** enfants existants, à l’aide de la syntaxe « Shape \<recordset reshape name> ».  
  
> [!NOTE]
>  Vous ne pouvez pas ajouter de colonnes à un **jeu d’enregistrements** existant, reformer un **Recordset** paramétré ou des objets **Recordset** dans une clause COMPUTE intermédiaire, ou effectuer des opérations d’agrégation sur tout descendant du **Recordset** à partir du **Recordset** en cours de remodelage. **Recordset** en cours de mise en forme et la nouvelle commande Shape doit utiliser la même [connexion](../../../ado/reference/ado-api/connection-object-ado.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de mise en forme des données](../../../ado/guide/data/data-shaping-example.md)
