---
title: Tableau de bord de groupe de disponibilité dans SSMS
description: Apprenez-en davantage sur la page Options disponible dans le tableau de bord SQL Server Always On dans SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Alwayson.Dashboard
ms.assetid: 4369b588-e982-4b57-80a1-beb2e879ce0b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6a7cfa4984ede1f6f6963006b5a5d5675a78dac8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349256"
---
# <a name="options-sql-server-always-on-dashboard-page"></a>Options (SQL Server Always On, page Tableau de bord)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Utilisez la page **Tableau de bord SQL Server Always On** de la boîte de dialogue [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]**Options** pour configurer le tableau de bord AlwaysOn.  
  
 **Pour accéder à cette page :**  
  
 Dans le menu **Outils** , cliquez sur **Options**, développez le dossier **SQL Server Always On** , puis cliquez sur **Tableau de bord**.  
  
## <a name="on-this-page"></a>Sur cette page  
 **Activez l'actualisation automatique.**  
 Cliquez pour activer l'actualisation automatique. Les options sont :  
  
-   Le champ **Intervalle d’actualisation (en secondes)** affiche le nombre de secondes auquel le tableau de bord sera actualisé. La valeur par défaut est 30. Lorsque l'actualisation automatique est activée, vous pouvez modifier ce champ afin de changer l'intervalle d'actualisation.  
  
-   Le **Nombre de nouvelles tentatives de connexion** affiche le nombre de fois que le tableau de bord essaie de se connecter à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui héberge un réplica de disponibilité pour un groupe de disponibilité surveillé par le tableau de bord. La valeur par défaut est 65535. Lorsque l'actualisation automatique est activée, vous pouvez modifier ce champ afin de changer le nombre de nouvelles tentatives de connexion.  
  
 **Activez votre stratégie Always On définie par l’utilisateur.**  
 Si vous avez défini votre propre stratégie Always On, cliquez sur cette option pour activer cette stratégie.  
  
## <a name="see-also"></a>Voir aussi  
 [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
