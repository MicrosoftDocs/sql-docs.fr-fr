---
description: Préparation et l’exécution de commandes
title: Préparation et exécution des commandes | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Command object [ADO], preparing and executing commands
ms.assetid: 7448d9ee-7f4b-47e3-be54-2df8c9bbac32
author: rothja
ms.author: jroth
ms.openlocfilehash: fd88799bce8f3b8a0842c9f2d2752eac062dc4b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037179"
---
# <a name="preparing-and-executing-commands"></a>Préparation et l’exécution de commandes
Les commandes sont des instructions émises à un fournisseur pour effectuer des opérations sur la source de données sous-jacente. Une instruction SQL, par exemple, est une commande de l’Fournisseur de données Microsoft SQL. Dans ADO, les commandes sont généralement représentées par des objets de **commande** , bien que des commandes simples puissent également être émises via des objets de **connexion** ou **d’objet Recordset** .  
  
 Vous pouvez utiliser l’objet **Command** pour demander tout type d’opération pris en charge par le fournisseur, en supposant que le fournisseur puisse interpréter correctement la chaîne de commande. Une opération courante pour les fournisseurs de données consiste à interroger une base de données et à retourner des enregistrements dans un objet **Recordset** , qui peut être considéré comme un conteneur pour contenir le résultat et un outil pour afficher le résultat. Comme avec de nombreux objets ADO, certaines collections, méthodes ou propriétés d’objets **Command** peuvent générer des erreurs lorsqu’elles sont référencées, en fonction de la fonctionnalité du fournisseur.  
  
 En plus d’utiliser des objets de **commande** , vous pouvez utiliser la méthode **Execute** sur l’objet **Connection** ou la méthode **Open** sur l’objet **Recordset** pour émettre une commande et l’exécuter. Toutefois, vous devez utiliser un objet de **commande** si vous devez réutiliser une commande dans votre code, ou si vous devez transmettre des informations de paramètres détaillées avec votre commande. Ces scénarios sont traités plus en détail plus loin dans cette section.  
  
> [!NOTE]
>  Certaines **commandes** peuvent retourner un jeu de résultats sous la forme d’un flux binaire ou sous la forme d’un enregistrement unique plutôt que d’un **Recordset**, si ce **dernier** est pris en charge par le fournisseur. En outre, certaines **commandes** ne sont pas destinées à retourner un jeu de résultats (par exemple, une requête de mise à jour SQL). Cette section décrit le scénario le plus courant, en revanche : l’exécution des **commandes** qui retournent les résultats sous la forme d’un objet **Recordset** . Pour plus d’informations sur le retour de **résultats dans** les enregistrements ou les **flux** de données, consultez [enregistrements et flux](../../../ado/guide/data/records-and-streams.md).  
  
 Cette section contient les rubriques suivantes :  
  
-   [Vue d’ensemble de l’objet Command](../../../ado/guide/data/command-object-overview.md)  
  
-   [Création et exécution d’une commande simple](../../../ado/guide/data/creating-and-executing-a-simple-command.md)  
  
-   [Paramètres de l’objet Command](../../../ado/guide/data/command-object-parameters.md)  
  
-   [Appel d’une procédure stockée avec une commande](../../../ado/guide/data/calling-a-stored-procedure-with-a-command.md)  
  
-   [Appel d’une procédure stockée en tant que méthode sur un objet de connexion](../../../ado/guide/data/calling-a-stored-procedure-as-a-method-on-a-connection-object.md)  
  
-   [Commandes nommées](../../../ado/guide/data/named-commands.md)  
  
-   [Passage de paramètres à une commande nommée](../../../ado/guide/data/passing-parameters-to-a-named-command.md)
