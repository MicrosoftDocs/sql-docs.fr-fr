---
description: 'Didacticiel : Utiliser le service Stockage Blob Azure avec SQL Server 2016'
title: 'Didacticiel : Utiliser le service Stockage Blob Azure avec SQL Server 2016'
ms.custom: seo-dt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.technology: ''
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016
ms.assetid: e69be67d-da1c-41ae-8c9a-6b12c8c2fb61
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 31407e6d056ce8ec847f52346b8aa3ddf8e621f3
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644185"
---
# <a name="tutorial-use-azure-blob-storage-service-with-sql-server-2016"></a>Didacticiel : Utiliser le service Stockage Blob Azure avec SQL Server 2016

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
Bienvenue dans le didacticiel sur l’utilisation de SQL Server 2016 dans le service Stockage Blob Microsoft Azure. Ce didacticiel explique comment utiliser le service Stockage Blob Microsoft Azure pour les fichiers de données SQL Server et les sauvegardes SQL Server.  
  
La prise en charge de l’intégration SQL Server pour le service Stockage Blob Microsoft Azure a commencé sous la forme d’une amélioration dans SQL Server 2012 Service Pack 1 CU2 et a été affinée avec SQL Server 2014 et SQL Server 2016. Pour obtenir une vue d’ensemble des fonctionnalités et des avantages offerts, consultez [Fichiers de données SQL Server dans Microsoft Azure](../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md). Pour une démonstration en situation réelle, consultez [Demo of Point in Time Restore](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo)(Démonstration de la restauration à un point dans le temps).  

Ce didacticiel vous montre comment utiliser des fichiers de données SQL Server dans le service Stockage Blob Microsoft Azure en plusieurs parties. Chaque partie porte sur une tâche spécifique et les différentes parties doivent être traitées dans l’ordre. Tout d’abord, vous allez apprendre à créer un conteneur de stockage d’objets Blob avec une stratégie d’accès stockée et une signature d’accès partagé. Ensuite, vous découvrirez comment créer des informations d’identification SQL Server pour intégrer SQL Server au stockage Blob Azure. Ensuite, vous sauvegarderez une base de données dans l’espace de stockage d’objets Blob et la restaurerez dans une machine virtuelle Azure. Vous utiliserez ensuite la sauvegarde du journal des transactions d’instantanés de fichiers SQL Server 2016 pour effectuer une restauration à un point dans le temps et dans une base de données. Enfin, pour illustrer les sauvegardes d’instantanés de fichiers et leur utilisation, le didacticiel vous montrera comment utiliser des fonctions et procédures stockées système de métadonnées.
  
## <a name="prerequisites"></a>Conditions préalables

