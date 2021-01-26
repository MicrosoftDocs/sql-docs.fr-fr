---
title: Configurer l’option de configuration de serveur media retention | Microsoft Docs
description: Découvrez l'option de rétention du support Découvrez comment l’utiliser pour spécifier la durée pendant laquelle SQL Server conserve les sauvegardes du journal des transactions et de la base de données.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- backup retention duration [SQL Server]
- backup sets [SQL Server], retention duration
- media retention option
ms.assetid: 12e9fe6a-20a5-4c6e-9cc9-d500c003b70a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5f2d67ee55cf43b607aeb91d23fd42980b3438a1
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783658"
---
# <a name="configure-the-media-retention-server-configuration-option"></a>Configurer l'option de configuration de serveur media retention
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment configurer l'option de configuration de serveur **media retention** dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'option **media retention** spécifie la durée de rétention de chaque jeu de sauvegarde. Cette option permet d'éviter l'écrasement des sauvegardes pendant le nombre de jours spécifié. Après avoir configuré l'option **media retention**, il est inutile de spécifier la durée de rétention des sauvegardes système à chaque sauvegarde. La valeur par défaut est de 0 jour et la valeur maximum de 365 jours.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour configurer l'option media retention, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir configuré l’option media retention](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Si vous utilisez le support de sauvegarde avant que le nombre de jours spécifié soit écoulé, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un message d'avertissement. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne génère aucun avertissement, sauf si vous modifiez la valeur par défaut.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Seul un administrateur de base de données qualifié ou un spécialiste agréé doit changer cette option avancée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   L'option **media retention** peut être annulée par l'utilisation de la clause RETAINDAYS de l'instruction [BACKUP](../../t-sql/statements/backup-transact-sql.md) .  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-the-media-retention-option"></a>Pour configurer l'option Rétention du support  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur et sélectionnez **Propriétés**.  
  
2.  Cliquez sur le nœud **Paramètres de base de données** .  
  
3.  Sous **Sauvegarde et restauration**, dans la zone **Délai de rétention par défaut du support de sauvegarde (jours)** , entrez ou sélectionnez une valeur comprise entre 0 et 365 pour définir le nombre de jours pendant lesquels le support de sauvegarde sera conservé après une sauvegarde de la base de données ou du journal des transactions.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-the-media-retention-option"></a>Pour configurer l'option Rétention du support  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour attribuer à l’option `media retention` la valeur `60` jours.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'media retention', 60 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 Pour plus d’informations, consultez [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-media-retention-option"></a><a name="FollowUp"></a> Suivi : Après avoir configuré l’option media retention  
 Le paramètre prend effet immédiatement sans redémarrage du serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
