---
description: RESTORE Statements - REWINDONLY (Transact-SQL)
title: RESTORE REWINDONLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RESTORE_REWINDONLY_TSQL
- RESTORE REWINDONLY
dev_langs:
- TSQL
helpviewer_keywords:
- closing backup devices
- backup devices [SQL Server], rewinding
- media [SQL Server]
- open back devices
- rewinding backup devices
- RESTORE REWINDONLY statement
ms.assetid: 7f825b40-2264-4608-9809-590d0f09d882
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: fc5e119a9f3bad173ca72dc7b2905a82481fa1ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186614"
---
# <a name="restore-statements---rewindonly-transact-sql"></a>RESTORE Statements - REWINDONLY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Rembobine et ferme les périphériques à bandes laissés ouverts par l'instruction BACKUP ou RESTORE exécutée avec l'option NOREWIND. Cette commande n'est prise en charge que dans le cas de périphériques à bandes.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
RESTORE REWINDONLY   
FROM <backup_device> [ ,...n ]  
[ WITH {UNLOAD | NOUNLOAD}]  
}   
[;]  
  
<backup_device> ::=  
{   
   { logical_backup_device_name |  
      @logical_backup_device_name_var }  
   | TAPE = { 'physical_backup_device_name' |  
       @physical_backup_device_name_var }   
}   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 **\<backup_device> ::=** 
  
 Spécifie les unités de sauvegarde logiques ou physiques à utiliser pour l'opération de restauration.  
  
 { *logical_backup_device_name* |  **@** _logical\_backup\_device\_name\_var_ }  
 Nom logique, qui doit respecter les règles applicables aux identificateurs, de l'unité de sauvegarde créée par **sp_addumpdevice** et à partir de laquelle est restaurée la base de données. S’il est fourni comme variable ( **@** _logical\_backup\_device\_name\_var_), le nom de l’unité de sauvegarde peut être spécifié sous la forme d’une constante de chaîne ( **@** _logical\_backup\_device\_name\_var_ = _logical\_backup\_device\_name_) ou d’une variable de type de données chaîne de caractères, sauf pour les types de données **ntext** ou **text**.  
  
 {DISK | TAPE } **=** { **'** _physical\_backup\_device\_name_ **'**  |  **@** _physical\_backup\_device\_name\_var_ }  
 Permet la restauration de sauvegardes à partir de l'unité de disque ou de bande spécifiée. Les types d’unité DISK et TAPE doivent être spécifiés avec le nom réel de l’unité (par exemple, le chemin complet et le nom de fichier) : DISK = 'C:\Program Files\Microsoft SQL Server\MSSQL\BACKUP\Mybackup.bak' ou TAPE = '\\\\.\TAPE0'. S’il est spécifié comme variable ( **@** _physical\_backup\_device\_name\_var_), le nom de l’unité de sauvegarde peut être spécifié sous la forme d’une constante de chaîne ( **@** _physical\_backup\_device\_name\_var_ = '*physcial_backup_device_name*') ou d’une variable de type de données chaîne de caractères, sauf pour les types de données **ntext** ou **text**.  
  
 Si vous utilisez un serveur réseau pourvu d'un nom UNC (qui doit contenir le nom de l'ordinateur), spécifiez le type d'unité DISK. Pour plus d’informations sur l’utilisation de noms UNC, consultez [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
 Le compte sous lequel vous exécutez Microsoft SQL Server doit disposer d’un accès en lecture (READ) au serveur réseau ou à l’ordinateur distant pour effectuer une opération RESTORE.  
  
 *n*  
 Espace réservé qui permet de spécifier plusieurs unités de sauvegarde et des unités de sauvegarde logiques. Le nombre maximal d’unités de sauvegarde permanentes ou logiques est **64**.  
  
 Le nombre d'unités de sauvegarde nécessaires varie si la restauration est hors ligne ou en ligne et devra peut-être correspondre au nombre utilisé pour créer le support de sauvegarde auquel les sauvegardes appartiennent. La restauration hors ligne permet de restaurer une sauvegarde en utilisant moins d'unités que le nombre nécessaire pour créer la sauvegarde. La restauration en ligne nécessite toutes les unités de sauvegarde. Toute tentative avec un nombre d'unités inférieur échoue.  
  
 Pour plus d’informations, consultez [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
> [!NOTE]  
>  Si vous restaurez une sauvegarde à partir d'un support de sauvegarde miroir, vous ne pouvez spécifier qu'un seul miroir pour chaque famille de supports. Néanmoins, en cas d'erreurs, les autres miroirs permettent de résoudre certains problèmes liés à la restauration rapidement. Vous pouvez remplacer un volume de supports endommagé par le volume correspondant d'un autre miroir. En ce qui concerne les restaurations hors ligne, vous pouvez restaurer un nombre de supports inférieur au nombre de familles de supports, mais chaque famille n'est traitée qu'une seule fois.  
  
 **WITH Options**  
  
 UNLOAD  
 Indique que la bande est automatiquement rembobinée et démontée lorsque la restauration est terminée. UNLOAD est définie par défaut lorsqu'une nouvelle session utilisateur démarre. Elle reste valide jusqu'à ce que NOUNLOAD soit spécifiée. Cette option n'est utilisée que dans le cas d'unités de bande. Si une unité autre qu'un lecteur de bandes est utilisée pour la restauration, cette option est ignorée.  
  
 NOUNLOAD  
 Indique que la bande ne sera pas démontée automatiquement du lecteur de bande après une restauration. NOUNLOAD reste valide jusqu'à ce que UNLOAD soit spécifiée.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 RESTORE REWINDONLY est une alternative à RESTORE LABELONLY FROM TAPE = \<name> WITH REWIND. Vous pouvez obtenir la liste des lecteurs de bande ouverts dans la vue de gestion dynamique [sys.dm_io_backup_tapes](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md).  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Tout utilisateur peut utiliser RESTORE REWINDONLY.  
  
## <a name="see-also"></a>Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Historique de sauvegarde et informations d’en-tête &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  

