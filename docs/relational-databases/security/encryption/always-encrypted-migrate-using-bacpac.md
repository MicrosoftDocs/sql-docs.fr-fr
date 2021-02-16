---
description: Exporter et importer des bases de données avec Always Encrypted
title: Exporter et importer des bases de données avec Always Encrypted | Microsoft Docs
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 674ad450fc1cd2ef45aad45333463632dafcf523
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345470"
---
# <a name="export-and-import-databases-using-always-encrypted"></a>Exporter et importer des bases de données avec Always Encrypted 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Cet article explique comment exporter et importer des bases de données contenant des colonnes protégées avec [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md).

Quand vous exportez une base de données, toutes les données stockées dans des colonnes chiffrées sont extraites de la base de données sous une forme chiffrée (texte chiffré) et placées dans le fichier [BACPAC](../../data-tier-applications/data-tier-applications.md) résultant. Le fichier BACPAC obtenu contient également les métadonnées pour les clés Always Encrypted.

Quand vous importez le fichier BACPAC dans une base de données, les données chiffrées dans le fichier BACPAC sont chargées dans la base de données et les métadonnées de clé Always Encrypted sont recréées. 

Si vous avez une application configurée pour interroger des colonnes chiffrées stockées dans la base de données source (celle que vous exportez), vous n’avez rien à faire de spécial pour que l’application puisse interroger les données chiffrées dans la base de données cible, car les clés dans les deux bases de données sont identiques.

Pour des informations détaillées sur l’exportation et l’importation d’une base de données, consultez :
- [Exporter une application de la couche Données](../../data-tier-applications/export-a-data-tier-application.md)
- [Importer un fichier BACPAC pour créer une nouvelle base de données utilisateur](../../data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database.md)
- [Exporter une base de données Azure SQL vers un fichier BACPAC](/azure/sql-database/sql-database-export)
- [Importer un fichier BACPAC vers une base de données dans Azure SQL Database](/azure/sql-database/sql-database-import)
- [SqlPackage.exe](../../../tools/sqlpackage/sqlpackage.md)

## <a name="permissions-for-migrating-databases-with-encrypted-columns"></a>Autorisations pour la migration des bases de données avec des colonnes chiffrées

Vous avez besoin des autorisations *ALTER ANY COLUMN MASTER KEY* et *ALTER ANY COLUMN ENCRYPTION KEY* sur la base de données source. Vous avez besoin des autorisations *ALTER ANY COLUMN MASTER KEY*, *ALTER ANY COLUMN ENCRYPTION KEY*, *VIEW ANY COLUMN MASTER KEY DEFINITION* et *VIEW ANY COLUMN ENCRYPTION DEFINITION* sur la base de données cible.

Vous n’avez pas besoin d’accéder aux clés principales de colonne configurées pour les colonnes chiffrées, car les données restent chiffrées pendant les opérations d’exportation et d’importation.

## <a name="next-steps"></a>Étapes suivantes
- [Développer des applications avec Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Voir aussi
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Sauvegarder et restaurer des bases de données avec Always Encrypted](always-encrypted-migrate-using-backup-restore.md)
- [Migrer des données à partir ou à destination de colonnes à l’aide d’Always Encrypted avec l’Assistant Importation et exportation SQL Server](always-encrypted-migrate-using-import-export-wizard.md)
- [Charger en bloc des données chiffrées dans des colonnes avec Always Encrypted](migrate-sensitive-data-protected-by-always-encrypted.md)