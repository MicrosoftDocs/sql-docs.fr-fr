---
title: Migrer des bases de données Access vers SQL Server Azure SQL Database | Microsoft Docs
description: Utilisez ce processus recommandé pour migrer des bases de données Access vers SQL Server ou Azure SQL Database à l’aide d’Assistant Migration SQL Server (SSMA).
ms.prod: sql
ms.custom: ''
ms.date: 08/15/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- instructions, migration
- migrating databases, overview
- overview, migration process
- procedure, migration
- recommended migration process
ms.assetid: 76a3abcf-2998-4712-9490-fe8d872c89ca
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 683c17a23b1eda96c040dabcf62d58b24fbb53ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066704"
---
# <a name="migrating-access-databases-to-sql-server---azure-sql-database-accesstosql"></a>Migration de bases de données Access vers des Azure SQL Database SQL Server (AccessToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration (SSMA) est un outil qui fournit un environnement complet qui vous permet de migrer rapidement des bases de données Access vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. En utilisant SSMA, vous pouvez passer en revue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les objets Access et ou Azure SQL Database, évaluer la base de données Access pour la migration, convertir les objets de base de données Access, les charger dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, puis migrer les données.  
  
## <a name="recommended-migration-process"></a>Processus de migration recommandé  
Pour migrer des objets et des données d’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, procédez comme suit :  
  
1.  [Créez un nouveau projet SSMA](creating-and-managing-projects-accesstosql.md). Après avoir créé le projet, vous pouvez [définir des options de projet](setting-conversion-and-migration-options-accesstosql.md), notamment des options de conversion, des options de migration et des mappages de types de données.  
  
2.  [Ajoutez des fichiers de base de données Access](adding-and-removing-access-database-files-accesstosql.md) au projet.  
  
    Vous pouvez ajouter des fichiers individuels, y compris des fichiers que vous trouvez sur l’ordinateur ou sur le réseau.  
  
3.  [Connectez-vous à l’instance cible de SQL Server](connecting-to-sql-server-accesstosql.md) ou [Connectez-vous à l’instance cible de SQL Azure](connecting-to-azure-sql-db-accesstosql.md).  
  
    Vous pouvez vous connecter à SQL Server ou SQL Azure.  
  
4.  Pour personnaliser le mappage entre une ou plusieurs bases de données Access et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure schémas,  [mappez les bases de données source et cible](mapping-source-and-target-databases-accesstosql.md).  
  
5.  Si vous le souhaitez, vous pouvez [créer un rapport d’évaluation](assessing-access-database-objects-for-conversion-accesstosql.md) pour déterminer si les objets de base de données Access peuvent être convertis en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL Azure ou non.  
  
6.  [Convertit des objets de base de données Access](converting-access-database-objects-accesstosql.md) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure des définitions d’objets.  
  
7.  [Chargez les objets de base de données convertis dans SQL Server](loading-converted-database-objects-into-sql-server-accesstosql.md).  
  
    Vous pouvez charger les objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure à l’aide de SSMA, ou vous pouvez enregistrer les [!INCLUDE[tsql](../../includes/tsql-md.md)] scripts.  
  
8.  [Migrez les données d’accès vers SQL Server](migrating-access-data-into-sql-server-azure-sql-db-accesstosql.md).  
  
    > [!NOTE]  
    > Vous pouvez convertir, charger et migrer des schémas et des données en une seule étape. Pour effectuer une migration en un clic, cliquez sur le bouton **convertir, charger et migrer** .  
  
9. Si vous souhaitez que vos applications Access utilisent les données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, utilisez [lier les tables Access aux tables SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md).  
  
Vous pouvez également utiliser l’Assistant Migration pour vous guider tout au long de ce processus. Pour plus d’informations, consultez [Assistant Migration](migration-wizard-accesstosql.md).  
  
## <a name="see-also"></a>Voir également  
[Prise en main avec Assistant Migration SQL Server pour l’accès](getting-started-with-sql-server-migration-assistant-for-access-accesstosql.md)  
[Préparation des bases de données Access pour la migration](preparing-access-databases-for-migration-accesstosql.md)
