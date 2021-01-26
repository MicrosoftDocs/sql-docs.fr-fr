---
title: Configurer l’option de configuration de serveur priority boost | Microsoft Docs
description: Découvrez l’option priority boost. Découvrez comment l’utiliser pour définir la base de priorité pour SQL Server dans le planificateur Windows 2008 ou Windows Server 2008 R2.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- priority boost option
ms.assetid: 765f1e83-dd52-44fb-b0c8-1078f213607b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c8889fd32fc0d494260bf73e790cdb3a428201d4
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783622"
---
# <a name="configure-the-priority-boost-server-configuration-option"></a>Configurer l'option de configuration de serveur priority boost
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment configurer l'option de configuration **priority boost** dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Utilisez l’option **priority boost** pour spécifier si [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être exécuté avec un degré plus élevé de priorité de planification [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 2008 ou Windows 2008 R2 que pour d’autres processus exécutés sur le même ordinateur. Si la valeur de cette option est égale à 1, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bénéficie d'une base de priorité de 13 dans le planificateur de Windows 2008 ou Windows Server 2008 R2. La valeur par défaut est 0, ce qui représente une base de priorité de 7.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour configurer l'option priority boost, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir configuré l’option priority boost](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Si vous attribuez une valeur trop élevée à la priorité, vous risquez d'épuiser les ressources affectées aux fonctions réseau et aux fonctions du système d'exploitation, ce qui peut entraîner des problèmes lors de l'arrêt de SQL Server ou de l'utilisation d'autres tâches du système d'exploitation sur le serveur.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-the-priority-boost-option"></a>Pour configurer l'option priority boost  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur et sélectionnez **Propriétés**.  
  
2.  Cliquez sur le nœud **Processeurs** .  
  
3.  Sous **Threads**, activez la case à cocher **Renforcer la priorité SQL Server** .  
  
4.  Arrêtez puis redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-the-priority-boost-option"></a>Pour configurer l'option priority boost  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour attribuer à l’option `priority boost` la valeur `1`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'priority boost', 1 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 Pour plus d’informations, consultez [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-priority-boost-option"></a><a name="FollowUp"></a> Suivi : Après avoir configuré l’option priority boost  
 Le serveur doit être redémarré pour que le paramètre puisse être effet.  
  
## <a name="see-also"></a>Voir aussi  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
