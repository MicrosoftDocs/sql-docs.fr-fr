---
title: Instructions Transact-SQL pour les groupes de disponibilité
description: Présente les instructions T-SQL (Transact-SQL) qui prennent en charge le déploiement, la création et la gestion de groupes de disponibilité Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], about
- Availability Groups [SQL Server], Transact-SQL statements
ms.assetid: 184d0a81-2259-4db9-9d0d-01aac0b502c8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 54f521c62ebae90a36ccbd315dc8d04a9ec7c61e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642926"
---
# <a name="transact-sql-statements-for-always-on-availability-groups"></a>Instructions Transact-SQL pour les groupes de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique présente les instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)] qui prennent en charge le déploiement de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , ainsi que la création et la gestion d'un groupe de disponibilité donné, d'un réplica de disponibilité et d'une base de données de disponibilité.  
  
 
##  <a name="create-endpoint"></a><a name="CreateEndpoint"></a> CREATE ENDPOINT  
 [CREATE ENDPOINT ... FOR DATABASE_MIRRORING](../../../t-sql/statements/create-endpoint-transact-sql.md) crée un point de terminaison de mise en miroir de bases de données, s’il n’en existe aucun sur l’instance de serveur. Chaque instance de serveur sur laquelle vous envisagez de déployer [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] ou la mise en miroir de bases de données requiert un point de terminaison de mise en miroir de bases de données.  
  
 Exécutez cette instruction sur l'instance de serveur sur laquelle vous créez le point de terminaison. Vous pouvez créer un seul point de terminaison de mise en miroir de bases de données sur une instance de serveur donnée. Pour plus d’informations, consultez [Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md).  
  
##  <a name="create-availability-group"></a><a name="CreateAG"></a> CREATE AVAILABILITY GROUP  
 [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md) crée un groupe de disponibilité et éventuellement un écouteur du groupe de disponibilité. Au minimum, vous devez spécifier votre instance de serveur locale, qui deviendra le réplica principal initial. Éventuellement, vous pouvez également spécifier jusqu'à quatre réplicas secondaires.  
  
 Exécutez CREATE AVAILABILITY GROUP sur l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que vous voulez utiliser pour héberger le réplica principal initial de votre nouveau groupe de disponibilité. Cette instance de serveur doit résider sur un nœud d’un cluster de basculement Windows Server (WSFC). Pour plus d’informations, consultez [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
##  <a name="alter-availability-group"></a><a name="AlterAG"></a> ALTER AVAILABILITY GROUP  
 [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) prend en charge la modification d’un groupe de disponibilité ou d’un écouteur de groupe de disponibilité existant, ainsi que le basculement d’un groupe de disponibilité.  
  
 Exécutez ALTER AVAILABILITY GROUP sur l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui héberge le réplica principal actuel.  
  
##  <a name="alter-database--set-hadr-"></a><a name="AlterDb"></a> ALTER DATABASE ... SET HADR ...  
 Les options de la clause [SET HADR](../../../t-sql/statements/alter-database-transact-sql-set-hadr.md) de l’instruction ALTER DATABASE vous permettent de joindre une base de données secondaire au groupe de disponibilité de la base de données primaire correspondante, de supprimer une base de données jointe, d’interrompre la synchronisation des données sur une base de données jointe et de reprendre la synchronisation des données.  
  
##  <a name="drop-availability-group"></a><a name="DropAG"></a> DROP AVAILABILITY GROUP  
 [DROP AVAILABILITY GROUP](../../../t-sql/statements/drop-availability-group-transact-sql.md) supprime un groupe de disponibilité spécifié et tous ses réplicas. L'instruction DROP AVAILABILITY GROUP peut être exécutée à partir de n'importe quel nœud [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] dans le cluster de basculement WSFC.  
  
##  <a name="restrictions-on-the-availability-group-transact-sql-statements"></a><a name="Restrictions"></a> Restrictions sur les instructions Transact-SQL AVAILABILITY GROUP  
 Les instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)] CREATE AVAILABILITY GROUP, ALTER AVAILABILITY GROUP et DROP AVAILABILITY GROUP ont les limitations suivantes :  
  
-   À l'exception de DROP AVAILABILITY GROUP, l'exécution de ces instructions requiert que le service HADR soit activé sur l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Activer et désactiver les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).  
  
-   Ces instructions ne peuvent pas être exécutées dans des transactions ou des lots.  
  
-   Même si elles s'efforcent de procéder au nettoyage après une défaillance, ces instructions ne garantissent pas la restauration de toutes les modifications après une défaillance. Toutefois, les systèmes doivent être en mesure de gérer correctement, puis d'ignorer les défaillances partielles.  
  
-   Ces instructions ne prennent pas en charge les expressions ni les variables.  
  
-   Si une instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] est exécutée alors qu'une autre récupération ou action de groupe de disponibilité est en cours, l'instruction retourne une erreur. Attendez la fin de l'action ou de la récupération, puis réessayez l'instruction, si nécessaire.  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
