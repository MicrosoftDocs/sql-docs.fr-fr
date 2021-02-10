---
title: Migration de bases de données Oracle vers SQL Server (OracleToSQL) | Microsoft Docs
description: Utilisez ce processus recommandé pour migrer des bases de données Oracle vers SQL Server ou Azure SQL Database à l’aide d’Assistant Migration SQL Server (SSMA).
ms.prod: sql
ms.custom: ''
ms.date: 04/22/2018
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 1d196dd6-4322-4c98-bb72-602c57d96134
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: df5bd55fb9c5490b8063712dbebe5328917e08b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063344"
---
# <a name="migrating-oracle-databases-to-sql-server-oracletosql"></a>Migration de bases de données Oracle vers SQL Server (OracleToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration (SSMA) pour Oracle est un environnement complet qui vous aide à migrer rapidement des bases de données Oracle vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Azure SQL Database ou Azure Synapse Analytics. En utilisant SSMA pour Oracle, vous pouvez examiner les données et les objets de base de données, évaluer les bases de données pour la migration, migrer des objets de base de données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Azure SQL Database ou Azure Synapse Analytics, puis migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Azure SQL Database ou Azure Synapse Analytics. Notez que vous ne pouvez pas migrer les schémas SYS et Oracle SYSTEM.
  
## <a name="recommended-migration-process"></a>Processus de migration recommandé  
Pour migrer des objets et des données de bases de données Oracle vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Azure SQL Database ou Azure Synapse Analytics, procédez comme suit :
  
1.  [Créez un nouveau projet SSMA](working-with-ssma-projects-oracletosql.md).  
  
    Après avoir créé le projet, vous pouvez définir les options de conversion de projet, de migration et de mappage de type. Pour plus d’informations sur les paramètres de projet, consultez [définition des options de projet &#40;OracleToSQL&#41;](../../ssma/oracle/setting-project-options-oracletosql.md). Pour plus d’informations sur la personnalisation des mappages de types de données, consultez [mappage des types de données Oracle et SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/mapping-oracle-and-sql-server-data-types-oracletosql.md).  
  
2.  [Connectez-vous au serveur de base de données Oracle](connecting-to-oracle-database-oracletosql.md).  
  
3.  [Connectez-vous à une instance de SQL Server](connecting-to-sql-server-oracletosql.md).  
  
4.  [Mappez les schémas de base de données Oracle aux schémas de base de données SQL Server](mapping-oracle-schemas-to-sql-server-schemas-oracletosql.md).  
  
5.  Vous pouvez également [créer des rapports d’évaluation](assessing-oracle-schemas-for-conversion-oracletosql.md) pour évaluer les objets de base de données pour la conversion et estimer la durée de la conversion.  
  
6.  [Convertit les schémas de base de données Oracle en schémas SQL Server](converting-oracle-schemas-oracletosql.md).  
  
7.  [Chargez les objets de base de données convertis dans SQL Server](loading-converted-database-objects-into-sql-server-oracletosql.md).  
  
    Vous pouvez le faire de l’une des manières suivantes :  
  
    -   Enregistrez un script et exécutez-le dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   Synchronisez les objets de base de données.  
  
8.  [Migrez les données vers SQL Server](migrating-oracle-data-into-sql-server-oracletosql.md).  
  
9. Si nécessaire, mettez à jour les applications de base de données.  
  
## <a name="see-also"></a>Voir aussi  
[Installation de SSMA pour Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/installing-ssma-for-oracle-oracletosql.md)  
[Prise en main avec SSMA pour Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/getting-started-with-ssma-for-oracle-oracletosql.md)  
  
