---
description: Modify a SQL Server Agent Proxy
title: Modify a SQL Server Agent Proxy
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- proxies [SQL Server Agent], modifying
- modifying SQL Server Agent proxy
ms.assetid: 6e1dfbaa-8089-4813-940c-d5a2e13d8552
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 06/03/2020
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: a17783d110990a1a6812e3d58df25cb50d574071
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251337"
---
# <a name="modify-a-sql-server-agent-proxy"></a>Modify a SQL Server Agent Proxy

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment modifier un proxy [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Agent dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitations et restrictions  
  
-   Les proxys de l'Agent[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisent les informations d'identification pour stocker les informations relatives aux comptes d'utilisateur Windows. L'utilisateur spécifié dans l'information d'identification doit être habilité à ouvrir une session en tant que programme de traitement par lots sur l'ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d'exécution.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vérifie l'accès au sous-système pour un proxy et donne accès au proxy à chaque exécution de l'étape de travail. Si le proxy n'a plus accès au sous-système, l'étape de travail échoue. Sinon, l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] emprunte l'identité de l'utilisateur spécifié dans le proxy et exécute l'étape de travail.  
  
-   Si la connexion de l'utilisateur a accès au proxy ou que l'utilisateur appartient à un rôle qui y a accès, l'utilisateur peut recourir au proxy dans une étape de travail.  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorisations  
Seuls les membres du rôle serveur fixe **sysadmin** peuvent créer, modifier ou supprimer des comptes proxy.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-modify-a-ssnoversion-agent-proxy"></a>Pour modifier un proxy de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1.  Dans **Explorateur d'objets**, cliquez sur le signe plus pour développer le serveur qui contient le compte proxy de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous souhaitez modifier.  
  
2.  Cliquez sur le signe plus (+) pour développer **Agent SQL Server**.  
  
3.  Cliquez sur le signe plus (+) pour développer le dossier **Proxys** .  
  
4.  Cliquez sur le signe plus pour développer le nœud du sous-système du proxy ( **Script ActiveX**, par exemple).  
  
5.  Cliquez avec le bouton droit sur le compte proxy que vous voulez modifier, puis sélectionnez **Propriétés**.  
  
6.  Dans la boîte de dialogue **Propriétés du compte proxy**_nom\_proxy_, apportez des modifications au compte proxy selon les besoins. Pour plus d’informations sur les options de cette boîte de dialogue, consultez [Créer un proxy de SQL Server Agent](../../ssms/agent/create-a-sql-server-agent-proxy.md).  
  
7.  Lorsque vous avez terminé, cliquez sur **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilisation de Transact-SQL  
  
#### <a name="to-modify-a-ssnoversion-agent-proxy"></a>Pour modifier un proxy de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```sql
    -- Disables the proxy named 'Catalog application proxy'.  
    USE msdb ;  
    GO  
    EXEC dbo.sp_update_proxy  
        @proxy_name = 'Catalog application proxy',  
        @enabled = 0;  
    GO  
    ```  
  
Pour plus d’informations, consultez [sp_update_proxy (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-proxy-transact-sql.md).  
