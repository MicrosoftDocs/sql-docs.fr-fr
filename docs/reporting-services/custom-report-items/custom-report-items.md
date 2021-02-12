---
title: Éléments de rapport personnalisés | Microsoft Docs
description: Découvrez les éléments de rapport personnalisés, ainsi que le composant run-time et le composant design-time qui les composent.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-report-items
ms.topic: reference
helpviewer_keywords:
- extending Reporting Services
- Reporting Services, extending
- custom report items
ms.assetid: 64dcaf2c-1af5-4937-8ff7-98f1ec3b367e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6ca390146b560e8de105661097637a42ff3ddfea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061924"
---
# <a name="custom-report-items"></a>Éléments de rapport personnalisés
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] propose un ensemble d'outils permettant de générer et de publier des rapports d'entreprise, de gérer la sécurité et les abonnements et d'étendre les fonctionnalités de création de rapports par le biais d'une API complète. Les rapports sont définis au moyen d'un langage XML appelé RDL (Report Definition Language). Ce langage fournit un ensemble d'instructions qui décrivent la disposition, les informations de requête et les types d'éléments d'un rapport. Il est possible d'étendre le langage RDL en écrivant un élément de rapport personnalisé. L'élément de rapport personnalisé regroupe un composant runtime, qui est appelé par le processeur de rapports au moment de l'exécution, et un composant design, qui permet à l'élément de rapport personnalisé d'être disponible dans le Concepteur de rapports.  
  
 Pour un exemple d’élément de rapport personnalisé totalement implémenté, consultez [SQL Server Reporting Services Product Samples](https://go.microsoft.com/fwlink/?LinkId=177889) (Exemples Reporting Services pour le produit SQL Server).  
  
## <a name="custom-report-item-scenarios"></a>Scénarios d'utilisation d'éléments de rapport personnalisés  
 Les développeurs qui doivent intégrer [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans leurs applications peuvent avoir besoin de fonctionnalités qui ne sont pas prises en charge en mode natif dans le langage RDL. Il peut s'agir d'éléments comme des contrôles de plan, des listes horizontales, des listes en colonnes ou encore des matrices pivotables. Un composant runtime d'élément de rapport personnalisé peut être développé et distribué avec une application pour répondre à ce besoin.  
  
 En plus de fournir des fonctionnalités qui ne sont pas prises en charge en mode natif, certains développeurs voudront étendre les fonctionnalités existantes en proposant d’autres versions des contrôles qui sont déjà inclus avec [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Dans ce scénario, un développeur peut fournir trois composants : un composant runtime, un composant design et un composant runtime de conversion d'éléments de rapport qui convertit un élément de rapport existant en élément de rapport personnalisé à la demande.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Architecture des éléments de rapports personnalisés](../../reporting-services/custom-report-items/custom-report-item-architecture.md)  
 Décrit les composants d'un élément de rapport personnalisé.  
  
 [Conditions de mise en œuvre des éléments de rapports personnalisés](../../reporting-services/custom-report-items/custom-report-item-implementation-requirements.md)  
 Décrit les conditions requises pour créer un élément de rapport personnalisé.  
  
 [Création d'un composant d'exécution d'éléments de rapport personnalisé](../../reporting-services/custom-report-items/creating-a-custom-report-item-run-time-component.md)  
 Décrit comment créer un composant runtime d'élément de rapport personnalisé.  
  
 [Création d'un composant au moment de la conception d'éléments de rapport personnalisé](../../reporting-services/custom-report-items/creating-a-custom-report-item-design-time-component.md)  
 Décrit comment créer un composant design d'élément de rapport personnalisé.  
  
 [Procédure : déployer un élément de rapport personnalisé](../../reporting-services/custom-report-items/how-to-deploy-a-custom-report-item.md)  
 Décrit comment déployer un élément de rapport personnalisé.  
  
 [Bibliothèques de classes d'éléments de rapports personnalisés](../../reporting-services/custom-report-items/custom-report-item-class-libraries.md)  
 Décrit les classes d’infrastructure d’éléments de rapport personnalisés et les classes wrapper managées dans l’espace de noms **Microsoft.ReportDesigner**.  
  
## <a name="see-also"></a>Voir aussi  
 [Informations techniques de référence &#40;SSRS&#41;](../../reporting-services/technical-reference-ssrs.md)  
  
  
