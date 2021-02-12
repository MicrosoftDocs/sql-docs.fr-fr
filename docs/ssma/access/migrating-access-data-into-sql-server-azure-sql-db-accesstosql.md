---
description: Migration de données Access vers SQL Server Azure SQL Database (AccessToSQL)
title: Migration de données Access vers SQL Server Azure SQL Database (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- bulk loading data
- data, loading into SQL Azure
- data, loading into SQL Server
- migrating databases, loading data
- migrating databases, options
- options, migrating data
- SQL Azure, migrating data into
- SQL Server, migrating data into
ms.assetid: f3b18af7-1af0-499d-a00d-a0af94895625
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a86ae9cff414e052865cf0efd1ec5c7889877d0f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066744"
---
# <a name="migrating-access-data-into-sql-server---azure-sql-database-accesstosql"></a>Migration de données Access vers SQL Server Azure SQL Database (AccessToSQL)
Une fois que vous avez créé avec succès les objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez migrer des données d’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure.  
  
## <a name="setting-migration-options"></a>Définition des options de migration  
Avant de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, passez en revue les options de migration de projet dans la boîte de dialogue **paramètres du projet** . Dans cette boîte de dialogue, vous pouvez définir la taille de lot de migration, le verrouillage de table, la vérification des contraintes, le déclenchement du déclencheur d’insertion, l’identité et la gestion des valeurs NULL, et comment gérer les dates qui se trouvent en dehors de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] plage. Pour plus d’informations, consultez [paramètres du projet (migration)](./project-settings-migration-accesstosql.md).  
  
## <a name="migrating-data"></a>Migration des données  
La migration des données est une opération de chargement en bloc qui déplace des lignes de données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure dans des transactions. Le nombre de lignes à charger dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure dans chaque transaction est configuré dans les paramètres du projet.  
  
Pour afficher les messages de migration, assurez-vous que le volet de sortie est visible. Si ce n’est pas le cas, dans le menu **affichage** , sélectionnez **sortie**.  
  
**Pour migrer des données**  
  
1.  Assurez-vous que vous avez chargé les objets de base de données Access dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure.  
  
2.  Dans l’Explorateur de métadonnées Access, sélectionnez les objets qui contiennent les données que vous souhaitez migrer :  
  
    -   Pour migrer des données pour une base de données entière, activez la case à cocher en regard du nom de la base de données.  
  
    -   Pour migrer des données à partir de tables individuelles, développez la base de données, développez **tables**, puis activez la case à cocher en regard de la table. Pour omettre les données de tables individuelles, désactivez la case à cocher.  
  
3.  Cliquez avec le bouton droit sur **bases de données** , puis sélectionnez **migrer des données**.  
  
Vous pouvez également migrer des données en dehors de SSMA à l’aide de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilitaire de ligne de commande **BCP** ou [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d’informations sur ces outils, consultez la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentation en ligne de.  
  
## <a name="next-step"></a>étape suivante  
Si vous avez accès à des applications de base de données que vous souhaitez continuer à utiliser après la migration, liez les tables de base de données Access aux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tables ou SQL Azure. Pour plus d’informations, consultez [liaison d’applications d’accès aux SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[Définition des options de conversion et de migration](setting-conversion-and-migration-options-accesstosql.md)  
