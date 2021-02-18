---
description: Définir la réponse à une alerte
title: Définir la réponse à une alerte
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, alerts
- alerts [SQL Server], responding to
- responding to alerts
ms.assetid: c86ca6eb-c59f-46e9-bc32-d474e7c3b170
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 53787a3f08aab02d1a2fee4500570f4033b579fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342713"
---
# <a name="define-the-response-to-an-alert"></a>Définir la réponse à une alerte

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment définir la manière dont [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] répond à des alertes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitations et restrictions  
  
-   Les options du récepteur de radiomessagerie et **net send** seront supprimées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans une version future de [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser ces fonctionnalités dans une nouvelle tâche de développement et prévoyez de modifier les applications qui les utilisent actuellement.  
  
-   Remarque : SQL Server Agent doit être configuré pour utiliser la messagerie de base de données pour envoyer des notifications aux opérateurs par messagerie électronique ou radiomessagerie. Pour plus d'informations, consultez [Affecter des alertes à un opérateur](assign-alerts-to-an-operator.md).  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est un outil dont l'interface graphique permet de gérer facilement les travaux. Son utilisation est recommandée pour créer et gérer l'infrastructure des travaux.  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorisations  
Seuls les membres du rôle serveur fixe **sysadmin** peuvent définir la réponse à une alerte.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-define-the-response-to-an-alert"></a>Pour définir la réponse à une alerte  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) pour développer le serveur qui contient l'alerte sur laquelle vous souhaitez définir une réponse.  
  
2.  Cliquez sur le signe plus (+) pour développer **Agent SQL Server**.  
  
3.  Cliquez sur le signe plus (+) pour développer le dossier **Alertes** .  
  
4.  Cliquez avec le bouton droit sur l'alerte dont vous voulez définir une réponse, puis sélectionnez **Propriétés**.  
  
5.  Dans la boîte de dialogue **Propriétés de l’alerte**_nom\_alerte_, sous **Sélectionner une page**, sélectionnez **Réponse**.  
  
6.  Sélectionnez la case à cocher **Exécuter le travail** , puis dans la liste figurant sous la case à cocher **Exécuter le travail** , sélectionnez un travail à exécuter quand une alerte se produit. Vous pouvez créer un nouveau travail en cliquant sur **Nouveau travail**. Vous pouvez afficher plus d'informations sur le travail en cliquant sur **Afficher le travail**. Pour plus d’informations sur les options disponibles dans les boîtes de dialogue **Nouveau travail** et **Propriétés du travail**_nom\_travail_, consultez [Créer un travail](../../ssms/agent/create-a-job.md) et [Afficher un travail](../../ssms/agent/view-a-job.md).  
  
7.  Activez la case à cocher **Notifier les opérateurs** si vous souhaitez avertir les opérateurs lorsque l'alerte est activée. Dans **Liste d'opérateurs**, sélectionnez une ou plusieurs des méthodes suivantes pour notifier le ou les opérateurs : **Messagerie électronique**, **Radiomessagerie** ou **Net Send**. Vous pouvez créer un nouvel opérateur en cliquant sur **Nouvel opérateur**. Vous pouvez afficher plus d'informations sur un opérateur en cliquant sur **Afficher l'opérateur**. Pour plus d'informations sur les options disponibles dans les boîtes de dialogue **Nouvel opérateur** et **Afficher les propriétés de l'opérateur** , consultez [Create an Operator](../../ssms/agent/create-an-operator.md) et [View Information About an Operator](../../ssms/agent/view-information-about-an-operator.md).  
  
8.  Lorsque vous avez terminé, cliquez sur **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilisation de Transact-SQL  
  
#### <a name="to-define-the-response-to-an-alert"></a>Pour définir la réponse à une alerte  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- adds an e-mail notification for Test Alert.  
    -- assumes that Test Alert already exists and that
    -- François Ajenstat is a valid operator name   
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_notification  
     @alert_name = N'Test Alert',  
     @operator_name = N'François Ajenstat',  
     @notification_method = 1 ;  
    GO  
    ```  
  
Pour plus d’informations, consultez [sp_add_notification (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md).