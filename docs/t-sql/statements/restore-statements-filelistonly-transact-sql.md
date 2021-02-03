---
description: Instructions RESTORE - REWINDONLY (Transact-SQL)
title: RESTORE FILELISTONLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RESTORE FILELISTONLY
- RESTORE_FILELISTONLY_TSQL
- FILELISTONLY
- FILELISTONLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backups [SQL Server], file lists
- RESTORE FILELISTONLY statement
- listing backed up files
ms.assetid: 0b4b4d11-eb9d-4f3e-9629-6c79cec7a81a
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 7a648b81b0c29799de837e76e6b745b4d460f61c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159327"
---
# <a name="restore-statements---filelistonly-transact-sql"></a>Instructions RESTORE - REWINDONLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]


  Retourne un jeu de résultats avec une liste des fichiers journaux et des fichiers de base de données contenus dans le jeu de sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

> [!NOTE]  
>  Pour une description des arguments, consultez [Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
RESTORE FILELISTONLY   
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
 Pour voir la description des arguments RESTORE FILELISTONLY, consultez [Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
## <a name="result-sets"></a>Jeux de résultats  
 Un client peut utiliser RESTORE FILELISTONLY pour obtenir une liste des fichiers contenus dans un jeu de sauvegarde. Ces informations sont retournées sous forme d'un jeu de résultats contenant une ligne par fichier.  
  
|Nom de la colonne|Type de données|Description|  
|-|-|-|  
|LogicalName|**nvarchar(128)**|Nom logique du fichier.|  
|PhysicalName|**nvarchar(260)**|Nom physique ou nom système du fichier.|  
|Type|**char(1)**|Type de fichier (l’un des suivants) :<br /><br /> **L** = Fichier journal Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> **D** = Fichier de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> **F** = Catalogue de texte intégral<br /><br /> **S** = FileStream, FileTable ou conteneur [!INCLUDE[hek_2](../../includes/hek-2-md.md)]|  
|FileGroupName|**nvarchar(128)** NULL|Nom du groupe de fichiers contenant le fichier.|  
|Taille|**numeric(20,0)**|Taille actuelle en octets.|  
|MaxSize|**numeric(20,0)**|Taille maximale autorisée en octets.|  
|FileID|**bigint**|Identificateur de fichier, unique dans la base de données.|  
|CreateLSN|**numeric(25,0)**|Numéro séquentiel dans le journal auquel le fichier a été créé.|  
|DropLSN|**numeric(25,0)** NULL|Numéro séquentiel dans le journal auquel le fichier a été supprimé. Si le fichier n'a pas été supprimé, cette valeur est NULL.|  
|UniqueID|**uniqueidentifier**|Identificateur global unique (GUID) du fichier.|  
|ReadOnlyLSN|**numeric(25,0) NULL**|Numéro séquentiel dans le journal auquel le groupe de fichiers contenant le fichier est passé de lecture-écriture à lecture seule (modification la plus récente).|  
|ReadWriteLSN|**numeric(25,0)** NULL|Numéro séquentiel dans le journal auquel le groupe de fichiers contenant le fichier est passé de lecture seule à lecture-écriture (modification la plus récente).|  
|BackupSizeInBytes|**bigint**|Taille de la sauvegarde en octets pour ce fichier.|  
|SourceBlockSize|**int**|Taille en octets des blocs du périphérique physique contenant le fichier (pas l'unité de sauvegarde).|  
|FileGroupID|**int**|Identificateur du groupe de fichiers.|  
|LogGroupGUID|**uniqueidentifier** NULL|NULL.|  
|DifferentialBaseLSN|**numeric(25,0)** NULL|Pour des sauvegardes différentielles, les modifications avec des numéros séquentiels dans le journal supérieurs ou égaux à **DifferentialBaseLSN** sont incluses.<br /><br /> Pour les autres types de sauvegarde, la valeur est NULL.|  
|DifferentialBaseGUID|**uniqueidentifier** NULL|Pour les sauvegardes différentielles, il s'agit de l'identificateur unique de la base différentielle.<br /><br /> Pour les autres types de sauvegarde, la valeur est NULL.|  
|IsReadOnly|**bit**|**1** = Le fichier est en lecture seule.|  
|IsPresent|**bit**|**1** = Le fichier est présent dans la sauvegarde.|  
|TDEThumbprint|**varbinary(32)** NULL|Affiche l'empreinte numérique de la clé de chiffrement de la base de données. L'empreinte numérique de chiffrement est un hachage SHA-1 du certificat avec lequel la clé est chiffrée. Pour plus d’informations sur le chiffrement des bases de données, consultez [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).|  
|SnapshotURL|**nvarchar(360)** NULL|URL de l’instantané Azure du fichier de base de données contenu dans la sauvegarde FILE_SNAPSHOT. Retourne NULL en cas d’absence de sauvegarde FILE_SNAPSHOT.|  
  
## <a name="security"></a>Sécurité  
 Une opération de sauvegarde peut éventuellement spécifier des mots de passe pour un support de sauvegarde, un jeu de sauvegarde ou les deux. Lorsqu'un mot de passe a été défini sur un support de sauvegarde ou un jeu de sauvegarde, vous devez entrer le ou les mots de passe corrects dans l'instruction RESTORE. Ces mots de passe empêchent les opérations non autorisées de restauration et d’ajout de jeux de sauvegarde au support à l’aide d’outils [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En revanche, un mot de passe n'empêche pas d'écraser les supports en cas d'utilisation de l'option FORMAT de l'instruction BACKUP.  
  
> [!IMPORTANT]  
>  Le niveau de protection de ce mot de passe est faible. Son but est d'éviter que des utilisateurs autorisés ou non autorisés effectuent une restauration incorrecte à l'aide des outils [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En aucun cas, elle n'empêche la lecture des données de la sauvegarde par d'autres moyens ou le remplacement du mot de passe. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] La méthode conseillé en matière de protection des sauvegardes consiste à stocker les bandes de sauvegarde dans un emplacement sûr ou à sauvegarder les fichiers disque protégés par une liste de contrôle d'accès (ACL). La liste de contrôle d'accès doit être définie à la racine du répertoire dans lequel les sauvegardes sont effectuées.  
  
### <a name="permissions"></a>Autorisations  
 Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], vous devez avoir l'autorisation CREATE DATABASE pour pouvoir obtenir des informations sur un jeu de sauvegardes ou sur une unité de sauvegarde. Pour plus d’informations, consultez [GRANT – octroi d’autorisations de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne les informations d'une unité de sauvegarde nommée `AdventureWorksBackups`. L'exemple utilise l'option `FILE` pour spécifier le deuxième jeu de sauvegarde sur l'unité.  
  
```sql  
RESTORE FILELISTONLY FROM AdventureWorksBackups   
   WITH FILE=2;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Historique de sauvegarde et informations d’en-tête &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  
