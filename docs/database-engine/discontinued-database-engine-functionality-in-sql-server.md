---
title: Fonctionnalités du moteur de base de données supprimées dans SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 02/02/2017
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
monikerRange: '>= sql-server-linux-2017  || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: a708aad1d483eaf28d559ff04424e515ec9f6aa7
ms.sourcegitcommit: ffb87aa292fc9b545c4258749c28df1bd88d7342
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71816664"
---
# <a name="discontinued-database-engine-functionality-in-sql-server"></a>Fonctionnalités du moteur de base de données supprimées dans SQL Server
[!INCLUDE[tsql-appliesto-ss-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique décrit les fonctionnalités du [!INCLUDE[ssDE](../includes/ssde-md.md)] qui ne sont plus disponibles dans [!INCLUDE[ssCurrent](../includes/ssnoversion-md.md)].  

## <a name="discontinued-features-in-includesssqlv15includessssqlv15-mdmd"></a>Fonctionnalités supprimées dans [!INCLUDE[ssSQLv15](../includes/sssqlv15-md.md)]  

- Les options de configuration étendue à la base de données suivantes sont supprimées :

  - `DISABLE_BATCH_MODE_ADAPTIVE_JOIN`
  - `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK`
  - `DISABLE_INTERLEAVED_EXECUTION_TVF`

Pour connaître les options de configuration actuelles, consultez [ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).

>[!NOTE]
>Aucune fonctionnalité n’a été supprimée dans [!INCLUDE[ssSQLv14](../includes/sssqlv14-md.md)].

## <a name="discontinued-features-in-includesssql15includessssql15-mdmd"></a>Fonctionnalités supprimées dans [!INCLUDE[ssSQL15](../includes/sssql15-md.md)]

- [!INCLUDE[ssSQL15](../includes/sssql15-md.md)] est une application 64 bits. L’installation 32 bits n’est plus disponible, même si certains éléments s’exécutent en tant que composants 32 bits.  

- Le niveau de compatibilité 90 n’est plus disponible. Pour plus d’informations, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  

- Le sous-système ActiveX n’est plus disponible. À la place, utilisez la ligne de commande ou des scripts PowerShell.

- Paramètres de démarrage **-h** et **-g**. Pour plus d’informations, consultez [Options de démarrage du service moteur de base de données](https://docs.microsoft.com/sql/database-engine/configure-windows/database-engine-service-startup-options?view=sql-server-2014).

- Le chiffrement SSL (Secure Sockets Layer) n’est plus disponible. Utilisez TLS (Transport Layer Security) à la place. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données](../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).

## <a name="previous-versions"></a>Versions précédentes

- [Fonctionnalités du moteur de base de données supprimées dans SQL Server 2014](https://docs.microsoft.com/sql/database-engine/discontinued-database-engine-functionality-in-sql-server-2016?view=sql-server-2014)

## <a name="see-also"></a>Voir aussi

- [Fonctionnalités du moteur de base de données déconseillées dans SQL Server 2016](../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)
- [Fonctionnalités dépréciées dans la réplication SQL Server](../relational-databases/replication/deprecated-features-in-sql-server-replication.md)