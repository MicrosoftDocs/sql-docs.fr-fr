---
description: Exemple de mise en forme des données
title: Exemple de mise en forme des données | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], about data shaping
ms.assetid: 1bfdcad4-52e1-45bc-ad21-783657ef0a44
author: rothja
ms.author: jroth
ms.openlocfilehash: 0199dfc2165d5ae54b4b59a45ec740a9c399f114
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033279"
---
# <a name="data-shaping-example"></a>Exemple de mise en forme des données
La commande de mise en forme de données suivante montre comment créer un **jeu d’enregistrements** hiérarchique à partir des tables **Customers** et **Orders** dans la base de données Northwind.  
  
```  
SHAPE {SELECT CustomerID, ContactName FROM Customers}   
APPEND ({SELECT OrderID, OrderDate, CustomerID FROM Orders} AS chapOrders   
RELATE customerID TO customerID)   
```  
  
 Lorsque cette commande est utilisée pour ouvrir un objet **Recordset** (comme indiqué dans [Visual Basic exemple de mise en forme des données](./visual-basic-example-of-data-shaping.md)), il crée un chapitre (**chapOrders**) pour chaque enregistrement retourné à partir de la table **Customers** . Ce chapitre se compose d’un sous-ensemble de l’ensemble d' **enregistrements** renvoyé par la table **Orders** . Le chapitre **chapOrders** contient toutes les informations demandées sur les commandes passées par le client donné. Dans cet exemple, le chapitre se compose de trois colonnes : **OrderID**, **OrderDate** et **CustomerID**.  
  
 Les deux premières entrées du **Recordset** mis en forme résultant sont les suivantes :  
  
|CustomerID|ContactName|OrderID|OrderDate|CustomerID|  
|----------------|-----------------|-------------|---------------|----------------|  
|ALFKI|Maria Ander|10643<br /><br /> 10692<br /><br /> 10702<br /><br /> 10835<br /><br /> 10952<br /><br /> 11011|1997-08-25<br /><br /> 1997-10-03<br /><br /> 1997-10-13<br /><br /> 1998-01-15<br /><br /> 1998-03-16<br /><br /> 1998-04-09|ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI|  
|ANATR|Ana Trujillo|10308<br /><br /> 10625<br /><br /> 10759<br /><br /> 10926|1996-09-18<br /><br /> 1997-08-08<br /><br /> 1997-11-28<br /><br /> 1998-03-04|ANATR<br /><br /> ANATR<br /><br /> ANATR<br /><br /> ANATR|  
  
 Dans une commande de forme, APPEND est utilisé pour créer un **jeu d’enregistrements** enfant associé à l' **objet Recordset** parent (tel qu’il est retourné à partir de la commande spécifique au fournisseur immédiatement après le mot clé Shape abordé précédemment) par la clause relate. Le parent et l’enfant ont généralement au moins une colonne en commun : la valeur de la colonne dans une ligne du parent est identique à la valeur de la colonne dans toutes les lignes de l’enfant.  
  
 Il existe une deuxième façon d’utiliser des commandes de forme : à savoir, pour générer un **jeu d’enregistrements** parent à partir d’un **jeu d’enregistrements** enfant. Les enregistrements de l’ensemble **d'** enregistrements enfant sont regroupés, en général à l’aide de la clause by, et une ligne est ajoutée au **jeu d’enregistrements** parent pour chaque groupe résultant dans l’enfant. Si la clause BY est omise, le **jeu d’enregistrements** enfant constitue un groupe unique et le **jeu d’enregistrements** parent contient exactement une ligne. Cela est utile pour calculer des agrégats « total général » sur l’ensemble de l’ensemble **d’enregistrements** enfant.  
  
 La construction de la commande SHAPE vous permet également de créer par programmation un **Recordset** mis en forme. Vous pouvez ensuite accéder aux composants du **Recordset** par programmation ou par le biais d’un contrôle visuel approprié. Une commande Shape est émise comme tout autre texte de commande ADO. Pour plus d’informations, consultez [Shape, commandes en général](./shape-commands-in-general.md).  
  
 Quelle que soit la façon dont le **jeu d’enregistrements** parent est formé, il contient une colonne de chapitre qui est utilisée pour le lier à un **Recordset** enfant. Si vous le souhaitez, le **jeu d’enregistrements** parent peut également avoir des colonnes qui contiennent des agrégats (somme, min, Max, etc.) sur les lignes enfants. Le parent et le **Recordset** enfant peuvent avoir des colonnes qui contiennent une expression sur la ligne de l’ensemble d' **enregistrements**, ainsi que des colonnes qui sont nouvelles et vides initialement.  
  
 Cette section se poursuit avec la rubrique suivante.  
  
-   [Exemple Visual Basic de mise en forme des données](./visual-basic-example-of-data-shaping.md)