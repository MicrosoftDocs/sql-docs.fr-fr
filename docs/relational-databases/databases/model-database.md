---
description: model, base de données
title: model, base de données | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- template databases [SQL Server]
- model database [SQL Server], about model databases
- model database [SQL Server]
ms.assetid: 4e4f739b-fd27-4dce-8be6-3d808040d8d7
author: stevestein
ms.author: sstein
ms.openlocfilehash: 9ef6744fa152bfb5c66cc82ba71de62bc5bfb1a9
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421971"
---
# <a name="model-database"></a>model, base de données
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   La base de données **model** fait office de modèle pour toutes les bases de données créées sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Étant donné que la base de données **tempdb** est créée chaque fois que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est démarré, la base de données **model** doit toujours exister sur un système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Tout le contenu de la base de données **model** , y compris ses options, est copié dans la nouvelle base de données. Certains paramètres de **model** sont également utilisés pour la création d'une nouvelle base de données **tempdb** au démarrage, de sorte que la base de données **model** doit toujours exister sur un système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Les bases de données utilisateur récemment créées utilisent le même [mode de récupération](../../relational-databases/backup-restore/recovery-models-sql-server.md) que la base de données model. Le mode par défaut est configurable par l'utilisateur. Pour connaître le mode de récupération actuel du modèle, consultez [Afficher ou modifier le mode de récupération d’une base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md).  
  
> [!IMPORTANT]  
>  Si vous modifiez la base de données **model** avec des informations de modèle propres à l’utilisateur, nous vous recommandons de sauvegarder **model**. Pour plus d’informations, consultez [Sauvegarder et restaurer des bases de données système &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md).  
  
## <a name="model-usage"></a>Utilisation de la base de données model  
 Lorsqu'une instruction CREATE DATABASE est émise, le système crée la première partie de la base de données en copiant le contenu de la base de données **model** . Le reste de la nouvelle base de données est ensuite rempli de pages vides.  
  
 Si vous modifiez la base de données **model** , toutes les bases de données créées ultérieurement héritent des modifications apportées. Par exemple, vous pouvez définir des autorisations ou des options de base de données, ou bien ajouter des objets tels que des tables, des fonctions ou des procédures stockées. Les propriétés de fichier de la base de données **model** sont une exception et sont ignorées, à l'exception de la taille initiale du fichier de données. La taille initiale par défaut des fichiers journaux et des fichiers de données de base de données model est de 8 Mo.  
  
## <a name="physical-properties-of-model"></a>Propriétés physiques de la base de données model  
 Le tableau ci-dessous répertorie les valeurs de configuration initiales des fichiers journaux et de données **model** .  
  
|Fichier|Nom logique|Nom physique|Croissance du fichier|  
|----------|------------------|-------------------|-----------------|  
|Données primaires|modeldev|model.mdf|Croissance automatique de 64 Mo jusqu’à saturation du disque.|  
|Journal|modellog|modellog.ldf|Croissance automatique de 64 Mo jusqu’à un maximum de 2 téraoctets.|  

En ce qui concerne SQL Server 2014, consultez [Base de données model](/previous-versions/sql/2014/relational-databases/databases/model-database?view=sql-server-2014&preserve-view=true) pour connaître les valeurs de croissance de fichier par défaut.  

 Pour déplacer la base de données **model** ou les fichiers journaux, consultez [Déplacer des bases de données système](../../relational-databases/databases/move-system-databases.md).  
  
### <a name="database-options"></a>Options de base de données  
 Le tableau ci-dessous indique la valeur par défaut de chaque option de la base de données **model** et précise si cette option est modifiable. Pour afficher les valeurs actuelles de ces options, utilisez l'affichage catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) .  
  
|Option de base de données|Valeur par défaut|Peut être modifiée|  
|---------------------|-------------------|---------------------|  
|ALLOW_SNAPSHOT_ISOLATION|OFF|Oui|  
|ANSI_NULL_DEFAULT|OFF|Oui|  
|ANSI_NULLS|OFF|Oui|  
|ANSI_PADDING|OFF|Oui|  
|ANSI_WARNINGS|OFF|Oui|  
|ARITHABORT|OFF|Oui|  
|AUTO_CLOSE|OFF|Oui|  
|AUTO_CREATE_STATISTICS|ACTIVÉ|Oui|  
|AUTO_SHRINK|OFF|Oui|  
|AUTO_UPDATE_STATISTICS|ACTIVÉ|Oui|  
|AUTO_UPDATE_STATISTICS_ASYNC|OFF|Oui|  
|CHANGE_TRACKING|OFF|Non|  
|CONCAT_NULL_YIELDS_NULL|OFF|Oui|  
|CURSOR_CLOSE_ON_COMMIT|OFF|Oui|  
|CURSOR_DEFAULT|GLOBAL|Oui|  
|Options de disponibilité de base de données|ONLINE<br /><br /> MULTI_USER<br /><br /> READ_WRITE|Non<br /><br /> Oui<br /><br /> Oui|  
|DATE_CORRELATION_OPTIMIZATION|OFF|Oui|  
|DB_CHAINING|OFF|Non|  
|ENCRYPTION|OFF|Non|  
|MIXED_PAGE_ALLOCATION|ACTIVÉ|Non|  
|NUMERIC_ROUNDABORT|OFF|Oui|  
|PAGE_VERIFY|CHECKSUM|Oui|  
|PARAMETERIZATION|SIMPLE|Oui|  
|QUOTED_IDENTIFIER|OFF|Oui|  
|READ_COMMITTED_SNAPSHOT|OFF|Oui|  
|RECOVERY|Dépend de l’édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *|Oui|  
|RECURSIVE_TRIGGERS|OFF|Oui|  
|Options de Service Broker|DISABLE_BROKER|Non|  
|TRUSTWORTHY|OFF|Non|  
  
 *Pour vérifier le mode de récupération actuel de la base de données, consultez [Afficher ou modifier le mode de récupération d’une base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md) ou [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
 Pour obtenir une description de ces options de base de données, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
## <a name="restrictions"></a>Restrictions  
 Les opérations suivantes ne peuvent pas être effectuées sur la base de données **model** :  
  
- ajout de groupes de fichiers ou de fichiers ;  
- Modification du classement. Le classement par défaut est le classement du serveur.  
- Modification du propriétaire de la base de données. La base de données **model** appartient à **sa**.  
- Suppression de la base de données  
- Suppression de l'utilisateur **Invité** de la base de données  
- Activation de la capture des données modifiées.  
- Participation à la mise en miroir de bases de données  
- Suppression du groupe de fichiers primaire, du fichier de données primaire ou du fichier journal  
- Changement du nom de la base de données ou du groupe de fichiers primaire  
- Affectation de la valeur OFFLINE à la base de données.  
- Affectation de la valeur READ_ONLY au groupe de fichiers primaire.  
- Création de procédures, de vues ou de déclencheurs à l'aide de l'option WITH ENCRYPTION. La clé de chiffrement est liée à la base de données dans laquelle l'objet est créé. Les objets chiffrés créés dans la base de données **model** peuvent être utilisés uniquement dans **model**.  
  
## <a name="related-content"></a>Contenu associé  
- [Bases de données système](../../relational-databases/databases/system-databases.md)  
- [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
- [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
- [Déplacer des fichiers de bases de données](../../relational-databases/databases/move-database-files.md)  
  
  
