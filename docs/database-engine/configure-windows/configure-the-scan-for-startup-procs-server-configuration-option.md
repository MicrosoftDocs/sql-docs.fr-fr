---
title: Configurer l’option de configuration de serveur scan for startup procs | Microsoft Docs
description: Découvrez l'option « scan for startup procs ». Découvrez comment elle spécifie si SQL Server recherche et exécute toutes les procédures stockées exécutées automatiquement définies sur le serveur.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- scan for startup procs option
ms.assetid: 6bf9d252-e766-458d-9dcd-23d895f032a2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 00426f84d2f8b3c0ca4626ffd8bddc234f27cf4d
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783174"
---
# <a name="configure-the-scan-for-startup-procs-server-configuration-option"></a>Configurer l'option de configuration de serveur scan for startup procs
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment configurer l'option de configuration de serveur **scan for startup procs** dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Utilisez l'option **scan for startup procs** pour rechercher les procédures stockées à exécution automatique au moment du démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si cette option a la valeur 1, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recherche et exécute toutes les procédures stockées à exécution automatique définies sur le serveur. La valeur par défaut de l’option **Recherche des procédures de démarrage** est égale à 0 (pas de recherche).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour configurer l'option scan for startup procs, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir configuré l’option scan for startup procs](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Seul un administrateur de base de données qualifié ou un spécialiste agréé doit changer cette option avancée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   La valeur de cette option peut être définie à l’aide de **sp_configure**; cependant, elle est automatiquement définie si vous utilisez **sp_procoption**, qui permet de sélectionner ou non l’exécution automatique des procédures stockées. Quand vous utilisez **sp_procoption** pour signaler que la première procédure stockée est une procédure à exécution automatique, cette option prend automatiquement la valeur 1. Lorsque vous utilisez **sp_procoption** pour désactiver l’exécution automatique, cette option prend automatiquement la valeur 0. Si vous utilisez **sp_procoption** pour activer ou désactiver l’exécution automatique des procédures et si vous supprimez systématiquement l’exécution automatique avant de supprimer les procédures correspondantes, il est inutile de définir manuellement cette option.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-the-scan-for-startup-procs-option"></a>Pour configurer l'option scan for startup procs  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur et sélectionnez **Propriétés**.  
  
2.  Cliquez sur le nœud **Avancé** .  
  
3.  Sous **Divers**, attribuez à l’option **Recherche des procédures de démarrage** la valeur True ou False en sélectionnant la valeur de votre choix dans la zone de liste déroulante.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-the-scan-for-startup-procs-option"></a>Pour configurer l'option scan for startup procs  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour attribuer à l’option `scan for startup procs` la valeur `1`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1 ;  
GO  
RECONFIGURE  
GO  
EXEC sp_configure 'scan for startup procs', 1 ;  
GO  
RECONFIGURE  
GO  
  
```  
  
##  <a name="follow-up-after-you-configure-the-scan-for-startup-procs-option"></a><a name="FollowUp"></a> Suivi : Après avoir configuré l’option scan for startup procs  
 Le serveur doit être redémarré pour que le paramètre puisse être effet.  
  
## <a name="see-also"></a>Voir aussi  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [sp_procoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md)  
  
  
