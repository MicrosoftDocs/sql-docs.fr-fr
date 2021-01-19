---
description: RESTORE Statements - VERIFYONLY (Transact-SQL)
title: RESTORE VERIFYONLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- VERIFYONLY
- RESTORE VERIFYONLY
- VERIFYONLY_TSQL
- RESTORE_VERIFYONLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE VERIFYONLY statement
- backups [SQL Server], verifying
- verifying backups
- checking backups
ms.assetid: cba3b6a0-b48e-4c94-812b-5b3cbb408bd6
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: eeae562c4cfbf093d3b7237a044c51084331e8a9
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172241"
---
# <a name="restore-statements---verifyonly-transact-sql"></a>RESTORE Statements - VERIFYONLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  Vérifie la sauvegarde sans la restaurer, et s'assure que le jeu de sauvegarde est complet et que tous les volumes sont lisibles. En revanche, l'instruction RESTORE VERIFYONLY ne vérifie pas la structure des données contenues dans les volumes de sauvegarde. Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l’instruction RESTORE VERIFYONLY a été améliorée afin de pouvoir effectuer un contrôle supplémentaire sur les données en vue d’accroître la probabilité de détection des erreurs. Le but est d'être le plus proche possible d'une opération de restauration réelle. Pour plus d'informations, consultez la section Notes.  

 Si la sauvegarde est valide, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] retourne un message de réussite.  
  
> [!NOTE]  
>  Pour une description des arguments, consultez [Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
RESTORE VERIFYONLY  
FROM <backup_device> [ ,...n ]  
[ WITH    
 {  
   LOADHISTORY   
  
--Restore Operation Option  
 | MOVE 'logical_file_name_in_backup' TO 'operating_system_file_name'   
          [ ,...n ]   
  
--Backup Set Options  
 | FILE = { backup_set_file_number | @backup_set_file_number }   
 | PASSWORD = { password | @password_variable }   
  
--Media Set Options  
 | MEDIANAME = { media_name | @media_name_variable }   
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }  
  
--Error Management Options  
 | { CHECKSUM | NO_CHECKSUM }   
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }  
  
--Monitoring Options  
 | STATS [ = percentage ]   
  
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
 Pour obtenir une description des arguments RESTORE VERIFYONLY, consultez [Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Le support de sauvegarde ou le jeu de sauvegarde doivent contenir un minimum d'informations correctes pour pouvoir être interprétés sous Microsoft Tape Format. Dans le cas contraire, RESTORE VERIFYONLY s'arrête et indique que le format de la sauvegarde est incorrect.  
  
 RESTORE VERIFYONLY se charge de vérifier :  
  
-   que le jeu de sauvegarde est complet et que tous les volumes sont lisibles ;  
  
-   certains champs d'en-tête des pages de la base de données, tels que l'ID de la page (comme si l'écriture des données allait avoir lieu) ;  
  
-   la somme de contrôle (si elle figure sur le support) ;  
  
-   s'il y a un espace suffisant sur les périphériques de destination.  
  
> [!NOTE]  
>  RESTORE VERIFYONLY ne fonctionne pas sur un instantané de base de données. Pour vérifier un instantané de base de données avant une opération de restauration, vous pouvez exécuter DBCC CHECKDB.  
  
> [!NOTE]  
>  Avec les sauvegardes d’instantanés, RESTORE VERIFYONLY vérifie l’existence des instantanés aux emplacements spécifiés dans le fichier de sauvegarde. Les sauvegardes d’instantanés sont une nouvelle fonctionnalité de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]. Pour plus d’informations sur la sauvegarde d’instantanés, consultez [Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
## <a name="security"></a>Sécurité  
 Une opération de sauvegarde peut éventuellement spécifier des mots de passe pour un support de sauvegarde, un jeu de sauvegarde ou les deux. Lorsqu'un mot de passe a été défini sur un support de sauvegarde ou un jeu de sauvegarde, vous devez entrer le ou les mots de passe corrects dans l'instruction RESTORE. Ces mots de passe empêchent les opérations non autorisées de restauration et d'ajouts de jeux de sauvegarde au support à l'aide d'outils [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En revanche, un mot de passe n'empêche pas d'écraser les supports en cas d'utilisation de l'option FORMAT de l'instruction BACKUP.  
  
> [!IMPORTANT]  
>  Le niveau de protection de ce mot de passe est faible. Son but est d'éviter que des utilisateurs autorisés ou non autorisés effectuent une restauration incorrecte à l'aide des outils [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En aucun cas, elle n'empêche la lecture des données de la sauvegarde par d'autres moyens ou le remplacement du mot de passe. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]La bonne pratique en matière de protection des sauvegardes consiste à stocker les bandes de sauvegarde dans un emplacement sûr ou à sauvegarder les fichiers disque protégés par une liste de contrôle d’accès (ACL). La liste de contrôle d'accès doit être définie à la racine du répertoire dans lequel les sauvegardes sont effectuées.  
  
### <a name="permissions"></a>Autorisations  
 Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], vous devez avoir l'autorisation CREATE DATABASE pour pouvoir obtenir des informations sur un jeu de sauvegardes ou sur une unité de sauvegarde. Pour plus d’informations, consultez [GRANT – octroi d’autorisations de base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
 
## <a name="examples"></a>Exemples  
 L’exemple suivant vérifie la sauvegarde à partir du disque.
  
```sql  
RESTORE VERIFYONLY FROM DISK = 'D:\AdventureWorks.bak';
GO
```  
  
## <a name="see-also"></a>Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Historique de sauvegarde et informations d’en-tête &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  
