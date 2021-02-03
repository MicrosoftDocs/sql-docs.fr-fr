---
description: Instructions RESTORE – HEADERONLY (Transact-SQL)
title: RESTORE HEADERONLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- HEADERONLY
- RESTORE HEADERONLY
- RESTORE_HEADERONLY_TSQL
- HEADERONLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backup sets [SQL Server], header information
- headers [SQL Server]
- RESTORE HEADERONLY statement
- backup header information [SQL Server]
ms.assetid: 4b88e98c-49c4-4388-ab0e-476cc956977c
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: a270c24ee685e6e04fa999737828caf9c9559475
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182984"
---
# <a name="restore-statements---headeronly-transact-sql"></a>Instructions RESTORE – HEADERONLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  Renvoie un jeu de résultats contenant toutes les dernières informations d'en-tête des sauvegardes pour tous les jeux de sauvegardes d'un périphérique de sauvegarde particulier dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
 
> [!NOTE]  
>  Pour une description des arguments, consultez [Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
RESTORE HEADERONLY   
FROM <backup_device>   
[ WITH   
 {  
--Backup Set Options  
   FILE = { backup_set_file_number | @backup_set_file_number }   
 | PASSWORD = { password | @password_variable }   
  
--Media Set Options  
 | MEDIANAME = { media_name | @media_name_variable }   
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }  
  
--Error Management Options  
 | { CHECKSUM | NO_CHECKSUM }   
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }  
  
--Tape Options  
 | { REWIND | NOREWIND }   
 | { UNLOAD | NOUNLOAD }    
 } [ ,...n ]  
]  
[;]  
  
<backup_device> ::=  
{   
   { logical_backup_device_name |  
      @logical_backup_device_name_var }  
   | { DISK | TAPE | URL } = { 'physical_backup_device_name' |  
       @physical_backup_device_name_var }   
}  
  
