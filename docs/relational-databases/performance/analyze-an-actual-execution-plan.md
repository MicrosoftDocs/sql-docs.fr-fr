---
title: Analyser un plan d’exécution réel | Microsoft Docs
description: Découvrez comment analyser les plans d’exécution graphiques réels, qui contiennent des informations d’exécution, à l’aide de la fonctionnalité d’analyse de plan de SQL Server Management Studio.
ms.custom: ''
ms.date: 10/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- analyzing execution plans
- analyzing actual execution plans
- execution plans [SQL Server], analyzing
ms.assetid: 9e583a18-5f4a-4054-bfe1-4b2a76630db6
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 33ed1ed03b9e340e295c14481a6432fca0d3a385
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350058"
---
# <a name="analyze-an-actual-execution-plan"></a>Analyser un plan d’exécution réel

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette rubrique explique comment analyser des plans d’exécution graphiques réels à l’aide de la fonctionnalité d’analyse de plan de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Cette fonctionnalité est disponible à partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v17.4. Nous recommandons généralement d’[installer la dernière version de SSMS](../../ssms/download-sql-server-management-studio-ssms.md).

> [!NOTE]
> Les plans d’exécution réels sont générés une fois que les requêtes ou les lots [!INCLUDE[tsql](../../includes/tsql-md.md)] ont été exécutés. Pour cette raison, un plan d’exécution réel contient des informations d’exécution, comme la quantité réelle de lignes, les avertissements d’exécution (s’il y en a) et les métriques d’utilisation des ressources. Pour plus d’informations, consultez [Afficher un plan d’exécution réel](../../relational-databases/performance/display-an-actual-execution-plan.md).
  
Le dépannage des performances de requêtes nécessite une réelle expertise en ce qui concerne les plans d’exécution et le traitement des requêtes, afin d’être en mesure de trouver et de corriger les causes racines.

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] inclut une fonctionnalité qui implémente un certain degré d’automatisation dans la tâche d’analyse de plan d’exécution réel, en particulier pour les plans volumineux et complexes. L’objectif est de faciliter l’identification des scénarios d’[Estimation de cardinalité](../../relational-databases/performance/cardinality-estimation-sql-server.md) inexacte, et d’obtenir des recommandations concernant les solutions d’atténuation disponibles.

> [!IMPORTANT]
> Veillez à effectuer des tests appropriés des solutions d’atténuation proposées avant de les appliquer dans des environnements de production.
  
## <a name="to-analyze-an-execution-plan-for-a-query"></a>Pour analyser un plan d’exécution pour une requête  
  
1.  Ouvrez un fichier de plan d’exécution de requête précédemment enregistré (.sqlplan) en cliquant sur **Ouvrir un fichier** dans le menu **Fichier**, ou en faisant glisser un fichier de plan vers la fenêtre [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]. En guise d’alternative, si vous venez d’exécuter une requête et que vous avez choisi d’afficher son plan d’exécution, accédez à l’onglet **Plan d’exécution** dans le volet de résultats. 

2.  Cliquez avec le bouton droit dans une zone vide du plan d’exécution et cliquez sur **Analyser le plan d’exécution réel**. 

    ![Cliquer avec le bouton droit sur Analyser le plan d’exécution réel](../../relational-databases/performance/media/plananalysismenuoption.png "Cliquer avec le bouton droit sur Analyser le plan d’exécution réel")   

3.  La fenêtre **Analyse du plan d’exécution de requêtes** s’ouvre dans la partie inférieure. L’onglet **Instructions multiples** est utile lors de l’analyse des plans comprenant plusieurs instructions, car il permet d’analyser la bonne instruction.

4.  Sélectionnez l’onglet Scénarios pour afficher des détails sur les problèmes détectés pour le plan d’exécution réel. Pour chaque opérateur listé dans le volet gauche, le volet droit affiche des détails sur le scénario dans le lien *Cliquez ici pour plus d’informations sur ce scénario*, et les raisons possibles pouvant expliquer ce scénario sont listées.

    ![Résultats de l’analyse du plan d’exécution](../../relational-databases/performance/media/plananalysis-scenarios.png "Résultats de l’analyse du plan d’exécution") 
