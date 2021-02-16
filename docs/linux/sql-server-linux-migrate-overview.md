---
title: Migrer des bases de données vers SQL Server sur Linux
description: Cet article décrit les différentes options de migration des bases de données et des données vers SQL Server sur Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1619489d-377a-4f32-8930-d4f536539689
ms.openlocfilehash: d267bc2861d94e46cdb10fa825d204c8110a9273
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345582"
---
# <a name="migrate-databases-and-structured-data-to-sql-server-on-linux"></a>Migrer des bases de données et des données structurées vers SQL Server sur Linux 

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Vous pouvez migrer vos bases de données et vos données vers SQL Server s’exécutant sur Linux. La méthode que vous choisissez d’utiliser dépend des données sources et de votre scénario spécifique. Les sections suivantes présentent les meilleures pratiques pour différents scénarios de migration.

## <a name="migrate-from-sql-server-on-windows"></a>Migrer à partir de SQL Server sur Windows
Si vous souhaitez migrer des bases de données SQL Server sur Windows vers SQL Server sur Linux, la technique recommandée consiste à utiliser la sauvegarde et la restauration SQL Server.

1. Créez une sauvegarde de la base de données sur la machine Windows.
2. Transférez le fichier de sauvegarde sur la machine Linux SQL Server cible.
3. Restaurez la sauvegarde sur la machine Linux. 

Pour obtenir un didacticiel sur la migration d’une base de données avec la sauvegarde et la restauration, consultez la rubrique suivante :

- [Restaurer une base de données SQL Server de Windows à Linux](sql-server-linux-migrate-restore-database.md).

Il est également possible d’exporter votre base de données vers un fichier BACPAC (fichier compressé contenant le schéma et les données de votre base de données). Si vous avez un fichier BACPAC, vous pouvez transférer ce fichier sur votre machine Linux, puis l’importer dans SQL Server. Pour plus d'informations, voir les rubriques suivantes :

- [Exporter et importer une base de données avec SSMS ou SqlPackage.exe](sql-server-linux-migrate-ssms.md)

## <a name="migrate-from-other-database-servers"></a>Migrer à partir d’autres serveurs de base de données
Vous pouvez migrer des bases de données sur d’autres systèmes de base de données vers SQL Server sur Linux. Cela inclut les bases de données Microsoft Access, DB2, MySQL, Oracle et Sybase. Dans ce scénario, utilisez l’Assistant de gestion SQL Server (SSMA) pour automatiser la migration vers SQL Server sur Linux. Pour plus d’informations, consultez [Utiliser SSMA pour migrer des bases de données vers SQL Server sur Linux](sql-server-linux-migrate-ssma.md).  

## <a name="migrate-structured-data"></a>Migrer des données structurées
Il existe également des techniques pour l’importation de données brutes. Vous pouvez avoir des fichiers de données structurés exportés à partir d’autres bases de données ou sources de données. Dans ce cas, vous pouvez utiliser l’outil bcp pour insérer les données en bloc. Ou, vous pouvez exécuter SQL Server Integration Services sur Windows pour importer les données dans une base de données SQL Server sur Linux. SQL Server Integration Services vous permet d’exécuter des transformations plus complexes sur les données pendant l’importation. 

Pour plus d'informations sur ces techniques, consultez les rubriques suivantes :

- [Copie en bloc des données avec bcp](sql-server-linux-migrate-bcp.md)
- [Extraire, transformer et charger des données pour SQL Server sur Linux avec SSIS](sql-server-linux-migrate-ssis.md) 
