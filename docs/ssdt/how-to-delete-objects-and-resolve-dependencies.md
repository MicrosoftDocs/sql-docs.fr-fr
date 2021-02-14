---
title: Supprimer des objets et résoudre des dépendances
description: Découvrez comment renommer ou supprimer des objets de base de données. Déterminez les objets dépendants que SSDT met automatiquement à jour, ainsi que les dépendances que vous devez mettre à jour.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- Microsoft.VisualStudio.Data.Tools.Project.HelpKeywords.SqlProjectDropDatabaseConfirmationDialog
- sql.data.tools.dropdatabaseconfirmation.dialog
- sql.data.tools.dropmultipledatabasesconfirmation.dialog
ms.assetid: fb31c2b1-ca4f-4e11-a0b6-5c26430f1c8c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 72e9c09651d473c25b2df84e0b6ccecee0964959
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018229"
---
# <a name="how-to-delete-objects-and-resolve-dependencies"></a>Procédure : Supprimer des objets et résoudre des dépendances

Lorsque vous renommez ou supprimez un objet dans l'**Explorateur d'objets SQL Server**, SQL Server Data Tools détecte automatiquement tous ses objets dépendants et prépare un script ALTER pour renommer ou supprimer les dépendances selon les besoins.  
  
> [!WARNING]  
> Les procédures suivantes utilisent les entités créées dans les procédures précédentes de la section [Développement d’une base de données connectée](../ssdt/connected-database-development.md).  
  
### <a name="to-delete-a-database"></a>Pour supprimer une base de données  
  
1.  Cliquez avec le bouton droit sur l’**Explorateur d'objets SQL Server** et sélectionnez **Supprimer**.  
  
2.  Acceptez tous les paramètres par défaut dans la boîte de dialogue **Supprimer la base de données** et cliquez sur **OK**.  
  
### <a name="to-rename-a-table"></a>Pour renommer une table  
  
1.  Vérifiez que la table `Customer` n'est pas ouverte dans le Concepteur de tables ou dans l'Éditeur Transact \-SQL.  
  
2.  Développez le nœud **Tables** dans l’**Explorateur d’objets SQL Server**. Cliquez avec le bouton droit sur la table **Customer** et sélectionnez **Renommer**.  
  
3.  Modifiez le nom de la table en **Customers** et appuyez sur ENTRÉE.  
  
4.  Notez qu'une opération de **mise à jour de la base de données** est appelée immédiatement en votre nom. SSDT appellera la procédure stockée sp_rename en votre nom pour renommer la table. S'il existe des objets dépendants, comme des contraintes de clés étrangères, ils seront aussi mis à jour.  
  
    > [!WARNING]  
    > Les dépendances basées sur un script, telles que les références à une table dans un affichage, ou les procédures stockées ne sont pas mises à jour automatiquement par SSDT. Après l'attribution du nouveau nom, vous pouvez utiliser le volet **Liste d'erreurs** pour rechercher toutes les autres dépendances et les corriger manuellement.  
  
5.  Appliquez la modification en suivant les étapes de la [Procédure : mettre à jour une base de données connectée avec Power Buffer](../ssdt/how-to-update-a-connected-database-with-power-buffer.md) suivante.  
  
6.  Dans l’**Explorateur d'objets SQL Server**, cliquez à nouveau avec le bouton droit sur **Customers** et sélectionnez **Afficher les données**. Notez que les données de la table sont intactes après le changement de nom.  
  
7.  Cliquez avec le bouton droit sur la table **Produits** et sélectionnez **Afficher le code**. Notez que la référence de clé étrangère a été automatiquement mise à jour vers `REFERENCES [dbo].[Customers] ([Id])` pour refléter le nouveau nom.  
  
### <a name="to-delete-a-table"></a>Pour supprimer une table  
  
1.  Dans l’**Explorateur d'objets SQL Server**, cliquez avec le bouton droit sur **Customers** et sélectionnez **Afficher les données**.  
  
2.  Dans la boîte de dialogue **Aperçu des mises à jour de la base de données**, sous **Action de l'utilisateur**, notez que SSDT a identifié tous les objets dépendants. Dans ce cas, une référence de clé étrangère qui sera supprimée.  
  
3.  Cliquez sur **Mettre à jour la base de données**.  
  
4.  Dans l’**Explorateur d'objets SQL Server**, cliquez avec le bouton droit sur **Produits** et sélectionnez **Afficher le code**. Notez que la référence de clé étrangère à la table `Customers` a disparu.  
  
    > [!WARNING]  
    > Si la table **Products** était ouverte dans le Concepteur de tables ou l'Éditeur Transact\-SQL lorsque la suppression s'est produite, elle ne sera pas actualisée automatiquement pour refléter la suppression de la référence de clé étrangère. De plus, des erreurs relatives à des références non résolues peuvent s'afficher dans la **Liste d'erreurs**. Pour résoudre ce problème, fermez le Concepteur de tables ou l'Éditeur Transact\-SQL et rouvrez la table Products.  
  
