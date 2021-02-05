---
description: Notify an Operator of Job Status
title: Notify an Operator of Job Status
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- status information [SQL Server], jobs
- jobs [SQL Server Agent], notification options
- SQL Server Agent jobs, status
- jobs [SQL Server Agent], status
- SQL Server Agent jobs, notification options
- notifications [SQL Server], job status
ms.assetid: e7399505-27ac-48d9-a637-73bf92b9df49
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: b220d8810491f5693110e26c67d79310e048f76c
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251336"
---
# <a name="notify-an-operator-of-job-status"></a>Notify an Operator of Job Status
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment définir des options de notification dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] ou SQL Server Management Objects, de sorte que [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent puisse envoyer des notifications aux opérateurs à propos des travaux.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
Pour plus d'informations, consultez [Implémenter la sécurité de SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-notify-an-operator-of-job-status"></a>Pour notifier l'état d'un travail à un opérateur  
  
1.  Dans **l’Explorateur d'objets** , connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]et développez-la.  
  
2.  Développez **Agent SQL Server**, puis **Travaux**, cliquez avec le bouton droit sur le travail à modifier et sélectionnez **Propriétés**.  
  
3.  Dans la boîte de dialogue **Propriétés du travail** , sélectionnez la page **Notifications** .  
  
4.  Pour avertir un opérateur par e-mail, cochez la case **Messagerie électronique**, sélectionnez un opérateur dans la liste, puis l’une des options suivantes :  
  
    -   **Lors de la réussite du travail** pour avertir l'opérateur quand le travail s'est effectué avec succès ;  
  
    -   **Lors de l'échec du travail** pour avertir l'opérateur quand le travail a échoué.  
  
    -   **Lorsque le travail est terminé** pour avertir l'opérateur, quel que soit l'état du travail à son achèvement.  
  
5.  Pour avertir un opérateur par radiomessagerie, activez la case à cocher **Radiomessagerie**, sélectionnez un opérateur dans la liste, puis l'une des options suivantes :  
  
    -   **Lors de la réussite du travail** pour avertir l'opérateur quand le travail s'est effectué avec succès ;  
  
    -   **Lors de l'échec du travail** pour avertir l'opérateur quand le travail a échoué.  
  
    -   **Lorsque le travail est terminé** pour avertir l'opérateur, quel que soit l'état du travail à son achèvement.  
  
6.  Pour avertir un opérateur par envoi réseau, activez la case à cocher **Envoi réseau**, sélectionnez un opérateur dans la liste, puis l'une des options suivantes :  
  
    -   **Lors de la réussite du travail** pour avertir l'opérateur quand le travail s'est effectué avec succès ;  
  
    -   **Lors de l'échec du travail** pour avertir l'opérateur quand le travail a échoué.  
  
    -   **Lorsque le travail est terminé** pour avertir l'opérateur, quel que soit l'état du travail à son achèvement.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Utilisation de Transact-SQL  
  
#### <a name="to-notify-an-operator-of-job-status"></a>Pour notifier l'état d'un travail à un opérateur  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- adds an e-mail notification for the specified alert (Test Alert).  
    -- This example assumes that Test Alert already exists
    --  and that François Ajenstat is a valid operator name.  
    USE msdb ;  
    GO  
    EXEC dbo.sp_add_notification   
    @alert_name = N'Test Alert',   
    @operator_name = N'François Ajenstat',   
    @notification_method = 1 ;  
    GO  
    ```  
  
Pour plus d’informations, consultez [sp_add_notification (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Utilisation de SQL Server Management Objects  
**Pour notifier l'état d'un travail à un opérateur**  
  
Utilisez la classe **Job** à l’aide du langage de programmation de votre choix, tel que Visual Basic, Visual C# ou PowerShell. Pour plus d’informations, consultez [SQL Server Management Objects (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md).  
