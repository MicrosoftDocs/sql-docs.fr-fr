---
title: Utiliser l’éditeur Transact-SQL pour créer des objets de base de données
description: Suivez ce tutoriel pour apprendre à utiliser l’éditeur Transact-SQL afin d’effectuer des tâches de base de données principales, y compris la création et la recherche d’objets de base de données.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18; seo-lt-2019
ms.date: 09/24/2018
ms.openlocfilehash: 1c91b067ff8396968b2d26c57bf5f78d4f5d4181
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039929"
---
# <a name="tutorial-use-the-transact-sql-editor-to-create-database-objects---azure-data-studio"></a>Tutoriel : Utiliser l’éditeur Transact-SQL pour créer des objets de base de données - Azure Data Studio

La création et l’exécution de requêtes, de procédures stockées, de scripts, etc. sont les principales tâches des professionnels des bases de données. Ce didacticiel présente les principales fonctionnalités de l’éditeur T-SQL pour créer des objets de base de données.

Dans ce didacticiel, vous apprendrez à utiliser [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] pour :
> [!div class="checklist"]
> * Rechercher des objets de base de données
> * Modifier des données de table 
> * Utiliser des extraits de code pour écrire rapidement du T-SQL
> * Afficher les détails de l'objet de base de données avec l’option *Aperçu de la définition* et *Atteindre la définition*


## <a name="prerequisites"></a>Prérequis

Ce didacticiel nécessite la base de données *TutorialDB* de SQL Server ou Azure SQL Database. Pour créer la base de données *TutorialDB*, suivez un des démarrages rapides suivants :

