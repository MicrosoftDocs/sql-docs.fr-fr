---
title: Créer une sauvegarde complète de base de données | Microsoft Docs
description: Cet article explique comment créer une sauvegarde complète de base de données dans SQL Server à l’aide de SQL Server Management Studio, de Transact-SQL ou de PowerShell.
ms.custom: sqlfreshmay19
ms.date: 09/12/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backing up databases [SQL Server], full backups
- backing up databases [SQL Server], SQL Server Management Studio
- backups [SQL Server], creating
- database backups [SQL Server], SQL Server Management Studio
ms.assetid: 586561fc-dfbb-4842-84f8-204a9100a534
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3eb0b33f2891c8f48d63b4446d31c1918fcb7aab
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076111"
---
# <a name="create-a-full-database-backup"></a>Créer une sauvegarde complète de base de données

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette rubrique explique comment créer une sauvegarde de base de données complète dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou de PowerShell.

Pour obtenir des informations sur la sauvegarde SQL Server dans le service Stockage Blob Azure, consultez [Sauvegarde et restauration SQL Server avec le service Stockage Blob Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md) et [Sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md).

## <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions

- L’instruction `BACKUP` n’est pas autorisée dans une transaction explicite ou implicite.
- Les sauvegardes créées avec une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peuvent pas être restaurées dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Pour obtenir une vue d’ensemble et approfondir vos connaissances des concepts de sauvegarde et des tâches, consultez [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md) avant de continuer.

## <a name="recommendations"></a><a name="Recommendations"></a> Recommandations

- À mesure que la taille d’une base de données augmente, les sauvegardes complètes de base de données nécessitent davantage de temps et d’espace de stockage. Pour les bases de données volumineuses, songez à compléter les sauvegardes complètes avec une série de [sauvegardes différentielles de base de données](../../relational-databases/backup-restore/differential-backups-sql-server.md).
- Vous pouvez estimer la taille d’une sauvegarde complète de base de données en utilisant la procédure stockée système [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) .
- Par défaut, chaque opération de sauvegarde réussie ajoute une entrée au journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et au journal des événements système. Si vous effectuez régulièrement une sauvegarde, ces messages de réussite s’accumuleront rapidement et vos journaux d’erreurs deviendront énormes. Cela peut rendre la recherche d’autres messages difficile. Dans ces cas-là, vous pouvez supprimer ces entrées de journaux de sauvegarde en utilisant l’indicateur de trace 3226 si aucun de vos scripts ne dépend de ces entrées. Pour plus d’informations, consultez [Indicateurs de trace &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

## <a name="security"></a><a name="Security"></a> Sécurité

**TRUSTWORTHY** a la valeur OFF pour une sauvegarde de base de données. Pour obtenir des informations sur la façon d’affecter la valeur ON à **TRUSTWORTHY**, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).

À compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], les options **PASSWORD** et **MEDIAPASSWORD** sont suspendues pour la création de sauvegardes. Vous pouvez toujours restaurer les sauvegardes créées avec des mots de passe.

## <a name="permissions"></a><a name="Permissions"></a> Autorisations

Les autorisations `BACKUP DATABASE` et `BACKUP LOG` reviennent par défaut aux membres du rôle serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_backupoperator**.

 Des problèmes de propriété et d'autorisations sur le fichier physique de l'unité de sauvegarde sont susceptibles de perturber une opération de sauvegarde. Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit pouvoir lire et écrire sur l’appareil, ce qui signifie que le compte sous lequel le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute doit avoir des autorisations d’écriture sur l’unité de sauvegarde. Toutefois, [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), qui ajoute une entrée pour une unité de sauvegarde dans les tables système, ne vérifie pas les autorisations d’accès au fichier. Par conséquent, il est possible qu’aucun problème ne se produise sur le fichier physique de l’unité de sauvegarde tant que la ressource physique n’est pas sollicitée lors d’une tentative de sauvegarde ou de restauration.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio

> [!NOTE]
> Si vous spécifiez une tâche de sauvegarde à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous pouvez générer le script [!INCLUDE[tsql](../../includes/tsql-md.md)] [BACKUP](../../t-sql/statements/backup-transact-sql.md) correspondant en cliquant sur le bouton **Script** et en sélectionnant une destination de script.

