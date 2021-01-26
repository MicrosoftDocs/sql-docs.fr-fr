---
title: Configurer l’option de configuration du serveur remote access | Microsoft Docs
description: Découvrez des alternatives à l’option deprecated remote access. Affichez d’autres sources pour résoudre des problèmes de connexions SQL Server.
ms.custom: ''
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- remote servers [SQL Server], stored procedure execution
ms.assetid: f5de748d-1c55-4714-9661-38fe62e5095f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: afe6721bef097dfa941b5578b6d4287fc370c3f2
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783125"
---
# <a name="configure-the-remote-access-server-configuration-option"></a>Configurer l'option de configuration du serveur remote access
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique concerne la fonctionnalité remote access. Cette option de configuration est une fonctionnalité de communication peu intelligible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Elle est dépréciée et son utilisation n’est pas recommandée. Si vous êtes sur cette page en raison de problèmes de connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez plutôt l’une des rubriques suivantes :  
  
-   [Tutoriel : Bien démarrer avec le moteur de base de données](../../relational-databases/tutorial-getting-started-with-the-database-engine.md)  
  
-   [Connexion à SQL Server](../../database-engine/configure-windows/logging-in-to-sql-server.md)  
  
-   [Se connecter à SQL Server lorsque les administrateurs système n'y ont plus accès](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)  
  
-   [Se connecter à un serveur inscrit &#40;SQL Server Management Studio&#41;](../../ssms/register-servers/connect-to-a-registered-server-sql-server-management-studio.md)  
  
-   [Se connecter à n'importe quel composant de SQL Server à partir de SQL Server Management Studio](../../ssms/f1-help/connect-to-any-sql-server-component-from-sql-server-management-studio.md)  
  
-   [Se connecter au moteur de base de données avec sqlcmd](../../ssms/scripting/sqlcmd-connect-to-the-database-engine.md)  
  
-   [Comment faire pour résoudre les problème de connexion au moteur de base de données SQL Server](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)  
  
 Les programmeurs peuvent s’intéresser aux rubriques suivantes :  
  
-   [Guide pratique pour établir une connexion à SQL Server à l’aide de l’authentification SQL dans ASP.NET 2.0](/previous-versions/msp-n-p/ff648340(v=pandp.10))  
  
-   [Connexion à une instance de SQL Server](../../relational-databases/server-management-objects-smo/create-program/connecting-to-an-instance-of-sql-server.md)  
  
-   [Procédure : créer des connexions à des bases de données SQL Server](/previous-versions/visualstudio/visual-studio-2008/s4yys16a(v=vs.90))  
  
 **Le corps de cette rubrique commence ici.**  
  
 Cette rubrique explique comment configurer l'option de configuration de serveur **remote access** dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'option **remote access** contrôle l'exécution des procédures stockées depuis les serveurs locaux ou distants sur lesquels des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont en cours d'exécution. La valeur par défaut de cette option est 1. Cette valeur autorise l'exécution des procédures stockées locales depuis des serveurs distants, ou des procédures stockées distantes depuis le serveur local. Pour empêcher l'exécution des procédures stockées locales depuis un serveur distant ou des procédures stockées distantes depuis le serveur local, attribuez la valeur 0 à cette option.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Utilisez de préférence [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour configurer l'option remote access, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir configuré l’option remote access](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   L’option **remote access** ne s’applique qu’aux serveurs ajoutés au moyen de [sp_addserver](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)et n’est incluse que pour des raisons de compatibilité descendante.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-the-remote-access-option"></a>Pour configurer l'option remote access  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur et sélectionnez **Propriétés**.  
  
2.  Cliquez sur le nœud **Connexions** .  
  
3.  Sous **Connexions au serveur distant**, activez ou désactivez la case à cocher **Autoriser les accès distants à ce serveur** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-the-remote-access-option"></a>Pour configurer l'option remote access  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour attribuer à l’option `remote access` la valeur `0`.  
  
```sql  
EXEC sp_configure 'remote access', 0 ;  
GO  
RECONFIGURE ;  
GO  
  
```  
  
 Pour plus d’informations, consultez [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-remote-access-option"></a><a name="FollowUp"></a> Suivi : Après avoir configuré l’option remote access  
 SQL Server doit être redémarré pour que ce paramètre prenne effet.  
  
## <a name="see-also"></a>Voir aussi  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
