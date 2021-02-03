---
title: Déplacer une base de données protégée par TDE vers un autre serveur SQL Server
description: Explique comment protéger une base de données à l’aide du chiffrement TDE (Transparent Data Encryption), puis comment la déplacer vers une autre instance de SQL Server à l’aide de SSMS (SQL Server Management Studio) ou de T-SQL (Transact-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Transparent Data Encryption, moving
- TDE, moving a database
ms.assetid: fb420903-df54-4016-bab6-49e6dfbdedc7
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 1f9b85e3d2f14eb534956c218d38f98b634dc89c
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813283"
---
# <a name="move-a-tde-protected-database-to-another-sql-server"></a>Déplacer une base de données protégée par le chiffrement transparent des données vers un autre serveur SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment protéger une base de données à l’aide du chiffrement transparent des données (TDE), puis la déplacer vers une autre instance de[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)]. TDE effectue le chiffrement et le déchiffrement d’E/S en temps réel des données et des fichiers journaux. Le chiffrement utilise une clé de chiffrement de base de données stockée dans l’enregistrement de démarrage de base de données à des fins de disponibilité lors de la récupération. La clé de chiffrement de base de données est une clé symétrique sécurisée à l'aide d'un certificat stocké dans la base de données **master** du serveur ou une clé asymétrique protégée par un module de gestion de clés extensible.   
   
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Lors du déplacement d'une base de données protégée par chiffrement transparent des données, vous devez également déplacer le certificat ou la clé asymétrique qui sert à ouvrir la clé DEK. Le certificat ou la clé asymétrique doit être installé dans la base de données **master** du serveur de destination, afin que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] puisse accéder aux fichiers de base de données. Pour plus d’informations, consultez [Transparent Data Encryption &#40;TDE&#41;](../../../relational-databases/security/encryption/transparent-data-encryption.md).  
  
-   Vous devez conserver des copies du fichier de certificat et du fichier de clé privée pour permettre la récupération du certificat. Le mot de passe de la clé privée ne doit pas forcément être le même que le mot de passe de la clé principale de la base de données.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] stocke les fichiers créés ici dans **C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA** par défaut. Vos noms et emplacements de fichier peuvent être différents.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
  
-   Requiert l'autorisation **CONTROL DATABASE** sur la base de données **master** pour créer la clé principale de base de données.  
  
-   Requiert l'autorisation **CREATE CERTIFICATE** sur la base de données **master** pour créer le certificat qui protège la clé DEK.  
  
-   Requiert l'autorisation **CONTROL DATABASE** sur la base de données chiffrée et l'autorisation **VIEW DEFINITION** sur le certificat ou la clé asymétrique qui sert à chiffrer la clé de chiffrement de la base de données.  
  
##  <a name="to-create-a-database-protected-by-transparent-data-encryption"></a><a name="SSMSProcedure"></a> Pour créer une base de données protégée par le chiffrement transparent des données  

Les procédures suivantes vous montrent comment créer une base de données protégée par TDE avec SQL Server Management Studio et Transact-SQL.
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSCreate"></a> Utilisation de SQL Server Management Studio  
  
1.  Créez une clé principale de la base de données et un certificat dans la base de données **master** . Pour plus d’informations, consultez **Utilisation de Transact-SQL** ci-dessous.  
  
2.  Créez une sauvegarde du certificat de serveur dans la base de données **master** . Pour plus d’informations, consultez **Utilisation de Transact-SQL** ci-dessous.  
  
3.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur le dossier **Bases de données** et sélectionnez **Nouvelle base de données**.  
  
4.  Dans la boîte de dialogue **Nouvelle base de données** , dans la zone **Nom de la base de données** , entrez le nom de la nouvelle base de données.  
  
5.  Dans la zone **Propriétaire** , entrez le nom du propriétaire de la nouvelle base de données. Vous pouvez également cliquer sur les points de suspension **(…)** pour ouvrir la boîte de dialogue **Sélectionner le propriétaire de la base de données**. Pour plus d'informations sur la création d'une nouvelle base de données, consultez [Create a Database](../../../relational-databases/databases/create-a-database.md).  
  
6.  Dans l'Explorateur d'objets, cliquez sur le signe plus pour développer le dossier **Bases de données** .  
  
