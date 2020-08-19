---
description: Référence du langage Integration Services
title: Référence du langage Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: c67b72f1-0a1e-42f0-878a-84e85efc915b
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2563d0029f9330dc07402e190d8553c8f509ef97
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88449868"
---
# <a name="integration-services-language-reference"></a>Référence du langage Integration Services

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Cette section décrit les API [!INCLUDE[tsql](../includes/tsql-md.md)] qui sont disponibles pour l'administration des projets [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] déployés dans une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] stocke des objets, des paramètres et des données opérationnelles dans une base de données référencée comme catalogue [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Le nom par défaut du catalogue [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] est SSISDB. Les objets stockés dans le catalogue incluent des projets, des packages, des paramètres, des environnements et l'historique opérationnel.  
  
 Le catalogue [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] stocke ses données dans des tables internes qui ne sont pas visibles aux utilisateurs. Cependant, il expose les informations dont vous avez besoin via un ensemble de vues publiques que vous pouvez interroger. Il fournit également un ensemble de procédures stockées que vous pouvez utiliser pour effectuer des tâches courantes sur le catalogue.  
  
 En général, vous gérez des objets [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] dans le catalogue en ouvrant [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]. Cependant, vous pouvez également utiliser directement les vues de base de données et les procédures stockées, ou écrire du code personnalisé qui appelle l'API managée. [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] et l'API managée interrogent les vues et appellent les procédures stockées décrites dans cette section pour effectuer de nombreuses tâches.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Vues &#40;catalogue Integration Services&#41;](../integration-services/system-views/views-integration-services-catalog.md)  
 Interrogez les vues pour inspecter des objets [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], des paramètres et des données opérationnelles.  
  
 [Procédures stockées &#40;catalogue Integration Services&#41;](../integration-services/system-stored-procedures/stored-procedures-integration-services-catalog.md)  
 Appelez les procédures stockées pour ajouter, supprimer ou modifier des objets [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] et des paramètres.  
  
 [Fonctions &#40;catalogue Integration Services&#41;](https://msdn.microsoft.com/library/9f2aec85-3d4c-415f-b1f8-8328a60b1c7f)  
 Appelez les fonctions pour gérer des projets [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].  
  
  
