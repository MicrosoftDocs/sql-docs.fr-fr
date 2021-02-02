---
description: Évaluer une stratégie de gestion basée sur des stratégies sur une planification
title: Évaluer une stratégie de gestion basée sur des stratégies sur une planification | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, evaluate policy
ms.assetid: bea09522-ff47-4037-ab55-a98ea7c10099
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 971a5107d09977a5ae731417c59625e2764bd6bd
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049002"
---
# <a name="evaluate-a-policy-based-management-policy-on-a-schedule"></a>Évaluer une stratégie de gestion basée sur des stratégies sur une planification
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment évaluer une stratégie de gestion basée sur des stratégies sur une planification dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour évaluer une stratégie sur une planification à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'appartenance au rôle PolicyAdministratorRole dans la base de données msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-evaluate-a-policy-on-a-schedule"></a>Pour évaluer une stratégie sur une planification  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) pour développer le serveur qui contient la planification que vous souhaitez évaluer.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Gestion** .  
  
3.  Cliquez sur le signe plus (+) pour développer **Gestion de la stratégie**.  
  
4.  Cliquez sur le signe plus (+) pour développer le dossier **Stratégies** .  
  
5.  Cliquez avec le bouton droit sur la stratégie dont vous voulez évaluer une planification, puis sélectionnez **Propriétés**.  
  
6.  Dans la boîte de dialogue **Ouvrir une stratégie -** _nom_stratégie_, dans la liste **Mode d’évaluation**, sélectionnez **Selon la planification**.  
  
7.  Sous **Planification**, cliquez sur **Choisir** pour spécifier une planification existante ou sur **Nouveau** pour créer une planification.  
  
8.  Lorsque vous avez terminé, cliquez sur **OK**.  
  
  
