---
title: Fonctionnalités du moteur de base de données supprimées
description: Découvrez les fonctionnalités du moteur de base de données qui ont été supprimées dans SQL Server 2019 (15.x), SQL Server 2016 (13.x) et les versions précédentes.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- VIA protocol
- unsupported features [SQL Server]
- SQL Mail
- discontinued functionality [SQL Server]
- RESTORE WITH DBO_ONLY
- BACKUP WITH PASSWORD
- user instances enabled
- BACKUP WITH MEDIAPASSWORD
- AWE
- SQL-DMO
- '*= and =*'
- 80 compatibility levels
- COMPUTE BY
- user instance timeout
- sp_dropalias
- COMPUTE
- SSL
- WITH APPEND
- sys.database_principal_aliases
- sp_dboption
- DATABASEPROPERTY
- FASTFIRSTROW hint
- SET DISABLE_DEF_CNST_CHK
ms.assetid: d686cdf0-d11d-4dba-9ec8-de1a5f189f25
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= sql-server-linux-2017  || >= sql-server-2016'
ms.openlocfilehash: 093e20773376cddeea566e202d9f18605f1a1c13
ms.sourcegitcommit: 58e7069b5b2b6367e27b49c002ca854b31b1159d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99552587"
---
# <a name="discontinued-database-engine-functionality-in-sql-server"></a>Fonctionnalités du moteur de base de données supprimées dans SQL Server
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Cette rubrique décrit les fonctionnalités du [!INCLUDE[ssDE](../includes/ssde-md.md)] qui ne sont plus disponibles dans [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)].  

## <a name="discontinued-features-in-sssql19"></a>Fonctionnalités supprimées dans [!INCLUDE[sssql19](../includes/sssql19-md.md)]  

- Les options de configuration étendue à la base de données suivantes sont supprimées :

  - `DISABLE_BATCH_MODE_ADAPTIVE_JOIN`
  - `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK`
  - `DISABLE_INTERLEAVED_EXECUTION_TVF`

Pour connaître les options de configuration actuelles, consultez [ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).

>[!NOTE]
>Aucune fonctionnalité n’a été supprimée dans [!INCLUDE[sssql14](../includes/sssql17-md.md)].

## <a name="discontinued-features-in-sssql15-md"></a>Fonctionnalités supprimées dans [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]

- [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] est une application 64 bits. L’installation 32 bits n’est plus disponible, même si certains éléments s’exécutent en tant que composants 32 bits.  

- Le niveau de compatibilité 90 n’est plus disponible. Pour plus d’informations, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  

- Le sous-système ActiveX n’est plus disponible. À la place, utilisez la ligne de commande ou des scripts PowerShell.

- Paramètres de démarrage **-h** et **-g**. Pour plus d’informations, consultez [Options de démarrage du service moteur de base de données](/previous-versions/sql/2014/database-engine/configure-windows/database-engine-service-startup-options?view=sql-server-2014&preserve-view=true).

- Le chiffrement SSL (Secure Sockets Layer) n’est plus disponible. Utilisez TLS (Transport Layer Security) à la place. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données](../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).

## <a name="previous-versions"></a>Versions précédentes

- [Discontinued Database Engine Functionality in SQL Server 2014 (Fonctionnalités du moteur de base de données non disponibles dans SQL Server 2014)](/previous-versions/sql/2014/database-engine/discontinued-database-engine-functionality-in-sql-server-2016?view=sql-server-2014&preserve-view=true)

### <a name="see-also"></a>Voir aussi

- [Fonctionnalités dépréciées du moteur de base de données dans SQL Server 2019](deprecated-database-engine-features-in-sql-server-version-15.md)
- [Fonctionnalités dépréciées du moteur de base de données dans SQL Server 2017](deprecated-database-engine-features-in-sql-server-2017.md)
- [Fonctionnalités du moteur de base de données dépréciées dans SQL Server 2016](../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)
- [Changements cassants dans les fonctionnalités du moteur de base de données de SQL Server 2019](breaking-changes-to-database-engine-features-in-sql-server-version-15.md)
- [Changements cassants dans les fonctionnalités du moteur de base de données de SQL Server 2017](breaking-changes-to-database-engine-features-in-sql-server-2017.md)
- [Changements cassants dans les fonctionnalités du moteur de base de données de SQL Server 2016](breaking-changes-to-database-engine-features-in-sql-server-2016.md)
- [Fonctionnalités dépréciées dans la réplication SQL Server](../relational-databases/replication/deprecated-features-in-sql-server-replication.md)