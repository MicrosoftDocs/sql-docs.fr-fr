---
title: Voir ou modifier les propriétés d’une condition de gestion basée sur des stratégies
description: Découvrez comment voir ou modifier les propriétés d’une condition de gestion basée sur des stratégies à l’aide de SSMS (SQL Server Management Studio) ou de T-SQL (Transact-SQL).
ms.custom: ''
ms.date: 10/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, view policy conditions
- Policy-Based Management, modify policy conditions
ms.assetid: 890d7384-8444-4767-bb6f-f5debb155747
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ffdb11c75726469d61c54c9a2cc12ff18a31ca1a
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076181"
---
# <a name="view-or-modify-the-properties-of-a-policy-based-management-condition"></a>Afficher ou modifier les propriétés d'une condition de gestion basée sur des stratégies
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment afficher ou modifier les propriétés d'une condition de gestion basée sur des stratégies dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  

  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'appartenance au rôle PolicyAdministratorRole dans la base de données msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-view-or-modify-a-conditions-properties"></a>Pour afficher ou modifier les propriétés d'une condition  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) pour développer le serveur qui contient la condition que vous souhaitez afficher ou modifier.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Gestion** .  
  
3.  Cliquez sur le signe plus (+) pour développer **Gestion de la stratégie**.  
  
4.  Cliquez sur le signe plus (+) pour développer le dossier **Conditions** .  
  
5.  Cliquez avec le bouton droit sur la condition à afficher ou supprimer, puis sélectionnez **Propriétés**. Pour plus d’informations sur les options disponibles dans la boîte de dialogue **Ouvrir une condition -** _nom_condition_ consultez [Boîte de dialogue Créer une nouvelle condition ou Ouvrir une condition, page Général](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-general-page.md), [Boîte de dialogue Ouvrir une condition, page Stratégies dépendantes](../../relational-databases/policy-based-management/open-condition-dialog-box-dependent-policies-page.md), [Boîte de dialogue Créer une nouvelle condition ou Ouvrir une condition, page Description](../../relational-databases/policy-based-management/create-new-condition-or-open-condition-dialog-box-description-page.md) et [Boîte de dialogue Modification avancée &#40;Condition&#41;](../../relational-databases/policy-based-management/advanced-edit-condition-dialog-box.md).  
  
6.  Lorsque vous avez terminé, cliquez sur **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-view-a-conditions-properties"></a>Pour afficher les propriétés d'une condition  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE msdb;  
    GO  
    SELECT name,  
       description,  
       facet,  
       expression,  
       is_name_condition,  
       obj_name  
    FROM syspolicy_conditions;  
    GO  
  
    ```  
  
 Pour plus d’informations, consultez [syspolicy_conditions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syspolicy-conditions-transact-sql.md).  
  
  
