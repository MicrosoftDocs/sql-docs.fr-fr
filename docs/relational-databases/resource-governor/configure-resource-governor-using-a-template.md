---
title: Configurer Resource Governor à l’aide d’un modèle | Microsoft Docs
description: Découvrez comment configurer Resource Governor à l’aide d’un modèle fourni dans SQL Server Management Studio. Vous devez disposer de l’autorisation CONTROL SERVER.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, templates
ms.assetid: f342dec2-d1d6-483e-b44e-98eb7d168b5e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9da1652b2a4814950bbc152e4ea74eb9e4c38a17
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96504863"
---
# <a name="configure-resource-governor-using-a-template"></a>Configurer Resource Governor à l'aide d'un modèle
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Vous pouvez configurer Resource Governor à l'aide d'un modèle fourni dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
-   **Avant de commencer :**  [Autorisations](#Permissions)  
  
-   **Pour créer un groupe de charge de travail, avec :** [un modèle](#ConfRGTemplate)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
 Utilisez les étapes suivantes pour ouvrir et modifier un modèle qui crée un pool de ressources et un groupe de charge de travail pour le pool. De plus, ce modèle vous permet de créer une fonction classifieur définie par l'utilisateur qui achemine de nouvelles connexions vers le groupe par défaut ou le groupe de charge de travail que vous créez.  
  
###  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] de Resource Governor dans le modèle requièrent l'autorisation CONTROL SERVER.  
  
##  <a name="configure-resource-governor-using-a-template"></a><a name="ConfRGTemplate"></a> Configurer Resource Governor à l'aide d'un modèle  
 **Pour configurer Resource Governor à l'aide d'un modèle dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]**  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], dans le menu **Affichage** , cliquez sur **Explorateur de modèles**.  
  
2.  Dans **Explorateur de modèles**, développez **Resource Governor**, puis double-cliquez sur **Configurer Resource Governor**.  
  
3.  Dans **Se connecter au moteur de base de données**, entrez les informations requises, puis cliquez sur **OK**. Le modèle Configure Ressource Governor.sql est fourni dans l'Éditeur de requête. Utilisez ce modèle pour créer et configurer un pool de ressources, un groupe de charges de travail et une fonction classifieur.  
  
4.  Pour modifier les valeurs dans le modèle, appuyez sur CTRL+Maj+M. Dans la fenêtre **Spécifier les valeurs des paramètres du modèle** , entrez les valeurs de votre choix.  
  
5.  Pour enregistrer les modifications que vous apportez au modèle, cliquez sur **OK**.  
  
6.  Pour exécuter la requête, cliquez sur **Exécuter**.  

## <a name="see-also"></a>Voir aussi  
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [Activer Resource Governor](../../relational-databases/resource-governor/enable-resource-governor.md)   
 [Pool de ressources de Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md)   
 [Groupe de charge de travail de Resource Governor](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [Fonction classifieur de Resource Governor](../../relational-databases/resource-governor/resource-governor-classifier-function.md)   
 [Afficher les propriétés de Resource Governor](../../relational-databases/resource-governor/view-resource-governor-properties.md)   
 [CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)   
 [CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)   
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
