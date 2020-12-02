---
title: Bonnes pratiques et résolution des problèmes liés à la sauvegarde vers une URL
description: Découvrez les bonnes pratiques et les conseils de dépannage relatifs à la sauvegarde et à la restauration SQL Server dans le stockage Blob Azure.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: de676bea-cec7-479d-891a-39ac8b85664f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4212c397c712351e951060032f6e7a2ece6a5c3f
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129035"
---
# <a name="sql-server-backup-to-url-best-practices-and-troubleshooting"></a>Bonnes pratiques et résolution des problèmes liés à la sauvegarde SQL Server vers une URL

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Cette rubrique présente des bonnes pratiques et des conseils de dépannage pour la sauvegarde et la restauration SQL Server dans le service Blob Azure.  
  
 Pour plus d’informations sur l’utilisation du service de stockage Blob Azure pour les opérations de sauvegarde et de restauration SQL Server, consultez :  
  
-   [Sauvegarde et restauration SQL Server avec le service Stockage Blob Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)  
  
-   [Tutoriel : Sauvegarde et restauration SQL Server avec le service Stockage Blob Azure](../../relational-databases/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service.md)  
  
## <a name="managing-backups"></a><a name="managing-backups-mb1"></a> Gestion des sauvegardes  
 La liste suivante comprend des recommandations générales sur la gestion des sauvegardes :  
  
-   Nous vous recommandons d'utiliser un nom de fichier unique pour chaque sauvegarde afin d'éviter tout remplacement accidentel des objets blob.  
  
-   Lors de la création d'un conteneur, nous vous recommandons de configurer le niveau d'accès sur **Privé**, afin que seuls les utilisateurs ou comptes qui peuvent fournir les informations d'identification requises puissent lire ou écrire les objets blob dans le conteneur.  
  
-   Pour les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui se trouvent sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécutant sur une machine virtuelle Azure, utilisez un compte de stockage qui se trouve dans la même région que la machine virtuelle, afin d’éviter les coûts de transfert des données entre les régions. L'utilisation de la même région garantit également des performances optimales pour les opérations de sauvegarde et de restauration.  
  
-   L'échec d'une activité de sauvegarde peut générer un fichier de sauvegarde non valide. Nous vous recommandons d'identifier périodiquement les sauvegardes en échec et de supprimer les fichiers d'objets blob. Pour plus d'informations, consultez [Deleting Backup Blob Files with Active Leases](../../relational-databases/backup-restore/deleting-backup-blob-files-with-active-leases.md)  
  
-   L'utilisation de `WITH COMPRESSION` pendant la sauvegarde peut réduire les coûts du stockage et des transactions de stockage. Elle peut également réduire le temps nécessaire pour terminer le processus de sauvegarde.  

- Définissez les arguments `MAXTRANSFERSIZE` et `BLOCKSIZE` comme cela est recommandé dans [Sauvegarde SQL Server vers une URL](./sql-server-backup-to-url.md).
  
## <a name="handling-large-files"></a>Gestion des fichiers volumineux  
  
