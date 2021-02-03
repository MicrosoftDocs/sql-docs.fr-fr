---
description: MSSQLSERVER_2527
title: MSSQLSERVER_2527 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2527 (Database Engine error)
ms.assetid: 1cef90ef-9c39-44e6-bc7f-316c8f53c10c
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1727d825bd3dac9df07824e5e26f58d6cc4ec625
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188941"
---
# <a name="mssqlserver_2527"></a>MSSQLSERVER_2527
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|2527|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBCC_INDEX_FILEGROUP_IS_OFFLINE|  
|Texte du message|Impossible de traiter l'index I_NAME de la table O_NAME car le groupe de fichiers F_NAME est hors connexion.|  
  
## <a name="explanation"></a>Explication  
Ce message d'information indique qu'il est impossible de vérifier l'index, car l'un des groupes de fichiers qui stocke les données de l'index n'est pas connecté. L'état des fichiers dans un groupe de fichiers détermine la disponibilité du groupe de fichiers tout entier. Pour qu'un groupe de fichiers soit disponible, tous ses fichiers doivent être en ligne. S’il n’y a pas d’autre problème, tous les autres index du même objet seront vérifiés.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Pour afficher l’état des fichiers du groupe de fichiers spécifié, interrogez l’affichage catalogue **sys.database_files** ou **sys.master_files**.  
  
Restaurez le fichier hors connexion à partir d'une sauvegarde.  
  
## <a name="see-also"></a>Voir aussi  
[sys.database_files &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
[sys.master_files &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
[RESTORE &#40;Transact-SQL&#41;](~/t-sql/statements/restore-statements-transact-sql.md)  
  