1. Après vous être connecté à l’instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)], dans l’**Explorateur d’objets**, développez l’arborescence du serveur.

1. Développez **Bases de données**, puis sélectionnez une base de données utilisateur ou développez **Bases de données système** et sélectionnez une base de données système.

1. Cliquez avec le bouton droit sur la base de données à sauvegarder, pointez sur **Tâches**, puis cliquez sur **Sauvegarder...** .

1. Dans la boîte de dialogue **Sauvegarder la base de données**, la base de données que vous avez sélectionnée apparaît dans la liste déroulante (vous pouvez la remplacer par toute autre base de données sur le serveur).

1. Dans la liste déroulante **Type de sauvegarde**, sélectionnez le type de sauvegarde souhaité. La valeur par défaut est **Complète**.

   > [!IMPORTANT]
   > Vous devez effectuer au moins une sauvegarde complète de la base de données avant d’effectuer une sauvegarde différentielle ou du journal des transactions.
   
1. Sous **Composant de sauvegarde**, sélectionnez **Base de données**.

1. Dans la section **Destination**, passez en revue l’emplacement par défaut du fichier de sauvegarde (dans le dossier ../mssql/data).

   Pour effectuer la sauvegarde sur un autre appareil, modifiez la sélection à l’aide de la liste déroulante **Sauvegarde sur**. Pour distribuer le jeu de sauvegarde sur plusieurs fichiers afin d’augmenter la vitesse de sauvegarde, cliquez sur **Ajouter** pour ajouter des objets et/ou des destinations de sauvegarde supplémentaires.
 
   Pour supprimer une destination de sauvegarde, sélectionnez-la, puis cliquez sur **Supprimer**. Pour afficher le contenu d’une destination de sauvegarde existante, sélectionnez-la, puis cliquez sur **Contenu**.

1. (facultatif) Passez en revue les autres paramètres disponibles dans les pages **Options de support** et **Options de sauvegarde**.

   Pour plus d’informations sur les différentes options de sauvegarde, consultez [Page Général](back-up-database-general-page.md), [Page Options de support](back-up-database-media-options-page.md) et [Page Options de sauvegarde](back-up-database-backup-options-page.md).

1. Cliquez sur **OK** pour lancer la sauvegarde.

1. Une fois la sauvegarde terminée, cliquez sur **OK** pour fermer la boîte de dialogue SQL Server Management Studio.

### <a name="additional-information"></a>Informations supplémentaires

- Après la création d’une sauvegarde de base de données complète, vous pouvez créer une [sauvegarde de base de données différentielle](create-a-differential-database-backup-sql-server.md) ou une [sauvegarde du journal des transactions](back-up-a-transaction-log-sql-server.md).

- (facultatif) Vous pouvez cocher la case **Sauvegarde de copie uniquement** pour créer une sauvegarde de copie uniquement. Une *sauvegarde de copie uniquement* est une sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] indépendante du mécanisme des sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conventionnelles. Pour plus d’informations, consultez [Sauvegardes de copie uniquement &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md). Une sauvegarde de copie uniquement n’est pas disponible pour le type de sauvegarde **Différentielle**.

- L’option **Remplacer le support** est désactivée dans la page **Options de support** si vous sauvegardez vos données vers une URL.

### <a name="examples"></a>Exemples

Pour les exemples suivants, créez une base de données de test avec le code Transact-SQL suivant :

```sql
USE [master]
GO

CREATE DATABASE [SQLTestDB]
GO

USE [SQLTestDB]
GO
CREATE TABLE SQLTest
   (
      ID INT NOT NULL PRIMARY KEY,
      c1 VARCHAR(100) NOT NULL,
      dt1 DATETIME NOT NULL DEFAULT getdate()
   );
GO

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
```

#### <a name="a-full-back-up-to-disk-to-default-location"></a>R. Sauvegarde complète sur disque à l’emplacement par défaut

Dans cet exemple, la base de données `SQLTestDB` est sauvegardée sur disque à l’emplacement de sauvegarde par défaut.

1. Après vous être connecté à l’instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)], dans l’**Explorateur d’objets**, développez l’arborescence du serveur.

1. Développez **Bases de données**, cliquez avec le bouton droit sur `SQLTestDB`, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**.

1. Cliquez sur **OK**.