-   L’opération de sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise plusieurs threads pour optimiser le transfert de données vers les services de stockage Blob Azure.  Toutefois, les performances dépendent de divers facteurs, tels que la bande passante de l'éditeur de logiciels et la taille de la base de données. Si vous envisagez de sauvegarder des bases de données ou groupes de fichiers volumineux à partir d'une base de données SQL Server locale, nous vous recommandons de commencer par tester le débit. Le [SLA pour Storage](https://azure.microsoft.com/support/legal/sla/storage/v1_0/) de Microsoft Azure impose des temps de traitement maximum pour les objets blob dont vous devez tenir compte.  
  
-   L’utilisation de l’option `WITH COMPRESSION`, comme recommandée dans la section [Gestion des sauvegardes](#managing-backups-mb1), est très importante lors de la sauvegarde de fichiers volumineux.  
  
## <a name="troubleshooting-backup-to-or-restore-from-url"></a>Dépannage des problèmes de sauvegarde vers une URL ou de restauration depuis une URL  
 Voici quelques méthodes rapides qui vous aideront à résoudre les erreurs survenant lors de la sauvegarde ou de la restauration vers/depuis le service de stockage Blob Azure.  
  
 Pour éviter les erreurs attribuables à des limitations ou à des options non prises en charge, consultez la liste des limitations, ainsi que l’aide sur les commandes BACKUP et RESTORE dans l’article [Sauvegarde et restauration SQL Server avec le service Microsoft Azure Blob Storage](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md) .  
  
 **Erreurs d'authentification :**  
  
-   `WITH CREDENTIAL` est une nouvelle option nécessaire pour la sauvegarde ou la restauration vers/depuis le service de stockage Blob Azure. Les défaillances liées aux informations d'identification peuvent être les suivantes :  
  
     Les informations d'identification spécifiées dans la commande **BACKUP** ou **RESTORE** n'existent pas. Pour éviter ce problème, vous pouvez inclure des instructions T-SQL afin de créer les informations d'identification si elles n'existent pas dans l'instruction de sauvegarde. Voici un exemple que vous pouvez utiliser :  
  
    ```sql  
    IF NOT EXISTS  
    (SELECT * FROM sys.credentials   
    WHERE credential_identity = 'mycredential')  
    CREATE CREDENTIAL <credential name> WITH IDENTITY = 'mystorageaccount'  
    , SECRET = '<storage access key>' ;  
    ```  
  
-   Les informations d'identification existent, mais le compte de connexion utilisé pour exécuter la commande de sauvegarde ne dispose pas des autorisations appropriées pour accéder aux informations d'identification. Utilisez un compte de connexion dans le rôle **db_backupoperator** avec des autorisations **_Modifier des informations d’identification_* _.  
  
-   Vérifiez le nom du compte de stockage et la valeur des clés. Les informations stockées dans les informations d’identification doivent correspondre aux valeurs de propriétés du compte de stockage Azure utilisé lors des opérations de sauvegarde et de restauration.  
  
 _ *Erreurs/Échecs de sauvegarde :* *  
  
-   Les sauvegardes parallèles dans un même objet blob provoquent l'échec d'une des sauvegardes avec l'erreur **Échec de l’initialisation** .  
  
-   Si vous utilisez des objets blob de page, par exemple `BACKUP... TO URL... WITH CREDENTIAL`, utilisez les journaux d'erreurs suivants pour vous aider à résoudre les erreurs de sauvegarde :  
  
    -   Définissez l'indicateur de trace 3051 pour activer la journalisation dans un journal des erreurs spécifique au format suivant :  
  
        `BackupToUrl-\<instname>-\<dbname>-action-\<PID>.log` où `\<action>` est un des éléments suivants :  
  
        -   **BdD**  
        -   **FILELISTONLY**  
        -   **LABELONLY**  
        -   **HEADERONLY**  
        -   **VERIFYONLY**  
  
    -   Vous pouvez également trouver des informations en examinant le journal des événements Windows nommé `SQLBackupToUrl` sous Journaux d’application.  

    -   Envisagez d’utiliser COMPRESSION, MAXTRANSFERSIZE, BLOCKSIZE et plusieurs arguments d’URL quand vous sauvegardez de grandes bases de données.  Consultez [Sauvegarde d’une base de données très volumineuse vers Stockage Blob Azure](/archive/blogs/sqlcat/backing-up-a-vldb-to-azure-blob-storage)
  
        ```console
        Msg 3202, Level 16, State 1, Line 1
        Write on "https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_0.bak" failed: 1117(The request could not be performed because of an I/O device error.)
        Msg 3013, Level 16, State 1, Line 1
        BACKUP DATABASE is terminating abnormally.
        ```

        ```sql  
        BACKUP DATABASE TestDb
        TO URL = 'https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_0.bak',
        URL = 'https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_1.bak',
        URL = 'https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_2.bak'
        WITH COMPRESSION, MAXTRANSFERSIZE = 4194304, BLOCKSIZE = 65536;  
        ```  

-   En cas de restauration d'une sauvegarde compressée, vous pouvez rencontrer l'erreur suivante :  
  
    -   `SqlException 3284 occurred. Severity: 16 State: 5  
        Message Filemark on device 'https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_0.bak' is not aligned.           Reissue the Restore statement with the same block size used to create the backupset: '65536' looks like a possible value.`  
  
        Pour résoudre cette erreur, réexécutez l’instruction **RESTORE** en spécifiant **BLOCKSIZE = 65536**.  
  
-   Erreur lors de la sauvegarde en raison d’objets blob pour lesquels un bail est actif : L’échec d’une activité de sauvegarde peut aboutir à des objets blob avec des bails actifs.  
  
     Si une instruction de sauvegarde est retentée, l'opération de sauvegarde échoue avec une erreur semblable à celle qui suit :  
  
     `Backup to URL received an exception from the remote endpoint. Exception Message: The remote server returned an error: (412) There is currently a lease on the blob and no lease ID was specified in the request.`  
  
     Si une instruction de restauration est tentée sur un fichier de sauvegarde d'objet blob dont le bail est actif, l'opération de restauration échoue avec une erreur semblable à celle qui suit :  
  
     `Exception Message: The remote server returned an error: (409) Conflict..`  
  
     Lorsqu'une telle erreur se produit, les fichiers d'objets blob doivent être supprimés. Pour plus d'informations sur ce scénario et la résolution du problème, consultez [Deleting Backup Blob Files with Active Leases](../../relational-databases/backup-restore/deleting-backup-blob-files-with-active-leases.md)  
  
## <a name="proxy-errors"></a>Erreurs de proxy  
 Si vous utilisez des serveurs proxy pour l'accès à Internet, les erreurs suivantes peuvent survenir :  
  
 **Limitation de la connexion par les serveurs proxy :**  
  
 Les serveurs proxy peuvent avoir des paramètres qui limitent le nombre de connexions par minute. Le processus de sauvegarde vers l'URL est un processus multithread et, par conséquent, il peut dépasser cette limite. Si cela se produit, le serveur proxy supprime la connexion. Pour résoudre ce problème, modifiez les paramètres du proxy afin que SQL Server n'utilise pas le proxy. Voici quelques exemples des types d'erreur ou des messages qui peuvent s'afficher dans le journal des erreurs :  
  
```console
Write on "https://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak" failed: Backup to URL received an exception from the remote endpoint. Exception Message: Unable to read data from the transport connection: The connection was closed.
```  
  
```console
A nonrecoverable I/O error occurred on file "https://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak:" Error could not be gathered from Remote Endpoint.  
  
Msg 3013, Level 16, State 1, Line 2  
  
BACKUP DATABASE is terminating abnormally.  
```

```console
BackupIoRequest::ReportIoError: write failure on backup device https://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak'. Operating system error Backup to URL received an exception from the remote endpoint. Exception Message: Unable to read data from the transport connection: The connection was closed.
```  
  
Si vous activez la journalisation détaillée à l'aide de l'indicateur de trace 3051, vous pouvez également voir le message suivant dans les journaux :  
  
`HTTP status code 502, HTTP Status Message Proxy Error (The number of HTTP requests per minute exceeded the configured limit. Contact your ISA Server administrator.)` 
  
 **Les paramètres du proxy par défaut ne sont pas sélectionnés :**  
  
Parfois, les paramètres par défaut ne sont pas sélectionnés et provoquent des erreurs d’authentification du proxy, comme celle qui est illustrée ci-dessous :
 
 `A nonrecoverable I/O error occurred on file "https://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak:" Backup to URL received an exception from the remote endpoint. Exception Message: The remote server returned an error: (407)* **Proxy Authentication Required.`  
  
Pour résoudre ce problème, créez un fichier de configuration qui permet au processus de sauvegarde vers l'URL d'utiliser les paramètres du proxy par défaut à l'aide des étapes suivantes :  
  
1.  Créez un fichier de configuration nommé `BackuptoURL.exe.config` avec le contenu XML suivant :  
  
    ```xml  
    <?xml version ="1.0"?>  
    <configuration>   
                    <system.net>   
                                    <defaultProxy enabled="true" useDefaultCredentials="true">   
                                                    <proxy usesystemdefault="true" />   
                                    </defaultProxy>   
                    </system.net>  
    </configuration>  
    ```  
  
2.  Placez le fichier de configuration dans le dossier Binn de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par exemple, si mon instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installée sur le lecteur C de l’ordinateur, placez le fichier de configuration dans `C:\Program Files\Microsoft SQL Server\MSSQL13.\<InstanceName>\MSSQL\Binn`.  
  
## <a name="see-also"></a> Voir aussi  
 [Restauration à partir de sauvegardes stockées dans Windows Azure](../../relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure.md)  
[BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)  
[RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)
