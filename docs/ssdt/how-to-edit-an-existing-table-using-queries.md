---
title: Modifier une table existante à l’aide de requêtes
description: Découvrez comment utiliser une requête Transact-SQL pour modifier une définition ou des données de table. Affichez des exemples de modification d’une définition de table et d’insertion de lignes dans une table.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 58f4de8e-97b4-4bcb-953f-f3d428432491
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: bd6ea94af644c981d368608a0fe864dbdf237181
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018219"
---
# <a name="how-to-edit-an-existing-table-using-queries"></a>Procédure : Modifier une table existante à l’aide de requêtes

Vous pouvez modifier la définition d'une table ou de ses données en écrivant une requête Transact\-SQL. Pour consulter ou entrer des données dans une table visuellement, utilisez l'Éditeur de données comme décrit dans [Développement d’une base de données connectée](../ssdt/connected-database-development.md).  
  
> [!WARNING]  
> Les procédures suivantes utilisent les entités créées dans les procédures précédentes de la section [Développement d’une base de données connectée](../ssdt/connected-database-development.md).  
  
### <a name="to-edit-the-definition-of-an-existing-table"></a>Pour modifier la définition d'une table existante  
  
1.  Dans l'**Explorateur d'objets SQL Server**, développez le nœud **Tables** de la base de données **Trade** et cliquez avec le bouton droit sur **dbo.Suppliers**.  
  
2.  Sélectionnez **Concepteur de vues** pour afficher le schéma de la table dans le Concepteur de tables.  
  
3.  Activez la case à cocher **Autoriser les valeurs NULL** correspondant à la colonne **Adresse**. Notez que le code correspondant dans le volet de script est changé en `NULL` immédiatement.  
  
4.  Mettez à jour la base de données en suivant les étapes de la rubrique [ Mettre à jour une base de données connectée avec Power Buffer](../ssdt/how-to-update-a-connected-database-with-power-buffer.md).  
  
### <a name="to-populate-data-in-new-tables-using-a-transact-sql-query"></a>Pour remplir de nouvelles tables de données à l'aide d'une requête Transact\-SQL  
  
1.  Cliquez avec le bouton droit sur le nœud de base de données **Trade** et sélectionnez **Nouvelle requête**.  
  
2.  Dans le volet de script, collez le code suivant.  
  
    ```  
    insert into dbo.Suppliers values  
    (1, 'NorthWind Traders', 'Seattle, WA'),  
    (2, 'Contoso', 'Tacoma, WA')  
    GO  
  
    insert dbo.Customer values  
    (1, 'Fourth Coffee')  
    GO  
  
    insert dbo.Products values  
    (1, 'Apples', 0, 1, 1),  
    (2, 'Instant Coffee', 1, 2, 1)  
    GO  
    ```  
  
3.  Cliquez sur le bouton **Exécuter la requête** pour exécuter cette requête. Les éléments suivants du volet **Message** indiquent que les lignes ont été ajoutées aux tables.  
  
**(2 ligne(s) affectée(s))(1 ligne(s) affectée(s))(2 ligne(s) affectée(s))**  
  
4.  Remplacez le code du volet de script par le code suivant et exécutez la requête. Ce code tentera d'ajouter une nouvelle ligne à la table `Products` avec un `ShelfLife` de 6.  
  
    ```  
    insert dbo.Products values  
    (3, 'Potato Chips', 6, 1, 1)  
    GO  
    ```  
  
5.  Le volet **Message** indique que l'instruction `INSERT` est en conflit avec votre contrainte de validation existante qui limite la valeur de `ShelfLife` à une valeur inférieure à 5. La table Products n'est pas mise à jour, car l'instruction se solde par un échec de contrainte existante.  
  
6.  Remplacez le code par le code suivant et réexécutez la requête. Notez que cette fois-ci, la ligne est mise à jour.  
  
    ```  
    insert dbo.Products values  
    (3, 'Potato Chips', 2, 1, 1)  
    GO  
    ```  
  
## <a name="see-also"></a>Voir aussi  
[Gérer des tables et des relations, et résoudre les erreurs](../ssdt/manage-tables-relationships-and-fix-errors.md)  
[Utiliser l'Éditeur Transact-SQL pour modifier et exécuter des scripts](../ssdt/use-transact-sql-editor-to-edit-and-execute-scripts.md)  
  