7.  Cliquez avec le bouton droit sur la base de données que vous avez créée, pointez sur **Tâches**, puis sélectionnez **Gérer le chiffrement de base de données**.  
  
     Les options suivantes sont disponibles dans la boîte de dialogue **Gérer le chiffrement de base de données** .  
  
     **Algorithme de chiffrement**  
     Affiche ou définit l'algorithme à utiliser pour le chiffrement de la base de données. **AES128** est l'algorithme par défaut. Ce champ ne peut pas être vide. Pour plus d'informations sur les algorithmes de chiffrement, consultez [Choose an Encryption Algorithm](../../../relational-databases/security/encryption/choose-an-encryption-algorithm.md).  
  
     **Utilisez un certificat de serveur**  
     Définit le chiffrement à sécuriser par un certificat. Sélectionnez une option dans la liste. Si vous n'avez pas l'autorisation **VIEW DEFINITION** sur les certificats de serveur, cette liste sera vide. Si une méthode de chiffrement de certificat est sélectionnée, cette valeur ne peut pas être vide. Pour plus d'informations sur les certificats, consultez [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
     **Utilisez une clé asymétrique de serveur**  
     Définit le chiffrement à sécuriser par une clé asymétrique. Seules les clés asymétriques disponibles sont affichées. Seule une clé asymétrique protégée par un module EKM peut chiffrer une base de données en utilisant le chiffrement transparent de données.  
  
     **Définir le chiffrement de la base de données sur**  
     Modifie la base de données pour activer ou désactiver le chiffrement transparent des données.  
  
8.  Lorsque vous avez terminé, cliquez sur **OK**.  

