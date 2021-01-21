---
title: Scripts d’objets SSMS
description: Générez des scripts T-SQL (Transact-SQL) pour différents objets qui figurent dans SQL Server Management Studio (SSMS).
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.topic: tutorial
keywords: SQL Server, SSMS, SQL Server Management Studio, scripts, génération de scripts
helpviewer_keywords:
- projects [SQL Server Management Studio], tutorials
- source controls [SQL Server Management Studio], tutorials
- Help [SQL Server], SQL Server Management Studio
- tutorials [SQL Server Management Studio]
- Transact-SQL tutorials
- solutions [SQL Server Management Studio], tutorials
- SQL Server Management Studio [SQL Server], tutorials
- scripts [SQL Server], SQL Server Management Studio
author: markingmyname
ms.author: maghan
ms.reviewer: sstein
ms.date: 03/13/2018
ms.openlocfilehash: 2607b029389d0a0d3f4b59e55e031b768baf4aac
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597251"
---
# <a name="script-objects-in-sql-server-management-studio"></a>Générer des scripts d’objet dans SQL Server Management Studio

Ce tutoriel explique comment générer des scripts T-SQL (Transact-SQL) pour différents objets qui figurent dans SQL Server Management Studio (SSMS). Dans ce tutoriel, vous trouverez des exemples de génération de scripts pour les objets suivants :

> [!div class="checklist"]
> * Requêtes quand vous exécutez des actions dans l’interface graphique utilisateur
> * Bases de données de deux manières différentes (Script comme et Générer des scripts)
> * Tables
> * Procédures stockées
> * Événements étendus

Pour générer un script d’objet dans l’**Explorateur d’objets**, cliquez avec le bouton droit sur l’objet et sélectionnez l’option **Scripter l’objet comme**. Ce tutoriel illustre le processus.

## <a name="prerequisites"></a>Conditions préalables requises

Pour suivre ce tutoriel, vous avez besoin de SQL Server Management Studio, de l’accès à un serveur qui exécute SQL Server et d’une base de données AdventureWorks.

