---
title: Sauvegarde et restauration des bases de données
description: Suivez ce tutoriel pour découvrir comment sauvegarder et restaurer des bases de données avec Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu
ms.custom: seodec18
ms.date: 11/04/2019
ms.openlocfilehash: 851fbdf950656f9edfba76e3d5a0aae54e8e3d99
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048169"
---
# <a name="tutorial-back-up-and-restore-databases-using-azure-data-studio"></a>Tutoriel : Sauvegarder et restaurer des bases de données avec Azure Data Studio

Dans ce didacticiel, vous apprendrez à utiliser Azure Data Studio pour :
> [!div class="checklist"]
> * Sauvegarder une base de données.
> * Afficher l’état de la sauvegarde.
> * Générer le script utilisé pour effectuer la sauvegarde.
> * Restaurer la base de données.
> * Afficher l’état de la tâche de restauration.

## <a name="prerequisites"></a>Prérequis

Ce tutoriel nécessite la base de données *TutorialDB* de SQL Server. Pour créer la base de données TutorialDB, effectuez le démarrage rapide suivant :

* [Utiliser Azure Data Studio pour vous connecter et interroger SQL Server](quickstart-sql-server.md)

Ce tutoriel nécessite une connexion à une base de données SQL Server. Azure SQL Database proposant des sauvegardes automatisées, Azure Data Studio n’effectue pas de sauvegarde et de restauration d’Azure SQL Database. Pour plus d’informations, consultez [En savoir plus sur les sauvegardes automatiques SQL Database](/azure/sql-database/sql-database-automated-backups).

## <a name="back-up-a-database"></a>Sauvegarder une base de données

1. Ouvrez le tableau de bord de base de données TutorialDB en ouvrant la barre latérale **SERVEURS**. Sélectionnez ensuite **CTRL+G**, développez **Bases de données**, cliquez avec le bouton droit sur **TutorialDB**, puis sélectionnez **Gérer**.

1. Ouvrez la boîte de dialogue **Sauvegarder la base de données** en sélectionnant **Sauvegarder** dans le widget **Tâches**.

   ![Capture d’écran montrant le widget Tâches.](./media/tutorial-backup-restore-sql-server/tasks.png)

1. Ce tutoriel utilise les options de sauvegarde par défaut, donc sélectionnez **Sauvegarder**.

   ![Capture d’écran montrant la boîte de dialogue Sauvegarde.](./media/tutorial-backup-restore-sql-server/backup-dialog.png)

Après avoir sélectionné **Sauvegarder**, la boîte de dialogue **Sauvegarder la base de données** disparaît et le processus de sauvegarde commence.

## <a name="view-the-backup-status-and-the-backup-script"></a>Afficher l’état et le script de sauvegarde

1. Le volet **Historique des tâches** s’affiche ou appuyez sur **Ctrl+T** pour l’ouvrir.

   ![Capture d’écran montrant le volet Historique des tâches.](./media/tutorial-backup-restore-sql-server/task-history.png)

1. Pour afficher le script de sauvegarde dans l’éditeur, cliquez avec le bouton droit sur **Sauvegarde de base de données réussie**, puis sélectionnez **Script**.

   ![Capture d’écran montrant un script de sauvegarde.](./media/tutorial-backup-restore-sql-server/task-script.png)

## <a name="restore-a-database-from-a-backup-file"></a>Restaurer une base de données à partir d'un fichier de sauvegarde

1. Ouvrez la barre latérale **SERVEURS** en sélectionnant **Ctrl+G**. Cliquez avec le bouton droit sur votre serveur, puis sélectionnez **Gérer**.

1. Ouvrez la boîte de dialogue **Restaurer la base de données** en sélectionnant **Restaurer** sur le widget **Tâches**.

   ![Capture d’écran montrant la restauration de la tâche.](media/tutorial-backup-restore-sql-server/tasks-restore.png)

1. Sélectionnez **Fichier de sauvegarde** dans le champ **Restaurer à partir de**.

1. Sélectionnez les points de suspension (...) dans le champ **Chemin d’accès du fichier de sauvegarde**, puis sélectionnez le fichier de sauvegarde le plus récent pour *TutorialDB*.

1. Saisissez **TutorialDB_Restored** dans le champ **Base de données cible** de la section **Destination** pour restaurer le fichier de sauvegarde dans une nouvelle base de données. Sélectionnez ensuite **Restaurer**.

   ![Capture d’écran montrant la sauvegarde de restauration.](./media/tutorial-backup-restore-sql-server/restore.png)

1. Pour afficher l’état de l’opération de restauration, appuyez sur **Ctrl+T** pour ouvrir **Historique des tâches**.

   ![Capture d’écran montrant la restauration de tâches de l’historique.](./media/tutorial-backup-restore-sql-server/task-history-restore.png)