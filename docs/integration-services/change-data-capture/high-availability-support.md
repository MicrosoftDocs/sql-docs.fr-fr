---
description: Prise en charge de la haute disponibilité
title: Prise en charge de la haute disponibilité | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 2e0f6d3f-0536-46d9-8630-835e199515bf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 225d746b5b86f3e0a4dbd2fb122ecdbfb11bf02f
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813152"
---
# <a name="high-availability-support"></a>Prise en charge de la haute disponibilité

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Le service de capture de données modifiées pour Oracle est conçu pour une haute disponibilité. Les fonctionnalités suivantes fournissent une partie de la prise en charge de la haute disponibilité :  
  
-   Le service de capture de données modifiées pour Oracle n'utilise aucune ressource de fichier (locale ou autre). L'intégralité de son état est stocké dans l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible. Il est ainsi facile de démarrer le service sur un autre ordinateur qui utilise la même instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si l'ordinateur sur lequel le service s'exécute est défaillant. Pour réduire le temps de récupération, les transactions Oracle longues sont conservées dans une table de mise en lots dans l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]cible, ce qui évite la nécessité d'une nouvelle analyse de nombreux journaux des transactions Oracle après un échec (ou le redémarrage du service).  
  
-   Le service de capture de données modifiées pour Oracle peut utiliser des instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cluster, de façon à récupérer après le basculement de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers un autre nœud de cluster. L'administrateur de l'ordinateur de service de capture de données modifiées Oracle doit uniquement spécifier les informations de connexion à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cluster lorsqu'il crée un service de capture de données modifiées Oracle.  
  
-   Le service de capture de données modifiées pour Oracle peut utiliser la fonctionnalité de mise en miroir de bases de données [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]**AlwaysOn** . Cette prise en charge requiert que les bases de données MSXDBCDC et toutes bases de données CDC soient membres du même groupe de disponibilité. Elle exige également que l’administrateur de l’ordinateur de service de capture de données modifiées Oracle spécifie les informations de connexion **AlwaysOn** appropriées au groupe de disponibilité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (par exemple, les propriétés de connexion `Failover_Partner and Network=dbmssocn`). Cela permet au service de capture de données modifiées de reprendre automatiquement le traitement sur une réplication secondaire des bases de données après un basculement.  
  
-   Le service de capture de données modifiées pour Oracle peut être configuré en tant que ressource de service générique sur un cluster de basculement Windows (conjointement avec, ou séparément de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]), ce qui simplifie le basculement du traitement CDC avec le cluster. Pour configurer le service de capture de données modifiées pour Oracle en tant que ressource dans un cluster de basculement, l'administrateur système doit définir le service de capture de données modifiées pour Oracle comme ressource de service générique sur chaque nœud du cluster de basculement.  
  
-   Le service de capture de données modifiées pour Oracle prend en charge Oracle RAC, ce qui lui permet de communiquer avec la base de données Oracle et de traiter les journaux même lorsqu'un des nœuds Oracle RAC est arrêté.  
  
  
