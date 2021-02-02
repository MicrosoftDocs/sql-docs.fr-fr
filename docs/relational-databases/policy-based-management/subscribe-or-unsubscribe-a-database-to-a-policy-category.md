---
title: Abonner ou désabonner une base de données à une catégorie de stratégie
description: Explique comment abonner ou désabonner une base de données à une catégorie de stratégie dans le cadre de la gestion basée sur des stratégies à l’aide de SQL Server Management Studio et T-SQL (Transact-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.dmf.groupsubscription.f1
ms.assetid: d2c31769-7098-428e-ad9c-ef56541b7c52
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: deb83a831430dd6ad57bf50eca26b9e30c49f22c
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076211"
---
# <a name="subscribe-or-unsubscribe-a-database--to-a-policy-category"></a>Abonner une base de données ou annuler son abonnement à une catégorie de stratégie
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment abonner ou désabonner une base de données à une catégorie de stratégie dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour abonner ou annuler l'abonnement d'une base de données à une catégorie de stratégie à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe db_owner.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-subscribe-or-unsubscribe-a-database-to-a-policy-category"></a>Pour abonner ou annuler l'abonnement d'une base de données à une catégorie de stratégie  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) pour développer le serveur qui contient la base de données dont vous souhaitez gérer les abonnements aux catégories.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Bases de données** .  
  
3.  Cliquez avec le bouton droit sur la base de données dont vous souhaitez gérer les abonnements aux catégories, pointez sur **Stratégies**, puis sélectionnez **Catégories**.  
  
     Les options suivantes sont disponibles dans la boîte de dialogue **Catégories** :  
  
     Développer la colonne  
     Cliquez sur cette option pour développer une catégorie de stratégie. Celle-ci répertorie toutes les stratégies incluses dans la catégorie.  
  
     **Nom**  
     Nom de la catégorie de stratégie.  
  
     **Abonné**  
     Indique si la cible est abonnée à la catégorie de stratégie. Si cette case à cocher est désactivée, la catégorie de stratégie est définie pour **Abonnements à la base de données autorisée**. Cela signifie que la catégorie de stratégie s'applique à toutes les bases de données du serveur.  
  
     **Stratégie**  
     Lorsque des groupes de stratégies sont développés, affiche les stratégies appartenant à la catégorie de stratégie.  
  
     **Activé**  
     Indique si les stratégies sont activées ou désactivées.  
  
     **Mode d'exécution**  
     Indique le mode d'exécution de la stratégie.  
  
     **History**  
     Cliquez sur le lien hypertexte Afficher l'historique pour ouvrir la Visionneuse du fichier journal et consulter l'historique de stratégie.  
  
4.  Pour l’abonnement à une catégorie de gestion basée sur des stratégies, cochez la case de la catégorie dans la colonne **Abonné** . Pour annuler l'abonnement à une catégorie, désactivez la case à cocher.  
  
5.  Lorsque vous avez terminé, cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-subscribe-a-database-to-a-policy-category"></a>Pour abonner une base de données à une catégorie de stratégie  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    -- Adds a subscription to the 'Finance' policy category for the AdventureWorks2012 database.  
    EXEC sys.sp_syspolicy_subscribe_to_policy_category @policy_category = N'Finance';  
    GO  
    ```  
  
 Pour plus d’informations, consultez [sp_syspolicy_subscribe_to_policy_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-subscribe-to-policy-category-transact-sql.md).  
  
#### <a name="to-unsubscribe-a-database-to-a-policy-category"></a>Pour annuler l'abonnement d'une base de données à une catégorie de stratégie  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    -- Deletes a subscription to the 'Finance' policy category for the AdventureWorks2012 database.  
    EXEC sys.sp_syspolicy_unsubscribe_from_policy_category @policy_category = N'Finance';  
    GO  
    ```  
  
 Pour plus d’informations, consultez [sp_syspolicy_unsubscribe_from_policy_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-unsubscribe-from-policy-category-transact-sql.md).  
  
  
