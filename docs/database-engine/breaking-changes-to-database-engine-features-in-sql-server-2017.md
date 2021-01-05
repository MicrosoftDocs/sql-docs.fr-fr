---
title: 'Moteur de base de données : Changements cassants | Microsoft Docs'
titleSuffix: SQL Server 2017
description: Découvrez les modifications susceptibles d’arrêter les applications, les scripts ou les fonctionnalités basés sur des versions antérieures de SQL Server.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- breaking changes 2017 [SQL Server]
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: affc62db57e4ae08e1886af0fef4ab537bcda86b
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97665883"
---
# <a name="breaking-changes-to-database-engine-features-in-sssqlv14-md"></a>Changements importants dans les fonctionnalités du moteur de base de données de [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)]
[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]


  Cette rubrique décrit les changements cassants dans le [!INCLUDE[ssDE](../includes/ssde-md.md)] [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)]. Ces modifications peuvent interrompre les applications, scripts ou fonctionnalités fondés sur les versions antérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Il se peut que vous rencontriez ces problèmes lors d'une mise à niveau.  
  
## <a name="breaking-changes-in-sssqlv14-md-ssde"></a>Changements cassants dans le [!INCLUDE[ssDE](../includes/ssde-md.md)] [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)]  
  
-  CLR utilise la sécurité d’accès du code (CAS) dans le .NET Framework, qui n’est plus pris en charge comme limite de sécurité. À compter de [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)][!INCLUDE[ssDE](../includes/ssde-md.md)], une option de `sp_configure` appelée `clr strict security` est introduite pour améliorer la sécurité des assemblys CLR. clr strict security est activée par défaut, et traite les assemblys CLR `SAFE` et `EXTERNAL_ACCESS` comme s’ils étaient marqués `UNSAFE`. L’option `clr strict security` peut être désactivée pour assurer une compatibilité descendante, mais ceci n’est pas recommandé. Quand `clr strict security` est désactivée, un assembly CLR créé avec `PERMISSION_SET = SAFE` peut être en mesure d’accéder à des ressources système externes, d’appeler du code non managé et d’acquérir des privilèges **sysadmin**. Une fois la sécurité stricte activée, le chargement des assemblys qui ne sont pas signés échoue. En outre, si une base de données a des assemblys `SAFE` ou `EXTERNAL_ACCESS`, les instructions `RESTORE` ou `ATTACH DATABASE` peuvent être exécutées, mais le chargement des assemblys peut échouer.   
  Pour charger les assemblys, vous devez modifier, ou bien supprimer et recréer, chaque assembly, de façon à ce qu’il soit signé avec un certificat ou une clé asymétrique qui a une connexion correspondante avec l’autorisation `UNSAFE ASSEMBLY` sur le serveur. Pour plus d’informations, consultez [CLR strict security](../database-engine/configure-windows/clr-strict-security.md). 
  
-  Les algorithmes MD2, MD4, MD5, SHA et SHA1 sont dépréciés dans [!INCLUDE[ssSQL15](../includes/sssql15-md.md)]. Jusqu’à [!INCLUDE[ssSQL15](../includes/sssql15-md.md)], un certificat auto-signé est créé à l’aide de l’algorithme SHA1. À partir de [!INCLUDE[ssSQL17](../includes/sssql17-md.md)], un certificat auto-signé est créé à l’aide de l’algorithme SHA2_256.

## <a name="previous-versions"></a><a name="previous-versions"></a> Versions précédentes  

- [Changements essentiels dans les fonctionnalités du moteur de base de données de SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md)

- [Changements essentiels dans les fonctionnalités du moteur de base de données de SQL Server 2014](/previous-versions/sql/2014/database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016?view=sql-server-2014&preserve-view=true#SQL14)

#### <a name="archived-documentation-for-very-old-versions-of-sql-server"></a>Documentation archivée pour les très anciennes versions de SQL Server

[!INCLUDE[Archived documentation for very old versions of SQL Server](../includes/paragraph-content/previous-versions-archive-documentation-sql-server.md)]

## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités du moteur de base de données dépréciées dans SQL Server 2016](../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)   
 [Fonctionnalités du moteur de base de données supprimées dans SQL Server 2016](./discontinued-database-engine-functionality-in-sql-server.md)   
 [Compatibilité descendante du moteur de base de données SQL Server](./discontinued-database-engine-functionality-in-sql-server.md)   
 [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md)  
  
