---
title: Afficher ou modifier des propriétés de serveur (SQL Server) | Microsoft Docs
description: Découvrez comment utiliser SQL Server Management Studio, Transact-SQL ou le Gestionnaire de configuration SQL Server pour afficher ou modifier les propriétés d’une instance de SQL Server.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.connectionproperties.f1
helpviewer_keywords:
- viewing server properties
- server properties [SQL Server]
- displaying server properties
- servers [SQL Server], viewing
- Connection Properties dialog box
ms.assetid: 55f3ac04-5626-4ad2-96bd-a1f1b079659d
author: markingmyname
ms.author: maghan
ms.custom: contperf-fy20q4
ms.openlocfilehash: aabe0f1c418440053998ce1a1b388e82d74b06c9
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878862"
---
# <a name="view-or-change-server-properties-sql-server"></a>Afficher ou modifier des propriétés de serveur (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment afficher ou modifier les propriétés d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]ou SQL Server Configuration Manager.  

Les étapes dépendent de l’outil :
+ [SQL Server Management Studio](#SSMSProcedure)  
+ [Transact-SQL](#TsqlProcedure)  
+ [Gestionnaire de configuration SQL Server](#PowerShellProcedure)  
    
## <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Lorsque vous utilisez sp_configure, vous devez exécuter RECONFIGURE ou RECONFIGURE WITH OVERRIDE après la définition d'une option de configuration. L'instruction RECONFIGURE WITH OVERRIDE est généralement réservée aux options de configuration qui doivent être utilisées avec une extrême prudence. Cependant, RECONFIGURE WITH OVERRIDE fonctionne avec toutes les options de configuration, et vous pouvez l'utiliser pour remplacer RECONFIGURE.  
  
    > [!NOTE]  
    >  RECONFIGURE s'exécute au sein d'une transaction. Si l'une des opérations de reconfiguration échoue, aucune de ces opérations ne prend effet.  
  
-   Certaines pages de propriétés présentent des informations fournies par l'intermédiaire de WMI (Windows Management Instrumentation). Pour afficher ces pages, WMI doit être installé sur l'ordinateur qui exécute [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="server-level-roles"></a><a name="Security"></a> Rôles de niveau serveur  
  
Pour plus d’informations, consultez [Rôles de niveau serveur](../../relational-databases/security/authentication-access/server-level-roles.md).  
  
Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
## <a name="sql-server-management-studio"></a><a name="SSMSProcedure"></a>SQL Server Management Studio  
  
#### <a name="to-view-or-change-server-properties"></a>Pour afficher ou modifier des propriétés de serveur  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur, puis cliquez sur **Propriétés**.  
  
2.  Dans la boîte de dialogue **Propriétés du serveur** , cliquez sur une page pour afficher ou modifier les informations du serveur relatives à cette page. Certaines propriétés sont en lecture seule.  
  
##  <a name="transact-sql"></a><a name="TsqlProcedure"></a>Transact-SQL  
  
### <a name="to-view-server-properties-by-using-the-serverproperty-built-in-function"></a>Pour afficher les propriétés du serveur à l'aide de la fonction intégrée SERVERPROPERTY  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple utilise la fonction intégrée [SERVERPROPERTY](../../t-sql/functions/serverproperty-transact-sql.md) dans une instruction `SELECT` pour retourner des informations sur le serveur actif. Ce scénario est utile lorsque plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont installées sur un serveur basé sur Windows et que le client doit ouvrir une autre connexion vers l'instance utilisée par la connexion actuelle.  
  
    ```sql  
    SELECT CONVERT( sysname, SERVERPROPERTY('servername'));  
    GO  
    ```  
  
### <a name="to-view-server-properties-by-using-the-sysservers-catalog-view"></a>Pour afficher les propriétés du serveur à l'aide de la vue de catalogue sys.servers  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L’exemple suivant interroge l’affichage catalogue [sys.servers](../../relational-databases/system-catalog-views/sys-servers-transact-sql.md) pour retourner le nom (`name`) et l’ID (`server_id`) du serveur actuel, ainsi que le nom du fournisseur OLE DB (`provider`) pour vous connecter à un serveur lié.  
  
    ```sql  
    USE AdventureWorks2012;   
    GO  
    SELECT name, server_id, provider  
    FROM sys.servers ;   
    GO  
  
    ```  
  
### <a name="to-view-server-properties-by-using-the-sysconfigurations-catalog-view"></a>Pour afficher les propriétés du serveur à l'aide de la vue de catalogue sys.configurations  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L'exemple suivant interroge la vue de catalogue [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) pour retourner des informations sur chaque option de configuration du serveur sur le serveur actuel. L'exemple retourne le nom (`name`) et la description (`description`) de l'option et indique si l'option est une option avancée (`is_advanced`).  
  
    ```wmimof  
    USE AdventureWorks2012;   
    GO  
    SELECT name, description, is_advanced  
    FROM sys.configurations ;   
    GO  
  
    ```  
  
### <a name="to-change-a-server-property-by-using-sp_configure"></a>Pour modifier une propriété de serveur à l'aide de sp_configure  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour modifier une propriété de serveur. L'exemple modifie la valeur de l'option `fill factor` sur `100`. Le serveur doit être redémarré pour appliquer le changement.  
  
```sql  
Use AdventureWorks2012;  
GO  
sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE;  
GO  
sp_configure 'fill factor', 100;  
GO  
RECONFIGURE;  
GO  
```  
  
 Pour plus d’informations, consultez [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
## <a name="sql-server-configuration-manager"></a><a name="PowerShellProcedure"></a>Gestionnaire de configuration SQL Server  
 Certaines propriétés du serveur peuvent être affichées ou modifiées à l'aide du gestionnaire de configuration SQL Server. Par exemple, vous pouvez afficher la version et l'édition de l'instance de SQL Server, ou modifier l'emplacement où les fichiers des journaux d'erreurs sont stockés. Vous pouvez aussi afficher ces propriétés en interrogeant les [fonctions et vues de gestion dynamique relatives au serveur](../../relational-databases/system-dynamic-management-views/server-related-dynamic-management-views-and-functions-transact-sql.md).  
  
### <a name="to-view-or-change-server-properties"></a>Pour afficher ou modifier des propriétés de serveur  
  
1.  Dans le menu **Démarrer** , pointez sur **Tous les programmes**, sur [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]et sur **Outils de configuration**, puis cliquez sur **Gestionnaire de configuration SQL Server**.  
  
2.  Dans le **Gestionnaire de configuration SQL Server**, cliquez sur **Services SQL Server**.  
  
3.  Dans le volet d’informations, cliquez avec le bouton droit sur **SQL Server (\<**_instancename_**>)** , puis cliquez sur **Propriétés**.  
  
4.  Dans la boîte de dialogue **Propriétés de SQL Server (\<**_instancename_**>)** , modifiez les propriétés du serveur sous l’onglet **Service** ou **Avancé**, puis cliquez sur **OK**.  
  
## <a name="restart-after-changes"></a><a name="FollowUp"></a>Redémarrer après les modifications

Pour certaines propriétés, vous devrez peut-être redémarrer le serveur pour que la modification puisse prendre effet.  
  
## <a name="next-steps"></a>Étapes suivantes  
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Configurer WMI pour afficher l’état du serveur dans les outils SQL Server](../../ssms/configure-wmi-to-show-server-status-in-sql-server-tools.md)   
 [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md)   
 [Fonctions de configuration &#40;Transact-SQL&#41;](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [Fonctions et vues de gestion dynamique relatives au serveur &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/server-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  
