---
title: Structurer et ajouter des extraits de code à un script Transact-SQL
description: En savoir plus sur les extraits de code fournis par SSDT. Découvrez comment insérer des extraits de code dans des applications et comment masquer et développer du code dans l’éditeur Transact-SQL.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 543e7ce7-8639-4281-8a91-85314755e5de
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: be1adf9c385273d5cac8afbe972fd3f2f594ca34
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018139"
---
# <a name="how-to-outline-and-add-snippets-to-transact-sql-script"></a>Procédure : Structurer et ajouter des extraits de code à un script Transact-SQL

SQL Server Data Tools comprend une bibliothèque de codes composée d’extraits de code prêts à être insérés dans une application. Chaque extrait de code effectue une tâche de script complète, par exemple créer une fonction, une table, un déclencheur, un index, un affichage, un type défini par l'utilisateur, etc. Vous pouvez insérer un extrait de code dans votre code source en quelques clics. Ces extraits de code augmentent votre productivité en réduisant le temps passé à la saisie.  
  
Lorsque vous devez rechercher un extrait spécifique, vous pouvez utiliser le sélecteur d'extraits de code afin d'obtenir des listes classées d'extraits de code. Une fois que vous avez ajouté l'extrait à votre code, certaines parties devront peut-être être personnalisées, par exemple, remplacer le nom des variables par un nom plus adapté, ou insérer la logique réelle d'une procédure stockée. Vous noterez que le code de l'extrait inséré comporte un ou plusieurs points de remplacement mis en surbrillance dans le code à cet effet. Si vous positionnez le pointeur de votre souris sur le point de remplacement, une Info-bulle s'affiche pour vous expliquer comment modifier le code.  
  
Par défaut, tout le texte est affiché dans l’Éditeur Transact\-SQL, mais vous pouvez choisir de masquer certaines parties du code. L’Éditeur Transact\-SQL permet de sélectionner une zone du code et de la rendre réductible, de façon à ce qu’elle s’affiche sous un signe plus (+). Vous pouvez ensuite la développer ou la masquer en cliquant sur le signe plus (+) à côté du symbole. Le code en mode Plan n'est pas supprimé, il est simplement masqué.  
  
> [!WARNING]  
> Les procédures suivantes utilisent les entités créées dans les procédures précédentes des sections [Développement de base de données connectée](../ssdt/connected-database-development.md) et [Développement de base de données hors connexion orienté projet](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-insert-snippets"></a>Pour insérer des extraits de code  
  
1.  Cliquez avec le bouton droit sur le projet **TradeDev** dans **l’Explorateur de solutions** et sélectionnez **Ajouter**, puis **Script**. Dans la boîte de dialogue **Ajouter un nouvel élément**, cliquez sur **Ajouter**.  
  
2.  Cliquez avec le bouton droit sur l’Éditeur Transact\-SQL et sélectionnez **Insérer un extrait de code**. Le Sélecteur d'extraits s'affiche.  
  
3.  Double-cliquez sur **Table** dans le sélecteur d’extraits de code, puis sur **Créer une table**.  
  
4.  Notez que les points de remplacement sont sélectionnés en jaune. Positionnez le curseur sur `Sample_Table`, et une info-bulle affiche une description du remplacement. Double-cliquez sur `Sample_Table` et remplacez-le par `Shipper2`.  
  
5.  Utilisez la touche TABULATION pour passer au point de remplacement au suivant, qui est `column_1`. Renommez-le `Id`. Répétez ces étapes pour renommer `column_2` en `name`, modifier son type de données en `nvarchar(128)` et autoriser `NULL`.  
  
### <a name="to-outline-code"></a>Pour passer du code en mode Plan  
  
1.  Remarquez la présence du signe **-** à côté de l’instruction CREATE TABLE. Cliquez sur le signe **-** situé à côté d’une section du script pour la masquer.  
  
2.  Cliquez avec le bouton droit sur l’Éditeur Transact\-SQL et sélectionnez **Mode Plan**, puis **Arrêter le mode Plan** pour supprimer les informations du mode Plan sans affecter le code sous-jacent dans l’éditeur.  
  
3.  Pour remettre le code en mode Plan, cliquez avec le bouton droit sur l’Éditeur Transact\-SQL et sélectionnez **Mode Plan**, puis **Démarrer le mode Plan automatique**. Vous pouvez aussi sélectionner **Basculer tout le plan** pour inverser les sections développées/masquées.  
  
