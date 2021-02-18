---
description: Create a SQL Server Agent Proxy
title: Create a SQL Server Agent Proxy
ms.custom: seo-lt-2019
ms.date: 05/04/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- proxies [SQL Server Agent], creating
ms.assetid: 142e0c55-a8b9-4669-be49-b9dc602d5988
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 7967f1a56b6198123178407930eba4c986e098a1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271706"
---
# <a name="create-a-sql-server-agent-proxy"></a>Create a SQL Server Agent Proxy
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment créer un proxy de SQL Server Agent dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
Un compte proxy de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définit un contexte de sécurité dans lequel une étape de travail peut être exécutée. Chaque proxy correspond à des informations d'identification de sécurité. Pour définir des autorisations pour une étape de travail particulière, créez un proxy possédant les autorisations requises pour un sous-système de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puis assignez ce proxy à l'étape de travail.  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitations et restrictions  
  
-   Vous devez créer des informations d'identification avant de créer un proxy si elles ne sont pas disponibles.  
  
-   Les proxys de l'Agent[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisent les informations d'identification pour stocker les informations relatives aux comptes d'utilisateur Windows. L’utilisateur spécifié dans les informations d’identification doit avoir l’autorisation « Accéder à cet ordinateur à partir du réseau » (SeNetworkLogonRight) sur l’ordinateur où [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vérifie l'accès au sous-système pour un proxy et donne accès au proxy à chaque exécution de l'étape de travail. Si le proxy n'a plus accès au sous-système, l'étape de travail échoue. Sinon, l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] emprunte l'identité de l'utilisateur spécifié dans le proxy et exécute l'étape de travail.  
  
-   La création d'un proxy ne modifie pas les autorisations de l'utilisateur spécifié dans l'information d'identification du proxy. Par exemple, vous pouvez créer un proxy pour un utilisateur qui n'est pas autorisé à se connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Dans ce cas, les étapes de travail qui utilisent ce proxy ne sont pas en mesure de se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Si la connexion de l'utilisateur a accès au proxy ou que l'utilisateur appartient à un rôle qui y a accès, l'utilisateur peut recourir au proxy dans une étape de travail.  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorisations  
  
-   Seuls les membres du rôle de serveur fixe **sysadmin** ont l'autorisation de créer, modifier ou supprimer des comptes proxy. Les utilisateurs qui ne sont pas des membres du rôle serveur fixe **sysadmin** doivent être ajoutés à l'un des rôles de bases de données fixes Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suivants dans la base de données **msdb** pour utiliser des proxys : **SQLAgentUserRole**, **SQLAgentReaderRole** ou **SQLAgentOperatorRole**.  
  
-   Nécessite l’autorisation **ALTER ANY CREDENTIAL** lors de la création d’informations d’identification en plus du proxy.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-a-sql-server-agent-proxy"></a>Pour créer un proxy de SQL Server Agent  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) pour développer le serveur sur lequel vous souhaitez créer un proxy sur SQL Server Agent.  
  
2.  Cliquez sur le signe plus (+) pour développer **Agent SQL Server**.  
  
3.  Cliquez avec le bouton droit sur le dossier **Proxys** et sélectionnez **Nouveau proxy**.  
  
4.  Dans la boîte de dialogue **Nouveau compte proxy** , sur la page **Général** , entrez le nom du compte proxy dans la zone **Nom du proxy** .  
  
5.  Dans la zone **Nom d'identification** , entrez le nom des informations d'identification de sécurité que le compte proxy utilisera.  
  
6.  Dans la zone **Description** , entrez une description du compte proxy  
  
7.  Sous **Actif pour les sous-systèmes suivants**, sélectionnez le ou les sous-systèmes appropriés pour ce proxy.  
  
8.  Sur la page **Principaux** , ajoutez ou supprimez des connexions ou des rôles pour accorder ou refuser un accès au compte proxy.  
  
9. Lorsque vous avez terminé, cliquez sur **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilisation de Transact-SQL  
  
#### <a name="to-create-a-sql-server-agent-proxy"></a>Pour créer un proxy de SQL Server Agent  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- creates credential CatalogApplicationCredential  
    USE msdb ;  
    GO  
    CREATE CREDENTIAL CatalogApplicationCredential WITH IDENTITY = 'REDMOND/TestUser',   
        SECRET = 'G3$1o)lkJ8HNd!';  
    GO  
    -- creates proxy "Catalog application proxy" and assigns
    -- the credential 'CatalogApplicationCredential' to it.  
    EXEC dbo.sp_add_proxy  
        @proxy_name = 'Catalog application proxy',  
        @enabled = 1,  
        @description = 'Maintenance tasks on catalog application.',  
        @credential_name = 'CatalogApplicationCredential' ;  
    GO  
    -- grants the proxy "Catalog application proxy" access to 
    -- the ActiveX Scripting subsystem.  
    EXEC dbo.sp_grant_proxy_to_subsystem  
        @proxy_name = N'Catalog application proxy',  
        @subsystem_id = 2 ;  
    GO  
    ```  
  
Pour plus d'informations, consultez les pages suivantes :  
  
-   [CREATE CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-credential-transact-sql.md)  
  
-   [sp_add_proxy (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)  
  
-   [sp_grant_proxy_to_subsystem (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grant-proxy-to-subsystem-transact-sql.md)  
