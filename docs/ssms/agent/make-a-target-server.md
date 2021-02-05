---
description: Créer un serveur cible
title: Créer un serveur cible
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.tsxwiz.complete.f1
- sql13.ag.tsxwiz.cover.f1
- sql13.ag.tsxwiz.credentials.f1
- sql13.ag.tsxwiz.msx.f1
helpviewer_keywords:
- Target Server Wizard
- SQL Server Agent jobs, target servers
- target servers [SQL Server], creating
ms.assetid: 13aabe2d-67fe-4c67-8d49-2928dd705b7a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 06/03/2020
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 93e9a3247cdbb06d20b531e6dcdb3a383dc14529
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251048"
---
# <a name="make-a-target-server"></a>Créer un serveur cible
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment créer un serveur cible dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou d'objets SMO (SQL Server Management Objects).  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
Les travaux distribués dont les étapes sont associées à un proxy sont exécutés dans le contexte du compte proxy du serveur cible. Assurez-vous que les conditions suivantes sont remplies ou que les étapes de travail associées à un proxy ne seront pas téléchargées du serveur maître vers la cible :  
  
-   La sous-clé de Registre du serveur maître **\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<&#42;nom_instance&#42;>\SQL Server Agent\AllowDownloadedJobsToMatchProxyName** (REG_DWORD) a pour valeur 1 (true). Par défaut, la valeur de cette sous-clé est 0 (False).  
  
-   Il existe sur le serveur cible un compte proxy possédant le même nom que le compte proxy du serveur maître sur lequel l'étape du travail est exécutée.  
  
Si les étapes du travail utilisant des comptes proxy échouent pendant leur téléchargement à partir du serveur maître vers le serveur cible, vous pouvez vérifier la colonne **error_message** dans la table **sysdownloadlist** de la base de données **msdb** pour les messages d’erreur suivants :  
  
-   « L'étape du travail nécessite un compte proxy, cependant la mise en correspondance de proxy est désactivée sur le serveur cible. »  
  
    Pour corriger ce problème, affectez à la sous-clé de Registre **AllowDownloadedJobsToMatchProxyName** la valeur 1.  
  
-   « Proxy introuvable. »  
  
    Pour résoudre ce problème, vérifiez qu'un compte proxy portant le même nom que le compte proxy du serveur maître sous lequel l'étape s'exécute existe sur le serveur cible.  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorisations  
Les autorisations d'exécution de cette procédure sont accordées par défaut aux membres du rôle de serveur fixe **sysadmin** .  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-make-a-target-server"></a>Pour créer un serveur cible  
  
1.  Dans **l’Explorateur d'objets**, connectez-vous à une instance [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)] et développez-la.  
  
2.  Cliquez avec le bouton droit sur **Agent SQL Server**, pointez sur **Administration multiserveur**, puis cliquez sur **Transformer en serveur cible**. L' **Assistant Création d'un serveur cible** vous aide à créer un serveur cible.  
  
3.  Dans la page **Sélectionner un serveur maître** , sélectionnez le serveur maître à partir duquel ce serveur cible va recevoir des travaux.  
  
    **Choisir le serveur**  
    Connectez-vous au serveur maître.  
  
    **Description de ce serveur**  
    Tapez une description de ce serveur cible. Le serveur cible télécharge cette description sur le serveur maître.  
  
4.  Dans la page **Infos d'identification de connexion du serveur maître** , créez une nouvelle connexion sur le serveur cible, si nécessaire.  
  
    **Créer une nouvelle connexion si nécessaire et lui attribuer des droits sur le serveur MSX**  
    Créez une nouvelle connexion sur le serveur cible si la connexion spécifiée n'existe pas encore.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilisation de Transact-SQL  
  
#### <a name="to-make-a-target-server"></a>Pour créer un serveur cible  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple inscrit le serveur actuel dans le serveur maître AdventureWorks1. L'emplacement du serveur actuel est Building 21, Room 309, Rack 5.  
  
    ```sql
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_msx_enlist N'AdventureWorks1',   
        N'Building 21, Room 309, Rack 5' ;   
    GO
    ```  
  
    Pour plus d’informations, consultez [sp_msx_enlist (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-msx-enlist-transact-sql.md).  
  
## <a name="see-also"></a> Voir aussi  
[Administration automatisée à l'échelle d'une entreprise](../../ssms/agent/automated-administration-across-an-enterprise.md)  