Pour suivre ce didacticiel, vous devez connaître les concepts de sauvegarde et de restauration [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et la syntaxe T-SQL. Pour utiliser ce tutoriel, vous avez besoin d'un compte de stockage Azure, de SQL Server Management Studio (SSMS), d'un accès à une instance de SQL Server sur site, d'un accès à une machine virtuelle Azure exécutant SQL Server 2016 et d’une base de données AdventureWorks2016. Par ailleurs, le compte utilisé pour émettre les commandes BACKUP et RESTORE doit figurer dans le rôle de base de données **db_backupoperator** avec les autorisations **modifier les informations d’identification**. 

- Obtenir gratuitement un [compte Azure](https://azure.microsoft.com/offers/ms-azr-0044p/).
- Créez un [compte de stockage Azure](/azure/storage/common/storage-quickstart-create-account?tabs=portal).
- Installez [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Approvisionnez une [machine virtuelle Azure exécutant SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)
- Installez [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md).
- Téléchargez les [exemples de bases de données AdventureWorks2016](../samples/adventureworks-install-configure.md).
- Affectez le compte utilisateur au rôle de [db_backupoperator](./security/authentication-access/database-level-roles.md) et autorisez [modifier les informations d’identification](../t-sql/statements/alter-credential-transact-sql.md). 

## <a name="1---create-stored-access-policy-and-shared-access-storage"></a>1 : créer une stratégie d’accès stockée et un stockage d’accès partagé

Dans cette partie, vous allez utiliser un script [Azure PowerShell](/powershell/azure/) pour créer une signature d’accès partagé sur un conteneur d’objets Blob Azure à l’aide d’une stratégie d’accès stockée.  
  
> [!NOTE]  
> Ce script est écrit à l’aide d’Azure PowerShell 5.0.10586.  
  
Une signature d’accès partagé est un URI qui octroie des droits d’accès restreints aux conteneurs, aux objets blob, aux files d’attente ou aux tables. Une stratégie d’accès stockée fournit un niveau de contrôle supplémentaire sur des signatures d’accès partagé côté serveur, notamment en matière de révocation, d’expiration ou d’extension d’un accès. Si vous utilisez cette nouvelle amélioration, vous devez créer une stratégie sur un conteneur avec au minimum des droits en lecture, en écriture et en création de liste.  
  
Vous pouvez créer une stratégie d’accès stockée et une signature d’accès partagé à l’aide d’Azure PowerShell, du kit de développement logiciel (SDK) Stockage Azure, de l’API REST Azure ou d’un utilitaire tiers. Ce didacticiel montre comment utiliser un script Azure PowerShell pour effectuer cette tâche. Le script utilise le modèle de déploiement Resource Manager et crée les ressources suivantes :  
  
-   Resource group   
-   Compte de stockage  
-   Conteneur d’objets blob Azure   
-   Stratégie SAP    

Ce script commence par déclarer plusieurs variables pour spécifier les noms des ressources ci-dessus et les noms des valeurs d’entrée requises suivantes :  
  
-   Un nom de préfixe utilisé pour nommer les autres objets de ressource    
-   Nom d’abonnement    
-   Emplacement du centre de données  

  
Le script se termine par la génération de l’instruction CREATE CREDENTIAL appropriée, que vous utiliserez dans [2 : créer des informations d’identification SQL Server à l’aide d’une signature d’accès partagé](#2---create-a-sql-server-credential-using-a-shared-access-signature). Cette instruction est automatiquement copiée dans le Presse-papiers et apparaît dans la console.  
  
Pour créer une stratégie sur le conteneur et générer une clé de signature d’accès partagé (SAP), procédez comme suit :  
  
1.  Ouvrez Windows PowerShell ou Windows PowerShell ISE (voir plus haut la version requise).  
  
2.  Modifiez, puis exécutez le script ci-dessous :  
  
    ```powershell
    # Define global variables for the script  
    $prefixName = '<a prefix name>'  # used as the prefix for the name for various objects  
    $subscriptionID = '<your subscription ID>'   # the ID  of subscription name you will use  
    $locationName = '<a data center location>'  # the data center region you will use  
    $storageAccountName= $prefixName + 'storage' # the storage account name you will create or use  
    $containerName= $prefixName + 'container'  # the storage container name to which you will attach the SAS policy with its SAS token  
    $policyName = $prefixName + 'policy' # the name of the SAS policy 

    # Set a variable for the name of the resource group you will create or use  
    $resourceGroupName=$prefixName + 'rg'   
  
    # Add an authenticated Azure account for use in the session   
    Connect-AzAccount    
  
    # Set the tenant, subscription and environment for use in the rest of   
    Set-AzContext -SubscriptionId $subscriptionID   
  
    # Create a new resource group - comment out this line to use an existing resource group  
    New-AzResourceGroup -Name $resourceGroupName -Location $locationName   
  
    # Create a new Azure Resource Manager storage account - comment out this line to use an existing Azure Resource Manager storage account  
    New-AzStorageAccount -Name $storageAccountName -ResourceGroupName $resourceGroupName -Type Standard_RAGRS -Location $locationName   
  
    # Get the access keys for the Azure Resource Manager storage account  
    $accountKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName  
  
    # Create a new storage account context using an Azure Resource Manager storage account  
    $storageContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $accountKeys[0].Value

    # Creates a new container in blob storage  
    $container = New-AzStorageContainer -Context $storageContext -Name $containerName  
  
    # Sets up a Stored Access Policy and a Shared Access Signature for the new container  
    $policy = New-AzStorageContainerStoredAccessPolicy -Container $containerName -Policy $policyName -Context $storageContext -StartTime $(Get-Date).ToUniversalTime().AddMinutes(-5) -ExpiryTime $(Get-Date).ToUniversalTime().AddYears(10) -Permission rwld

    # Gets the Shared Access Signature for the policy  
    $sas = New-AzStorageContainerSASToken -name $containerName -Policy $policyName -Context $storageContext
    Write-Host 'Shared Access Signature= '$($sas.Substring(1))''  

    # Sets the variables for the new container you just created
    $container = Get-AzStorageContainer -Context $storageContext -Name $containerName
    $cbc = $container.CloudBlobContainer 
  
    # Outputs the Transact SQL to the clipboard and to the screen to create the credential using the Shared Access Signature  
    Write-Host 'Credential T-SQL'  
    $tSql = "CREATE CREDENTIAL [{0}] WITH IDENTITY='Shared Access Signature', SECRET='{1}'" -f $cbc.Uri,$sas.Substring(1)   
    $tSql | clip  
    Write-Host $tSql 

    # Once you're done with the tutorial, remove the resource group to clean up the resources. 
    # Remove-AzResourceGroup -Name $resourceGroupName  
    ```  

3.  une fois le script terminé, l’instruction CREATE CREDENTIAL se trouve dans le Presse-papiers pour une utilisation dans la partie suivante.  


## <a name="2---create-a-sql-server-credential-using-a-shared-access-signature"></a>2 : créer des informations d’identification SQL Server à l’aide d’une signature d’accès partagé

Dans cette partie, vous allez créer des informations d’identification pour stocker les informations de sécurité utilisées par SQL Server pour écrire et lire dans le conteneur Azure que vous avez créé à l’étape précédente.  
  
Les informations d'identification SQL Server sont des objets utilisés pour stocker les informations d'authentification requises pour la connexion à une ressource en dehors de SQL Server. Les informations d’identification contiennent le chemin de l’URI du conteneur de stockage et la signature d’accès partagé pour ce conteneur.  

Pour créer des informations d’identification SQL Server, procédez comme suit :  
  
1.  Connectez-vous à SQL Server Management Studio.  
2.  Ouvrez une nouvelle fenêtre de requête et connectez-vous à l’instance SQL Server du moteur de base de données dans votre environnement local.  
  
3.  Dans la nouvelle fenêtre de requête, collez l’instruction CREATE CREDENTIAL avec la signature d’accès partagé issue de la parte 1, puis exécutez ce script.  
  
    Le script ressemble au code suivant.  
  
    ```sql   
    /* Example:
    USE master  
    CREATE CREDENTIAL [https://msfttutorial.blob.core.windows.net/containername] 
    WITH IDENTITY='SHARED ACCESS SIGNATURE'   
    , SECRET = 'sharedaccesssignature' 
    GO */
    
    USE master  
    CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>] 
      -- this name must match the container path, start with https and must not contain a forward slash at the end
    WITH IDENTITY='SHARED ACCESS SIGNATURE' 
      -- this is a mandatory string and should not be changed   
     , SECRET = 'sharedaccesssignature' 
       -- this is the shared access signature key that you obtained in section 1.   
    GO    
    ```  
  
4.  Pour voir toutes les informations d’identification disponibles, exécutez l’instruction suivante dans une fenêtre de requête connectée à votre instance :  
  
    ```sql  
    SELECT * from sys.credentials  
    ```  
  
5.  Ouvrez une nouvelle fenêtre de requête et connectez-vous à l’instance SQL Server du moteur de base de données sur votre machine virtuelle Azure.  
  
6.  Dans la nouvelle fenêtre de requête, collez l’instruction CREATE CREDENTIAL avec la signature d’accès partagé issue de la parte 1, puis exécutez ce script.  
  
7.  Répétez les étapes 5 et 6 pour toute instance SQL Server supplémentaire devant avoir accès au conteneur Azure.  

## <a name="3---database-backup-to-url"></a>3 : sauvegarder une base de données vers une URL

Dans cette partie, vous sauvegardez la base de données AdventureWorks2016 dans votre instance de SQL Server 2016 sur site dans le conteneur Azure que vous avez créé dans la [Partie 1](#1---create-stored-access-policy-and-shared-access-storage).
  
> [!NOTE]  
> Si vous souhaitez effectuer une sauvegarde d’une base de données SQL Server 2012 SP1 CU2 ou version ultérieure ou d’une base de données SQL Server 2014 dans ce conteneur Azure, vous pouvez utiliser la syntaxe déconseillée décrite [ici](/previous-versions/sql/2014/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2014&preserve-view=true) pour sauvegarder dans une URL à l’aide de la syntaxe WITH CREDENTIALS.  
  
Pour sauvegarder une base de données dans un stockage Blob, procédez comme suit :  
  
1.  Connectez-vous à SQL Server Management Studio.  
2.  Ouvrez une nouvelle fenêtre de requête et connectez-vous à l’instance SQL Server 2016 du moteur de base de données sur votre machine virtuelle Azure.  
3.  Copiez et collez le script Transact-SQL suivant dans la fenêtre de requête. Modifiez l’URL en fonction du nom de votre compte de stockage et du conteneur que vous avez spécifiés dans la partie 1, puis exécutez ce script.  
  
    ```sql  
  
    -- To permit log backups, before the full database backup, modify the database to use the full recovery model.  
    USE master;  
    ALTER DATABASE AdventureWorks2016  
       SET RECOVERY FULL;  
  
    -- Back up the full AdventureWorks2016 database to the container that you created in section 1  
    BACKUP DATABASE AdventureWorks2016   
       TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_onprem.bak'  
  
    ```  
  
4.  Ouvrez l’Explorateur d’objets et connectez-vous au stockage Azure à l’aide de votre compte de stockage et de la clé de compte. 
    1.  Développez Conteneurs, développez le conteneur que vous avez créé dans la partie 1 et vérifiez que la sauvegarde de l’étape 3 ci-dessus s’affiche dans ce conteneur.  
  
   ![Connectez-vous au compte Stockage Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/connect-to-azure-storage.png)


## <a name="4----restore-database-to-virtual-machine-from-url"></a>4 : restaurer une base de données sur une machine virtuelle à partir d’une URL

Dans cette partie, vous restaurez la base de données AdventureWorks2016 sur votre instance de SQL Server 2016 sur votre machine virtuelle Azure.
  
> [!NOTE]  
> Pour simplifier ce didacticiel, nous utilisons le même conteneur pour les fichiers journaux et les données que celui que nous avons utilisé pour la sauvegarde de base de données. Dans un environnement de production, vous utilisez probablement plusieurs conteneurs et souvent plusieurs fichiers de données. Avec SQL Server 2016, vous pouvez également envisager de répartir votre sauvegarde sur plusieurs objets blob pour augmenter les performances de sauvegarde quand vous sauvegardez une base de données volumineuse.  
  
Pour restaurer la base de données AdventureWorks2016 à partir d’un stockage Blob Azure sur votre instance de SQL Server 2016 sur votre machine virtuelle, procédez comme suit :  
  
1.  Connectez-vous à SQL Server Management Studio.   
2.  Ouvrez une nouvelle fenêtre de requête et connectez-vous à l’instance SQL Server 2016 du moteur de base de données sur votre machine virtuelle Azure.   
3.  Copiez et collez le script Transact-SQL suivant dans la fenêtre de requête. Modifiez l’URL en fonction du nom de votre compte de stockage et du conteneur que vous avez spécifiés dans la partie 1, puis exécutez ce script.  
  
    ```sql  
    -- Restore AdventureWorks2016 from URL to SQL Server instance using Azure blob storage for database files  
    RESTORE DATABASE AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_onprem.bak'   
       WITH  
          MOVE 'AdventureWorks2016_data' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Data.mdf'  
         ,MOVE 'AdventureWorks2016_log' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Log.ldf'  
    --, REPLACE  
  
    ```  
  
4.  Ouvrez l’Explorateur d’objets et connectez-vous à votre instance Azure SQL Server 2016.   
5.  Dans l’Explorateur d’objets, développez le nœud Bases de données et vérifiez que la base de données AdventureWorks2016 a été restaurée (actualisez le nœud si nécessaire).    
    1. Cliquez avec le bouton droit sur AdventureWorks2016, puis sélectionnez Propriétés.
    1. Cliquez sur Fichiers et vérifiez que les chemins des deux fichiers de base de données sont des URL qui pointent sur des objets blob dans votre conteneur d’objets blob Azure (sélectionnez Annuler lorsque vous avez terminé).
    
     ![Base de données AdventureWorks sur une machine virtuelle Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/adventureworks-on-azure-vm.png)

    
8.  Dans l’Explorateur d’objets, connectez-vous au stockage Azure.
    1. Développez Conteneurs, développez le conteneur que vous avez créé dans la partie 1 et vérifiez que les fichiers AdventureWorks2016_Data.mdf et AdventureWorks2016_Log.ldf de l’étape 3 ci-dessus s’affichent dans ce conteneur, ainsi que le fichier de sauvegarde de la partie 3 (actualisez le nœud si nécessaire).  
  
   ![Fichiers de données au sein du conteneur sur Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/data-files-in-container.png)

## <a name="5---backup-database-using-file-snapshot-backup"></a>5 : sauvegarder une base de données à l’aide d’une sauvegarde d’instantanés de fichiers

Dans cette partie, vous sauvegardez la base de données AdventureWorks2016 sur votre machine virtuelle Azure à l’aide de la sauvegarde d’instantanés de fichiers pour effectuer une sauvegarde quasi instantanée au moyen d’instantanés Azure. Pour plus d’informations sur les sauvegardes d’instantanés de fichiers, consultez [Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)  
  
Pour sauvegarder la base de données AdventureWorks2016 à l’aide de la sauvegarde d’instantanés de fichiers, procédez comme suit :  
  
1.  Connectez-vous à SQL Server Management Studio.   
2.  Ouvrez une nouvelle fenêtre de requête et connectez-vous à l’instance SQL Server 2016 du moteur de base de données sur votre machine virtuelle Azure.  
3.  Copiez, collez et exécutez le script Transact-SQL suivant dans la fenêtre de requête (ne fermez pas cette fenêtre de requête, vous réexécuterez ce script à l’étape 5. Ce système de procédure stockées vous permet d’afficher les sauvegardes d’instantanés de fichiers existantes pour chaque fichier qui comprend une base de données spécifiée. Notez qu’il n’y a aucune sauvegarde d’instantanés de fichiers pour cette base de données.  
  
    ```sql  
    -- Verify that no file snapshot backups exist  
    SELECT * FROM sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
    ```  
  
4.  Copiez et collez le script Transact-SQL suivant dans la fenêtre de requête. Modifiez l’URL en fonction du nom de votre compte de stockage et du conteneur que vous avez spécifiés dans la partie 1, puis exécutez ce script. Notez la rapidité d’exécution de cette sauvegarde.  
  
    ```sql  
    -- Backup the AdventureWorks2016 database with FILE_SNAPSHOT  
    BACKUP DATABASE AdventureWorks2016   
       TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Azure.bak'   
       WITH FILE_SNAPSHOT;    
    ```  
  
5.  Après avoir vérifié que le script de l’étape 4 a été correctement exécuté, réexécutez le script suivant. Notez que l’opération de sauvegarde d’instantanés de fichiers de l’étape 4 a généré des instantanés de fichiers des données et du fichier journal.  
  
    ```sql  
    -- Verify that two file-snapshot backups exist  
    SELECT * FROM sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
  
    ```  
  
    ![Résultats de fn_db_backup_file_snapshots affichant des captures instantanées](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/results-showing-snapshot.png) 
  
6.  Dans l’Explorateur d’objets, dans l’instance SQL Server 2016 sur votre machine virtuelle Azure, développez le nœud Bases de données et vérifiez que la base de données AdventureWorks2016 a été restaurée dans cette instance (actualisez le nœud si nécessaire).  
7.  Dans l’Explorateur d’objets, connectez-vous au stockage Azure.   
8.  Développez Conteneurs, développez le conteneur que vous avez créé dans la partie 1 et vérifiez que le fichier AdventureWorks2016_Azure.bak de l’étape 4 ci-dessus s’affiche dans ce conteneur, ainsi que le fichier de sauvegarde de la partie 3 et les fichiers de base de données de la partie 4 (actualisez le nœud si nécessaire).  
  
    ![Sauvegarde d’instantanés sur Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/snapshot-backup-on-azure.PNG)

## <a name="6----generate-activity-and-backup-log-using-file-snapshot-backup"></a>6 : générer un journal d’activité et de sauvegarde à l’aide d’une sauvegarde d’instantanés de fichiers

Dans cette partie, vous allez générer une activité dans la base de données AdventureWorks2016 et créer régulièrement des sauvegardes de fichiers journaux des transactions à l’aide de sauvegardes d’instantanés de fichiers. Pour plus d’informations sur l’utilisation de sauvegardes d’instantanés de fichiers, consultez [Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
Pour générer une activité dans la base de données AdventureWorks2016 et créer régulièrement des sauvegardes du journal des transactions à l’aide de sauvegardes d’instantanés de fichiers, procédez comme suit :  
  
1.  Connectez-vous à SQL Server Management Studio.   
2.  Ouvrez deux nouvelles fenêtres de requête et connectez chacune d’elles à l’instance SQL Server 2016 du moteur de base de données dans votre machine virtuelle Azure.   
3.  Copiez, collez et exécutez le script Transact-SQL suivant dans l’une des fenêtres de requête. Notez que la table Production.Location contient 14 lignes avant que nous ajoutions de nouvelles lignes à l’étape 4.  
  
    ```sql 
    -- Verify row count at start  
    SELECT COUNT (*) from AdventureWorks2016.Production.Location;    
    ```  
  
4.  Copiez et collez les deux scripts Transact-SQL suivants dans les deux fenêtres de requête. Modifiez l’URL en fonction du nom de votre compte de stockage et du conteneur que vous avez spécifié dans la partie 1, puis exécutez ces scripts simultanément dans les fenêtres de requête. L’exécution de ces scripts prendra environ sept minutes.  
  
    ```sql  
    -- Insert 30,000 new rows into the Production.Location table in the AdventureWorks2014 database in batches of 75  
    DECLARE @count INT=1, @inner INT;  
    WHILE @count < 400  
       BEGIN  
          BEGIN TRAN;  
             SET @inner =1;  
                WHILE @inner <= 75  
                   BEGIN;  
                      INSERT INTO AdventureWorks2016.Production.Location    
                         (Name, CostRate, Availability, ModifiedDate)   
                            VALUES (NEWID(), .5, 5.2, GETDATE());  
                      SET @inner = @inner + 1;  
                   END;  
          COMMIT;  
       WAITFOR DELAY '00:00:01';   
       SET @count = @count + 1;  
       END;  
    SELECT COUNT (*) from AdventureWorks2014.Production.Location;    
    ```  
  
    ```sql  
    --take 7 transaction log backups with FILE_SNAPSHOT, one per minute, and include the row count and the execution time in the backup file name   
    DECLARE @count INT=1, @device NVARCHAR(120), @numrows INT;  
    WHILE @count <= 7  
       BEGIN  
             SET @numrows = (SELECT COUNT (*) FROM AdventureWorks2016.Production.Location);  
             SET @device = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/tutorial-' + CONVERT (varchar(10),@numrows) + '-' + FORMAT(GETDATE(), 'yyyyMMddHHmmss') + '.bak';  
             BACKUP LOG AdventureWorks2016 TO URL = @device WITH FILE_SNAPSHOT;  
             SELECT * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
          WAITFOR DELAY '00:1:00';   
             SET @count = @count + 1;  
       END;  
    ```  
  
5.  Examinez la sortie du premier script ; le nombre de lignes final est désormais 29 939.  
  
    ![Le nombre de lignes 29 939 s’affiche](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/29-thousand-rows.png)
  
6.  Examinez la sortie du deuxième script ; chaque fois que l’instruction BACKUP LOG est exécutée, deux instantanés de fichiers sont créés, à savoir un instantané de fichier du fichier journal et un instantané de fichier du fichier de données, soit un total de deux instantanés de fichiers par fichier de base de données. Une fois le deuxième script terminé, il existe un total de 16 instantanés de fichiers, à raison de 8 par fichier de base de données (un issu de l’instruction BACKUP DATABASE et un pour chaque exécution de l’instruction BACKUP LOG).  
  
   ![Résultats de l’instantané de sauvegarde](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/snapshot-back-results.png)
  
7.  Dans l’Explorateur d’objets, connectez-vous au stockage Azure.  
  
8.  Développez Conteneurs, développez le conteneur que vous avez créé dans la partie 1 et vérifiez que sept nouveaux fichiers de sauvegarde apparaissent, ainsi que les fichiers de données issus des parties précédentes (actualisez le nœud si nécessaire).  
  
    ![Plusieurs instantanés dans le conteneur Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/tutorial-snapshots-in-container.png)

## <a name="7---restore-a-database-to-a-point-in-time"></a>7 : restaurer une base de données à un moment donné

Dans cette partie, vous restaurez la base de données AdventureWorks2016 à un moment donné entre deux sauvegardes de fichier journal.  
  
Avec les sauvegardes traditionnelles, pour obtenir une restauration à un moment donné, vous deviez utiliser la sauvegarde complète de la base de données, peut-être une sauvegarde différentielle, et tous les fichiers journaux des transactions jusqu’au moment où vous vouliez effectuer la restauration et juste après. Avec les sauvegardes d’instantanés de fichiers, vous n’avez besoin que des deux fichiers adjacents de sauvegarde du journal qui délimitent le moment où vous voulez effectuer la restauration. Vous n’avez besoin que de deux jeux de sauvegarde d’instantanés de fichiers journaux, car chaque sauvegarde de journal crée un instantané de chaque fichier de base de données (chaque fichier de données et le fichier journal).  
  
Pour restaurer une base de données à un point spécifié dans le temps à partir de jeux de sauvegarde d’instantanés de fichiers, procédez comme suit :  
  
1.  Connectez-vous à SQL Server Management Studio.  
2.  Ouvrez une nouvelle fenêtre de requête et connectez-vous à l’instance SQL Server 2016 du moteur de base de données sur votre machine virtuelle Azure.   
3.  Copiez, collez et exécutez le script Transact-SQL suivant dans la fenêtre de requête. Vérifiez que la table Production.Location a 29 939 lignes avant de la restaurer à un point dans le temps (il y a moins de lignes à l’étape 5).  
  
    ```sql
    -- Verify row count at start  
    SELECT COUNT (*) from AdventureWorks2016.Production.Location   
    ```  
  
    ![Le nombre de lignes 29 939 s’affiche](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/29-thousand-rows.png) 
  
4.  Copiez et collez le script Transact-SQL suivant dans la fenêtre de requête. Sélectionnez deux fichiers de sauvegarde de journal adjacents et remplacez le nom de fichier par la date et l’heure dont vous avez besoin pour ce script. Modifiez l’URL en fonction du nom de votre compte de stockage et du conteneur que vous avez spécifiés dans la partie 1, fournissez les noms des premier et deuxième fichiers de sauvegarde du journal, fournissez l’heure STOPAT au format « June 26, 2018 01:48 PM », puis exécutez ce script. Le script s’exécute pendant quelques minutes  
  
    ```sql  
    -- restore and recover to a point in time between the times of two transaction log backups, and then verify the row count  
    ALTER DATABASE AdventureWorks2016 SET SINGLE_USER WITH ROLLBACK IMMEDIATE;  
    RESTORE DATABASE AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<firstbackupfile>.bak'   
       WITH NORECOVERY,REPLACE;  
    RESTORE LOG AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<secondbackupfile>.bak'    
       WITH RECOVERY, STOPAT = 'June 26, 2018 01:48 PM';  
    ALTER DATABASE AdventureWorks2016 set multi_user;  
    -- get new count  
    SELECT COUNT (*) FROM AdventureWorks2016.Production.Location ;
    ```  
  
5.  Passez en revue la sortie. Notez qu’après la restauration, le nombre de lignes est 18 389, nombre qui se situe entre les sauvegardes de journal 5 et 6 (le nombre de lignes peut varier).  
  
    ![18-thousand-rows.JPG](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/18-thousand-rows.png)

## <a name="8----restore-as-new-database-from-log-backup"></a>8 : restaurer une nouvelle base de données à partir de la sauvegarde de journal

Dans cette partie, vous allez restaurer la base de données AdventureWorks2016 en tant que nouvelle base de données à partir de la sauvegarde du journal des transactions sous forme de fichier instantané.  
  
Dans ce scénario, vous effectuez une restauration vers une instance de SQL Server sur une machine virtuelle différente à des fins d’analyse des activités et de création de rapports. La restauration vers une autre instance sur une autre machine virtuelle permet de déplacer la charge de travail vers une machine virtuelle dédiée et dimensionnée à cet effet, et dont les besoins en ressources n’affectent pas le système transactionnel.  
  
Effectuer une restauration à partir d’une sauvegarde de journal des transactions avec une sauvegarde d’instantanés de fichiers est très rapide, notamment par rapport aux sauvegardes en continu traditionnelles. Dans le cas des sauvegardes en continu traditionnelles, vous devez utiliser la sauvegarde complète de la base de données, éventuellement une sauvegarde différentielle, et tout ou partie des sauvegardes du journal des transactions (ou une nouvelle sauvegarde complète de la base de données). Toutefois, dans le cas des sauvegardes de journaux d’instantanés de fichiers, vous avez uniquement besoin de la dernière sauvegarde de journal (ou toute autre sauvegarde de journal ou deux sauvegardes de journaux voisines pour une restauration à un point dans le temps situé entre deux heures de sauvegarde de journal). En clair, vous avez besoin d’un seul jeu de sauvegarde d’instantanés de fichiers journaux, car chaque sauvegarde de journal d’instantanés de fichiers crée un instantané de fichier de chaque fichier de base de données (chaque fichier de données et le fichier journal).  
  
Pour restaurer une base de données dans une nouvelle base de données à partir d’une sauvegarde du journal des transactions à l’aide de la sauvegarde d’instantanés de fichiers, procédez comme suit :  
  
1.  Connectez-vous à SQL Server Management Studio.   
2.  Ouvrez une nouvelle fenêtre de requête et connectez-vous à l’instance SQL Server 2016 du moteur de base de données dans une machine virtuelle Azure.  
  
    > [!NOTE]  
    > S’il s’agit d’une machine virtuelle Azure différente de celle que vous avez utilisée pour les parties précédentes, assurez-vous d’avoir suivi les étapes de [ 2 : créer des informations d’identification SQL Server à l’aide d’une signature d’accès partagé](#2---create-a-sql-server-credential-using-a-shared-access-signature). Si vous souhaitez restaurer vers un autre conteneur, suivez les étapes de [1 : créer une stratégie d’accès stockée et le stockage de l’accès partagé](#1---create-stored-access-policy-and-shared-access-storage) pour le nouveau conteneur.  
  
3.  Copiez et collez le script Transact-SQL suivant dans la fenêtre de requête. Sélectionnez le fichier de sauvegarde de journal à utiliser. Modifiez l’URL en fonction du nom de votre compte de stockage et du conteneur que vous avez spécifiés dans la partie 1, fournissez le nom du fichier de sauvegarde du journal, puis exécutez ce script.  
  
    ```sql  
    -- restore as a new database from a transaction log backup file  
    RESTORE DATABASE AdventureWorks2016_EOM   
        FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<logbackupfile.bak>'    
        WITH MOVE 'AdventureWorks2016_data' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_EOM_Data.mdf'  
       , MOVE 'AdventureWorks2016_log' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_EOM_Log.ldf'  
       , RECOVERY  
    --, REPLACE   
    ```  
  
4.  Examinez la sortie pour vérifier que la restauration a réussi.   
5.  Dans l’Explorateur d’objets, connectez-vous au stockage Azure.    
6.  Développez Conteneurs, développez le conteneur que vous avez créé dans la partie 1 (actualisez si nécessaire) et vérifiez que les nouveaux fichiers journaux et de données apparaissent dans le conteneur, ainsi que les objets blob issus des parties précédentes.  
  
    ![Conteneur Azure montrant les données et les fichiers journaux de la nouvelle base de données](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/new-db-in-azure-container.png)

## <a name="9---manage-backup-sets-and-file-snapshot-backups"></a>Partie 9 : gérer des jeux de sauvegarde et des sauvegardes d’instantanés de fichiers

Dans cette partie, vous allez supprimer un jeu de sauvegarde à l’aide de la procédure [sp_delete_backup &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md) stockée dans le système. Cette procédure stockée système supprime le fichier de sauvegarde et la capture instantanée de fichier sur chaque fichier de base de données associé à ce jeu de sauvegarde.  
  
> [!NOTE]  
> Si vous essayez de supprimer un jeu de sauvegarde en supprimant simplement le fichier de sauvegarde du conteneur d’objets Blob Azure, vous supprimerez uniquement le fichier de sauvegarde proprement dit : les captures instantanées de fichiers associées ne seront pas supprimées. Si vous vous trouvez dans ce scénario, utilisez la fonction système [sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md) pour identifier l’URL des captures instantanées de fichiers orphelines et utilisez la procédure stockée système [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) pour supprimer chaque capture instantanée de fichier orpheline. Pour plus d’informations, consultez  [Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
Pour supprimer un jeu de sauvegarde de captures instantanées de fichiers, procédez comme suit :  
  
1.  Connectez-vous à SQL Server Management Studio.  
2.  Ouvrez une nouvelle fenêtre de requête et connectez-vous à l’instance de SQL Server 2016 du moteur de base de données sur votre machine virtuelle Azure (ou à n’importe quelle instance de SQL Server 2016 disposant d’autorisations de lecture et d’écriture sur ce conteneur).   
3.  Copiez et collez le script Transact-SQL suivant dans la fenêtre de requête. Sélectionnez la sauvegarde du journal à supprimer, ainsi que ses captures instantanées de fichiers associées. Modifiez l’URL en fonction du nom de votre compte de stockage et du conteneur que vous avez spécifiés dans la partie 1, fournissez le nom du fichier de sauvegarde du journal, puis exécutez ce script.  
  
  ```sql
  sys.sp_delete_backup 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/tutorial-21764-20181003205236.bak';  
  ```  
  
4.  Dans l’Explorateur d’objets, connectez-vous au stockage Azure.  
5.  Développez Conteneurs, développez le conteneur que vous avez créé lors de la partie 1 et vérifiez que le fichier de sauvegarde que vous avez utilisé à l’étape 3 n’apparaît plus dans ce conteneur (actualisez le nœud si nécessaire).  
  
    ![Conteneur Azure montrant la suppression de l’objet blob de sauvegarde du journal](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/deleted-backup-snapshot.png)
  
6.  Copiez, collez et exécutez le script Transact-SQL suivant dans la fenêtre de requête pour vérifier que les deux captures instantanées de fichiers ont été supprimées.  
  
    ```sql  
    -- verify that two file snapshots have been removed  
    SELECT * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016');   
    ```  
    ![Volet de résultats montrant 2 captures instantanées de fichier supprimées](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/results-of-two-deleted-snapshot-files.png)

## <a name="10---remove-resources"></a>10 : supprimer des ressources

Une fois que vous avez terminé ce tutoriel, pour préserver les ressources, prenez soin de supprimer le groupe de ressources créé dans ce tutoriel. 

Pour supprimer le groupe de ressources, exécutez le code powershell suivant :

  ```powershell
  # Define global variables for the script  
  $prefixName = '<prefix name>'  # should be the same as the beginning of the tutorial
  
  # Set a variable for the name of the resource group you will create or use  
  $resourceGroupName=$prefixName + 'rg'   
  
  # Adds an authenticated Azure account for use in the session   
  Connect-AzAccount    
  
  # Set the tenant, subscription and environment for use in the rest of   
  Set-AzContext -SubscriptionId $subscriptionID    
    
  # Remove the resource group
  Remove-AzResourceGroup -Name $resourceGroupName   
  ```


  
## <a name="see-also"></a>Voir aussi

[Fichiers de données SQL Server dans Microsoft Azure](../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md)  
[Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)  
[Sauvegarde de SQL Server sur une URL](../relational-databases/backup-restore/sql-server-backup-to-url.md) 
[Signatures d’accès partagé, partie 1 : présentation du modèle SAP](/azure/storage/common/storage-sas-overview)  
[Créer un conteneur](/rest/api/storageservices/Create-Container)  
[Set Container ACL](/rest/api/storageservices/Set-Container-ACL)  
[Obtenir la liste de contrôle d’accès du conteneur](/rest/api/storageservices/Get-Container-ACL)
[Informations d’identification &#40;moteur de base de données&#41;](../relational-databases/security/authentication-access/credentials-database-engine.md)  
[CREATE CREDENTIAL &#40;Transact-SQL&#41;](../t-sql/statements/create-credential-transact-sql.md)  
[sys.credentials &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)  
[sp_delete_backup &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md)  
[sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md)  
[sp_delete_backup_file_snapshot &#40;Transact-SQL&#41; ](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) [Sauvegardes instantanées de fichiers pour les fichiers de base de données dans Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)