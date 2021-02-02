---
title: Configurer les propriétés générales de la gestion basée sur des stratégies
description: Découvrez comment configurer les propriétés de la gestion basée sur des stratégies à l’aide de SSMS (SQL Server Management Studio) ou de T-SQL (Transact-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.dmf.PolicyManagement.f1
helpviewer_keywords:
- Policy-Based Management, configure properties
ms.assetid: 6d1e0e37-29ea-408a-a055-384984d884be
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 453ed74f518c248caac2540540eefbba3c1718b5
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048832"
---
# <a name="configure-the-general-properties-of-policy-based-management"></a>Configurer les propriétés générales de la gestion basée sur des stratégies
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment configurer les propriétés de la gestion basée sur des stratégies dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour configurer la gestion basée sur des stratégies à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe PolicyAdministratorRole.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-policy-based-management"></a>Pour configurer la Gestion basée sur des stratégies  
  
1.  Dans l’ **Explorateur d’objets**, cliquez sur le signe plus (+) pour développer le serveur sur lequel vous voulez configurer les propriétés de la gestion basée sur des stratégies.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Gestion** .  
  
3.  Cliquez avec le bouton droit sur **Gestion de la stratégie** , puis sélectionnez **Propriétés**.  
  
     Les options suivantes sont disponibles dans la boîte de dialogue **Propriétés de gestion de la stratégie** .  
  
     **Activé**  
     Spécifie si la Gestion basée sur des stratégies est activée.  
  
     **HistoryRetentionInDays**  
     Spécifie le nombre de jours pendant lesquels l'historique des évaluations de stratégies doit être conservé. Si cette valeur est 0 (valeur par défaut), l'historique n'est pas supprimé automatiquement.  
  
     **LogOnSuccess**  
     Spécifie si la Gestion basée sur des stratégies consigne les évaluations de stratégies réussies.  
  
    -   Lorsque cette valeur est False (valeur par défaut), seules les évaluations de stratégies qui ont échoué sont consignées.  
  
    -   Lorsque cette valeur est True, les réussites et les échecs des évaluations de stratégies sont enregistrés.  
  
4.  Lorsque vous avez terminé, cliquez sur **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-policy-based-management"></a>Pour configurer la Gestion basée sur des stratégies  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- enables Policy-Based Management   
    USE msdb;  
    GO  
    EXEC dbo.sp_syspolicy_configure   
         @name = N'Enabled',   
         @value = 1;  
    GO  
    ```  
  
 Pour plus d’informations, consultez [sp_syspolicy_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-configure-transact-sql.md).  
  
  
