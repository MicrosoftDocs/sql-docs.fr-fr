---
description: sys.sysdatabases (Transact-SQL)
title: Bases de données sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysdatabases_TSQL
- sys.sysdatabases_TSQL
- sys.sysdatabases
- sysdatabases
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysdatabases compatibility view
- sysdatabases system table
ms.assetid: 60a93880-62f1-4eda-a886-f046706ba90c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f742ce25d05d588cb70e9d68b254e06f8e478500
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201414"
---
# <a name="syssysdatabases-transact-sql"></a>sys.sysdatabases (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Contient une ligne pour chaque base de données dans une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé pour la première fois, **sysdatabases** contient des entrées pour les bases de données **Master**, **Model**, **msdb** et **tempdb** .  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom de la base de données|  
|**dbid**|**smallint**|ID de base de données|  
|**sid**|**varbinary(85)**|ID système du créateur de la base de données.|  
|**mode**|**smallint**|Champ utilisé de manière interne pour verrouiller une base de données pendant sa création.|  
|**statut**|**int**|Bits d’État, dont certains peuvent être définis à l’aide [de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) comme indiqué :<br /><br /> 1 = **fermeture** de la base de données (ALTER DATABASE)<br /><br /> 4 = **select into/bulkcopy** (ALTER DATABASE à l’aide de Set Recovery)<br /><br /> 8 = **trunc. log on chkpt** (ALTER DATABASE à l’aide de Set Recovery)<br /><br /> 16 = **détection de page endommagée** (ALTER DATABASE)<br /><br /> 32 = **chargement** en cours<br /><br /> 64 = **prérécupération**<br /><br /> 128 = **récupération**<br /><br /> 256 = **non récupéré**<br /><br /> 512 = **hors connexion** (ALTER DATABASE)<br /><br /> 1024 = **lecture seule** (ALTER DATABASE)<br /><br /> 2048 = **utilisation dbo uniquement** (ALTER DATABASE à l’aide de Set RESTRICTED_USER)<br /><br /> 4096 = **utilisateur unique** (ALTER DATABASE)<br /><br /> 32768 = **mode urgence**<br /><br /> 65536 = **checksum** (ALTER DATABASE)<br /><br /> 4194304 = **réduction** automatique (ALTER DATABASE)<br /><br /> 1073741824 = **arrêt correct**<br /><br /> Plusieurs bits peuvent être activés à la fois.|  
|**status2**|**int**|16384 = valeur **par défaut ANSI null** (ALTER DATABASE)<br /><br /> 65536 = **concat null donne NULL** (ALTER DATABASE)<br /><br /> 131072 = **déclencheurs récursifs** (ALTER DATABASE)<br /><br /> 1048576 = **curseur local par défaut** (ALTER DATABASE)<br /><br /> 8388608 = **identificateur entre guillemets** (ALTER DATABASE)<br /><br /> 33554432 = **fermeture du curseur lors de la validation** (ALTER DATABASE)<br /><br /> 67108864 = **valeurs ANSI null** (ALTER DATABASE)<br /><br /> 268435456 = **avertissements ANSI** (ALTER DATABASE)<br /><br /> 536870912 = **activation du texte intégral** (définie à l’aide de **sp_fulltext_database**)|  
|**crdate**|**datetime**|Date de création|  
|**réservé**|**datetime**|Réservé pour un usage futur.|  
|**category**|**int**|Contient un bitmap des informations utilisées pour la réplication :<br /><br /> 1 = Publié pour la réplication transactionnelle et d'instantané.<br /><br /> 2 = Abonné à une publication transactionnelle ou d'instantané.<br /><br /> 4 = Publié pour une réplication de fusion.<br /><br /> 8 = Abonné à une publication de fusion.<br /><br /> 16 = Base de données de distribution.|  
|**cmptlevel**|**tinyint**|Niveau de compatibilité pour la base de données. Pour plus d’informations, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).|  
|**extension**|**nvarchar(260)**|Nom et chemin d'accès du système d'exploitation pour le fichier primaire de la base de données.<br /><br /> le **nom de fichier** est visible pour **dbcreator**, **sysadmin**, le propriétaire de la base de données avec les autorisations CREATE ANY DATABASE ou les bénéficiaires qui ont l’une des autorisations suivantes : ALTER ANY DATABASE, CREATE ANY DATABASE, View any Definition. Pour retourner le chemin d’accès et le nom de fichier, interrogez l’affichage de compatibilité des [ fichierssys.sys](../../relational-databases/system-compatibility-views/sys-sysfiles-transact-sql.md) ou la vue [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) .|  
|**version**|**smallint**|Numéro de version interne du code [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec lequel la base de données a été créée. [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Mappage de tables système à des vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
