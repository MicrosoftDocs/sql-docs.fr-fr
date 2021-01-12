---
description: sysopentapes (Transact-SQL)
title: sysopentapes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysopentapes
- sysopentapes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backup media [SQL Server], sysopentapes system table
- sysopentapes system table
ms.assetid: c066ca9b-9cfd-46b1-90a3-5c8dc9e7b6ae
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0cb05c0ac88f7f362651b872fa84598a656ff25d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096056"
---
# <a name="sysopentapes-transact-sql"></a>sysopentapes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque unité de bande actuellement ouverte. Cette vue est stockée dans la base de données **Master** .  
  
> [!IMPORTANT]  
>  Cette table système est incluse en tant que vue pour la compatibilité descendante. Au lieu de cela, utilisez l' [sys.dm_io_backup_tapes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md) vue de gestion dynamique.  
  
> [!NOTE]  
>  Vous ne pouvez pas supprimer la vue **sysopentapes** .  

  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**openTape**|**nvarchar (64)**|Nom de fichier physique de l'unité de bande ouverte. Pour plus d’informations sur l’ouverture et la libération de périphériques à bandes, consultez [BACKUP &#40;Transact-sql&#41;](../../t-sql/statements/backup-transact-sql.md) et [Restore &#40;transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).|  
  
## <a name="permissions"></a>Autorisations  
 L’utilisateur doit disposer de l’autorisation VIEW SERVER STATE sur le serveur.  
  
  
