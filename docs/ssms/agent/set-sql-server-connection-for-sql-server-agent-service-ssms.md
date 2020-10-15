---
description: Définir la connexion SQL Server pour le service SQL Server Agent
title: Définir la connexion SQL Server pour le service SQL Server Agent
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, connections
- connections [SQL Server], SQL Server Agent service
ms.assetid: 28b6178b-0a9e-4f2c-8562-7a62d2d2a285
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 9ac8349306d78075de65e3a14989d2f68261393f
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92037780"
---
# <a name="set-the-sql-server-connection-for-the-sql-server-agent-service"></a>Définir la connexion SQL Server pour le service SQL Server Agent

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique décrit la configuration de la connexion entre l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le [!INCLUDE[ssDE](../../includes/ssde_md.md)] dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] au moyen de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Le service de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut se connecter à une instance locale de SQL Server en utilisant l'authentification Windows.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitations et restrictions  
  
-   Cependant, l'Explorateur d'objets affiche le nœud de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent uniquement si vous avez l'autorisation de l'utiliser.  
  
-   À compter de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent ne prend pas en charge l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette option est disponible uniquement lorsque vous administrez une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorisations  
Pour exécuter ses fonctions, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être configuré pour utiliser les informations d'identification d'un compte qui est membre du rôle serveur fixe **sysadmin** dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le compte doit avoir les autorisations Windows suivantes :  
  
-   Ouvrir une session en tant que service (SeServiceLogonRight)  
  
-   Remplacer un jeton de niveau processus (SeAssignPrimaryTokenPrivilege)  
  
-   Outrepasser le contrôle de parcours (SeChangeNotifyPrivilege)  
  
-   Changer les quotas de mémoire d'un processus (SeIncreaseQuotaPrivilege)  
  
Pour plus d’informations sur les autorisations Windows nécessaires pour le compte de service de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, consultez [Sélectionner un compte pour le service SQL Server Agent](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md) et [Setting Up Windows Service Accounts](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)(Configuration des comptes de service Windows).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-set-the-sql-server-connection"></a>Pour configurer la connexion à SQL Server  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) pour développer le serveur que vous souhaitez configurer avec une connexion à son service SQL Server Agent.  
  
2.  Cliquez avec le bouton droit sur **SQL Server Agent** , puis sélectionnez **Propriétés**.  
  
3.  Dans la boîte de dialogue **Propriétés de SQL Server Agent**, sous **Sélectionner une page**, cliquez sur **Connexion**.  
  
4.  Sous **Connexion SQL Server**, sélectionnez **Utiliser l'authentification Windows** pour permettre à l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de se connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde_md.md)] avec [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Authentication. Les connexions aux bases de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures nécessitent l'authentification Windows.  
