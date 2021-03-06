---
description: Affichages catalogue du témoin de mise en miroir de bases de données-sys.database_mirroring_witnesses
title: sys.database_mirroring_witnesses (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.database_mirroring_witnesses
- sys.database_mirroring_witnesses_TSQL
- database_mirroring_witnesses
- database_mirroring_witnesses_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], catalog views
- sys.database_mirroring_witnesses catalog view
- witness [SQL Server], sys.database_mirroring_witnesses catalog view
ms.assetid: 0dd5b794-733b-4a3c-b5a4-62f9f1f0f22d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 63599e8dfbeafc6136e2d4904a546e5ebbd5dd96
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465097"
---
# <a name="database-mirroring-witness-catalog-views---sysdatabase_mirroring_witnesses"></a>Affichages catalogue du témoin de mise en miroir de bases de données-sys.database_mirroring_witnesses
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque rôle de témoin qu'un serveur utilise dans un partenariat de mise en miroir de base de données. 
  
  Dans une session de mise en miroir de bases de données, le basculement automatique nécessite un serveur témoin. Dans l'idéal, le témoin réside sur un ordinateur distinct du serveur miroir et du principal. Il ne sert pas la base de données, mais contrôle l'état du principal et du serveur miroir. En cas de défaillance du serveur principal, le témoin peut lancer un basculement automatique vers le serveur miroir. 
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|Nom des deux copies de la base de données dans la session de mise en miroir de la base de données.|  
|**principal_server_name**|**sysname**|Nom du serveur partenaire dont la copie de la base de données est actuellement la base de données principale.|  
|**mirror_server_name**|**sysname**|Nom du serveur partenaire dont la copie de la base de données est actuellement la base de données miroir.|  
|**safety_level**|**tinyint**|Paramètre de sécurité de transaction pour les mises à jour sur la base de données miroir :<br /><br /> 0 = État inconnu<br /><br /> 1 = désactivée (asynchrone)<br /><br /> 2 = totale (synchrone)<br /><br /> L'utilisation d'un témoin pour le basculement automatique nécessite une sécurité de transaction totale, ce qui est la valeur par défaut.|  
|**safety_level_desc**|**nvarchar(60)**|Description de la garantie de la sécurité des mises à jour sur la base de données miroir :<br /><br /> UNKNOWN<br /><br /> OFF<br /><br /> FULL|  
|**safety_sequence_number**|**int**|Mettez à jour le numéro de séquence des modifications apportées à **safety_level**.|  
|**role_sequence_number**|**int**|Numéro de séquence de mise à jour pour les modifications apportées aux rôles principaux/miroirs utilisés par les partenaires de la mise en miroir.|  
|**mirroring_guid**|**uniqueidentifier**|Identificateur du partenariat de mise en miroir.|  
|**family_guid**|**uniqueidentifier**|Identificateur de la famille de sauvegarde pour la base de données. Sert à détecter les états de restauration concordants.|  
|**is_suspended**|**bit**|La mise en miroir de la base de données est suspendue.|  
|**is_suspended_sequence_number**|**int**|Numéro de séquence pour la définition de **is_suspended**.|  
|**partner_sync_state**|**tinyint**|État de synchronisation de la session de mise en miroir :<br /><br /> 5 = les partenaires sont synchronisés. Le basculement est éventuellement possible. Pour plus d’informations sur la configuration requise pour le basculement, consultez [basculement de rôle durant une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).<br /><br /> 6 = les partenaires ne sont pas synchronisés. Le basculement n'est maintenant pas possible.|  
|**partner_sync_state_desc**|**nvarchar(60)**|Description de l'état de synchronisation de la session de mise en miroir :<br /><br /> SYNCHRONIZED<br /><br /> UNSYNCHRONIZED|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Témoin de mise en miroir de base de données](../../database-engine/database-mirroring/database-mirroring-witness.md)   
 [sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md)   
 [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)   
 [Questions fréquentes sur l'interrogation des catalogues système de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
