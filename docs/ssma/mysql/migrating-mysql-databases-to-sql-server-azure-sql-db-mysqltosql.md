---
title: Migrer des bases de données MySQL vers SQL Server Azure SQL Database | Microsoft Docs
description: Utilisez ce processus recommandé pour migrer des bases de données MySQL vers SQL Server ou Azure SQL Database à l’aide d’Assistant Migration SQL Server (SSMA).
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8006f9a0-394d-4238-8dc5-44255134628b
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2f4599389fd313447a926cfbc898735fc60f4823
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014649"
---
# <a name="migrating-mysql-databases-to-sql-server---azure-sql-database-mysqltosql"></a>Migration de bases de données MySQL vers des Azure SQL Database SQL Server (MySQLToSql)
Assistant Migration SQL Server (SSMA) pour MySQL est un environnement complet qui vous permet de migrer rapidement des bases de données MySQL vers SQL Server ou SQL Azure. En utilisant SSMA pour MySQL, vous pouvez examiner les données et les objets de base de données, évaluer les bases de données pour la migration, migrer des objets de base de données vers SQL Server ou SQL Azure, puis migrer des données vers SQL Server ou SQL Azure.  
  
## <a name="recommended-migration-process"></a>Processus de migration recommandé  
Pour migrer des objets et des données de bases de données MySQL vers SQL Server ou SQL Azure, procédez comme suit :  
  
1.  [Utilisation des projets SSMA &#40;&#41;MySQLToSQL ](../../ssma/mysql/working-with-ssma-projects-mysqltosql.md).  
  
    Après avoir créé le projet, vous pouvez définir les options de conversion de projet, de migration et de mappage de type. Pour plus d’informations sur les paramètres de projet, consultez [définition des options de projet &#40;MySQLToSQL&#41;](../../ssma/mysql/setting-project-options-mysqltosql.md). Pour plus d’informations sur la personnalisation des mappages de types de données, consultez [mappage des types de données MySQL et SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
  
2.  [Connexion à MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
3.  [Connexion à SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
4.  [Mappage de bases de données MySQL à des schémas de SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md)  
  
5.  [Connexion à Azure SQL Database &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md)  
  
6.  Vous pouvez également [évaluer les bases de données MySQL pour la conversion &#40;MySQLToSQL&#41;](../../ssma/mysql/assessing-mysql-databases-for-conversion-mysqltosql.md) pour évaluer les objets de base de données pour la conversion et estimer la durée de la conversion.  
  
7.  [Conversion de bases de données MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
8.  [Synchronisation](loading-converted-database-objects-into-sql-server-mysqltosql.md)  
  
9. Vous pouvez le faire de l’une des manières suivantes :  
  
    -   Enregistrez un script et exécutez-le sur SQL Server ou SQL Azure.  
  
    -   Synchronisez les objets de base de données.  
  
10. [Migration de données MySQL vers SQL Server Azure SQL Database &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
  
11. Si nécessaire, mettez à jour les applications de base de données.  
  
> [!NOTE]  
> Vous ne pouvez pas migrer des schémas Information_schema et MySQL.  
  
## <a name="see-also"></a>Voir aussi  
[Installation de SSMA pour MySQL &#40;MySqlToSql&#41;](../../ssma/mysql/installing-ssma-for-mysql-mysqltosql.md)  
[Prise en main avec SSMA pour MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/getting-started-with-ssma-for-mysql-mysqltosql.md)  
  
