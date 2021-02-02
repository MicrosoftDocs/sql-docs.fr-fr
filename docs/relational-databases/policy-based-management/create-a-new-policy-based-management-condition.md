---
description: Créer une condition de gestion basée sur des stratégies.
title: Créer une condition de gestion basée sur des stratégies | Microsoft Docs
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, creating policy conditions
ms.assetid: 8a612f7e-6c70-49db-a4de-48431e097cc5
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f8eef68a189d9e3c3c8cce16de803ce3b87f0207
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048814"
---
# <a name="create-a-new-policy-based-management-condition"></a>Créer une condition de gestion basée sur des stratégies.
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment créer une condition de gestion basée sur des stratégies dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour créer une condition à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'appartenance au rôle PolicyAdministratorRole dans la base de données msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-a-condition"></a>Pour créer une condition  
  
1.  Dans l’ **Explorateur d’objets**, cliquez sur le signe plus (+) pour développer le serveur sur lequel vous souhaitez créer une condition de gestion basée sur des stratégies.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Gestion** .  
  
3.  Cliquez sur le signe plus (+) pour développer **Gestion de la stratégie**.  
  
4.  Cliquez sur le signe plus (+) pour développer le dossier **Facettes** .  
  
5.  Cliquez avec le bouton droit sur la facette dans laquelle vous souhaitez créer une nouvelle condition et sélectionnez **Nouvelle condition**.  
  
6.  Dans la boîte de dialogue **Créer une nouvelle condition** , dans la zone **Nom** , tapez le nom de la nouvelle condition.  
  
7.  Confirmez la facette correcte dans la liste **Facette** ou sélectionnez une facette différente.  
  
8.  Sous **Expression**, construisez des expressions de condition en sélectionnant une propriété de facette dans la zone **Champ** , avec son opérateur et sa valeur associés. Lorsque vous ajoutez plusieurs expressions, celles-ci peuvent être jointes à l'aide de **Et** ou **Ou**. Pour plus d’informations sur les options disponibles dans cette boîte de dialogue, consultez [Boîte de dialogue Créer une nouvelle condition ou Ouvrir une condition, page Général](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-general-page.md), [Boîte de dialogue Créer une nouvelle condition ou Ouvrir une condition, page Description](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-description-page.md) et [Boîte de dialogue Modification avancée &#40;Condition&#41;](../../relational-databases/policy-based-management/advanced-edit-condition-dialog-box.md).  
  
9. Lorsque vous avez terminé, cliquez sur **OK**.  
  
  
