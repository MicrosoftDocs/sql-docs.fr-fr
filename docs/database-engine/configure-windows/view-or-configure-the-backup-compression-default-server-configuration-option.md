---
title: Afficher ou configurer l’option de configuration de serveur backup compression default | Microsoft Docs
description: Découvrez l'option « backup compression default ». Découvrez comment elle détermine si SQL Server crée des sauvegardes compressées par défaut et comment la définir.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], backup compression default option
- backup compression [SQL Server], backup compression default Option
ms.assetid: 23029395-3e93-4c29-b7d6-e5a47a3526ff
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ae3f7070ade723af31ed191d76dd274c4137a115
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783566"
---
# <a name="view-or-configure-the-backup-compression-default-server-configuration-option"></a>Afficher ou configurer l'option de configuration de serveur backup compression default
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment afficher ou configurer l’option de configuration de serveur **backup compression default** dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. L’option **valeur par défaut de compression de la sauvegarde** détermine si l’instance de serveur crée des sauvegardes compressées par défaut. Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé, l’option **valeur par défaut de compression de la sauvegarde** est désactivée.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour afficher ou configurer l'option valeur par défaut de compression de la sauvegarde, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir configuré l’option Compression par défaut des sauvegardes](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   La compression de la sauvegarde n'est pas disponible dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   Par défaut, la compression augmente considérablement l'utilisation de l'UC et l'UC supplémentaire consommée par le processus de compression peut avoir un impact néfaste sur les opérations simultanées. Par conséquent, il peut être préférable de créer une sauvegarde compressée de priorité basse dans une session où l’utilisation de l’UC est limitée par [Resource Governor](../../relational-databases/resource-governor/resource-governor.md). Pour plus d'informations, consultez [Utiliser Resource Governor pour limiter l’utilisation de l’UC par compression de la sauvegarde &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Lors de la création d'une sauvegarde individuelle, de la configuration de la copie des journaux de transaction ou de la création d'un plan de maintenance, vous pouvez remplacer la valeur par défaut au niveau du serveur.  
  
-   La compression de la sauvegarde est prise en charge pour les unités de sauvegarde sur disque et les périphériques de sauvegarde sur bande.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-view-or-configure-the-backup-compression-default-option"></a>Pour afficher ou configurer l'option backup compression default  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur et sélectionnez **Propriétés**.  
  
2.  Cliquez sur le nœud **Paramètres de base de données** .  
  
3.  Sous **Sauvegarde et restauration**, **Compresser la sauvegarde** affiche le paramètre actuel de l’option **valeur par défaut de compression de la sauvegarde** . Ce paramètre détermine la valeur par défaut au niveau du serveur pour la compression des sauvegardes, comme suit :  
  
    -   Si la case à cocher **Compresser la sauvegarde** est désactivée, les nouvelles sauvegardes ne sont pas compressées par défaut.  
  
    -   Si la case à cocher **Compresser la sauvegarde** est activée, les nouvelles sauvegardes sont compressées par défaut.  
  
     Si vous êtes membre du rôle serveur fixe **sysadmin** ou **serveradmin** , vous pouvez également modifier le paramètre par défaut en activant la case à cocher **Compresser la sauvegarde** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-view-the-backup-compression-default-option"></a>Pour afficher l'option backup compression default  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L'exemple suivant interroge la vue de catalogue [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) pour déterminer la valeur de `backup compression default`. La valeur 0 indique que la compression de la sauvegarde est désactivée, et la valeur 1 indique que la compression de la sauvegarde est activée.  
  
```sql  
SELECT value   
FROM sys.configurations   
WHERE name = 'backup compression default' ;  
GO  
```  
  
#### <a name="to-configure-the-backup-compression-default-option"></a>Pour configurer l'option backup compression default  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour configurer l’instance de serveur afin de créer des sauvegardes compressées par défaut.  
  
```sql  
EXEC sp_configure 'backup compression default', 1 ;  
RECONFIGURE;  
GO 
```  
  
 Pour plus d’informations, consultez [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-backup-compression-default-option"></a><a name="FollowUp"></a> Suivi : Après avoir configuré l’option Compression par défaut des sauvegardes  
 Le paramètre prend effet immédiatement sans redémarrage du serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)  
  
  

