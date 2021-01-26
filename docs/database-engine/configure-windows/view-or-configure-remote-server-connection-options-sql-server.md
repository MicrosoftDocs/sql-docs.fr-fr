---
title: Afficher ou configurer des options de connexion au serveur distant (SQL Server) | Microsoft Docs
description: Découvrez comment afficher ou configurer des options de connexion au serveur distant au niveau du serveur. Vous pouvez utiliser SQL Server Management Studio ou Transact-SQL pour effectuer cette tâche.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- remote servers [SQL Server], connection options
- servers [SQL Server], remote
- connections [SQL Server], remote servers
ms.assetid: 356d3e6b-8514-4bd2-a683-9de147949b2b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7505c7ce47f0804f2c9cf83951a5d47847edf18b
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783021"
---
# <a name="view-or-configure-remote-server-connection-options-sql-server"></a>Afficher ou configurer des options de connexion au serveur distant (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment afficher ou configurer des options de connexion au serveur distant au niveau du serveur dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour afficher ou configurer des options de connexion au serveur distant, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir configuré des options de connexion au serveur distant](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 L’exécution de **sp_serveroption** nécessite l’autorisation ALTER ANY LINKED SERVER sur le serveur.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-view-or-configure-remote-server-connection-options"></a>Pour afficher ou configurer des options de connexion au serveur distant  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur, puis cliquez sur **Propriétés**.  
  
2.  Dans la boîte de dialogue **Propriétés de SQL Server - \<**_server_name_**>** , cliquez sur **Connexions**.  
  
3.  Sur la page **Connexions** , vérifiez les paramètres **Connexions au serveur distant** et au besoin modifiez-les.  
  
4.  Répétez les étapes 1 à 3 sur l'autre serveur de la paire de serveurs distants.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-view-remote-server-connection-options"></a>Pour afficher les options de connexion au serveur distant  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple utilise [sp_helpserver](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md) pour retourner des informations sur tous les serveurs distants.  
  
```sql  
USE master;  
GO  
EXEC sp_helpserver ;  
```  
  
#### <a name="to-configure-remote-server-connection-options"></a>Pour configurer des options de connexion au serveur distant  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_serveroption](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md) pour configurer un serveur distant. Cet exemple configure un serveur distant correspondant à une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], `SEATTLE3`, de façon à ce qu'il soit compatible avec le classement de l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
USE master;  
EXEC sp_serveroption 'SEATTLE3', 'collation compatible', 'true';  
```  
  
##  <a name="follow-up-after-you-configure-remote-server-connection-options"></a><a name="FollowUp"></a> Suivi : Après avoir configuré des options de connexion au serveur distant  
 Le serveur distant doit être arrêté et redémarré pour que le paramètre puisse prendre effet.  
  
## <a name="see-also"></a>Voir aussi  
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [Serveurs distants](../../database-engine/configure-windows/remote-servers.md)   
 [Serveurs liés &#40;moteur de base de données&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md)   
 [sp_linkedservers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)  
  
  
