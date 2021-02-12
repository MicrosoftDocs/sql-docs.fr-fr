---
title: Migrer des bases de données Sybase ASE vers SQL Server-Azure SQL Database | Microsoft Docs
description: Utilisez ce processus recommandé pour migrer des bases de données SAP Adaptive Server Enterprise vers SQL Server ou Azure SQL Database à l’aide d’Assistant Migration SQL Server (SSMA).
ms.custom: ''
ms.date: 11/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: ed7952d4-8331-44d7-bccf-3440e17238b2
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 222ce0d6d9a91a0aa97f9dc29dee06fe01754a2b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074561"
---
# <a name="migrating-sap-ase-databases-to-sql-server---azure-sql-database-sybasetosql"></a>Migration de bases de données SAP ASE vers SQL Server Azure SQL Database (SybaseToSQL)
Assistant Migration SQL Server (SSMA) pour SAP Adaptive Server Enterprise (ASE) est un environnement complet qui vous permet de migrer rapidement des bases de données SAP ASE vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. En utilisant SSMA pour SAP ASE, vous pouvez examiner les données et les objets de base de données, évaluer les bases de données pour la migration, migrer des objets de base de données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database, puis migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.  
  
## <a name="recommended-migration-process"></a>Processus de migration recommandé  
Pour migrer des objets et des données de bases de données SAP ASE vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database, procédez comme suit :  
  
1.  [Créez un nouveau projet SSMA](working-with-ssma-projects-sybasetosql.md).  
  
    Après avoir créé le projet, vous pouvez définir les options de conversion de projet, de migration et de mappage de type. Pour plus d’informations sur les paramètres de projet, consultez [définition des options de projet &#40;SybaseToSQL&#41;](../../ssma/sybase/setting-project-options-sybasetosql.md). Pour plus d’informations sur la personnalisation des mappages de types de données, consultez [mappage des types de données Sybase ASE et SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md).  
  
2.  [Connectez-vous au serveur de base de données SAP ASE](connecting-to-sybase-ase-sybasetosql.md).  
  
3.  [Connectez-vous à une instance SQL Server](connecting-to-sql-server-sybasetosql.md) ou [Connectez-vous à une instance de Azure SQL Database](connecting-to-azure-sql-db-sybasetosql.md).  
  
4.  [Mappez les schémas de base de données SAP ASE aux schémas de base de données SQL Server/Azure SQL Database](./mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql.md).  
  
5.  Vous pouvez également [créer des rapports d’évaluation](assessing-sybase-ase-database-objects-for-conversion-sybasetosql.md) pour évaluer les objets de base de données pour la conversion et estimer la durée de la conversion.  
  
6.  [Convertissez les schémas de base de données SAP ASE en schémas SQL Server/Azure SQL Database](./converting-sybase-ase-database-objects-sybasetosql.md).  
  
7.  [Chargez les objets de base de données convertis dans SQL Server/Azure SQL Database](./loading-converted-database-objects-into-sql-server-sybasetosql.md).  
  
    Vous pouvez enregistrer un script et l’exécuter dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database, ou synchroniser les objets de base de données.  
  
8.  [Migrez les données vers SQL Server/Azure SQL Database](./migrating-sybase-ase-data-into-sql-server-azure-sql-db-sybasetosql.md).  
  
9. Si nécessaire, mettez à jour vos applications de base de données.  
  
## <a name="see-also"></a>Voir également  
[Installation de SSMA pour SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-sybasetosql.md)  
[Prise en main avec SSMA pour SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/getting-started-with-ssma-for-sybase-sybasetosql.md)  
