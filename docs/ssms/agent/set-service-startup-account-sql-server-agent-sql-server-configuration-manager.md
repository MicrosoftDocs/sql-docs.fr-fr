---
description: Set the Service Startup Account for SQL Server Agent (SQL Server Configuration Manager)
title: Configurer le compte de démarrage du service
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, service accounts
- startup accounts [SQL Server]
- service startup accounts [SQL Server Agent]
ms.assetid: 46ffe818-ebb5-43a0-840b-923f219a2472
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 30944838b0ed66bc87bc49ff26fbba0efc05a683
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344703"
---
# <a name="set-the-service-startup-account-for-sql-server-agent-sql-server-configuration-manager"></a>Set the Service Startup Account for SQL Server Agent (SQL Server Configuration Manager)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Le compte de démarrage du service Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet de définir le compte Windows sous lequel s'exécute l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ainsi que ses autorisations réseau. Cette rubrique explique comment définir le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent à l'aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] avec [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitations et restrictions  
  
-   À compter de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'exige plus que le compte de démarrage du service soit membre du groupe Administrateurs [!INCLUDE[msCoName](../../includes/msconame_md.md)] . Toutefois, le compte de démarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être membre du rôle serveur fixe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]sysadmin. En cas d’utilisation du traitement de travaux multiserveur, le compte doit être membre du rôle de base de données msdb TargetServersRole sur le serveur maître.  
  
-   Cependant, l'Explorateur d'objets affiche le nœud de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent uniquement si vous avez l'autorisation de l'utiliser.  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorisations  
Pour exécuter ses fonctions, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être configuré pour utiliser les informations d'identification d'un compte qui est membre du rôle serveur fixe **sysadmin** dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le compte doit avoir les autorisations Windows suivantes :  
  
-   Ouvrir une session en tant que service (SeServiceLogonRight)  
  
-   Remplacer un jeton de niveau processus (SeAssignPrimaryTokenPrivilege)  
  
-   Outrepasser le contrôle de parcours (SeChangeNotifyPrivilege)  
  
-   Changer les quotas de mémoire d'un processus (SeIncreaseQuotaPrivilege)  
  
Pour plus d’informations sur les autorisations Windows nécessaires pour le compte de service de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, consultez [Sélectionner un compte pour le service SQL Server Agent](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md) et [Setting Up Windows Service Accounts](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)(Configuration des comptes de service Windows).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-set-the-service-startup-account-for-sql-server-agent"></a>Pour configurer le compte de démarrage du service pour l'Agent SQL Server  
  
1.  Dans **Serveurs inscrits**, cliquez sur le signe plus pour développer **Moteur de base de données**.  
  
2.  Cliquez sur le signe plus pour développer le dossier **Groupes de serveurs locaux** .  
  
3.  Cliquez avec le bouton droit sur l’instance de serveur où vous souhaitez configurer le compte de démarrage du service, puis sélectionnez **Gestionnaire de configuration SQL Server...**.  
  
4.  Dans la boîte de dialogue **Contrôle de compte d’utilisateur**, cliquez sur **Oui**.  
  
5.  Dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , sélectionnez **Services SQL Server** dans le volet de la console.  
  
6.  Dans le volet d’informations, cliquez avec le bouton droit sur **Agent SQL Server**_(nom\_serveur)_, où *nom_serveur* représente le nom de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent pour laquelle vous voulez changer le compte de démarrage du service, puis sélectionnez **Propriétés**.  
  
7.  Dans la boîte de dialogue **Propriétés** de **SQL Server Agent** _(nom\_serveur)_ , sous l’onglet **Se connecter**, sélectionnez l’une des options suivantes sous **Se connecter en tant que** :  
  
    -   **Compte intégré**: sélectionnez cette option si vos travaux nécessitent des ressources du serveur local uniquement. Pour plus d’informations sur la façon de choisir un type de compte intégré Windows, consultez [Sélection d’un compte pour le service SQL Server Agent](./select-an-account-for-the-sql-server-agent-service.md).  
  
        > [!IMPORTANT]  
        >  Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent ne prend pas en charge le compte **Service local** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
    -   **Ce compte**: sélectionnez cette option si vos travaux nécessitent des ressources sur le réseau, notamment des ressources d’application, si vous souhaitez transférer des événements vers d’autres journaux d’applications Windows, ou si vous souhaitez notifier des opérateurs par courrier électronique ou radiomessagerie.  
  
        Si vous sélectionnez cette option :  
  
        1.  Dans la zone **Nom du compte** , entrez le compte qui sera utilisé pour exécuter SQL Server Agent. Vous pouvez également cliquer sur **Parcourir** pour ouvrir la boîte de dialogue **Sélectionnez l'utilisateur ou le groupe** et sélectionner le compte à utiliser.  
  
        2.  Dans la zone **Mot de passe** , entrez le mot de passe du compte. Dans la zone **Confirmer le mot de passe** , retapez le mot de passe.  
  
8.  Cliquez sur **OK**.  
  
9. Cliquez sur le bouton **Fermer**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le Gestionnaire de configuration.  
