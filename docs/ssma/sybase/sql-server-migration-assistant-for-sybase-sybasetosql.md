---
title: Assistant Migration SQL Server pour Sybase (SybaseToSQL) | Microsoft Docs
description: Découvrez SSMA pour Sybase et suivez les instructions pas à pas pour migrer des bases de données ASE vers SQL Server ou Azure SQL Database.
ms.custom: ''
ms.date: 10/10/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 59e63eac-8a7e-4d54-be1c-0633a9bf510d
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: d961301d94eb9c14bb4fc924cf826c769843483e
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596614"
---
# <a name="sql-server-migration-assistant-for-sybase-sybasetosql"></a>Assistant Migration SQL Server pour Sybase (SybaseToSQL)

[!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Assistant Migration (SSMA) pour Sybase Adaptive Server Enterprise (ASE) est un outil qui permet de migrer des bases de données ASE vers [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012, [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014, [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016, [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2017 sur Windows et Linux, [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2019 sur Windows et Linux, ou [!INCLUDE[msCoName](../../includes/msconame_md.md)] Azure SQL Database. SSMA pour Sybase convertit les objets de base de données ASE en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets de base de données, crée ces objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database, puis migre les données de ASE vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.
  
Cette documentation vous présente SSMA pour Sybase et fournit des instructions pas à pas pour migrer des bases de données ASE vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database et des informations sur les problèmes qui peuvent se produire après la migration. Pour en savoir plus, consultez les articles suivants.  
  
## <a name="contents"></a>Contenu  
  
|Section|Description|
|-----------|---------------|
|[Nouveautés de SSMA pour Sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/what-s-new-in-ssma-for-sybase-sybasetosql.md)|Répertorie les modifications apportées aux versions de SSMA.|  
|[Installation de SSMA pour Sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-sybasetosql.md)|Contient des articles qui fournissent des conditions préalables et des instructions pour l’installation du client SSMA pour Sybase et des composants requis sur l’ordinateur qui exécute l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance.|  
|[Prise en main avec SSMA pour Sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/getting-started-with-ssma-for-sybase-sybasetosql.md)|Présente l’interface utilisateur, les projets et les options de configuration.|  
|[Migration de bases de données Sybase ASE vers SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)|Fournit une vue d’ensemble du processus de conversion et des informations détaillées sur chaque étape du processus.|  
|[Référence de l’interface utilisateur &#40;SybaseToSQL&#41;](../../ssma/sybase/user-interface-reference-sybasetosql.md)|Contient la documentation pour les boîtes de dialogue SSMA pour Sybase.|  
|[Utilisation de la console SSMA pour Sybase](working-with-ssma-for-sybase-console-sybasetosql.md)|Contient la documentation sur l’application de console SSMA.|  
|[Obtention de SSMA pour l’assistance Sybase](../sql-server-migration-assistant.md)|Fournit des informations sur l’obtention d’une assistance supplémentaire.|