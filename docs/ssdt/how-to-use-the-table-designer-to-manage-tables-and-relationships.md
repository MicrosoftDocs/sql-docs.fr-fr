---
title: Utiliser le Concepteur de tables pour gérer les tables et les relations
description: Familiarisez-vous avec le Concepteur de tables. Découvrez comment utiliser cet outil pour créer et modifier une structure de table de base de données et pour afficher des relations entre des objets de base de données.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.design.table.columnspecification.index.dialog
- sql.data.tools.design.table.columnsgrid.view
- sql.data.tools.design.table.scriptpanel
ms.assetid: 322a2903-d7a6-4f52-9048-1bd413b4c799
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: a71009351c187bdda50c00b4f3b00bfee0fd2acf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017999"
---
# <a name="how-to-use-the-table-designer-to-manage-tables-and-relationships"></a>Procédure : Utiliser le Concepteur de tables pour gérer les tables et les relations

Le Concepteur de tables offre une expérience visuelle parallèle à l’Éditeur Transact\-SQL pour la création et la modification de la structure des tables, y compris des objets de programmation propres aux tables, pour les bases de données SQL Server.  Il s’exécute lorsque l’utilisateur crée une table pour une base de données connectée ou un projet, ou double-clique pour modifier une table dans l’Explorateur d’objets SQL Server ou l’Explorateur de solutions.  
  
Le concepteur comprend la Grille Colonnes, un volet de script et un volet contextuel. La Grille Colonnes répertorie toutes les colonnes de la table. Vous pouvez ajouter, modifier et supprimer des colonnes dans cette grille.  Le volet contextuel présente un affichage logique de la définition de table (clés, indices, contraintes, déclencheurs, etc.) et permet de sélectionner un objet pour mettre en surbrillance ses relations avec chaque colonne. Vous pouvez aussi ajouter de nouveaux objets à la table dans ce volet et modifier les propriétés d'un objet sélectionné dans la grille des propriétés. Le volet de script affiche la définition de la structure de la table, et met en surbrillance le script des objets sélectionnés dans le volet contextuel ou la Grille Colonnes. Vous pouvez modifier le script côte à côte avec la Grille Colonnes et le volet contextuel dans l'affichage. Les modifications apportées à l'un des trois volets seront propagées aux deux autres immédiatement.  
  
> [!WARNING]  
> Les procédures suivantes utilisent les entités créées dans les procédures des sections [Développement de base de données connectée](../ssdt/connected-database-development.md) et [Développement de base de données hors connexion orienté projet](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-create-a-new-table"></a>Pour créer une table  
  
1.  Ouvrez le projet TradeDev que vous avez utilisé au cours des procédures précédentes.  
  
2.  Dans **l’Explorateur de solutions**, développez le dossier **dbo**, cliquez avec le bouton droit sur le dossier **Tables**, puis sélectionnez **Ajouter** et **Table**.  
  
3.  Nommez la nouvelle table **Shipper** et cliquez sur **Ajouter**.  
  
4.  Le Concepteur de tables s'ouvre. Dans la Grille Colonnes, ajoutez une nouvelle colonne à la table en lui donnant le nom **ShipperName** et le type de données **int**.  
  
5.  Sachez que vous pouvez aussi modifier les propriétés des colonnes dans la fenêtre **Propriétés**. Cliquez sur la colonne **ShipperName** et, dans la fenêtre **Propriétés**, changez le **DataType** de cette colonne en **nvarchar** et **length** en **128**. Notez que lorsque vous déplacez le focus hors du champ, le volet de script et la Grille Colonnes du concepteur sont automatiquement mises à jour pour refléter vos modifications.  
  
### <a name="to-create-a-new-foreign-key-constraint"></a>Pour créer une nouvelle contrainte de clé étrangère  
  
1.  Cliquez avec le bouton droit sur le nœud **Clés étrangères** dans le volet contextuel du concepteur et sélectionnez **Ajouter une nouvelle clé étrangère**.  
  
2.  Notez que le nombre de nœuds est incrémenté automatiquement de 1. Appuyez sur Entrée pour accepter le nom par défaut de la contrainte.  
  
3.  Remplacez la définition par défaut de la contrainte dans le volet de script par la définition suivante :  
  
    ```  
    CONSTRAINT [FK_Shipper_Products] FOREIGN KEY ([Id]) REFERENCES [dbo].[Products]([Id])  
    ```  
  
    Notez que l'expérience de création et modification d'entités de base de données pour un projet en mode hors connexion est identique à celle d'exécution des tâches avec une base de données connectée.  
  
## <a name="see-also"></a>Voir aussi  
[Procédure : créer des objets de base de données avec le Concepteur de tables](../ssdt/how-to-create-database-objects-using-table-designer.md)  
  