1. Une fois la sauvegarde terminée, cliquez sur **OK** pour fermer la boîte de dialogue SQL Server Management Studio.

![Effectuer la sauvegarde SQL](media/quickstart-backup-restore-database/backup-db-ssms.png)

#### <a name="b-full-back-up-to-disk-to-non-default-location"></a>B. Sauvegarde complète sur disque à un emplacement autre que celui par défaut

Dans cet exemple, la base de données `SQLTestDB` est sauvegardée sur disque à l’emplacement de votre choix.

1. Après vous être connecté à l’instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)], dans l’**Explorateur d’objets**, développez l’arborescence du serveur.

1. Développez **Bases de données**, cliquez avec le bouton droit sur `SQLTestDB`, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**.

1. Dans la page **Général** de la section **Destination** , sélectionnez **Disque** dans la liste déroulante **Sauvegarde sur :** .

1. Sélectionnez **Supprimer** jusqu’à ce que tous les fichiers de sauvegarde existants aient été supprimés.

1. Sélectionnez **Ajouter** pour ouvrir la boîte de dialogue **Sélectionner la destination de la sauvegarde** .

1. Entrez un chemin et un nom de fichier valides dans la zone de texte **Nom de fichier** et utilisez **.bak** comme extension pour simplifier la classification de ce fichier.

1. Cliquez sur **OK**, puis de nouveau sur **OK** pour lancer la sauvegarde.

1. Une fois la sauvegarde terminée, cliquez sur **OK** pour fermer la boîte de dialogue SQL Server Management Studio.

![Changer l’emplacement de la base de données](media/create-a-full-database-backup-sql-server/change-db-location.png)

#### <a name="c-create-an-encrypted-backup"></a>C. Créer une sauvegarde chiffrée

Dans cet exemple, la base de données `SQLTestDB` est sauvegardée avec chiffrement à l’emplacement de sauvegarde par défaut.

1. Après vous être connecté à l’instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)], dans l’**Explorateur d’objets**, développez l’arborescence du serveur.

1. Développez **Bases de données**, **Bases de données système**, cliquez avec le bouton droit sur `master`, puis cliquez sur **Nouvelle requête** pour ouvrir une fenêtre de requête avec une connexion à votre base de données `SQLTestDB`.

1. Exécutez les commandes suivantes pour créer une [**clé principale de base de données**](../../relational-databases/security/encryption/create-a-database-master-key.md) et un [**certificat**](../../t-sql/statements/create-certificate-transact-sql.md) dans la base de données `master`.  

   ```sql
   -- Create the master key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe';  

   -- If the master key already exists, open it in the same session that you create the certificate (see next step)
   OPEN MASTER KEY DECRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe'

   -- Create the certificate encrypted by the master key
   CREATE CERTIFICATE MyCertificate
   WITH SUBJECT = 'Backup Cert', EXPIRY_DATE = '20201031';  
   ```

1. Dans l’**Explorateur d’objets**, dans le nœud **Bases de données**, cliquez avec le bouton droit sur `SQLTestDB`, pointez sur **Tâches**, puis cliquez sur **Sauvegarder...** .

1. Dans la page **Options de support**, dans la section **Remplacer le support**, sélectionnez **Sauvegarder sur un nouveau support de sauvegarde et effacer tous les jeux de sauvegarde existants**.

1. Dans la page **Options de sauvegarde** de la section **Chiffrement** sélectionnez la case à cocher **Chiffrer la sauvegarde** .

1. Dans la liste déroulante Algorithme, sélectionnez **AES 256**.

1. Dans la liste déroulante **Certificat ou clé asymétrique** , sélectionnez `MyCertificate`.

1. Sélectionnez **OK**.

![Sauvegarde chiffrée](media/create-a-full-database-backup-sql-server/encrypted-backup.png)

#### <a name="d-back-up-to-the-azure-blob-storage-service"></a>D. Sauvegarder sur le service Stockage Blob Azure

L’exemple ci-dessous effectue une sauvegarde complète de la base de données `SQLTestDB` vers le service Stockage Blob Azure. Cet exemple part du principe que vous disposez déjà d’un compte de stockage avec un conteneur d’objets blob. Cet exemple crée une signature d’accès partagé pour vous et il échoue si le conteneur dispose d’une signature d’accès partagé existante.

