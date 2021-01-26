---
title: Configurer l’option de configuration de serveur nested triggers | Microsoft Docs
description: Apprenez-en davantage sur l’option Déclencheurs imbriqués. Découvrez comment l’utiliser pour définir le nombre de niveaux de déclencheurs AFTER pouvant être exécutés en cascade dans SQL Server.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- nested triggers option
ms.assetid: 29d7372b-d406-4a5b-80c6-a2d231d25211
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5c5330bad05b0949c4df32f5593aa3fe39e2ad50
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783639"
---
# <a name="configure-the-nested-triggers-server-configuration-option"></a>Configurer l'option de configuration de serveur nested triggers
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment configurer l'option de configuration de serveur **nested triggers** dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'option **Déclencheurs imbriqués** contrôle si un déclencheur AFTER peut s'exécuter en cascade, autrement dit, effectuer une action qui active un autre déclencheur, qui active un autre déclencheur, et ainsi de suite. Lorsque l'option **nested triggers** a la valeur 0, les déclencheurs AFTER ne peuvent pas s'exécuter en cascade. Quand l’option **nested triggers** a la valeur 1 (valeur par défaut), les déclencheurs AFTER peuvent s’exécuter en cascade sur un maximum de 32 niveaux. Les déclencheurs INSTEAD OF peuvent être imbriqués quel que soit le paramétrage de cette option.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour configurer l'option nested triggers, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir configuré l’option nested triggers](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-the-nested-triggers-option"></a>Pour configurer l’option nested triggers  
  
1.  Dans **l’Explorateur d’objets**, cliquez avec le bouton droit sur un serveur, puis sélectionnez **Propriétés**.  
  
2.  Dans la page **Avancées** , attribuez à l’option **Autoriser les déclencheurs à activer d’autres déclencheurs** la valeur **True** (valeur par défaut) ou **False**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-the-nested-triggers-option"></a>Pour configurer l’option nested triggers  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour attribuer à l’option `nested triggers` la valeur `0`.  
  
```wmimof  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'nested triggers', 0 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 Pour plus d’informations, consultez [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-nested-triggers-option"></a><a name="FollowUp"></a> Suivi : Après avoir configuré l’option nested triggers  
 Le paramètre prend effet immédiatement sans redémarrage du serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer des déclencheurs imbriqués](../../relational-databases/triggers/create-nested-triggers.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
