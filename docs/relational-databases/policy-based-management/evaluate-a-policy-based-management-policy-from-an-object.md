---
title: Évaluer une stratégie de la gestion basée sur des stratégies à partir d’un objet
description: Découvrez comment évaluer une stratégie à partir d’une instance, d’une base de données ou d’un objet de base de données SQL Server à l’aide de SSMS (SQL Server Management Studio).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, evaluate policy
ms.assetid: b9e9d646-4894-4dee-a02a-0c71a8dc020e
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 0f46e36b5be5d4f9e1ef8104fca8933d05d15189
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049012"
---
# <a name="evaluate-a-policy-based-management-policy-from-an-object"></a>Évaluer une stratégie de gestion basée sur des stratégies à partir d'un objet
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment évaluer une stratégie d'une instance de serveur, d'une base de données ou d'un objet de base de données dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour évaluer une stratégie à partir d'un objet à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Le mode d'exécution est défini dans le cadre de la stratégie et ne peut pas être modifié dans la boîte de dialogue **Évaluer les stratégies** .  
  
-   La boîte de dialogue **Évaluer les stratégies** affiche seulement les stratégies appropriées à l'objet de base de données.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'appartenance au rôle PolicyAdministratorRole dans la base de données msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-evaluate-a-policy-from-an-object"></a>Pour évaluer une stratégie à partir d'un objet  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur une instance de serveur, une base de données ou un objet de base de données, pointez sur **Stratégies**, puis sélectionnez **Évaluer**.  
  
2.  Dans la boîte de dialogue **Évaluer les stratégies** , sélectionnez une ou plusieurs stratégies, puis cliquez sur **Évaluer** pour exécuter la stratégie en mode d'évaluation. Cela génère un rapport de conformité pour le jeu de cibles mais n'applique pas la conformité future et ne reconfigure pas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour les cibles qui ne sont pas conformes aux stratégies sélectionnées et ont des propriétés qui peuvent être reconfigurées par la Gestion basée sur des stratégies, vous pouvez imposer la conformité aux stratégies en cliquant sur **Appliquer**. Pour plus d'informations sur les options disponibles de la boîte de dialogue **Évaluer les stratégies** , consultez [Evaluate Policies Dialog Box, Policy Selection Page](../../relational-databases/policy-based-management/evaluate-policies-dialog-box-policy-selection-page.md), [Evaluate Policies Dialog Box, Evaluation Results Page](../../relational-databases/policy-based-management/evaluate-policies-dialog-box-evaluation-results-page.md)et [Results Detailed View Dialog Box](../../relational-databases/policy-based-management/results-detailed-view-dialog-box.md).  
  
3.  Lorsque vous avez terminé, cliquez sur **Fermer**.  
  
  