Si vous n’avez pas de conteneur d’objets blob Azure dans un compte de stockage, créez-en un avant de continuer. Pour plus d’informations, consultez [Créer un compte de stockage universel](/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal) et [Créer un conteneur](/azure/storage/blobs/storage-quickstart-blobs-portal#create-a-container).

1. Après vous être connecté à l’instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)], dans l’**Explorateur d’objets**, développez l’arborescence du serveur.

1. Développez **Bases de données**, cliquez avec le bouton droit sur `SQLTestDB`, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**.

1. Dans la page **Général** de la section **Destination** , sélectionnez **URL** dans la liste déroulante **Sauvegarde sur** .

1. Cliquez sur **Ajouter** pour ouvrir la boîte de dialogue **Sélectionner la destination de la sauvegarde** .

1. Si vous avez déjà inscrit le conteneur de stockage Azure que vous souhaitez utiliser avec SQL Server Management Studio, sélectionnez-le. Sinon, cliquez sur **Nouveau conteneur** pour inscrire un nouveau conteneur.

1. Dans la boîte de dialogue **Se connecter à un abonnement Microsoft**, connectez-vous à votre compte.

1. Dans la zone de texte déroulante **Sélectionner un compte de stockage**, sélectionnez votre compte de stockage.

1. Dans la zone de texte déroulante **Sélectionner un conteneur d’objets blob**, sélectionnez votre conteneur d’objets blob.

1. Dans la zone de calendrier déroulante **Expiration de la stratégie d’accès partagé**, sélectionnez une date d’expiration pour la stratégie d’accès partagé que vous créez dans cet exemple.

1. Cliquez sur **Créer des informations d’identification** pour générer une signature d’accès partagé et des informations d’identification dans SQL Server Management Studio.

1. Cliquez sur **OK** pour fermer la boîte de dialogue **Se connecter à un abonnement Microsoft**.

1. Dans la zone de texte **Fichier de sauvegarde**, modifiez le nom du fichier de sauvegarde (facultatif).

1. Cliquez sur **OK** pour fermer la boîte de dialogue **Sélectionner la destination de la sauvegarde**.

1. Cliquez sur **OK** pour lancer la sauvegarde.

1. Une fois la sauvegarde terminée, cliquez sur **OK** pour fermer la boîte de dialogue SQL Server Management Studio.

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL

Créez une sauvegarde de base de données complète en exécutant l’instruction `BACKUP DATABASE` et en spécifiant les éléments suivants :

- le nom de la base de données à sauvegarder ;
- l'unité de sauvegarde où est écrite la sauvegarde complète de la base de données.

La syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] de base nécessaire pour une sauvegarde de base de données complète est la suivante :

 BACKUP DATABASE *database* TO *unité_sauvegarde* [ **,** ...*n* ] [ WITH *options_with* [ **,** ...*o* ] ] ;

