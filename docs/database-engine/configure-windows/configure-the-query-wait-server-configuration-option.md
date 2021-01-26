---
title: Configurer l’option de configuration de serveur query wait | Microsoft Docs
description: Découvrez l’option query wait. Découvrez comment l’utiliser pour spécifier le nombre de secondes pendant lesquelles une requête SQL Server attend des ressources avant d’expirer.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- queries [SQL Server], timing out
- time [SQL Server], query wait time
- query wait option [SQL Server]
ms.assetid: 0fc4aa01-65a3-4a33-9ef4-caca41add238
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 15844802836883d80c4bf50d0df58e7ddbe633d0
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783156"
---
# <a name="configure-the-query-wait-server-configuration-option"></a>Configurer l'option de configuration de serveur query wait
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment configurer l'option de configuration de serveur **query wait** dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Les requêtes utilisant beaucoup de mémoire (par exemple les requêtes incluant des opérations de tri et de hachage) sont mises en attente si la quantité de mémoire est insuffisante pour leur exécution. L’option **query wait** spécifie le délai (exprimé en secondes, de 0 à 2147483647) pendant lequel une requête peut attendre des ressources avant d’expirer. La valeur par défaut de cette option est -1. Cela signifie que le délai d'attente est calculé comme étant 25 fois le coût estimé de la requête.  
  
> [!IMPORTANT]  
>  Une transaction contenant la requête en attente peut contenir des verrous en attendant une quantité supplémentaire de mémoire. Dans de rares situations, il est possible qu'un blocage indétectable se produise. En réduisant la durée d’attente de la requête, vous réduisez également la probabilité de déclenchement de ces verrous. À la fin de celle-ci, une requête en attente est arrêtée et les verrous de la transaction sont libérés. Cependant, l'augmentation de la durée d'attente maximale risque d'augmenter la durée nécessaire pour l'achèvement de la requête. Il est déconseillé de modifier cette option.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour configurer l'option query wait, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir configuré l’option query wait](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Seul un administrateur de base de données qualifié ou un spécialiste agréé doit changer cette option avancée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-the-query-wait-option"></a>Pour configurer l'option query wait  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur et sélectionnez **Propriétés**.  
  
2.  Cliquez sur le nœud **Avancé** .  
  
3.  Sous **Parallélisme**, tapez la valeur de votre choix pour l'option **query wait** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-the-query-wait-option"></a>Pour configurer l'option query wait  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour attribuer à l’option `query wait` la valeur `7500` secondes.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'query wait', 7500 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 Pour plus d’informations, consultez [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-query-wait-option"></a><a name="FollowUp"></a> Suivi : Après avoir configuré l’option query wait  
 Le paramètre prend effet immédiatement sans redémarrage du serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
