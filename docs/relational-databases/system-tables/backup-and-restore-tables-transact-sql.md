---
description: Sauvegarder et restaurer des tables (Transact-SQL)
title: Sauvegarder et restaurer des tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- system tables [SQL Server], backup tables
- backup system tables [SQL Server]
- system tables [SQL Server], restore tables
- restore system tables [SQL Server]
ms.assetid: aa615add-54e6-40f5-8b55-3728b26884ee
author: markingmyname
ms.author: maghan
ms.openlocfilehash: abb69d6cefa270330a41d90bea0352981dd7ee8a
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89540978"
---
# <a name="backup-and-restore-tables-transact-sql"></a>Sauvegarder et restaurer des tables (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Les rubriques de cette section détaillent les tables système qui stockent les informations utilisées par les procédures de sauvegarde et de restauration des bases de données.  
  
## <a name="in-this-section"></a>Dans cette section  
 [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md)  
 Contient une ligne pour chaque fichier de données ou fichier journal d'une base de données.  
  
 [backupfilegroup](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)  
 Contient une ligne pour chaque groupe de fichiers d'une base de données au moment de la sauvegarde.  
  
 [backupmediafamily](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)  
 Contient une ligne pour chaque famille de supports de sauvegarde.  
  
 [backupmediaset](../../relational-databases/system-tables/backupmediaset-transact-sql.md)  
 Contient une ligne pour chaque support de sauvegarde.  
  
 [backupset](../../relational-databases/system-tables/backupset-transact-sql.md)  
 Contient une ligne pour chaque jeu de sauvegarde.  
  
 [logmarkhistory](../../relational-databases/system-tables/logmarkhistory-transact-sql.md)  
 Contient une ligne par transaction marquée validée.  
  
 [restorefile](../../relational-databases/system-tables/restorefile-transact-sql.md)  
 Contient une ligne pour chaque fichier restauré. Les fichiers restaurés indirectement par nom de groupe de fichiers sont compris.  
  
 [restorefilegroup](../../relational-databases/system-tables/restorefilegroup-transact-sql.md)  
 Contient une ligne par groupe de fichiers restaurés.  
  
 [restorehistory](../../relational-databases/system-tables/restorehistory-transact-sql.md)  
 Contient une ligne par opération de restauration.  
  
 [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md)  
 Contient une ligne par page ayant échoué avec une erreur 824 (avec une limite de 1 000 lignes).  
  
 [sysopentapes](../../relational-databases/system-tables/sysopentapes-transact-sql.md)  
 Contient une ligne pour chaque unité de bande actuellement ouverte.  
  
  
