---
title: 'Moteur de base de données : Changements cassants | Microsoft Docs'
titleSuffix: SQL Server 2016
description: Apprenez-en davantage sur les modifications apportées au moteur de base de données dans SQL Server 2016 (13.x) et versions antérieures qui pourraient altérer la fonctionnalité de version précédente lors de la mise à niveau.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], what's new
- breaking changes [SQL Server]
ms.assetid: 47edefbd-a09b-4087-937a-453cd5c6e061
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: ea6799570df43f5d49cadd93b98dc89528acacdd
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171051"
---
# <a name="breaking-changes-to-database-engine-features-in-sql-server-2016"></a>Changements cassants dans les fonctionnalités du moteur de base de données de SQL Server 2016

[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]  

  Cette rubrique décrit les changements cassants dans le [!INCLUDE[ssDE](../includes/ssde-md.md)] [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] et les versions antérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Ces modifications peuvent interrompre les applications, scripts ou fonctionnalités fondés sur les versions antérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Il se peut que vous rencontriez ces problèmes lors d'une mise à niveau.  
  
##  <a name="breaking-changes-in-sssql15"></a><a name="SQL15"></a> Modifications avec rupture dans [!INCLUDE[ssSQL15](../includes/sssql16-md.md)]  
  
-   La colonne *sample_ms* de `sys.dm_io_virtual_file_stats` est passée du type de données **int** à **bigint**.  
  
-   La colonne *TimeStamp* de `sys.fn_virtualfilestats` est passée du type de données **int** à **bigint**.  

-   Sous le niveau de compatibilité de base de données 130, les conversions implicites des types de données **datetime** en **datetime2** offrent une meilleure précision en prenant en compte les fractions de milliseconde, ce qui génère différentes valeurs converties. Utilisez un transtypage explicite vers le type de données datetime2 chaque fois qu’il existe un scénario de comparaison mixte entre les types de données datetime et datetime2. Pour plus d’informations, consultez cet [article du support technique Microsoft](https://support.microsoft.com/help/4010261).

-   En dessous du niveau de compatibilité 130 de la base de données, les opérations qui effectuent des conversions implicites entre certains types de données numériques et date/heure offrent une meilleure précision et peuvent entraîner des valeurs converties différentes. Cela inclut l’utilisation de fonctions qui requièrent des calculs comme, par exemple, `DATEDIFF` et `ROUND`. Pour plus d’informations, consultez cet [article du support technique Microsoft](https://support.microsoft.com/help/4010261).

## <a name="previous-versions"></a><a name="previous-versions"></a> Versions précédentes  

Pour plus d’informations sur les modifications avec rupture dans [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], et dans certaines versions antérieures, consultez [Modifications avec rupture dans SQL Server 2014](/previous-versions/sql/2014/database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016).

#### <a name="archived-documentation-for-very-old-versions-of-sql-server"></a>Documentation archivée pour les très anciennes versions de SQL Server

[!INCLUDE[Archived documentation for very old versions of SQL Server](../includes/paragraph-content/previous-versions-archive-documentation-sql-server.md)]

## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités du moteur de base de données dépréciées dans SQL Server 2016](../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)   
 [Fonctionnalités du moteur de base de données supprimées dans SQL Server 2016](./discontinued-database-engine-functionality-in-sql-server.md)   
 [Compatibilité descendante du moteur de base de données SQL Server](./discontinued-database-engine-functionality-in-sql-server.md)   
 [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md)   
 [Améliorations de SQL Server 2016 ou SQL Server 2017 sur Windows dans le traitement de certains types de données et les opérations peu courantes](https://support.microsoft.com/help/4010261)