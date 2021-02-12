---
title: Créer des objets de base de données avec le Concepteur de tables
description: Découvrez comment créer une nouvelle base de données dans l’Explorateur d’objets SQL Server. Consultez comment créer des tables, des contraintes et des références de clé étrangère nouvelles dans le Concepteur de tables.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.design.table.scriptpanel
- sql.data.tools.design.table.context.view
- Microsoft.Data.Relational.Design.PW.RelationshipsDescriptor.OnDelete
ms.assetid: 9c9479c1-9bfc-4039-837e-e53fce67723d
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: a055bd1f8de41686e1c3477e3e4810e78d6b5025
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018309"
---
# <a name="how-to-create-database-objects-using-table-designer"></a>Procédure : Créer des objets de base de données avec le Concepteur de tables

Non seulement le nouveau nœud **SQL Server** de l'**Explorateur d'objets SQL Server** est très similaire à SSMS visuellement, mais vous pouvez aussi créer de nouveaux objets à l'aide de menus contextuels dont le fonctionnement est semblable à celui de leurs homologues SSMS.  
  
Par exemple, vous pouvez créer une nouvelle base de données sous le nœud **Bases de données**. De la même façon, vous pouvez sélectionner une base de données spécifique et créer ou modifier des définitions de tables et leurs objets de programmation associés à la volée à l'aide du nouveau Concepteur de tables. Dans le Concepteur de tables, vous pouvez basculer vers un volet de script qui vous permet de modifier directement le script qui définit cette table.  
  
### <a name="to-create-a-new-database"></a>Pour créer une nouvelle base de données  
  
1.  Dans l'**Explorateur d'objets SQL Server**, sous le nœud **SQL Server**, développez l'instance de serveur connecté.  
  
2.  Cliquez avec le bouton droit sur le nœud **Bases de données** et sélectionnez **Ajouter une nouvelle base de données**.  
  
3.  Renommez la nouvelle base de données **Trade**.  
  
### <a name="to-create-new-tables-using-the-table-designer"></a>Pour créer de nouvelles tables à l'aide du Concepteur de tables  
  
1.  Développez le nouveau nœud **Trade** créé. Cliquez avec le bouton droit sur le nœud **Tables** et sélectionnez **Ajouter une nouvelle table**.  
  
2.  Le Concepteur de tables s'ouvre dans une nouvelle fenêtre. Le concepteur comprend la Grille Colonnes, un volet de script et un volet contextuel. La Grille Colonnes répertorie toutes les colonnes de la table. Nous examinerons d'autres composants du concepteur dans des procédures ultérieures.  
  
3.  Dans le volet de script, renommez la table en `Suppliers`. Plus spécifiquement, remplacez  
  
    ```  
    CREATE TABLE [dbo].[Table1]  
    ```  
  
    par  
  
    ```  
    CREATE TABLE [dbo].[Suppliers]  
    ```  
  
4.  Cliquez sur la ligne vide dans la Grille Colonnes pour ajouter une nouvelle colonne à la table.  Entrez **CompanyName** pour le champ **Nom**, **nvarchar (128)** pour **Type de données** et désactivez le champ **Autoriser les valeurs NULL**. Lorsque vous accédez par tabulation à un autre champ, notez que le volet de script est mis à jour immédiatement.  
  
5.  Ajoutez une autre colonne. Entrez **Adresse** pour le champ **Nom**, **nvarchar (MAX)** pour **Type de données**, et désactivez le champ **Autoriser les valeurs NULL**.  
  
    > [!WARNING]  
    > Lorsque vous modifiez des objets d'une base de données connectée, ne les enregistrez pas sur le disque local. Pour enregistrer correctement vos modifications dans la base de données, suivez les étapes de la prochaine [Procédure : mettre à jour une base de données connectée avec Power Buffer](../ssdt/how-to-update-a-connected-database-with-power-buffer.md).  
  
6.  Répétez les étapes précédentes pour créer une autre table nommée **Customer**. Cette fois-ci, ajoutez les colonnes suivantes à la table Customer à l'aide de la Grille Colonnes. Et n’oubliez pas de modifier le script afin que le nom de la table soit `[dbo].[Customer]`.  
  
    |Nom|Type de données|**Null autorisé**|  
    |--------|-------------|-------------------|  
    |Id|int|non cochée|  
    |Nom|nvarchar (128)|non cochée|  
  
7.  Créez une autre table nommée **Products**. Ajoutez les colonnes suivantes à la table Products à l'aide de la Grille Colonnes. Et n’oubliez pas de modifier le script afin que le nom de la table soit `[dbo].[Products]`.  
  
    |Nom|Type de données|**Null autorisé**|  
    |--------|-------------|-------------------|  
    |Id|int|non cochée|  
    |Nom|nvarchar (128)|non cochée|  
    |ShelfLife|int|cochée|  
    |SupplierId|int|cochée|  
    |CustomerId|int|cochée|  
  
### <a name="to-create-a-new-check-constraint-using-the-table-designer"></a>Pour créer une nouvelle contrainte de validation à l'aide du Concepteur de tables  
  
1.  Le volet contextuel du Concepteur de tables propose un affichage logique de la définition de table (clés, contraintes, déclencheurs, etc.), et vous permet de sélectionner un objet pour mettre en surbrillance ses relations par rapport à des colonnes individuelles.  
  
    Pour la table Products, cliquez avec le bouton droit sur le nœud **Contraintes de validation** dans le volet contextuel du Concepteur de tables et sélectionnez **Ajouter une nouvelle contrainte de validation**.  
  
2.  Notez que le nombre de nœuds est incrémenté automatiquement de 1.  
  
3.  Cliquez dans le volet de script et remplacez la définition par défaut de la contrainte par la définition suivante :  
  
    ```  
    CONSTRAINT [CK_Products_ShelfLife] CHECK ([ShelfLife] <5),  
    ```  
  
    Cette contrainte limitera la valeur de ShelfLife d'une ligne à une valeur inférieure à 5.  
  
### <a name="to-create-new-foreign-key-references-using-the-table-designer"></a>Pour créer une nouvelle référence de clé étrangère à l'aide du Concepteur de tables  
  
1.  Pour la table Products, cliquez avec le bouton droit sur le nœud **Clés étrangères** dans le volet contextuel et sélectionnez **Ajouter une nouvelle clé étrangère**.  
  
2.  Notez que le nombre de nœuds est incrémenté automatiquement de 1.  
  
3.  Cliquez dans le volet de script et remplacez la définition par défaut de la référence de clé étrangère par la définition suivante :  
  
    ```  
    CONSTRAINT [FK_Products_SupplierId] FOREIGN KEY ([SupplierId]) REFERENCES [dbo].[Suppliers] ([Id]),  
    ```  
  
4.  Répétez les étapes ci-dessus pour ajouter une autre référence de clé étrangère à la table Products. Cette fois-ci, remplacez la définition par défaut par la définition suivante :  
  
    ```  
    CONSTRAINT [FK_Products_CustomerId] FOREIGN KEY ([CustomerId]) REFERENCES [dbo].[Customer] ([Id])  
    ```  
  
## <a name="see-also"></a>Voir aussi  
[Gérer des tables et des relations, et résoudre les erreurs](../ssdt/manage-tables-relationships-and-fix-errors.md)  
  
