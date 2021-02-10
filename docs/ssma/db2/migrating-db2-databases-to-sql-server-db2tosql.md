---
title: Migration de bases de données DB2 vers SQL Server (DB2ToSQL) | Microsoft Docs
description: Utilisez ce processus recommandé pour migrer des bases de données DB2 vers SQL Server ou Azure SQL Database à l’aide d’Assistant Migration SQL Server (SSMA).
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 14d2e655-af7e-4aa5-ba28-0e3d0d025518
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 912f1ffd342517add68d1ce7f628251fc86dc6aa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017699"
---
# <a name="migrating-db2-databases-to-sql-server-db2tosql"></a>Migration de bases de données DB2 vers SQL Server (DB2ToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration (SSMA) pour DB2 est un environnement complet qui vous aide à migrer rapidement des bases de données DB2 vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. En utilisant SSMA pour DB2, vous pouvez examiner les données et les objets de base de données, évaluer les bases de données pour la migration, migrer des objets de base de données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database, puis migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. Notez que vous ne pouvez pas migrer les schémas SYS et système DB2.  
  
## <a name="recommended-migration-process"></a>Processus de migration recommandé  
Pour migrer des objets et des données de bases de données DB2 vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database, procédez comme suit :  
  
1.  [Nouveau projet SSMA](./new-project-db2tosql.md).  
  
    Après avoir créé le projet, vous pouvez définir les options de conversion de projet, de migration et de mappage de type. Pour plus d’informations sur les paramètres de projet, consultez [paramètres du projet &#40;Conversion&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-conversion-db2tosql.md) et les sections associées. Pour plus d’informations sur la personnalisation des mappages de types de données, consultez [mappage des types de données DB2 et SQL Server &#40;&#41;DB2ToSQL ](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md).  
  
2.  [Connectez-vous à la base de données DB2](./connecting-to-db2-database-db2tosql.md).  
  
3.  [Connexion à SQL Server](./connecting-to-sql-server-db2tosql.md).  
  
4.  [Mappez les schémas DB2 aux schémas de SQL Server](./mapping-db2-schemas-to-sql-server-schemas-db2tosql.md).  
  
5.  Éventuellement, des [rapports d’évaluation](./assessment-report-db2tosql.md) pour évaluer les objets de base de données pour la conversion et estimer la durée de la conversion.  
  
6.  [Convertissez les schémas DB2](./converting-db2-schemas-db2tosql.md).  
  
7.  [Chargez les objets de base de données convertis dans SQL Server](./loading-converted-database-objects-into-sql-server-db2tosql.md).  
  
    Vous pouvez le faire de l’une des manières suivantes :  
  
    -   Enregistrez un script et exécutez-le dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   Synchronisez les objets de base de données.  
  
8.  [Migration des données DB2 vers SQL Server](./migrating-db2-data-into-sql-server-db2tosql.md).  
  
9. Si nécessaire, mettez à jour les applications de base de données.  
  
## <a name="see-also"></a>Voir aussi  
[Installation de SSMA pour DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/installing-ssma-for-db2-db2tosql.md)  
[Prise en main avec SSMA pour DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/getting-started-with-ssma-for-db2-db2tosql.md)  
