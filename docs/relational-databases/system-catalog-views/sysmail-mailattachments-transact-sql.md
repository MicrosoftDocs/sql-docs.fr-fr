---
description: sysmail_mailattachments (Transact-SQL)
title: sysmail_mailattachments (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_mailattachments_TSQL
- sysmail_mailattachments
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_mailattachments database mail view
ms.assetid: aee87059-a4c1-459a-a95c-641b4e3f0e73
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: adffc878cb36a7719748f8e5214bb23dab16fee2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199700"
---
# <a name="sysmail_mailattachments-transact-sql"></a>sysmail_mailattachments (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque pièce jointe soumise à la messagerie de base de données. Utilisez cette vue lorsque vous voulez des informations sur les pièces jointes de la messagerie de base de données. Pour passer en revue tous les courriers électroniques traités par Database Mail utilisez [sysmail_allitems &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**attachment_id**|**int**|Identificateur de la pièce jointe.|  
|**mailitem_id**|**int**|Identificateur de l'élément de messagerie qui contenait la pièce jointe.|  
|**extension**|**nvarchar (520)**|Nom de fichier de la pièce jointe. Lorsque **attach_query_result** a la valeur 1 et que **query_attachment_filename** a la valeur null, Database mail crée un nom de fichier arbitraire.|  
|**taille**|**int**|Taille de la pièce jointe en octets.|  
|**position**|**varbinary(max)**|Contenu de la pièce jointe.|  
|**last_mod_date**|**datetime**|Date et heure de la dernière modification de la ligne.|  
|**last_mod_user**|**sysname**|Dernier utilisateur qui a modifié la ligne.|  
  
## <a name="remarks"></a>Notes  
 En cas de résolution des problèmes de la messagerie de base de données, utilisez cette vue pour voir les propriétés des pièces jointes.  
  
 Les pièces jointes stockées dans les tables système peuvent entraîner une augmentation de la taille de la base de données **msdb** . Utilisez **sysmail_delete_mailitems_sp** pour supprimer des éléments de messagerie et leurs pièces jointes associées. Pour plus d’informations, consultez [créer un travail de SQL Server Agent pour archiver les Messages Database mail et les journaux des événements](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md).  
  
## <a name="permissions"></a>Autorisations  
 Accordé au rôle serveur fixe **sysadmin** et au rôle de base de données **DatabaseMailUserRole** . Lorsqu’elle est exécutée par un membre du rôle serveur fixe **sysadmin** , cette vue affiche toutes les pièces jointes. Les autres utilisateurs voient uniquement les pièces jointes des messages qu'ils ont essayé d'envoyer.  
  
## <a name="see-also"></a>Voir aussi  
 [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md)   
 [sysmail_faileditems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md)   
 [sysmail_sentitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md)   
 [sysmail_unsentitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md)   
 [sysmail_event_log &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md)  
  
  
