---
title: 'Démarrage rapide : Sauvegarder et restaurer avec le service Stockage Blob Azure'
description: 'Démarrage rapide : découvrez comment écrire des sauvegardes et les restaurer à partir du service Stockage Blob Azure. Créez un conteneur d’objets Blob Azure, écrivez une sauvegarde, puis effectuez une restauration.'
ms.custom: seo-dt-2019
ms.date: 12/21/2020
ms.prod: sql
ms.technology: backup-restore
ms.prod_service: backup-restore
ms.reviewer: ''
ms.topic: quickstart
author: cawrites
ms.author: chadam
ms.openlocfilehash: d27f53f80c8f987106f90816a4566339d777e6f6
ms.sourcegitcommit: bb54e4c9dd8c97365b7a96dfcd557b8b86d06978
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/22/2020
ms.locfileid: "97736897"
---
# <a name="quickstart-sql-backup-and-restore-to-azure-blob-storage-service"></a>Démarrage rapide : Sauvegarde et restauration de SQL Server avec le service de stockage Blob Azure

[!INCLUDE [sqlserver2016-asdbmi](../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Ce guide de démarrage rapide vous aide à comprendre comment écrire des sauvegardes et les restaurer à partir du service Stockage Blob Azure.  L’article explique comment créer un conteneur d’objets blob Azure, écrire une sauvegarde dans le service blob, puis effectuer une restauration.

> [!NOTE]
> SQL Server 2012 SP1 CU2 a introduit la prise en charge de la sauvegarde dans le stockage Blob Azure. SQL Server 2014 et les versions antérieures ne prennent pas en charge la signature d’accès partagé (SAP) décrite dans cet article de démarrage rapide.
>
> Pour SQL Server 2014 et versions antérieures, utilisez [Didacticiel : Sauvegarde et restauration SQL Server 2014 sur le stockage Blob Microsoft Azure](/previous-versions/sql/2014/relational-databases/backup-restore/sql-server-backup-to-url).
>
  
## <a name="prerequisites"></a>Prérequis

Pour suivre ce guide de démarrage rapide, vous devez connaître les concepts de sauvegarde et de restauration [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et la syntaxe T-SQL.  Vous avez besoin d’un compte de stockage Azure, de SQL Server Management Studio (SSMS) et d’un accès à un serveur qui exécute SQL Server ou Azure SQL Managed Instance. Par ailleurs, le compte utilisé pour émettre les commandes BACKUP et RESTORE doit figurer dans le rôle de base de données **db_backupoperator** avec les autorisations **modifier les informations d’identification**. 

- Obtenir gratuitement un [compte Azure](https://azure.microsoft.com/offers/ms-azr-0044p/).
- Créez un [compte de stockage Azure](/azure/storage/common/storage-quickstart-create-account?tabs=portal).
- Installez [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md).
- Installez [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads) ou déployez [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance-get-started) avec une connectivité établie via une [machine virtuelle SQL Azure](/azure/sql-database/sql-database-managed-instance-configure-vm) ou une connexion [point à site](/azure/sql-database/sql-database-managed-instance-configure-p2s).
- Affectez le compte utilisateur au rôle de [db_backupoperator](./security/authentication-access/database-level-roles.md) et autorisez [modifier les informations d’identification](../t-sql/statements/alter-credential-transact-sql.md). 

## <a name="create-azure-blob-container"></a>Créer un conteneur d’objets blob Azure
Un conteneur regroupe un ensemble d’objets blob. Tous les objets blob doivent figurer dans un conteneur. Un compte de stockage peut contenir un nombre illimité de conteneurs, mais doit comporter au moins un conteneur. Un conteneur peut stocker un nombre illimité d’objets blob. 

Pour créer un conteneur, suivez ces étapes :

1. Ouvrez le portail Azure. 
1. Accédez à votre compte de stockage. 
1. Sélectionnez le compte de stockage et faites défiler l’affichage jusqu'à **Services d’objets Blob**.
1. Sélectionnez **Objets blob**, puis **+ Conteneur** pour ajouter un nouveau conteneur. 
1. Entrez le nom du conteneur et notez le nom de conteneur que vous avez spécifié. Ces informations sont utilisées dans l’URL (chemin du fichier de sauvegarde) dans les instructions T-SQL, plus loin dans ce guide de démarrage rapide. 
1. Sélectionnez **OK**. 
    
    ![Nouveau conteneur](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/new-container.png)

  > [!NOTE]
  > L'authentification auprès du compte de stockage est requise pour la sauvegarde et la restauration SQL Server même si vous choisissez de créer un conteneur public. Vous pouvez également créer un conteneur par programme à l'aide des API REST. Pour plus d'informations, consultez [Créer un conteneur](/rest/api/storageservices/Create-Container).

## <a name="create-a-test-database"></a>Créer une base de données de test 
À cette étape, créez une base de données de test à l’aide de SQL Server Management Studio (SSMS). 

1. Lancez [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) et connectez-vous à votre instance de SQL Server.
1. Ouvrez une fenêtre **Nouvelle requête**. 
1. Exécutez le code Transact-SQL (T-SQL) suivant pour créer votre base de données de test. Actualisez le nœud **Bases de données** dans l’**Explorateur d’objets** pour voir votre nouvelle base de données. Comme le chiffrement TDE est automatiquement activé sur les bases de données nouvellement créées sur SQL Managed Instance, vous devez le désactiver pour continuer. 

```sql
USE [master]
GO

-- Create database
CREATE DATABASE [SQLTestDB]
GO

-- Create table in database
USE [SQLTestDB]
GO
CREATE TABLE SQLTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO

-- Populate table 
USE [SQLTestDB]
GO

INSERT INTO SQLTest (ID, c1) VALUES (1, 'test1')
INSERT INTO SQLTest (ID, c1) VALUES (2, 'test2')
INSERT INTO SQLTest (ID, c1) VALUES (3, 'test3')
INSERT INTO SQLTest (ID, c1) VALUES (4, 'test4')
INSERT INTO SQLTest (ID, c1) VALUES (5, 'test5')
GO

SELECT * FROM SQLTest
GO

-- Disable TDE for newly-created databases on SQL Managed Instance 
USE [SQLTestDB];
GO
ALTER DATABASE [SQLTestDB] SET ENCRYPTION OFF;
GO
```

## <a name="create-credential"></a>Créer des informations d’identification

Utilisez l’interface graphique utilisateur de SQL Server Management Studio pour créer les informations d’identification en suivant les étapes ci-dessous. Vous pouvez également créer les informations d’identification [programmatiquement](tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#2---create-a-sql-server-credential-using-a-shared-access-signature). 

1. Développez le nœud **Bases de données** dans l’**Explorateur d'objets** de [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md).
1. Cliquez avec le bouton droit sur votre nouvelle base de données `SQLTestDB`, pointez sur **Tâches**, puis sélectionnez **Sauvegarder...** pour lancer l’Assistant **Sauvegarder la base de données**. 
1. Sélectionnez **URL** dans le menu déroulant de destination **Sauvegarder vers**, puis sélectionnez **Ajouter** pour lancer la boîte de dialogue **Sélectionner la destination de sauvegarde**. 

   ![Sauvegarde vers l’URL](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/back-up-to-url.png)

1. Sélectionnez **Nouveau conteneur** dans la boîte de dialogue **Sélectionner la destination de sauvegarde** pour lancer la fenêtre **Se connecter à un abonnement Microsoft**. 

   ![Capture d’écran de la fenêtre de boîte de dialogue Sélectionner la destination de la sauvegarde avec l’option Nouveau conteneur en évidence.](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/select-backup-destination.png)

1. Connectez-vous au portail Azure en sélectionnant **Se connecter...** , puis poursuivez le processus de connexion. 
1. Sélectionnez votre **abonnement** dans la liste déroulante. 
1. Sélectionnez votre **compte de stockage** dans la liste déroulante. 
1. Dans la liste déroulante, sélectionnez le conteneur que vous avez créé précédemment. 
1. Sélectionnez **Créer des informations d’identification** pour générer votre *signature d’accès partagé (SAP)* .  **Enregistrez cette valeur, car vous en aurez besoin pour la restauration.**

   ![Créer des informations d’identification](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/create-credential.png)

1. Cliquez sur **OK** pour fermer la fenêtre **Se connecter à un abonnement Microsoft**. Cela renseigne la valeur *Conteneur de stockage Azure* dans la boîte de dialogue **Sélectionner la destination de sauvegarde**. Sélectionnez **OK** pour choisir le conteneur de stockage sélectionné, puis fermez la boîte de dialogue. 
1. À ce stade, vous pouvez passer directement à l’étape 4 de la section suivante pour effectuer la sauvegarde de la base de données, ou fermer l’Assistant **Sauvegarder la base de données** si vous souhaitez continuer à utiliser Transact-SQL pour sauvegarder la base de données. 


## <a name="back-up-database"></a>Sauvegarder la base de données
Dans cette étape, sauvegardez la base de données `SQLTestDB` dans votre compte de stockage d’objets Blob Azure à l’aide de l’interface graphique utilisateur dans SQL Server Management Studio ou de Transact-SQL (T-SQL). 

# <a name="ssms"></a>[SSMS](#tab/SSMS)

1. Si l’Assistant **Sauvegarder la base de données** n’est pas encore ouvert, développez le nœud **Bases de données** dans l’**Explorateur d'objets** de [SQL Server Management Studio(SSMS)](../ssms/download-sql-server-management-studio-ssms.md).
1. Cliquez avec le bouton droit sur votre nouvelle base de données `SQLTestDB`, pointez sur **Tâches**, puis sélectionnez **Sauvegarder...** pour lancer l’Assistant **Sauvegarder la base de données**. 
1. Sélectionnez **URL** dans le menu déroulant **Sauvegarder vers**, puis sélectionnez **Ajouter** pour lancer la boîte de dialogue **Sélectionner la destination de sauvegarde**. 

   ![Sauvegarde vers l’URL](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/back-up-to-url.png)

1. Sélectionnez le conteneur que vous avez créé à l’étape précédente dans la liste déroulante **Conteneur de stockage Azure**. 

   ![Conteneur de stockage Windows Azure](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/azure-storage-container.png)

1. Sélectionnez **OK** dans l’Assistant **Sauvegarder la base de données** pour sauvegarder votre base de données. 
1. Sélectionnez **OK** pour fermer toutes les fenêtres liées à la sauvegarde une fois que votre base de données a été sauvegardée. 

   > [!TIP]
   > Vous pouvez générer un script Transact-SQL qui se trouve derrière cette commande en sélectionnant **Script** en haut de l’Assistant **Sauvegarder la base de données** : ![Commande de script](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/script-backup-command.png)


# <a name="transact-sql"></a>[Transact-SQL](#tab/tsql)

Sauvegardez votre base de données à l’aide de Transact-SQL en exécutant la commande suivante : 


```sql
USE [master]

BACKUP DATABASE [SQLTestDB] 
TO  URL = N'https://msftutorialstorage.blob.core.windows.net/sql-backup/sqltestdb_backup_2020_01_01_000001.bak' 
WITH  COPY_ONLY, CHECKSUM
GO
```

---

## <a name="delete-database"></a>Supprimer la base de données
Dans cette étape, supprimez la base de données avant d’effectuer la restauration. Cette étape est uniquement nécessaire dans le cadre de ce tutoriel, mais est peu susceptible d’être utilisée dans les procédures normales de gestion de base de données. Vous pouvez ignorer cette étape, mais vous devrez alors modifier le nom de la base de données pendant la restauration sur une instance gérée, ou exécuter la commande de restauration `WITH REPLACE` pour restaurer la base de données localement. 

# <a name="ssms"></a>[SSMS](#tab/SSMS)

1. Développez le nœud **Bases de données** dans l’**Explorateur d’objets**, cliquez avec le bouton droit sur la base de données `SQLTestDB` et sélectionnez Supprimer pour lancer l’Assistant **Supprimer l’objet** . 
1. Sur une instance gérée, sélectionnez **OK** pour supprimer la base de données. Localement, activez la case à cocher **Fermer les connexions existantes**, puis sélectionnez **OK** pour supprimer la base de données. 

# <a name="transact-sql"></a>[Transact-SQL](#tab/tsql)

Supprimez la base de données en exécutant la commande Transact-SQL suivante :

```sql
USE [master]
GO
DROP DATABASE [SQLTestDB]
GO

-- If connections currently exist on-premises, you'll need to set the database into single user mode first
USE [master]
GO
ALTER DATABASE [SQLTestDB] SET  SINGLE_USER WITH ROLLBACK IMMEDIATE
GO
USE [master]
GO
DROP DATABASE [SQLTestDB]
GO
```

---


## <a name="restore-database"></a>Restaurer la base de données 
Dans cette étape, restaurez la base de données à l’aide de l’interface graphique utilisateur de SQL Server Management Studio ou de Transact-SQL. 

# <a name="ssms"></a>[SSMS](#tab/SSMS)

1. Cliquez avec le bouton droit sur le nœud **Bases de données** dans l’**Explorateur d’objets** dans SQL Server Management Studio et sélectionnez **Restaurer la base de données**. 
1. Sélectionnez **Appareil**, puis sélectionnez les points de suspension (...) pour choisir l’appareil. 

   ![Sélectionner Restaurer l’appareil](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/select-restore-device.png)

1. Sélectionnez **URL** dans la liste déroulante **Type de support de sauvegarde** et sélectionnez **Ajouter** pour ajouter votre appareil. 

   ![Ajouter une unité de sauvegarde](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/add-backup-device.png)

1. Sélectionnez le conteneur dans la liste déroulante, puis collez la signature d’accès partagé (SAP) que vous avez enregistrée lors de la création des informations d’identification. 

   ![Capture d’écran de la boîte de dialogue Sélectionner un emplacement de fichier de sauvegarde avec le champ Signature d’accès partagé rempli.](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/restore-from-container.png)

1. Sélectionnez **OK** pour sélectionner l’emplacement du fichier de sauvegarde. 
1. Développez **Conteneurs** et sélectionnez le conteneur où se trouve votre fichier de sauvegarde. 
1. Sélectionnez le fichier de sauvegarde que vous souhaitez restaurer, puis sélectionnez **OK**. Si aucun fichier n’est visible, vous utilisez peut-être une clé SAP incorrecte. Vous pouvez régénérer la clé SAP en suivant les mêmes étapes que précédemment pour ajouter le conteneur. 

   ![Sélectionner Restaurer le fichier](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/select-restore-file.png)

1. Sélectionnez **OK** pour fermer la boîte de dialogue **Sélectionner les unités de sauvegarde**.
1. Sélectionnez **OK** pour restaurer votre base de données.

# <a name="transact-sql"></a>[Transact-SQL](#tab/tsql)

Pour restaurer votre base de données locale à partir du stockage d’objets Blob Azure, modifiez la commande Transact-SQL suivante afin d’utiliser votre propre compte de stockage, puis exécutez-la dans une nouvelle fenêtre de requête.

```sql
USE [master]
RESTORE DATABASE [SQLTestDB] FROM 
URL = N'https://msftutorialstorage.blob.core.windows.net/sql-backup/sqltestdb_backup_2020_01_01_000001.bak'
```

---


## <a name="see-also"></a>Voir aussi 
Les ressources suivantes sont des lectures recommandées afin de mieux comprendre les concepts et les bonnes pratiques pour l’utilisation du service Stockage Blob Azure pour les sauvegardes [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
-   [Sauvegarde et restauration SQL Server avec le service Stockage Blob Microsoft Azure](../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)   
-   [Bonnes pratiques et résolution des problèmes liés à la sauvegarde SQL Server vers une URL](../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)  