###  <a name="using-transact-sql"></a><a name="TsqlCreate"></a> Utilisation de Transact-SQL  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```sql  
    -- Create a database master key and a certificate in the master database.  
    USE master ;  
    GO  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '*rt@40(FL&dasl1';  
    GO  
    CREATE CERTIFICATE TestSQLServerCert   
    WITH SUBJECT = 'Certificate to protect TDE key'  
    GO  
    -- Create a backup of the server certificate in the master database.  
    -- The following code stores the backup of the certificate and the private key file in the default data location for this instance of SQL Server   
    -- (C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA).  
  
    BACKUP CERTIFICATE TestSQLServerCert   
    TO FILE = 'TestSQLServerCert'  
    WITH PRIVATE KEY   
    (  
        FILE = 'SQLPrivateKeyFile',  
        ENCRYPTION BY PASSWORD = '*rt@40(FL&dasl1'  
    );  
    GO  
    -- Create a database to be protected by TDE.  
    CREATE DATABASE CustRecords ;  
    GO  
    -- Switch to the new database.  
    -- Create a database encryption key, that is protected by the server certificate in the master database.   
    -- Alter the new database to encrypt the database using TDE.  
    USE CustRecords;  
    GO  
    CREATE DATABASE ENCRYPTION KEY  
    WITH ALGORITHM = AES_128  
    ENCRYPTION BY SERVER CERTIFICATE TestSQLServerCert;  
    GO  
    ALTER DATABASE CustRecords  
    SET ENCRYPTION ON;  
    GO  
    ```  
  
 Pour plus d'informations, consultez les pages suivantes :  
  
-   [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
-   [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/backup-certificate-transact-sql.md)  
  
-   [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../../t-sql/statements/create-database-transact-sql.md)  
  
-   [CREATE DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-database-encryption-key-transact-sql.md)  
  
-   [ALTER DATABASE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-database-transact-sql.md)  
  
##  <a name="to-move-a-database-protected-by-transparent-data-encryption"></a><a name="TsqlProcedure"></a> Pour déplacer une base de données protégée par le chiffrement transparent des données 

Les procédures suivantes vous montrent comment déplacer une base de données protégée par TDE avec SQL Server Management Studio et Transact-SQL.
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSMove"></a> Utilisation de SQL Server Management Studio  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur la base de données que vous avez chiffrée précédemment, pointez sur **Tâches** et sélectionnez **Détacher…** .  
  
     Les options suivantes sont disponibles dans la boîte de dialogue **Détacher la base de données** .  
  
     **Bases de données à détacher**  
     Répertorie les bases de données à détacher.  
  
     **Database Name**  
     Spécifie le nom de la base de données à détacher.  
  
     **Supprimer les connexions**  
     Permet de déconnecter les connexions à la base de données spécifiée.  
  
    > [!NOTE]  
    >  Vous ne pouvez pas détacher une base de données avec des connexions actives.  
  
     **Mettre à jour les statistiques**  
     Par défaut, l'opération de détachement conserve toutes les statistiques d'optimisation obsolètes avant de procéder au détachement ; pour actualiser les statistiques existantes, activez cette case à cocher.  
  
     **Conserver les catalogues de texte intégral**  
     Par défaut, l'opération de détachement conserve tous les catalogues de texte intégral associés à la base de données. Pour les supprimer, décochez la case **Conserver les catalogues de texte intégral** . Cette option s'affiche uniquement lors de la mise à niveau d'une base de données à partir de [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
     **État**  
     Affiche un des états suivants : **Prêt** ou **Non prêt**.  
  
     **Message**  
     La colonne **Message** peut indiquer des informations sur la base de données, comme suit :  
  
    -   Lorsqu'une base de données est impliquée dans la réplication, l' **État** est **Non prêt** et la colonne **Message** indique **Base de données répliquée**.  
  
    -   Lorsqu'une base de données possède une ou plusieurs connexions actives, l’**État** est **Non prêt** et la colonne **Message** indique _\<number\_of\_active\_connections\>_ **connexion(s) active(s)** , par exemple : **1 connexion(s) active(s)** . Avant de détacher la base de données, vous devez déconnecter toutes les connexions actives en cliquant sur **Supprimer les connexions**.  
  
     Pour obtenir plus d'informations sur un message, cliquez sur le texte du lien hypertexte pour ouvrir le Moniteur d'activité.  
  
2.  Cliquez sur **OK**.  
  
3.  À l'aide de l'Explorateur Windows, déplacez ou copiez les fichiers de base de données depuis le serveur source vers le même emplacement sur le serveur de destination.  
  
4.  À l'aide de l'Explorateur Windows, déplacez ou copiez la sauvegarde du certificat de serveur et le fichier de clé privée depuis le serveur source vers le même emplacement sur le serveur de destination.  
  
5.  Créez une clé principale de base de données sur l'instance de destination de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez **Utilisation de Transact-SQL** ci-dessous.  
  
6.  Recréez le certificat de serveur à l'aide du fichier de sauvegarde du certificat de serveur d'origine. Pour plus d’informations, consultez **Utilisation de Transact-SQL** ci-dessous.  
  
7.  Dans l’Explorateur d’objets, dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], cliquez avec le bouton droit sur le dossier **Bases de données** et sélectionnez **Attacher…** .  
  
8.  Dans la boîte de dialogue **Attacher des bases de données** , sous **Bases de données à attacher**, cliquez sur **Ajouter**.  
  
9. Dans la boîte de dialogue **Rechercher les fichiers de base de données-** _nom\_serveur_, sélectionnez le fichier de base de données à attacher au nouveau serveur et cliquez sur **OK**.  
  
     Les options suivantes sont disponibles dans la boîte de dialogue **Attacher des bases de données** .  
  
     **Bases de données à attacher**  
     Affiche des informations sur les bases de données sélectionnées.  
  
     \<no column header>  
     Affiche une icône indiquant l'état de l'opération d'attachement. Les icônes possibles sont décrites dans la section **État** ci-dessous.  
  
     **Emplacement du fichier MDF**  
     Affiche le chemin d'accès et le nom du fichier MDF sélectionné.  
  
     **Database Name**  
     Affiche le nom de la base de données.  
  
     **Attacher en tant que**  
     Permet de spécifier éventuellement un autre nom sous lequel la base de données doit être attachée.  
  
     **Propriétaire**  
     Fournit une liste déroulante des propriétaires de bases de données possibles dans laquelle vous pouvez sélectionner un autre propriétaire.  
  
     **État**  
     Affiche l'état de la base de données conformément au tableau ci-après.  
  
    |Icône|Texte d'état|Description|  
    |----------|-----------------|-----------------|  
    |(Aucune icône)|(Aucun texte)|L'opération d'attachement n'a pas démarré ou est peut-être en attente pour cet objet. Il s'agit de la valeur par défaut lorsque la boîte de dialogue est ouverte.|  
    |Triangle vert dirigé vers la droite|En cours|L'opération d'attachement a démarré, mais n'est pas terminée.|  
    |Coche verte|Succès|L'attachement de l'objet a réussi.|  
    |Cercle rouge contenant une croix blanche|Error|L'opération d'attachement a rencontré une erreur et ne s'est pas terminée correctement.|  
    |Cercle contenant deux quartiers noirs (à gauche et à droite) et deux quartiers blancs (en haut et en bas)|Arrêté|L'opération d'attachement n'a pas réussi, car l'utilisateur l'a interrompue.|  
    |Cercle contenant une flèche courbe pointant dans le sens inverse des aiguilles d'une montre|Restauré|L'opération d'attachement a réussi, mais a été restaurée en raison d'une erreur lors de l'attachement d'un autre objet.|  
  
     **Message**  
     Affiche un message vierge ou un lien hypertexte « Fichier introuvable ».  
  
     **Ajouter**  
     Permet de rechercher les principaux fichiers de base de données nécessaires. Lorsque l'utilisateur sélectionne un fichier .mdf, les informations applicables sont automatiquement remplies dans les champs respectifs de la grille **Bases de données à attacher** .  
  
     **Remove**  
     Supprime le fichier sélectionné de la grille **Bases de données à attacher** .  
  
     **Détails de la base de données "** _<nom_base_de_données>_ **"**  
     Affiche le nom des fichiers à attacher. Pour vérifier ou changer le nom du chemin d’accès d’un fichier, cliquez sur le bouton **Parcourir** ( **...** ).  
  
    > [!NOTE]  
    >  Si un fichier n'existe pas, la colonne **Message** affiche « Introuvable ». Si un fichier journal est introuvable, cela signifie qu'il se trouve dans un autre répertoire ou qu'il a été supprimé. Vous devez mettre à jour le chemin d'accès du fichier dans la grille **Détails de la base de données** pour désigner l'emplacement correct ou supprimer le fichier journal de la grille. Si un fichier de données .ndf est introuvable, vous devez mettre à jour son chemin d'accès dans la grille pour désigner l'emplacement correct.  
  
     **Nom du fichier d'origine**  
     Affiche le nom du fichier attaché appartenant à la base de données.  
  
     **Type de fichier**  
     Indique le type du fichier, **Données** ou **Journal**.  
  
     **Chemin d'accès au fichier actuel**  
     Affiche le chemin d'accès au fichier de base de données sélectionné. Le chemin d'accès peut être modifié manuellement.  
  
     **Message**  
     Affiche un message vierge ou un lien hypertexte «**Fichier introuvable**».  
  
###  <a name="using-transact-sql"></a><a name="TsqlMove"></a> Utilisation de Transact-SQL  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```sql  
    -- Detach the TDE protected database from the source server.   
    USE master ;  
    GO  
    EXEC master.dbo.sp_detach_db @dbname = N'CustRecords';  
    GO  
    -- Move or copy the database files from the source server to the same location on the destination server.   
    -- Move or copy the backup of the server certificate and the private key file from the source server to the same location on the destination server.   
    -- Create a database master key on the destination instance of SQL Server.   
    USE master;  
    GO  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '*rt@40(FL&dasl1';  
    GO  
    -- Recreate the server certificate by using the original server certificate backup file.   
    -- The password must be the same as the password that was used when the backup was created.  
  
    CREATE CERTIFICATE TestSQLServerCert   
    FROM FILE = 'TestSQLServerCert'  
    WITH PRIVATE KEY   
    (  
        FILE = 'SQLPrivateKeyFile',  
        DECRYPTION BY PASSWORD = '*rt@40(FL&dasl1'  
    );  
    GO  
    -- Attach the database that is being moved.   
    -- The path of the database files must be the location where you have stored the database files.  
    CREATE DATABASE [CustRecords] ON   
    ( FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\CustRecords.mdf' ),  
    ( FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\CustRecords_log.LDF' )  
    FOR ATTACH ;  
    GO  
    ```  
  
 Pour plus d'informations, consultez les pages suivantes :  
  
-   [sp_detach_db &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)  
  
-   [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
-   [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../../t-sql/statements/create-database-transact-sql.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Attacher et détacher une base de données &#40;SQL Server&#41;](../../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [Transparent Data Encryption avec Azure SQL Database](/azure/azure-sql/database/transparent-data-encryption-tde-overview)  
  
