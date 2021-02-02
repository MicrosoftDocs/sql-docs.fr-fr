---
title: Voir ou modifier les propriétés d’une stratégie de la gestion basée sur des stratégies
description: Découvrez comment voir ou modifier les propriétés d’une stratégie de la gestion basée sur des stratégies pour SQL Server à l’aide de SSMS (SQL Server Management Studio) ou de T-SQL (Transact-SQL).
ms.custom: seo-lt-2019
ms.date: 10/06/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, modify policies
- Policy-Based Management, view policies
ms.assetid: ba805504-5db5-4731-a8da-a0e89cb20c37
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 1947e5d6b1363cfab43607882f09988074d25eed
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076987"
---
# <a name="view-or-modify-the-properties-of-a-policy-based-management-policy"></a>Afficher ou modifier les propriétés d'une stratégie de gestion basée sur des stratégies
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique décrit comment afficher ou modifier les propriétés d'une stratégie de gestion basée sur des stratégies dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'appartenance au rôle PolicyAdministratorRole dans la base de données msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-view-the-properties-of-all-policies-on-an-object"></a>Pour afficher les propriétés de toutes les stratégies sur un objet  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur, un objet de serveur, une base de données ou un objet de base de données, pointez sur **Stratégies** et sélectionnez **Afficher**. Pour plus d’informations sur les options disponibles dans la boîte de dialogue **Afficher les stratégies -** _nom_objet_, consultez [Boîte de dialogue Afficher les stratégies](../../relational-databases/policy-based-management/view-policies-dialog-box.md).  
  
2.  Lorsque vous avez terminé, cliquez sur **Fermer**.  
  
#### <a name="to-view-or-modify-a-specific-policys-properties"></a>Pour afficher ou modifier les propriétés d'une stratégie spécifique  
  
1.  Dans **l’Explorateur d’objets**, cliquez sur le signe plus (+) pour développer la stratégie de gestion basée sur des stratégies que vous souhaitez afficher ou modifier.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Gestion** .  
  
3.  Cliquez sur le signe plus (+) pour développer **Gestion de la stratégie**.  
  
4.  Cliquez sur le signe plus (+) pour développer le dossier **Stratégies** .  
  
5.  Cliquez avec le bouton droit sur la stratégie que vous souhaitez afficher ou modifier, puis sélectionnez **Propriétés**. Pour plus d’informations sur les options disponibles dans la boîte de dialogue **Créer une nouvelle stratégie -** _nom_stratégie_, consultez [Boîte de dialogue Créer une nouvelle stratégie ou Ouvrir une stratégie, page Général](../../relational-databases/policy-based-management/create-new-policy-or-open-policy-dialog-box-general-page.md) et [Boîte de dialogue Créer une nouvelle stratégie ou Ouvrir une stratégie, page Description](../../relational-databases/policy-based-management/create-new-policy-or-open-policy-dialog-box-description-page.md).  
  
6.  Lorsque vous avez terminé, cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-view-a-policys-properties"></a>Pour afficher les propriétés d'une stratégie  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE msdb;  
    GO  
    SELECT name,  
       execution_mode,  
       description,  
       is_enabled,  
       job_id  
    FROM syspolicy_policies;  
    GO  
    ```  
  
 Pour plus d’informations, consultez [syspolicy_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syspolicy-policies-transact-sql.md).  
  
  
