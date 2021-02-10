---
description: Qu’est qu’un curseur ?
title: Qu’est qu’un curseur ? | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], about cursors
ms.assetid: 596eb4b6-c22f-4cde-b23f-172dd66c3161
author: rothja
ms.author: jroth
ms.openlocfilehash: 915d41f78de6c789c82bca3d729aa2c28ff26f27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032261"
---
# <a name="what-is-a-cursor"></a>Qu’est qu’un curseur ?
Les opérations réalisées dans une base de données relationnelle s'exécutent sur un ensemble complet de lignes. L'ensemble de lignes retourné par une instruction SELECT contient toutes les lignes satisfaisant aux conditions de la clause WHERE de l'instruction. Cet ensemble complet de lignes retournées par l'instruction est appelé ensemble de résultats. Les applications, en particulier celles qui sont interactives et en ligne, ne peuvent pas toujours travailler efficacement avec l’ensemble du jeu de résultats en tant qu’unité. Ces applications ont besoin d'un mécanisme leur permettant de travailler avec une seule ligne ou avec un petit bloc de lignes à la fois. Les curseurs sont une extension des ensembles de résultats et fournissent ce mécanisme.  
  
 Un curseur est implémenté par une bibliothèque de curseurs. Une bibliothèque de curseurs est un logiciel, souvent implémenté dans le cadre d’un système de base de données ou d’une API d’accès aux données, qui est utilisé pour gérer les attributs des données retournées à partir d’une source de données (jeu de résultats). Ces attributs incluent la gestion de la concurrence, la position dans le jeu de résultats, le nombre de lignes retournées et si vous pouvez avancer ou reculer (ou les deux) dans le jeu de résultats (défilement).  
  
 Un curseur effectue le suivi de la position dans le jeu de résultats et vous permet d’effectuer plusieurs opérations ligne par ligne sur un jeu de résultats, avec ou sans revenir à la table d’origine. En d’autres termes, les curseurs renvoient de manière conceptuelle un jeu de résultats basé sur des tables dans les bases de données. Le curseur est donc nommé, car il indique la position actuelle dans le jeu de résultats, tout comme le curseur sur l’écran de l’ordinateur indique la position actuelle.  
  
 Il est important de se familiariser avec le concept de curseurs avant de passer à en vue d’en savoir plus sur les spécificités de leur utilisation dans ADO.  
  
 Les curseurs vous permettent d’utiliser les éléments suivants :  
  
-   Spécifiez le positionnement au niveau de lignes spécifiques dans le jeu de résultats.  
  
-   Récupère une ligne ou un bloc de lignes en fonction de la position actuelle du jeu de résultats.  
  
-   Modifiez les données dans les lignes à la position actuelle dans le jeu de résultats.  
  
-   Définir différents niveaux de sensibilité aux modifications apportées aux données par d’autres utilisateurs.  
  
 Par exemple, considérez une application qui affiche une liste des produits disponibles pour un acheteur potentiel. L’acheteur fait défiler la liste pour voir les détails et le coût du produit, puis sélectionne un produit à acheter. Le défilement et la sélection supplémentaires se produisent pour le reste de la liste. En ce qui concerne l’acheteur, les produits s’affichent l’un après l’autre, mais l’application utilise un curseur de défilement pour naviguer vers le haut et vers le haut dans le jeu de résultats.  
  
 Vous pouvez utiliser les curseurs de plusieurs façons :  
  
-   Sans aucune ligne.  
  
-   Avec une partie ou l’ensemble des lignes d’une table unique.  
  
-   Avec une partie ou la totalité des lignes de tables jointes de manière logique.  
  
-   En lecture seule ou modifiable au niveau du curseur ou du champ.  
  
-   Comme avant uniquement ou avec défilement complet.  
  
-   Avec le jeu de clés de curseur situé sur le serveur.  
  
-   Sensible aux modifications de table sous-jacentes provoquées par d’autres applications (telles que l’appartenance, le tri, les insertions, les mises à jour et les suppressions).  
  
-   Existant sur le serveur ou le client.  
  
 Les curseurs en lecture seule aident les utilisateurs à parcourir le jeu de résultats et les curseurs de lecture/écriture peuvent implémenter des mises à jour de lignes individuelles. Les curseurs complexes peuvent être définis avec des jeux de lignes qui repointent vers les lignes de la table de base. Bien que certains curseurs soient en lecture seule dans le sens inverse, d’autres peuvent se déplacer vers l’avant et fournir une actualisation dynamique du jeu de résultats en fonction des modifications apportées par d’autres applications à la base de données.  
  
 Toutes les applications n’ont pas besoin d’utiliser des curseurs pour accéder aux données ou les mettre à jour. Certaines requêtes ne nécessitent simplement pas la mise à jour directe de lignes à l’aide d’un curseur. Les curseurs doivent être l’une des dernières techniques que vous choisissez pour récupérer des données. ensuite, vous devez choisir le curseur d’impact le plus faible possible. Lorsque vous créez un jeu de résultats à l’aide d’une procédure stockée, le jeu de résultats ne peut pas être mis à jour à l’aide des méthodes Edit ou Update du curseur.  
  
## <a name="concurrency"></a>Accès concurrentiel  
 Dans certaines applications multi-utilisateur, il est très important que les données présentées à l’utilisateur final soient aussi actuelles que possible. Un exemple classique de ce type de système est un système de réservation d’une compagnie aérienne, où de nombreux utilisateurs peuvent être confrontés à un même siège sur un vol donné (et par conséquent, un seul enregistrement). Dans un tel cas, la conception de l’application doit gérer les opérations simultanées sur un seul enregistrement.  
  
 Dans d’autres applications, la concurrence n’est pas aussi importante. Dans ce cas, les dépenses liées à la conservation des données en permanence ne peuvent pas être justifiées.  
  
## <a name="position"></a>Position  
 Un curseur effectue également le suivi de la position actuelle dans un jeu de résultats. Considérez la position du curseur comme un pointeur vers l’enregistrement actif, similaire à la façon dont un index de tableau pointe vers la valeur à cet emplacement particulier dans le tableau.  
  
## <a name="scrollability"></a>Capacité de défilement  
 Le type de curseur utilisé par votre application affecte également la possibilité de se déplacer vers l’avant et vers l’arrière dans les lignes d’un jeu de résultats ; Cela est parfois appelé « défilement ». La possibilité de se déplacer vers l’avant *et* vers l’arrière dans un jeu de résultats augmente la complexité du curseur et est donc plus coûteuse à implémenter. Pour cette raison, vous devez demander un curseur avec cette fonctionnalité uniquement lorsque cela est nécessaire.