* Installez [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).
* Installez [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
* Téléchargez les [exemples de bases de données AdventureWorks2016](https://github.com/Microsoft/sql-server-samples/releases).

Les instructions de restauration des bases de données dans SSMS se trouvent ici : [Restaurer une base de données](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md). 

## <a name="script-queries-from-the-gui"></a>Générer des scripts pour des requêtes à partir de l’interface graphique utilisateur

Vous pouvez générer le code T-SQL associé pour une tâche chaque fois que vous utilisez l’interface graphique utilisateur dans SSMS pour la compléter. Les exemples suivants montrent comment procéder quand vous sauvegardez une base de données et quand vous réduisez le journal des transactions. Ces mêmes étapes peuvent être appliquées à une action effectuée via l’interface graphique utilisateur.

### <a name="script-t-sql-when-you-back-up-a-database"></a>Script T-SQL quand vous sauvegardez une base de données

1. Connectez-vous à un serveur qui exécute SQL Server.

2. Développez le nœud **Bases de données** .

3. Cliquez avec le bouton droit sur la base de données **Adventureworks2016** > **Tâches** > **Sauvegarder** :

    ![Sauvegarder une base de données](media/scripting-ssms/backupdb.png)

4. Configurez la sauvegarde comme vous le souhaitez. Dans ce tutoriel, tous les éléments conservent les valeurs par défaut. Toutefois, toutes les modifications apportées dans la fenêtre sont également répercutées dans le script. 

5. Sélectionnez **Script** > **Action de script dans une nouvelle fenêtre de requête** :

    ![Générer un script de sauvegarde de base de données : action de script](media/scripting-ssms/scriptdbbackup.PNG)
6. Passez en revue le script T-SQL renseigné dans la fenêtre de requête.

    ![Générer un script de sauvegarde de base de données : examen du T-SQL](media/scripting-ssms/dbbackupscript.PNG)
7. Sélectionnez **Exécuter** pour exécuter la requête permettant de sauvegarder la base de données via T-SQL. 

### <a name="script-t-sql-when-you-shrink-the-transaction-log"></a>Script T-SQL quand vous réduisez le journal des transactions

1. Cliquez avec le bouton droit sur la base de données **Adventureworks2016** > **Tâches** > **Réduire** > **Fichiers** :

     ![Réduire les fichiers](media/scripting-ssms/shrinkfiles.png)

2. Sélectionnez **Journal** dans la zone de liste déroulante **Type de fichier** :

    ![Réduire le journal des transactions](media/scripting-ssms/shrinktlog.png)

3. Sélectionnez **Script** et **Action de script dans le Presse-papiers** :

    ![Générer un script dans le Presse-papiers](media/scripting-ssms/scriptactiontoclipboard.png)

4. Ouvrez une fenêtre **Nouvelle requête** et collez le contenu du Presse-papiers. (Cliquez avec le bouton droit dans la fenêtre, puis sélectionnez **Coller**.)

    ![Coller le script](media/scripting-ssms/paste.png)

5. Sélectionnez **Exécuter** pour exécuter la requête et réduire le journal des transactions.

## <a name="script-databases"></a>Générer des scripts pour les bases de données

La section suivante décrit comment générer le script de la base de données à l’aide des options **Script comme** et **Générer des scripts**. L’option **Script comme** recrée la base de données et ses options de configuration. Vous pouvez générer un script pour le schéma et pour les données à l’aide de l’option **Générer des scripts**. Dans cette section, vous allez créer deux nouvelles bases de données. Vous utiliserez l’option **Script comme** pour créer *AdventureWorks2016a*. Vous utiliserez l’option **Générer des scripts** pour créer *AdventureWorks2016b*.

### <a name="script-a-database-by-using-the-script-option"></a>Générer un script de base de données à l’aide de l’option Script

1. Connectez-vous à un serveur qui exécute SQL Server.

2. Développez le nœud **Bases de données** .

3. Cliquez avec le bouton droit sur la base de données **AdventureWorks2016** > **Générer un script de la base de données en tant que** > **Créer dans** > **Nouvelle fenêtre d’éditeur de requête** :

    ![Générer un script de base de données](media/scripting-ssms/scriptdb.png)

4. Passez en revue la requête de création de base de données dans la fenêtre :

    ![Base de données hors script](media/scripting-ssms/scriptedoutdb.png) Cette option génère un script uniquement pour les options de configuration de base de données.

5. Sur votre clavier, sélectionnez Ctrl+F pour ouvrir la boîte de dialogue **Rechercher**. Sélectionnez la flèche vers le bas pour ouvrir l’option **Remplacer**. Sur la ligne **Rechercher** en haut, tapez AdventureWorks2016, et sur la ligne **Remplacer** en bas, tapez AdventureWorks2016a.

6. Sélectionnez **Remplacer tout** pour remplacer toutes les instances de *AdventureWorks2016* par *AdventureWorks2016a*. 

    ![Rechercher et remplacer](media/scripting-ssms/findandreplace.png)

7. Sélectionnez **Exécuter** pour exécuter la requête et créer votre nouvelle base de données AdventureWorks2016a. 

### <a name="script-a-database-by-using-the-generate-scripts-option"></a>Générer un script de base de données à l’aide de l’option Générer des scripts

1. Connectez-vous à un serveur qui exécute SQL Server.

2. Développez le nœud **Bases de données** .

3. Cliquez avec le bouton droit sur **AdventureWorks2016** > **Tâches** > **Générer des scripts**:

    ![Générer des scripts pour des bases de données](media/scripting-ssms/generatescriptsfordb.png)

4. La page **Introduction** s’ouvre. Sélectionnez **Suivant** pour ouvrir la page **Sélectionner les objets**. Vous pouvez sélectionner la base de données entière ou des objets spécifiques de la base de données. Sélectionnez **Générer un script de la base de données entière et de tous les objets de base de données**.

    ![Générer des scripts pour des objets](media/scripting-ssms/scriptobjects.png)

5. Sélectionnez **Suivant** pour ouvrir la page **Définir les options de script**. Ici, vous pouvez configurer où enregistrer le script et certaines options avancées supplémentaires. 

    a. Sélectionnez **Enregistrer dans une nouvelle fenêtre de requête**.

    b. Sélectionnez **Avancé** et vérifiez que ces options sont définies :

      * **Générer un script des statistiques** défini sur *Générer un script des statistiques*.
      * **Types de données à inclure dans le script** défini sur *Schéma uniquement*.
      * **Générer un script pour les index** défini sur *True*.

   ![Scripter des objets](media/scripting-ssms/advancedscripts.png)

   > [!NOTE]
   > Vous pouvez scripter les données de la base de données quand vous sélectionnez *Schéma et données* pour l’option **Types de données à inclure dans le script**. Toutefois, cette action n’est pas idéale avec les grandes bases de données, car elle peut prendre plus de mémoire que SSMS ne peut en allouer. Cette limitation n’est pas un problème pour les petites bases de données. Si vous souhaitez déplacer les données d’une base de données plus grande, utilisez l’[Assistant Importation et exportation](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

6. Sélectionnez **OK**, puis **Suivant**.

7. Sélectionnez **Suivant** dans le **Récapitulatif**. Ensuite, sélectionnez **Suivant** pour générer le script dans une fenêtre **Nouvelle requête**.

8. Sur votre clavier, ouvrez la boîte de dialogue **Rechercher** (Ctrl+F). Sélectionnez la flèche vers le bas pour ouvrir l’option **Remplacer**. Sur la ligne **Rechercher** en haut, entrez *AdventureWorks2016*. Sur la ligne **Remplacer** en bas, entrez *AdventureWorks2016b*.

9. Sélectionnez **Remplacer tout** pour remplacer toutes les instances de *AdventureWorks2016* par *AdventureWorks2016b*.

    ![AdventureWorks 2016](media/scripting-ssms/adventureworks2016b.png)

10. Sélectionnez **Exécuter** pour exécuter la requête et créer votre nouvelle base de données AdventureWorks2016b.

## <a name="script-tables"></a>Scripter des tables

Cette section explique comment scripter les tables de votre base de données. Utilisez cette option pour créer la table, ou supprimer et créer la table. Vous pouvez aussi utiliser cette option pour générer un script du code T-SQL associé à la modification de la table, par exemple pour des opération d’insertion ou de mise à jour. Dans cette section, vous allez supprimer une table, puis la recréer. 

1. Connectez-vous à un serveur qui exécute SQL Server.

2. Développez le nœud **Bases de données**.

3. Développez le nœud de base de données **AdventureWorks2016**. 

4. Développez le nœud **Tables**.

5. Cliquez avec le bouton droit sur **dbo.ErrorLog** > **Générer un script de la table en tant que** > **DROP et CREATE To** > **Nouvelle fenêtre d’éditeur de requête** :

    ![Scripter la table](media/scripting-ssms/scripttable.png)

6. Sélectionnez **Exécuter** pour exécuter la requête. Cette action supprime la table *Errorlog* et la recrée. 

    >[!NOTE]
    > La table *Errorlog* est vide par défaut dans la base de données AdventureWorks2016. Par conséquent, vous ne perdez pas de données en la supprimant. Toutefois, la réalisation de ces étapes sur une table avec des données entraîne une perte de données.

## <a name="script-stored-procedures"></a>Scripter des procédures stockées

Dans cette section, vous allez découvrir comment supprimer et créer une procédure stockée.  

1. Connectez-vous à un serveur qui exécute SQL Server.

2. Développez le nœud **Bases de données**.

3. Développez le nœud **Programmabilité**. 

4. Développez le nœud **Procédure stockée**.

5. Cliquez avec le bouton droit sur la procédure stockée **dbo.uspGetBillOfMaterials** > **Générer un script de la procédure stockée en tant que** > **DROP et CREATE To** > **Nouvelle fenêtre d’éditeur de requête** :

    ![Scripter des procédures stockées](media/scripting-ssms/script-stored-procedure.png)

## <a name="script-extended-events"></a>Scripter des événements étendus

Cette section explique comment scripter des [événements étendus](../../relational-databases/extended-events/extended-events.md).

1. Connectez-vous à un serveur qui exécute SQL Server.

2. Développez le nœud **Gestion**.

3. Développez le nœud **Événements étendus**.

4. Développez le nœud **Sessions**.

5. Cliquez avec le bouton droit sur la session étendue qui vous intéresse dans > **Générer un script de la session en tant que** > **CREATE To** > **Nouvelle fenêtre d’éditeur de requête** :

    ![Session étendue de nouvelle fenêtre d’éditeur de requête](media/scripting-ssms/scriptxevents.png)

6. Dans la **Nouvelle fenêtre d’éditeur de requête**, remplacez le nouveau nom de la session *system_health* par *system_health2*. Sélectionnez **Exécuter** pour exécuter la requête.

7. Cliquez avec le bouton droit sur **Sessions** dans l’**Explorateur d’objets**. Sélectionnez **Actualiser** pour voir votre nouvelle session d’événements étendus. L’icône verte en regard de la session indique que celle-ci est en cours d’exécution. L’icône rouge indique que la session est arrêtée.

    ![Nouvelle session d’événements étendus](media/scripting-ssms/newxevent.png)

    >[!NOTE]
    > Vous pouvez démarrer la session en cliquant avec le bouton droit et en sélectionnant **Démarrer**. Toutefois, puisqu’il s’agit d’une copie de la session **system_health** déjà en cours, vous pouvez ignorer cette étape. Vous pouvez supprimer la copie de la session d’événements étendus en cliquant avec le bouton droit et en sélectionnant **Supprimer**.

## <a name="next-steps"></a>Étapes suivantes

La meilleure façon de se familiariser avec SSMS est d’effectuer des exercices pratiques. Ces articles *Tutoriel* et *Procédure* vous aident à vous familiariser avec les différentes fonctionnalités disponibles dans SSMS. Ces articles vous apprennent à gérer les composants de SSMS et à trouver les fonctionnalités utilisées régulièrement.

* [Se connecter à une instance et l’interroger](../quickstarts/ssms-connect-query-sql-server.md)
* [Utilisation de modèles dans SSMS](../template/templates-ssms.md)
* [Configuration de SSMS](ssms-configuration.md)
* [Conseils et astuces supplémentaires pour utiliser SSMS](ssms-tricks.md)