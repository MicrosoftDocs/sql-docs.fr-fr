---
title: Configurer la copie des journaux de transaction pour SQL Server sur Linux
description: Ce didacticiel présente un exemple de base de la procédure de réplication d’une instance sur Linux sur une instance secondaire à l’aide de la copie des journaux de transaction.
author: VanMSFT
ms.author: vanto
ms.date: 07/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 9385bcc04935c90ff060201d38e676b6da226e33
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346216"
---
# <a name="get-started-with-log-shipping-on-linux"></a>Prise en main de la copie des journaux de transaction sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

La copie des journaux de transaction SQL Server est une configuration à haute disponibilité où une base de données d’un serveur principal est répliquée sur un ou plusieurs serveurs secondaires. En résumé, une sauvegarde de la base de données source est restaurée sur le serveur secondaire. Ensuite, le serveur principal crée régulièrement des sauvegardes du journal des transactions et les serveurs secondaires les restaurent, en mettant à jour la copie secondaire de la base de données. 

  ![Diagramme du workflow de copie des journaux de transaction](https://preview.ibb.co/hr5Ri5/logshipping.png)

Comme décrit dans cette image, une session de copie des journaux de transaction implique les étapes suivantes :

- Sauvegarde du fichier des journaux des transactions au niveau de l'instance principale
- Copie du fichier de sauvegarde du journal des transactions sur le réseau vers une ou plusieurs instances secondaires
- Restauration du fichier de sauvegarde du journal des transactions sur les instances secondaires

## <a name="prerequisites"></a>Conditions préalables requises
- [Installer SQL Server Agent sur Linux](./sql-server-linux-setup-sql-agent.md)

## <a name="setup-a-network-share-for-log-shipping-using-cifs"></a>Configurer un partage réseau pour la copie des journaux de transaction à l’aide de CIFS 

> [!NOTE] 
> Ce didacticiel utilise CIFS + Samba pour configurer le partage réseau. Si vous souhaitez utiliser NFS, laissez un commentaire et nous l’ajouterons au document.       

### <a name="configure-primary-server"></a>Configurer le serveur principal
-   Exécutez la commande suivante pour installer Samba

    ```bash
    sudo apt-get install samba #For Ubuntu
    sudo yum -y install samba #For RHEL/CentOS
    ```
-   Créez un répertoire pour stocker les journaux de la copie des journaux de transaction et accordez à mssql les autorisations requises

    ```bash
    mkdir /var/opt/mssql/tlogs
    chown mssql:mssql /var/opt/mssql/tlogs
    chmod 0700 /var/opt/mssql/tlogs
    ```

-   Modifiez le fichier /etc/samba/smb.conf (vous avez besoin d’autorisations racine pour cela) et ajoutez la section suivante :

    ```bash
    [tlogs]
    path=/var/opt/mssql/tlogs
    available=yes
    read only=yes
    browsable=yes
    public=yes
    writable=no
    ```

-   Créer un utilisateur mssql pour Samba

    ```bash
    sudo smbpasswd -a mssql
    ```

-   Redémarrer les services Samba
    ```bash
    sudo systemctl restart smbd.service nmbd.service
    ```
 
### <a name="configure-secondary-server"></a>Configurer le serveur secondaire

-   Exécuter la commande suivante pour installer le client CIFS
    ```bash   
    sudo apt-get install cifs-utils #For Ubuntu
    sudo yum -y install cifs-utils #For RHEL/CentOS
    ```

-   Créez un fichier pour stocker vos informations d’identification. Utilisez le mot de passe que vous avez récemment défini pour votre compte mssql Samba 

    ```console
        vim /var/opt/mssql/.tlogcreds
        #Paste the following in .tlogcreds
        username=mssql
        domain=<domain>
        password=<password>
    ```

-   Exécutez les commandes suivantes pour créer un répertoire vide pour le montage et définir l’autorisation et de la propriété correctement
    ```bash   
    mkdir /var/opt/mssql/tlogs
    sudo chown root:root /var/opt/mssql/tlogs
    sudo chmod 0550 /var/opt/mssql/tlogs
    sudo chown root:root /var/opt/mssql/.tlogcreds
    sudo chmod 0660 /var/opt/mssql/.tlogcreds
    ```

-   Ajouter la ligne à etc/fstab pour conserver le partage 

    ```console
        //<ip_address_of_primary_server>/tlogs /var/opt/mssql/tlogs cifs credentials=/var/opt/mssql/.tlogcreds,ro,uid=mssql,gid=mssql 0 0
    ```

-   Monter les partages
    ```bash   
    sudo mount -a
    ```
       
## <a name="setup-log-shipping-via-t-sql"></a>Configurer la copie des journaux de transaction via T-SQL

- Exécuter ce script à partir de votre serveur principal

    ```sql
    BACKUP DATABASE SampleDB
    TO DISK = '/var/opt/mssql/tlogs/SampleDB.bak'
    GO
    ```

    ```sql
    DECLARE @LS_BackupJobId AS uniqueidentifier 
    DECLARE @LS_PrimaryId   AS uniqueidentifier 
    DECLARE @SP_Add_RetCode As int 
    EXECUTE @SP_Add_RetCode = master.dbo.sp_add_log_shipping_primary_database 
             @database = N'SampleDB' 
            ,@backup_directory = N'/var/opt/mssql/tlogs' 
            ,@backup_share = N'/var/opt/mssql/tlogs' 
            ,@backup_job_name = N'LSBackup_SampleDB' 
            ,@backup_retention_period = 4320
            ,@backup_compression = 2
            ,@backup_threshold = 60 
            ,@threshold_alert_enabled = 1
            ,@history_retention_period = 5760 
            ,@backup_job_id = @LS_BackupJobId OUTPUT 
            ,@primary_id = @LS_PrimaryId OUTPUT 
            ,@overwrite = 1 

    IF (@@ERROR = 0 AND @SP_Add_RetCode = 0) 
    BEGIN 

    DECLARE @LS_BackUpScheduleUID   As uniqueidentifier 
    DECLARE @LS_BackUpScheduleID    AS int 

    EXECUTE msdb.dbo.sp_add_schedule 
            @schedule_name =N'LSBackupSchedule' 
            ,@enabled = 1 
            ,@freq_type = 4 
            ,@freq_interval = 1 
            ,@freq_subday_type = 4 
            ,@freq_subday_interval = 15 
            ,@freq_recurrence_factor = 0 
            ,@active_start_date = 20170418 
            ,@active_end_date = 99991231 
            ,@active_start_time = 0 
            ,@active_end_time = 235900 
            ,@schedule_uid = @LS_BackUpScheduleUID OUTPUT 
            ,@schedule_id = @LS_BackUpScheduleID OUTPUT 

    EXECUTE msdb.dbo.sp_attach_schedule 
            @job_id = @LS_BackupJobId 
            ,@schedule_id = @LS_BackUpScheduleID  

    EXECUTE msdb.dbo.sp_update_job 
            @job_id = @LS_BackupJobId 
            ,@enabled = 1 
            
    END 

    EXECUTE master.dbo.sp_add_log_shipping_alert_job 

    EXECUTE master.dbo.sp_add_log_shipping_primary_secondary 
            @primary_database = N'SampleDB' 
            ,@secondary_server = N'<ip_address_of_secondary_server>' 
            ,@secondary_database = N'SampleDB' 
            ,@overwrite = 1 
    ```


- Exécuter ce script à partir de votre serveur secondaire

    ```sql
    RESTORE DATABASE SampleDB FROM DISK = '/var/opt/mssql/tlogs/SampleDB.bak'
    WITH NORECOVERY;
    ```
    
    ```sql
    DECLARE @LS_Secondary__CopyJobId    AS uniqueidentifier 
    DECLARE @LS_Secondary__RestoreJobId AS uniqueidentifier 
    DECLARE @LS_Secondary__SecondaryId  AS uniqueidentifier 
    DECLARE @LS_Add_RetCode As int 

    EXECUTE @LS_Add_RetCode = master.dbo.sp_add_log_shipping_secondary_primary 
            @primary_server = N'<ip_address_of_primary_server>' 
            ,@primary_database = N'SampleDB' 
            ,@backup_source_directory = N'/var/opt/mssql/tlogs/' 
            ,@backup_destination_directory = N'/var/opt/mssql/tlogs/' 
            ,@copy_job_name = N'LSCopy_SampleDB' 
            ,@restore_job_name = N'LSRestore_SampleDB' 
            ,@file_retention_period = 4320 
            ,@overwrite = 1 
            ,@copy_job_id = @LS_Secondary__CopyJobId OUTPUT 
            ,@restore_job_id = @LS_Secondary__RestoreJobId OUTPUT 
            ,@secondary_id = @LS_Secondary__SecondaryId OUTPUT 

    IF (@@ERROR = 0 AND @LS_Add_RetCode = 0) 
    BEGIN 

    DECLARE @LS_SecondaryCopyJobScheduleUID As uniqueidentifier 
    DECLARE @LS_SecondaryCopyJobScheduleID  AS int 

    EXECUTE msdb.dbo.sp_add_schedule 
            @schedule_name =N'DefaultCopyJobSchedule' 
            ,@enabled = 1 
            ,@freq_type = 4 
            ,@freq_interval = 1 
            ,@freq_subday_type = 4 
            ,@freq_subday_interval = 15 
            ,@freq_recurrence_factor = 0 
            ,@active_start_date = 20170418 
            ,@active_end_date = 99991231 
            ,@active_start_time = 0 
            ,@active_end_time = 235900 
            ,@schedule_uid = @LS_SecondaryCopyJobScheduleUID OUTPUT 
            ,@schedule_id = @LS_SecondaryCopyJobScheduleID OUTPUT 

    EXECUTE msdb.dbo.sp_attach_schedule 
            @job_id = @LS_Secondary__CopyJobId 
            ,@schedule_id = @LS_SecondaryCopyJobScheduleID  

    DECLARE @LS_SecondaryRestoreJobScheduleUID  As uniqueidentifier 
    DECLARE @LS_SecondaryRestoreJobScheduleID   AS int 

    EXECUTE msdb.dbo.sp_add_schedule 
            @schedule_name =N'DefaultRestoreJobSchedule' 
            ,@enabled = 1 
            ,@freq_type = 4 
            ,@freq_interval = 1 
            ,@freq_subday_type = 4 
            ,@freq_subday_interval = 15 
            ,@freq_recurrence_factor = 0 
            ,@active_start_date = 20170418 
            ,@active_end_date = 99991231 
            ,@active_start_time = 0 
            ,@active_end_time = 235900 
            ,@schedule_uid = @LS_SecondaryRestoreJobScheduleUID OUTPUT 
            ,@schedule_id = @LS_SecondaryRestoreJobScheduleID OUTPUT 

    EXECUTE msdb.dbo.sp_attach_schedule 
            @job_id = @LS_Secondary__RestoreJobId 
            ,@schedule_id = @LS_SecondaryRestoreJobScheduleID  
            
    END 
    DECLARE @LS_Add_RetCode2    As int 
    IF (@@ERROR = 0 AND @LS_Add_RetCode = 0) 
    BEGIN 

    EXECUTE @LS_Add_RetCode2 = master.dbo.sp_add_log_shipping_secondary_database 
            @secondary_database = N'SampleDB' 
            ,@primary_server = N'<ip_address_of_primary_server>' 
            ,@primary_database = N'SampleDB' 
            ,@restore_delay = 0 
            ,@restore_mode = 0 
            ,@disconnect_users  = 0 
            ,@restore_threshold = 45   
            ,@threshold_alert_enabled = 1 
            ,@history_retention_period  = 5760 
            ,@overwrite = 1 

    END 

    IF (@@error = 0 AND @LS_Add_RetCode = 0) 
    BEGIN 

    EXECUTE msdb.dbo.sp_update_job 
            @job_id = @LS_Secondary__CopyJobId 
            ,@enabled = 1 

    EXECUTE msdb.dbo.sp_update_job 
            @job_id = @LS_Secondary__RestoreJobId 
            ,@enabled = 1 

    END 
    ```

## <a name="verify-log-shipping-works"></a>Vérifier le bon fonctionnement de la copie des journaux de transaction

- Vérifier que la copie des journaux de transaction fonctionne en démarrant la tâche suivante sur le serveur principal

    ```sql
    USE msdb ;  
    GO  

    EXECUTE dbo.sp_start_job N'LSBackup_SampleDB' ;  
    GO  
    ```

- Vérifier que la copie des journaux de transaction fonctionne en démarrant la tâche suivante sur le serveur secondaire
 
    ```sql
    USE msdb ;  
    GO  

    EXECUTE dbo.sp_start_job N'LSCopy_SampleDB' ;  
    GO  
    EXECUTE dbo.sp_start_job N'LSRestore_SampleDB' ;  
    GO  
    ```
 - Vérifiez que le basculement de la copie des journaux de transaction fonctionne en exécutant la commande suivante.
 
    > [!WARNING]
    > Cette commande met en ligne la base de données secondaire et interrompt la configuration de la copie des journaux de transaction. Vous devrez reconfigurer la copie des journaux de transaction après avoir exécuté cette commande.
 
    ```sql
    RESTORE DATABASE SampleDB WITH RECOVERY;
    ```