|Option|Description|
|------------|-----------------|
|*database*|Base de données à sauvegarder|
|*unité_sauvegarde* [ **,** ...*n* ]|Spécifie une liste de 1 à 64 unités de sauvegarde à utiliser pour l'opération de sauvegarde. Vous pouvez spécifier une unité de sauvegarde physique ou une unité de sauvegarde logique correspondante, si celle-ci est déjà définie. Pour spécifier une unité de sauvegarde physique, utilisez l'option DISK ou TAPE :<br /><br /> { DISK &#124; TAPE } **=** _nom\_appareil\_sauvegarde\_physique_<br /><br /> Pour plus d’informations, consultez [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).|
|WITH *options_with* [ **,** ...*o* ]|Spécifie éventuellement une ou plusieurs options supplémentaires, *o*. Pour obtenir des informations de base sur les options, consultez l'étape 2.|
|||

Spécifiez éventuellement une ou plusieurs options **WITH**. Quelques options **WITH** de base sont décrites ici. Pour obtenir des informations sur toutes les options **WITH**, consultez [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).

Options **WITH** de base relatives au jeu de sauvegarde :

- **{ COMPRESSION | NO_COMPRESSION }** : Dans [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] et versions ultérieures uniquement, spécifie si la [compression de la sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md) est effectuée sur cette sauvegarde, remplaçant la valeur par défaut au niveau du serveur.
- **ENCRYPTION (ALGORITHM, SERVER CERTIFICATE | ASYMMETRIC KEY)** : Dans SQL Server 2014 ou les versions ultérieures, spécifiez l'algorithme de chiffrement à utiliser, ainsi que le certificat ou la clé asymétrique pour sécuriser le chiffrement.
- **DESCRIPTION** **=** { **'** _text_ **'**  |  **@** _text\_variable_ }: Spécifie le texte au format libre servant à décrire le jeu de sauvegarde. La chaîne peut compter jusqu'à 255 caractères.
- **NAME = { *nom_jeu_sauvegarde* |  **@** _variable\_nom\_jeu\_sauvegarde_ }**  : Spécifie le nom du jeu de sauvegarde. Les noms peuvent contenir jusqu'à 128 caractères. Si l'option NAME n'est pas spécifiée, le nom reste vide.

Par défaut, `BACKUP` ajoute la sauvegarde à un support de sauvegarde existant, préservant les jeux de sauvegarde existants. Pour spécifier explicitement ceci, utilisez l’option `NOINIT`. Pour plus d’informations sur l’ajout à des jeux de sauvegarde existants, consultez [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).

Une autre méthode pour formater le support de sauvegarde consiste à utiliser l’option **FORMAT** :

 FORMAT [ **,** MEDIANAME **=** { *nom_support* |  **@** _variable\_nom\_support_ } ] [ **,** MEDIADESCRIPTION **=** { *texte* |  **@** _variable\_texte_ } ]

 Utilisez la clause **FORMAT** si vous utilisez le support pour la première fois ou si vous souhaitez écraser toutes les données existantes. Assignez éventuellement un nom et une description au nouveau support.

 > [!IMPORTANT]
 > Soyez extrêmement vigilant lorsque vous utilisez la clause **FORMAT** de l’instruction `BACKUP`, car elle entraîne la destruction de toutes les sauvegardes préalablement stockées sur le support de sauvegarde.

### <a name="examples"></a><a name="TsqlExample"></a> Exemples

Pour les exemples suivants, créez une base de données de test avec le code Transact-SQL suivant :

```sql
USE [master]
GO

CREATE DATABASE [SQLTestDB]
GO

USE [SQLTestDB]
GO
CREATE TABLE SQLTest (
   ID INT NOT NULL PRIMARY KEY,
   c1 VARCHAR(100) NOT NULL,
   dt1 DATETIME NOT NULL DEFAULT GETDATE()
)
GO

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
```

#### <a name="a-back-up-to-a-disk-device"></a>R. Sauvegarder sur une unité de disque

L'exemple suivant sauvegarde entièrement la base de données `SQLTestDB` sur disque, à l'aide de `FORMAT` , pour créer une nouveau jeu de supports.

```sql
USE SQLTestDB;
GO
BACKUP DATABASE SQLTestDB
TO DISK = 'c:\tmp\SQLTestDB.bak'
   WITH FORMAT,
      MEDIANAME = 'SQLServerBackups',
      NAME = 'Full Backup of SQLTestDB';
GO
```

#### <a name="b-back-up-to-a-tape-device"></a>B. Sauvegarder sur un périphérique à bandes

 L’exemple suivant sauvegarde la base de données `SQLTestDB` complète sur bande, en ajoutant la sauvegarde aux sauvegardes précédentes.

```sql
USE SQLTestDB;
GO
BACKUP DATABASE SQLTestDB
   TO TAPE = '\\.\Tape0'
   WITH NOINIT,
      NAME = 'Full Backup of SQLTestDB';
GO
```

#### <a name="c-back-up-to-a-logical-tape-device"></a>C. Sauvegarder sur un périphérique à bandes logique

L'exemple suivant crée une unité de sauvegarde logique pour un périphérique à bandes. Il sauvegarde ensuite la base de données SQLTestDB complète sur ce périphérique.

```sql
-- Create a logical backup device,
-- SQLTestDB_Bak_Tape, for tape device \\.\tape0.
USE master;
GO
EXEC sp_addumpdevice 'tape', 'SQLTestDB_Bak_Tape', '\\.\tape0'; USE SQLTestDB;
GO
BACKUP DATABASE SQLTestDB
   TO SQLTestDB_Bak_Tape
   WITH FORMAT,
      MEDIANAME = 'SQLTestDB_Bak_Tape',
      MEDIADESCRIPTION = '\\.\tape0',
      NAME = 'Full Backup of SQLTestDB';
GO
```

## <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell

Utilisez l’applet de commande **Backup-SqlDatabase** . Pour indiquer explicitement qu’il s’agit d’une sauvegarde complète de la base de données, spécifiez le paramètre **-BackupAction** avec sa valeur par défaut, **Database**. Ce paramètre est facultatif pour les sauvegardes complètes de base de données.

> [!NOTE]
> Ces exemples nécessitent le module SqlServer. Pour déterminer s’il est installé, exécutez `Get-Module -Name SqlServer`. Pour installer ce module, exécutez `Install-Module -Name SqlServer` dans une session administrateur de PowerShell.
>
> Pour plus d’informations, consultez [Fournisseur PowerShell SQL Server](../../powershell/sql-server-powershell-provider.md).

> [!IMPORTANT]
> Si vous ouvrez une fenêtre PowerShell dans SQL Server Management Studio pour vous connecter à une installation de SQL Server, vous pouvez ignorer la partie informations d’identification de cet exemple, car vos informations d’identification dans SSMS sont automatiquement utilisées pour établir la connexion entre PowerShell et votre instance SQL Server.

### <a name="examples"></a>Exemples

#### <a name="a-full-backup-local"></a>R. Sauvegarde complète (locale)

L'exemple suivant crée une sauvegarde complète de la base de données `<myDatabase>` à l'emplacement de sauvegarde par défaut de l'instance de serveur `Computer\Instance`. Cet exemple spécifie, de manière facultative, **-BackupAction Database**.

Pour obtenir la syntaxe complète et des exemples supplémentaires, consultez [Backup-SqlDatabase](/powershell/module/sqlserver/backup-sqldatabase).

```powershell
$credential = Get-Credential

Backup-SqlDatabase -ServerInstance Computer[\Instance] -Database <myDatabase> -BackupAction Database -Credential $credential
```

#### <a name="b-full-backup-to-azure"></a>B. Sauvegarde complète vers Azure

L’exemple suivant crée une sauvegarde complète de la base de données `<myDatabase>` sur l’instance `<myServer>` pour le service Stockage Blob Azure. Une stratégie d’accès stockée a été créée avec des droits de lecture, écriture et liste. Les informations d’identification SQL Server, `https://<myStorageAccount>.blob.core.windows.net/<myContainer>`, ont été créées à l’aide d’une signature d’accès partagé associée à la stratégie d’accès stockée. La commande PowerShell utilise le paramètre **BackupFile** pour spécifier l’emplacement (URL) et le nom du fichier de sauvegarde.

```powershell
$credential = Get-Credential
$container = 'https://<myStorageAccount>blob.core.windows.net/<myContainer>'
$fileName = '<myDatabase>.bak'
$server = '<myServer>'
$database = '<myDatabase>'
$backupFile = $container + '/' + $fileName

Backup-SqlDatabase -ServerInstance $server -Database $database -BackupFile $backupFile -Credential $credential
```

## <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks

- [Sauvegarder une base de données (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)
- [Créer une sauvegarde différentielle de base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)
- [Restaurer une sauvegarde de base de données à l’aide de SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)
- [Restaurer une sauvegarde de base de données en mode de récupération simple &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-a-database-backup-under-the-simple-recovery-model-transact-sql.md)
- [Restaurer une base de données jusqu’au point d’échec en mode de récupération complète &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)
- [Restaurer une base de données à un nouvel emplacement &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)
- [Utiliser l'Assistant Plan de maintenance](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)

## <a name="see-also"></a>Voir aussi

- [Dépannage des opérations de sauvegarde et de restauration SQL Server](https://support.microsoft.com/kb/224071)
- [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)
- [Sauvegardes des journaux de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)
- [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)
- [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)
- [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)
- [Sauvegarder la base de données &#40;page Général&#41;](../../relational-databases/backup-restore/back-up-database-general-page.md)
- [Sauvegarder la base de données &#40;page Options de sauvegarde&#41;](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)
- [Sauvegardes différentielles &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)
- [Sauvegardes complètes de bases de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-database-backups-sql-server.md)