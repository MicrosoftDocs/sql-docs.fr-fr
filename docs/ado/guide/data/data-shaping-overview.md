---
description: Vue d’ensemble de la mise en forme des données
title: Vue d’ensemble de la mise en forme des données | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], overview
ms.assetid: 4cb5fd29-4e56-46ac-ae48-a6771c321c0c
author: rothja
ms.author: jroth
ms.openlocfilehash: dbe025e38960b458c2ad5a224bad44c37868d51f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037649"
---
# <a name="data-shaping-overview"></a>Vue d’ensemble de la mise en forme des données
La mise en *forme des données* consiste à créer des relations hiérarchiques entre deux ou plusieurs entités logiques dans une requête. La hiérarchie peut être consultée dans les relations parent-enfant entre un enregistrement d’un [jeu](../../reference/ado-api/recordset-object-ado.md)d’enregistrements et un ou plusieurs enregistrements (également appelé « chapitre ») d’un autre **Recordset**. Dans une relation parent-enfant, le **jeu d’enregistrements** parent contient le **Recordset** enfant. Les clients et les commandes sont un exemple de cette relation hiérarchique. Pour chaque client d’une base de données, il peut y avoir zéro ou plusieurs commandes. La relation hiérarchique peut être récursive, ce qui signifie que les enregistrements petits-enfants peuvent être imbriqués dans un enregistrement enfant. En principe, un enregistrement hiérarchique peut être imbriqué à n’importe quelle profondeur. En pratique, ADO limite la récursivité à un maximum de 512 **Recordset**.  
  
 En général, les colonnes d’un **Recordset** mis en forme peuvent contenir des données d’un fournisseur de données tel que Microsoft® SQL Server, des références à un autre **jeu d’enregistrements**, des valeurs dérivées d’un calcul sur une seule ligne d’un **jeu d’enregistrements** ou des valeurs dérivées d’une opération sur une colonne d’un **jeu d’enregistrements** entier. Une colonne peut également être créée et vide.  
  
 Lorsque vous récupérez la valeur d’une colonne qui contient une référence à un autre **Recordset**, ADO retourne automatiquement le **jeu d’enregistrements** réel représenté par la référence. La référence à un **jeu d’enregistrements** est en fait une référence à un sous-ensemble de l’enfant, appelé *chapitre*. Un seul parent peut faire référence à plusieurs **recordsets** enfants.  
  
 La prise en charge d’ADO pour la mise en forme des données vous permet d’interroger une source de données et de retourner un **jeu d’enregistrements** dans lequel un enregistrement (parent) représente un **Recordset**(enfant). Dans le scénario de commande client, vous pouvez utiliser la mise en forme des données pour récupérer les informations des clients, ainsi que les commandes passées par chaque client dans une seule requête. Le jeu **d’enregistrements** résultant est également appelé **Recordset** mis en forme.  
  
 En outre, la mise en forme des données dans ADO vous permet de créer des objets **Recordset** sans source de données sous-jacente en utilisant le mot clé **New** pour décrire les champs des **jeux d’enregistrements** parents et enfants. Le nouvel objet **Recordset** peut ensuite être rempli avec les données et stockées de manière permanente. Les développeurs peuvent également effectuer divers calculs ou agrégations (par exemple, **Sum**, **AVG** et **Max**) sur les champs enfants. La mise en forme des données peut également créer un **jeu d’enregistrements** parent à partir d’un **jeu d'** enregistrements enfant en regroupant les enregistrements dans l’enfant et en plaçant une ligne dans le parent pour chaque groupe de l’enfant.  
  
 Le SQL standard vous permet de récupérer des données à l’aide de la syntaxe **join** , mais cela peut être inefficace et difficile, car les données parent redondantes sont répétées dans chaque enregistrement retourné pour une relation parent-enfant donnée. La mise en forme des données peut associer un enregistrement parent unique dans le **jeu d'** enregistrements parent à plusieurs enregistrements enfants dans le **jeu d’enregistrements** enfant, évitant ainsi la redondance d’une **jointure**. La plupart des gens recherchent le modèle de programmation de plusieurs **recordsets** parent-enfant plus naturel et plus facile à utiliser que le modèle de **jointure d’un seul Recordset** .