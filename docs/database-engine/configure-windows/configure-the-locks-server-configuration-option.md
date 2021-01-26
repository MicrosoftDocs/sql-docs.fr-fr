---
title: Configurer l’option de configuration de serveur locks | Microsoft Docs
description: En savoir plus sur l’option locks. Découvrez comment l’utiliser pour limiter la quantité de mémoire que le moteur de base de données SQL Server utilise pour les verrous.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- locks option [SQL Server]
ms.assetid: b0cf0f86-7652-4574-a9fb-908e10d03973
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a30c2cb2955ef82380bde00714098afec153ed95
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783686"
---
# <a name="configure-the-locks-server-configuration-option"></a>Configurer l'option de configuration de serveur locks
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment configurer l'option de configuration de serveur **locks** dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'option **locks** définit le nombre maximum de verrous disponibles, limitant ainsi la quantité de mémoire utilisée pour eux par le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . La valeur par défaut est 0 ; elle permet au [!INCLUDE[ssDE](../../includes/ssde-md.md)] d'allouer et de libérer des structures de verrous de manière dynamique en fonction des modifications de la configuration requise.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour définir l'option locks, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir configuré l’option locks](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Seul un administrateur de base de données qualifié ou un spécialiste agréé doit changer cette option avancée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Lorsque le serveur est démarré alors que la valeur 0 est attribuée à l'option **Verrous** , le gestionnaire de verrous acquiert suffisamment de mémoire auprès du [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour un pool initial de 2 500 structures de verrous. Lorsque ce pool est épuisé, le gestionnaire de verrous redemande de la mémoire.  
  
     En règle générale, si une quantité de mémoire supérieure à celle du pool de mémoires du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est requise pour le pool de verrous, et s’il reste de la mémoire sur l’ordinateur (c’est-à-dire si le seuil de l’option **Mémoire maximum du serveur** n’a pas été atteint), le [!INCLUDE[ssDE](../../includes/ssde-md.md)] alloue dynamiquement de la mémoire afin de satisfaire la demande de verrous. Cependant, si l'allocation de mémoire entraîne une pagination au niveau du système d'exploitation (par exemple, si une autre application est exécutée sur le même ordinateur en tant qu'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et utilise cette mémoire), aucun espace supplémentaire n'est alloué pour les verrous. Le pool de verrous dynamiques n’acquiert pas plus de 60 pour cent de la mémoire allouée au [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Une fois que le pool de verrous a atteint 60 pour cent de la mémoire acquise par une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)], ou s’il ne reste plus de mémoire disponible sur l’ordinateur, les autres demandes de verrous génèrent une erreur.  
  
     La configuration recommandée est l'autorisation de l'utilisation dynamique des verrous par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cependant, vous pouvez définir l'option **locks** et empêcher [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'allouer des ressources de verrous de façon dynamique. Quand l’option **locks** a une valeur différente de 0, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne peut pas allouer plus de verrous que ce **nombre**. Augmentez cette valeur si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche un message vous informant que vous avez dépassé le nombre de verrous disponibles. Puisque chaque verrou consomme de la mémoire (96 octets par verrou), il est possible que l’augmentation de cette valeur vous oblige à augmenter la mémoire destinée au serveur.  
  
-   L'option **locks** influence également le moment où a lieu l'escalade de verrous. Quand l’option **locks** a la valeur 0, l’escalade de verrous se produit quand la mémoire utilisée par les structures de verrous actuelles atteint 40 pour cent du pool de mémoire du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Lorsque l'option **locks** a une valeur différente de 0, l'escalade de verrous intervient quand le nombre de verrous atteint 40 pour cent de la valeur spécifiée pour **Verrous**.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-the-locks-option"></a>Pour définir l'option locks  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur et sélectionnez **Propriétés**.  
  
2.  Cliquez sur le nœud **Avancé** .  
  
3.  Sous **Parallelism**, entrez la valeur appropriée de l'option **locks** .  
  
     Utilisez l'option **locks** pour définir le nombre maximal de verrous disponibles et limiter ainsi la quantité de mémoire qu'utilise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec les verrous.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-the-locks-option"></a>Pour définir l'option locks  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour attribuer à l’option `locks` de définition du nombre de verrous disponibles pour tous les utilisateurs la valeur `20000`.  
  
```sql  
Use AdventureWorks2012 ;  
GO  
sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE;  
GO  
sp_configure 'locks', 20000;  
GO  
RECONFIGURE;  
GO  
```  
  
 Pour plus d’informations, consultez [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-locks-option"></a><a name="FollowUp"></a> Suivi : Après avoir configuré l’option locks  
 Le serveur doit être redémarré pour que le paramètre puisse être effet.  
  
## <a name="see-also"></a>Voir aussi  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
