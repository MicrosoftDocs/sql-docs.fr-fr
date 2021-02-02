---
title: Voir les propriétés d’une facette de la gestion basée sur des stratégies
description: Découvrez comment voir les propriétés d’une facette de la gestion basée sur des stratégies dans SQL Server à l’aide de SSMS (SQL Server Management Studio).
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, view facet properties
ms.assetid: 022a244c-c2e7-4467-b9a2-c7a27859be22
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 97f5cbfccd7cc086163b73642d0c6ec3e928427a
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076191"
---
# <a name="view-the-properties-of-a-policy-based-management-facet"></a>Afficher les propriétés d'une facette de gestion basée sur des stratégies
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment afficher les propriétés d'une facette de gestion basée sur des stratégies dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour afficher les propriétés d'une facette à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'appartenance au rôle PolicyAdministratorRole dans la base de données msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-view-the-properties-of-a-facet"></a>Pour afficher les propriétés d'une facette  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) pour développer le serveur qui contient la facette dont vous souhaiter afficher les propriétés.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Gestion** .  
  
3.  Cliquez sur le signe plus (+) pour développer **Gestion de la stratégie**.  
  
4.  Cliquez sur le signe plus (+) pour développer le dossier **Facettes** .  
  
5.  Cliquez avec le bouton droit sur la facette dont vous souhaitez afficher les propriétés, puis sélectionnez **Propriétés**. Pour plus d’informations sur les options disponibles dans la boîte de dialogue **Propriétés de la facette -** _nom_facette_, consultez [Boîte de dialogue Propriétés de la facette, page Général](../../relational-databases/policy-based-management/facet-properties-dialog-box-general-page.md), [Boîte de dialogue Propriétés de la facette, page Stratégies dépendantes](../../relational-databases/policy-based-management/facet-properties-dialog-box-dependent-policies-page.md) et [Boîte de dialogue Propriétés de la facette, page Conditions dépendantes](../../relational-databases/policy-based-management/facet-properties-dialog-box-dependent-conditions-page.md).  
  
6.  Lorsque vous avez terminé, cliquez sur **Fermer**.  

