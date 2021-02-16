---
description: Sauvegarder et restaurer des bases de données avec Always Encrypted
title: Sauvegarder et restaurer des bases de données avec Always Encrypted | Microsoft Docs
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1b5f491846b51b1d70b36c73029ad96706cb1b3e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345484"
---
# <a name="backup-and-restore-databases-using-always-encrypted"></a>Sauvegarder et restaurer des bases de données avec Always Encrypted 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Cet article explique comment sauvegarder et restaurer une base de données contenant des colonnes protégées avec [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md).

Quand vous sauvegardez une base de données, le fichier de sauvegarde résultant contient des données chiffrées stockées dans les colonnes chiffrées et toutes les métadonnées pour les clés Always Encrypted.

Quand vous restaurez une base de données, toutes les données chiffrées et toutes les métadonnées pour les clés Always Encrypted sont restaurées. 

Si vous avez restauré la base de données sur un autre serveur ou sous un autre nom, vous n’avez rien de spécial à faire pour permettre à l’application d’interroger les données chiffrées dans la base de données cible, car les clés sont identiques dans les deux bases de données.

Pour plus d’informations sur la façon de sauvegarder et de restaurer une base de données, consultez :
- [Backup Overview (SQL Server)](../../backup-restore/backup-overview-sql-server.md)
- [Restaurer une base de données sur une instance managée](/azure/sql-database/sql-database-managed-instance-get-started-restore)

## <a name="next-steps"></a>Étapes suivantes
- [Interroger des colonnes en utilisant Always Encrypted avec SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Développer des applications en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-client-development.md) 

## <a name="see-also"></a>Voir aussi
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Exporter et importer des bases de données avec Always Encrypted](always-encrypted-migrate-using-bacpac.md)
- [Migrer des données à partir ou à destination de colonnes à l’aide d’Always Encrypted avec l’Assistant Importation et exportation SQL Server](always-encrypted-migrate-using-import-export-wizard.md)
- [Charger en bloc des données chiffrées dans des colonnes avec Always Encrypted](migrate-sensitive-data-protected-by-always-encrypted.md)