- [Se connecter à et interroger SQL Server avec [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-server.md)
- [Se connecter à et interroger Azure SQL Database avec [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-database.md)


## <a name="quickly-locate-a-database-object-and-perform-a-common-task"></a>Localiser rapidement un objet de base de données et effectuer une tâche courante

[!INCLUDE[name-sos-short](../includes/name-sos-short.md)] fournit un widget de recherche pour rechercher rapidement des objets de base de données. La liste des résultats fournit un menu contextuel pour les tâches courantes relatives à l’objet sélectionné, telles que *Modifier les données* pour une table.

1. Ouvrez la barre latérale SERVEURS (**Ctrl +G**), développez **Bases de données**, puis sélectionnez **TutorialDB**. 

1. Ouvrez le *Tableau de bord TutorialDB* en cliquant avec le bouton droit sur **TutorialDB**, puis en sélectionnant **Gérer** dans le menu contextuel :

   ![Menu contextuel - Gérer](./media/tutorial-sql-editor/insight-open-dashboard.png)

1. Dans le tableau de bord, cliquez avec le bouton droit sur **dbo.Customers** (dans le widget de recherche) et sélectionnez **Modifier les données**.
   
   > [!TIP]
   > Pour les bases de données comportant de nombreux objets, utilisez le widget de recherche pour localiser rapidement la table, la vue, etc. que vous recherchez.

   ![Widget de recherche rapide](./media/tutorial-sql-editor/quick-search-widget.png)

1. Modifiez la colonne **Email** de la première ligne, saisissez *orlando0\@adventure-works.com*, puis appuyez sur **Entrée** pour enregistrer la modification.

   ![modifier les données](./media/tutorial-sql-editor/edit-data.png)

## <a name="use-t-sql-snippets-to-create-stored-procedures"></a>Utiliser des extraits de code T-SQL pour créer des procédures stockées

Azure Data Studio fournit de nombreux extraits de code T-SQL intégrés pour créer rapidement des instructions.


1. Ouvrez un nouvel éditeur de requête en appuyant sur **Ctrl + N**.

2. Saisissez **sql** dans l’éditeur, appuyez sur la flèche vers le bas jusqu’à **sqlCreateStoredProcedure**, puis appuyez sur la touche *Tab* (ou sur *Entrée*) pour charger l’extrait de code de création de procédure stockée.

   ![Capture d’écran montrant l’éditeur de requête avec sql tapé dans l’éditeur et l’option sqlCreateStoredProcedure mise en évidence.](./media/tutorial-sql-editor/snippet-list.png)

3. L’extrait de code de création de procédure stockée a deux champs configurés pour une modification rapide, *StoredProcedureName* et *SchemaName*. Sélectionnez *StoredProcedureName*, cliquez avec le bouton droit, puis sélectionnez **Modifier toutes les occurrences**. Maintenant, saisissez *getCustomer* et toutes les entrées *StoredProcedureName* sont modifiées en *getCustomer*.

   ![Capture d’écran montrant l’éditeur de requête avec mise en évidence de l’option Modifier toutes les occurrences.](./media/tutorial-sql-editor/snippet.png)

5. Remplacez toutes les occurrences de *SchemaName* en *dbo*. 
6. Cet extrait de code contient des paramètres d’espace réservé et de texte de corps nécessitant une mise à jour. L' instruction *EXECUTE* contient également le texte de l’espace réservé, car elle ne sait pas le nombre de paramètres que la procédure aura. Pour ce didacticiel, mettez à jour l’extrait de code afin qu’il ressemble à l’exemple suivant :

    ```sql
    -- Create a new stored procedure called 'getCustomer' in schema 'dbo'
    -- Drop the stored procedure if it already exists
    IF EXISTS (
    SELECT *
    FROM INFORMATION_SCHEMA.ROUTINES
    WHERE SPECIFIC_SCHEMA = N'dbo'
    AND SPECIFIC_NAME = N'getCustomer'
    )
    DROP PROCEDURE dbo.getCustomer
    GO
    -- Create the stored procedure in the specified schema
    CREATE PROCEDURE dbo.getCustomer
    @ID int
    -- add more stored procedure parameters here
    AS
    -- body of the stored procedure
    SELECT  c.CustomerId, 
    c.Name, 
    c.Location, 
    c.Email
    FROM dbo.Customers c
    WHERE c.CustomerId = @ID
    FOR JSON PATH

    GO
    -- example to execute the stored procedure we just created
    EXECUTE dbo.getCustomer 1
    GO
    ```
    
5. Pour créer la procédure stockée et lui attribuer une série de tests, appuyez sur **F5**.

La procédure stockée est maintenant créée, et le volet **RESULTS** affiche le client retourné au format JSON. Pour voir le format JSON mis en forme, cliquez sur l’enregistrement retourné. 


## <a name="use-peek-definition"></a>Utiliser l’aperçu de définition 

Azure Data Studio offre la possibilité d’afficher une définition d’objets à l’aide de la fonctionnalité d’aperçu de définition. Cette section crée une deuxième procédure stockée et utilise l’aperçu de définition pour voir quelles colonnes se trouvent dans une table pour créer rapidement le corps de la procédure stockée.

1. Ouvrez un nouvel éditeur en appuyant sur **Ctrl + N**. 

2. Saisissez *sql* dans l’éditeur, appuyez sur la flèche vers le bas jusqu’à *sqlCreateStoredProcedure*, puis appuyez sur la touche *Tab* (ou sur *Entrée*) pour charger l’extrait de code de création de procédure stockée.
3. Saisissez *setCustomer* pour *StoredProcedureName* et *dbo* pour *SchemaName*

3. Remplacez les espaces réservés @param par la définition de paramètre suivante :

   ```sql
   @json_val nvarchar(max)
   ```

4. Remplacez le corps de la procédure stockée par le code suivant :
   ```sql
   INSERT INTO dbo.Customers
   ```

5. Dans la ligne *INSERT* que vous venez d’ajouter, cliquez avec le bouton droit sur **dbo.Customers** et sélectionnez **Aperçu de la définition**.

   ![Aperçu de définition](./media/tutorial-sql-editor/peek-definition.png)

6. La définition de la table s’affiche pour vous permettre de voir rapidement les colonnes qui se trouvent dans la table. Reportez-vous à la liste des colonnes pour compléter facilement les instructions de votre procédure stockée. Terminez la création de l’instruction INSERT que vous avez ajoutée précédemment pour terminer le corps de la procédure stockée, puis fermez la fenêtre d’aperçu de définition:

   ```sql
   INSERT INTO dbo.Customers (CustomerId, Name, Location, Email)
       SELECT CustomerId, Name, Location, Email
       FROM OPENJSON (@json_val)
       WITH(   CustomerId int, 
               Name nvarchar(50), 
               Location nvarchar(50), 
               Email nvarchar(50)
    )
   ```
7. Supprimez (ou commentez) la commande *EXECUTE* en bas de la requête.
8. L’instruction complète doit ressembler au code suivant :

   ```sql
   -- Create a new stored procedure called 'setCustomer' in schema 'dbo'
   -- Drop the stored procedure if it already exists
   IF EXISTS (
   SELECT *
       FROM INFORMATION_SCHEMA.ROUTINES
       WHERE SPECIFIC_SCHEMA = N'dbo'
       AND SPECIFIC_NAME = N'setCustomer'
   )
   DROP PROCEDURE dbo.setCustomer
   GO
   -- Create the stored procedure in the specified schema
   CREATE PROCEDURE dbo.setCustomer
       @json_val nvarchar(max) 
   AS
       -- body of the stored procedure
       INSERT INTO dbo.Customers (CustomerId, Name, Location, Email)
       SELECT CustomerId, Name, Location, Email
       FROM OPENJSON (@json_val)
       WITH(   CustomerId int, 
               Name nvarchar(50), 
               Location nvarchar(50), 
               Email nvarchar(50)
       )
   GO
   ```

8. Pour créer la procédure stockée *setCustomer*, appuyez sur **F5**.

## <a name="use-save-query-results-as-json-to-test-the-setcustomer-stored-procedure"></a>Utiliser l’enregistrement des résultats de la requête au format JSON pour tester la procédure stockée setCustomer

La procédure stockée *setCustomer* créée dans la section précédente nécessite que les données JSON soient passées dans le paramètre *\@json_val*. Cette section montre comment obtenir du JSON correctement mis en forme à transmettre au paramètre afin de pouvoir tester la procédure stockée.

1. Dans le volet **SERVERS**, cliquez avec le bouton droit sur *dbo.Customers*, puis cliquez sur **SELECT TOP 1000 Rows**.

2. Sélectionnez la première ligne de la vue de résultats. Assurez-vous que la ligne entière est sélectionnée (cliquez sur le numéro 1 dans la colonne la plus à gauche), puis sélectionnez **Enregistrer en tant que JSON**.  
3. Remplacez le dossier par un emplacement dont vous vous souviendrez pour pouvoir supprimer le fichier ultérieurement (par exemple le bureau), puis cliquez sur **Enregistrer**. Le fichier au format JSON s’ouvre.

   ![Enregistrer au format JSON](./media/tutorial-sql-editor/save-as-json.png)

4. Sélectionnez les données JSON dans l’éditeur et copiez-les.
5. Ouvrez un nouvel éditeur en appuyant sur **Ctrl + N**.
6. Les étapes précédentes montrent comment obtenir facilement les données correctement mises en forme pour terminer l’appel de la procédure *setCustomer*. Vous pouvez voir que le code suivant utilise le même format JSON avec les nouveaux détails du client afin de pouvoir tester la procédure *setCustomer*. L’instruction comprend une syntaxe pour déclarer le paramètre et exécuter les nouvelles procédures d’obtention et de définition. Vous pouvez coller les données copiées à partir de la section précédente et les modifier afin qu’elles soient identiques à celles de l’exemple suivant, ou simplement coller l’instruction suivante dans l’éditeur de requête.

   ```sql
   -- example to execute the stored procedure we just created
   declare @json nvarchar(max) =
   N'[
       {
           "CustomerId": 5,
           "Name": "Lucy",
           "Location": "Canada",
           "Email": "lucy0@adventure-works.com"
       }
   ]'

   EXECUTE dbo.setCustomer @json_val = @json
   GO

   EXECUTE dbo.getCustomer @ID = 5
   ```

7. Exécutez le script en appuyant sur **F5**. Le script insère un nouveau client et retourne les informations du nouveau client au format JSON. Cliquez sur le résultat pour ouvrir une vue mise en forme.

   ![résultat du test](./media/tutorial-sql-editor/test-result.png)

## <a name="next-steps"></a>Étapes suivantes
Dans ce didacticiel, vous avez appris à :
> [!div class="checklist"]
> * Rechercher rapidement des objets de schéma
> * Modifier des données de table 
> * Écriture de scripts T-SQL à l’aide d’extraits de code
> * Apprenez-en davantage sur les détails de l'objet de base de données avec l’option Aperçu de la définition et Atteindre la définition


Pour savoir comment activer le widget **Cinq requêtes les plus lentes**, suivez le didacticiel suivant :

> [!div class="nextstepaction"]
> [Activer le widget d’aperçu des requêtes les plus lentes](tutorial-qds-sql-server.md)