```  
> [!NOTE] 
> Pour spécifier l’emplacement et le nom de fichier du Stockage Blob Microsoft Azure, utilisez le format URL pris en charge à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 Cu2. Bien que le stockage Microsoft Azure soit un service, son implémentation est similaire à celle d’un disque ou d’une bande afin d’offrir une expérience de restauration cohérente et fluide pour les trois appareils.

## <a name="arguments"></a>Arguments  
 Pour une description des arguments RESTORE HEADERONLY, consultez [Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
## <a name="result-sets"></a>Jeux de résultats  
 Pour chaque sauvegarde réalisée sur une unité donnée, le serveur envoie une ligne d'informations d'en-tête composée des colonnes suivantes :  
  
> [!NOTE]
>  RESTORE HEADERONLY recherche tous les jeux de sauvegarde sur le support. Par conséquent, la production de ce jeu de résultats en utilisant des lecteurs de bande à capacité élevée risque de prendre du temps. Pour obtenir un aperçu du support sans récupérer les informations propres à chaque jeu de sauvegarde, utilisez RESTORE LABELONLY ou définissez l’argument FILE **=** _backup_set_file_number_.  
> 
> [!NOTE]
>  Par sa nature, [!INCLUDE[msCoName](../../includes/msconame-md.md)] Tape Format autorise la cohabitation de jeux de sauvegarde créés avec d’autres logiciels sur le même support que les jeux de sauvegarde [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le jeu de résultats renvoyé par RESTORE HEADERONLY inclue une ligne pour chacun de ces autres jeux de sauvegardes.  
  
|Nom de la colonne|Type de données|Description des jeux de sauvegardes SQL Server|  
|-----------------|---------------|--------------------------------------------|  
|**BackupName**|**nvarchar(128)**|Nom du jeu de sauvegardes.|  
|**BackupDescription**|**nvarchar(255)**|Description du jeu de sauvegardes.|  
|**BackupType**|**smallint**|Type de sauvegarde :<br /><br /> **1** = Base de données<br /><br /> **2** = Journal des transactions<br /><br /> **4** = Fichier<br /><br /> **5** = Base de données différentielle<br /><br /> **6** = Fichier différentiel<br /><br /> **7** = Partiel<br /><br /> **8** = Partiel différentiel|  
|**ExpirationDate**|**datetime**|Date d'expiration du jeu de sauvegardes.|  
|**Compressed**|**BIT(1)**|Indique si le jeu de sauvegarde a fait l'objet d'une compression logicielle :<br /><br /> **0** = Non<br /><br /> **1** = Oui|  
|**Position**|**smallint**|Position du jeu de sauvegardes dans le volume (utilisée avec l'option FILE =).|  
|**DeviceType**|**tinyint**|Numéro correspondant à l'unité utilisée pour la sauvegarde.<br /><br /> Disque :<br /><br /> **2** = Logique<br /><br /> **102** = Physique<br /><br /> Bande :<br /><br /> **5** = Logique<br /><br /> **105** = Physique<br /><br /> Unité virtuelle :<br /><br /> **7** = Logique<br /><br /> **107** = Physique<br /><br /> URL<br /><br /> **9** = Logique<br /><br /> **109** = Physique<br /><br />  Les noms d’unité logiques et les numéros d’unités se trouvent dans **sys.backup_devices** ; pour plus d’informations, consultez [sys.backup_devices &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md).|  
|**UserName**|**nvarchar(128)**|Nom de l'utilisateur qui a effectué l'opération de sauvegarde.|  
|**ServerName**|**nvarchar(128)**|Nom du serveur qui a permis l'écriture du jeu de sauvegardes.|  
|**DatabaseName**|**nvarchar(128)**|Nom de la base de données qui a été sauvegardée.|  
|**DatabaseVersion**|**int**|Version de la base de données à partir de laquelle la sauvegarde a été créée.|  
|**DatabaseCreationDate**|**datetime**|Date et heure de création de la base de données.|  
|**BackupSize**|**numeric(20,0)**|Taille de la sauvegarde en octets.|  
|**FirstLSN**|**numeric(25,0)**|Numéro séquentiel dans le journal correspondant au premier enregistrement du journal dans le jeu de sauvegarde.|  
|**LastLSN**|**numeric(25,0)**|Numéro séquentiel dans le journal correspondant à l'enregistrement du journal suivant après le jeu de sauvegarde.|  
|**CheckpointLSN**|**numeric(25,0)**|Numéro séquentiel dans le journal correspondant au point de contrôle le plus récent au moment où la sauvegarde a été créée.|  
|**DatabaseBackupLSN**|**numeric(25,0)**|Numéro séquentiel dans le journal correspondant à la sauvegarde complète la plus récente de la base de données.<br /><br /> **DatabaseBackupLSN** constitue le « début du point de contrôle » déclenché au lancement de la sauvegarde. Ce LSN coïncide avec **FirstLSN** si la sauvegarde est effectuée quand la base de données est inactive et qu’aucune réplication n’a été configurée.|  
|**BackupStartDate**|**datetime**|Date et heure de début de l'opération de sauvegarde.|  
|**BackupFinishDate**|**datetime**|Date et heure de fin de l'opération de sauvegarde.|  
|**SortOrder**|**smallint**|Ordre de tri du serveur. Cette colonne n'est valide que pour les sauvegardes de base de données. Fourni pour la compatibilité ascendante.|  
|**CodePage**|**smallint**|Page de codes du serveur ou jeu de caractères utilisé par le serveur.|  
|**UnicodeLocaleId**|**int**|Option de configuration de ID locale Unicode du serveur utilisée pour le tri des données caractères Unicode. Fourni pour la compatibilité ascendante.|  
|**UnicodeComparisonStyle**|**int**|Option de configuration du style de comparaison Unicode du serveur qui offre un contrôle supplémentaire du tri des données Unicode. Fourni pour la compatibilité ascendante.|  
|**CompatibilityLevel**|**tinyint**|Paramètre de niveau de compatibilité de la base à partir de laquelle la sauvegarde a été créée.|  
|**SoftwareVendorId**|**int**|Numéro d'identification du fournisseur de logiciel. Pour SQL Server, ce numéro est **4608** (ou, en hexadécimal, **0x1200**).|  
|**SoftwareVersionMajor**|**int**|Numéro de version principal du serveur qui a créé le jeu de sauvegardes.|  
|**SoftwareVersionMinor**|**int**|Numéro de version secondaire du serveur qui a créé le jeu de sauvegardes.|  
|**SoftwareVersionBuild**|**int**|Numéro de build du serveur qui a créé le jeu de sauvegardes.|  
|**MachineName**|**nvarchar(128)**|Nom de l'ordinateur qui a effectué l'opération de sauvegarde.|  
|**Indicateurs**|**int**|Signification des différents bits d’indicateurs avec la valeur **1** :<br /><br /> **1** = La sauvegarde de journal contient des opérations de journalisation en bloc.<br /><br /> **2** = Sauvegarde d’instantané.<br /><br /> **4** = La base de données était accessible en lecture seule au moment de la sauvegarde.<br /><br /> **8** = La base de données était accessible en mode mono-utilisateur au moment de la sauvegarde.<br /><br /> **16** = La sauvegarde contient des sommes de contrôle de sauvegarde.<br /><br /> **32** = La base de données a été endommagée pendant la sauvegarde, mais l’opération de sauvegarde doit continuer malgré les erreurs.<br /><br /> **64** = Sauvegarde de la fin du journal.<br /><br /> **128** = Sauvegarde de la fin du journal avec des métadonnées incomplètes.<br /><br /> **256** = Sauvegarde de la fin du journal avec NORECOVERY.<br /><br /> **Important :** Au lieu d’utiliser des **indicateurs**, nous vous conseillons les colonnes de valeur booléenne individuelles (listées ci-dessous, en commençant à **HasBulkLoggedData**, jusqu’à **IsCopyOnly**).|  
|**BindingID**|**uniqueidentifier**|ID de liaison de la base de données. Cela correspond à **sys.database_recovery_status database_guid**. Lors de la restauration d'une base de données, une nouvelle valeur est attribuée. Voir aussi **FamilyGUID** (ci-dessous).|  
|**RecoveryForkID**|**uniqueidentifier**|ID de la fourchette de récupération de fin. Cette colonne correspond à **last_recovery_fork_guid** dans la table [backupset](../../relational-databases/system-tables/backupset-transact-sql.md).<br /><br /> Pour les sauvegardes de données, **RecoveryForkID** équivaut à **FirstRecoveryForkID**.|  
|**Classement**|**nvarchar(128)**|Classement utilisé par la base de données.|  
|**FamilyGUID**|**uniqueidentifier**|ID de la base de données d'origine lors de sa création. Cette valeur reste identique lors de la restauration de la base de données.|  
|**HasBulkLoggedData**|**bit**|**1** = Sauvegarde de journal contenant des opérations de journalisation en bloc.|  
|**IsSnapshot**|**bit**|**1** = Sauvegarde d’instantané.|  
|**IsReadOnly**|**bit**|**1** = La base de données était accessible en lecture seule au moment de la sauvegarde.|  
|**IsSingleUser**|**bit**|**1** = La base de données était en mode mono-utilisateur au moment de la sauvegarde.|  
|**HasBackupChecksums**|**bit**|**1** = La sauvegarde contient des sommes de contrôle de sauvegarde.|  
|**IsDamaged**|**bit**|**1** = La base de données a été endommagée pendant la sauvegarde, mais l’opération de sauvegarde doit continuer malgré les erreurs.|  
|**BeginsLogChain**|**bit**|**1** = Il s’agit de la première d’une chaîne continue de sauvegardes de journal. Une séquence de journaux démarre par la première sauvegarde journalisée effectuée après la création de la base de données, ou lorsqu'elle passe du mode de récupération simple à complète ou utilisant les journaux de transactions.|  
|**HasIncompleteMetaData**|**bit**|**1** = Sauvegarde de la fin du journal avec des métadonnées incomplètes.<br /><br /> Pour plus d’informations sur les sauvegardes de fin de journal avec des métadonnées incomplètes, consultez [Sauvegardes de la fin du journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).|  
|**IsForceOffline**|**bit**|**1** = Sauvegarde effectuée avec NORECOVERY ; la base de données a été placée en mode hors connexion par la sauvegarde.|  
|**IsCopyOnly**|**bit**|**1** = Sauvegarde de copie uniquement.<br /><br /> Une sauvegarde de copie unique n'influe pas sur les procédures globales de sauvegarde et de restauration de la base de données. Pour plus d’informations, consultez [Sauvegardes de copie uniquement &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).|  
|**FirstRecoveryForkID**|**uniqueidentifier**|ID de la fourchette de récupération de début. Cette colonne correspond à **first_recovery_fork_guid** dans la table [backupset](../../relational-databases/system-tables/backupset-transact-sql.md).<br /><br /> Pour les sauvegardes de données, **FirstRecoveryForkID** équivaut à **RecoveryForkID**.|  
|**ForkPointLSN**|**numeric(25,0)** NULL|Si **FirstRecoveryForkID** n’équivaut pas à **RecoveryForkID**, il s’agit du numéro séquentiel dans le journal du point de branchement. Dans les autres cas, cette valeur est NULL.|  
|**RecoveryModel**|**nvarchar(60)**|Modèle de restauration de la base de données, parmi<br /><br /> FULL<br /><br /> BULK-LOGGED<br /><br /> SIMPLE|  
|**DifferentialBaseLSN**|**numeric(25,0)** NULL|Pour une sauvegarde différentielle unique, la valeur équivaut à **FirstLSN** de la base différentielle ; les modifications avec un numéro de séquence d’enregistrement (LSN) supérieur ou égal à **DifferentialBaseLSN** sont incluses dans le différentiel.<br /><br /> Pour une sauvegarde différentielle multiple, la valeur est NULL, tandis que le LSN de base doit être déterminé au niveau du fichier. Pour plus d’informations, consultez [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).<br /><br /> Pour les types de sauvegarde non différentiels, la valeur est toujours NULL.<br /><br /> Pour plus d’informations, consultez [Sauvegardes différentielles &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md).|  
|**DifferentialBaseGUID**|**uniqueidentifier**|Pour une sauvegarde différentielle unique, cette valeur constitue l'identificateur unique de la base différentielle.<br /><br /> Pour les sauvegardes différentielles multiples, cette valeur est NULL, tandis que la base différentielle doit être déterminée par fichier.<br /><br /> Pour les types de sauvegarde non différentiels, la valeur est NULL.|  
|**BackupTypeDescription**|**nvarchar(60)**|Type de sauvegarde en tant que chaîne, parmi :<br /><br /> DATABASE<br /><br /> JOURNAL DES TRANSACTIONS<br /><br /> FICHIER OU GROUPE DE FICHIERS<br /><br /> BASE DE DONNÉES DIFFÉRENTIELLE<br /><br /> FICHIER DIFFÉRENTIEL PARTIEL<br /><br /> DIFFÉRENTIEL PARTIEL|  
|**BackupSetGUID**|**uniqueidentifier** NULL|Numéro d'identification unique du jeu de sauvegardes, par lequel s'effectue l'identification sur le support.|  
|**CompressedBackupSize**|**bigint**|Nombre d'octets du jeu de sauvegarde. Pour les sauvegardes non compressées, il s’agit de la même valeur que **BackupSize**.<br /><br /> Pour calculer le taux de compression, utilisez **CompressedBackupSize** et **BackupSize**.<br /><br /> Pendant une mise à niveau de **msdb**, cette valeur est configurée pour correspondre à la valeur de la colonne **BackupSize**.|  
|**containment**|**tinyint** non NULL|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Indique l'état de la relation contenant-contenu de la base de données.<br /><br /> 0 = La relation contenant-contenu de base de données est désactivée<br /><br /> 1 = La base de données est dans une relation contenant-contenu partielle|  
|**KeyAlgorithm**|**nvarchar(32)**|**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] (CU1) via la version actuelle.<br /><br /> Algorithme de chiffrement utilisé pour chiffrer la sauvegarde. La valeur NO_Encryption indique que la sauvegarde n'est pas chiffrée. Lorsque la valeur correcte ne peut pas être déterminée, la valeur doit être NULL.|  
|**EncryptorThumbprint**|**varbinary(20)**|**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] (CU1) via la version actuelle.<br /><br /> Empreinte numérique du chiffreur pouvant être utilisé pour rechercher un certificat ou la clé asymétrique dans la base de données. Si la sauvegarde n'est pas chiffrée, cette valeur est NULL.|  
|**EncryptorType**|**nvarchar(32)**|**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] (CU1) via la version actuelle.<br /><br /> Type de chiffreur utilisé : certificat ou clé asymétrique. Si la sauvegarde n'est pas chiffrée, cette valeur est NULL.|  
  
> [!NOTE]  
>  Si des mots de passe sont définis pour les jeux de sauvegarde, RESTORE HEADERONLY n'affiche que les informations complètes relatives au jeu de sauvegarde dont le mot de passe correspond à la définition de l'option PASSWORD de la commande. RESTORE HEADERONLY affiche également les informations complètes relatives aux jeux de sauvegarde non protégés. La colonne **BackupName** des autres jeux de sauvegarde du support protégés par mot de passe prend la valeur '**_Password Protected_**', et toutes les autres colonnes ont la valeur NULL.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Un client peut utiliser RESTORE HEADERONLY pour récupérer toutes les informations des en-têtes de sauvegarde pour toutes les sauvegardes figurant sur une unité particulière. Pour chaque sauvegarde réalisée sur une unité de sauvegarde, le serveur envoie les informations d'en-tête sous forme de ligne.  
  
## <a name="security"></a>Sécurité  
 Une opération de sauvegarde peut éventuellement spécifier des mots de passe pour un support de sauvegarde, un jeu de sauvegarde ou les deux. Lorsqu'un mot de passe a été défini sur un support de sauvegarde ou un jeu de sauvegarde, vous devez entrer le ou les mots de passe corrects dans l'instruction RESTORE. Ces mots de passe empêchent les opérations non autorisées de restauration et d’ajout de jeux de sauvegarde au support à l’aide d’outils [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En revanche, un mot de passe n'empêche pas d'écraser les supports en cas d'utilisation de l'option FORMAT de l'instruction BACKUP.  
  
> [!IMPORTANT]  
>  Le niveau de protection de ce mot de passe est faible. Son but est d'éviter que des utilisateurs autorisés ou non autorisés effectuent une restauration incorrecte à l'aide des outils [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En aucun cas, elle n'empêche la lecture des données de la sauvegarde par d'autres moyens ou le remplacement du mot de passe. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]La bonne pratique en matière de protection des sauvegardes consiste à stocker les bandes de sauvegarde dans un emplacement sûr ou à sauvegarder les fichiers disque protégés par une liste de contrôle d’accès (ACL). La liste de contrôle d'accès doit être définie à la racine du répertoire dans lequel les sauvegardes sont effectuées.  
  
### <a name="permissions"></a>Autorisations  
 Vous devez avoir l'autorisation CREATE DATABASE pour pouvoir obtenir des informations sur un jeu de sauvegardes ou sur une unité de sauvegarde. Pour plus d’informations, consultez [GRANT – octroi d’autorisations de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant renvoie les informations contenues dans l'en-tête pour le fichier de disque `C:\AdventureWorks-FullBackup.bak`.  
  
```sql 
RESTORE HEADERONLY   
FROM DISK = N'C:\AdventureWorks-FullBackup.bak';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Historique de sauvegarde et informations d’en-tête &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)   
 [Activer ou désactiver des sommes de contrôle de sauvegarde au cours d’opérations de sauvegarde ou de restauration &#40;SQL Server&#41;](../../relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server.md)   
 [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)  
  
  
