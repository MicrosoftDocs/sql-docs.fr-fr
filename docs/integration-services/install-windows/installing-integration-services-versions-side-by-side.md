---
description: Installation de plusieurs versions Integration Services côte à côte
title: Installation de plusieurs versions Integration Services côte à côte | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- interoperability and coexistence [Integration Services]
- Integration Services, interoperability and coexistence
ms.assetid: edfbcd56-012f-462e-a542-95491394fda9
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1be851dfaf8423ec830ad0a134a6e8610789eec5
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835357"
---
# <a name="installing-integration-services-versions-side-by-side"></a>Installation de plusieurs versions Integration Services côte à côte

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Vous pouvez installer plusieurs versions de [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] Integration Services (SSIS) côte à côte. Cette rubrique décrit certaines limitations des installations côte à côte.  
  
## <a name="designing-and-maintaining-packages"></a>Conception et gestion de packages  
 Pour concevoir et gérer des packages ciblant SQL Server 2016, SQL Server 2014 ou SQL Server 2012, utilisez SQL Server Data Tools (SSDT) pour Visual Studio 2015. Pour obtenir SSDT, voir [Télécharger la dernière version de SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md).  
  
 Dans les pages de propriétés d’un projet Integration Services, au niveau de l’onglet **Général** de **Propriétés de configuration**, sélectionnez la propriété **TargetServerVersion** et choisissez SQL Server 2016, SQL Server 2014 ou SQL Server 2012.  
  
|Version cible de SQL Server|Environnement de développement de packages SSIS|  
|----------------------------------|-----------------------------------------------|  
|2016|SQL Server Data Tools pour Visual Studio 2015|  
|2014|SQL Server Data Tools pour Visual Studio 2015<br /><br /> ou<br /><br /> SQL Server Data Tools - Business Intelligence pour Visual Studio 2013|  
|2012|SQL Server Data Tools pour Visual Studio 2015<br /><br /> ou<br /><br /> SQL Server Data Tools – Business Intelligence pour Visual Studio 2012|  
|2008|Business Intelligence Development Studio dans SQL Server 2008|  
  
 Lorsque vous ajoutez un package existant à un projet existant, le package est converti au format ciblé par le projet.  
  
## <a name="running-packages"></a>Exécution des packages  
 Pour exécuter des packages Integration Services créés par des versions antérieures des outils de développement, vous pouvez utiliser la version [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] de l’utilitaire **dtexec** ou de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Lorsque ces outils [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] chargent un package développé dans une version antérieure des outils de développement, l’outil convertit momentanément le package en mémoire dans le format de package utilisé par [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] . Si le package présente des problèmes empêchant une conversion, l’outil [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] ne peut pas exécuter le package tant que ces problèmes ne sont pas résolus. Pour plus d’informations, consultez [Mettre à niveau des packages Integration Services](../../integration-services/install-windows/upgrade-integration-services-packages.md).  
  
  
