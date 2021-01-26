---
title: Configurer l’option de configuration de serveur cost threshold for parallelism | Microsoft Docs
description: Découvrez l'option cost threshold for parallelism. Découvrez comment sa valeur détermine si SQL Server exécute des plans parallèles pour les requêtes et comment la définir.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- cost threshold for parallelism option
ms.assetid: dad21bee-fe28-41f6-9d2f-e6ababfaf9db
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ea4681fd02065129c4b565d0e9dbb744a321f9ae
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783299"
---
# <a name="configure-the-cost-threshold-for-parallelism-server-configuration-option"></a>Configurer l'option de configuration de serveur cost threshold for parallelism
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment configurer l'option de configuration de serveur **cost threshold for parallelism** dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'option **cost threshold for parallelism** spécifie le seuil de création et d'exécution des plans parallèles par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée et exécute un plan parallèle pour une requête uniquement lorsque le coût estimé de l'exécution d'un plan en série pour la même requête est supérieur à la valeur définie dans **Seuil de coût pour le parallélisme**. Ce coût est une estimation pour l’exécution du plan de série sur une configuration matérielle spécifique. Ce n’est pas une unité de temps. L'option **cost threshold for parallelism** peut prendre toute valeur comprise entre 0 et 32 767. La valeur par défaut est 5.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour configurer l'option cost threshold for parallelism, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir configuré l’option cost threshold for parallelism](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Ce coût est une unité de coût abstraite et non une unité de temps estimé. Spécifiez **cost threshold for parallelism** uniquement sur des multiprocesseurs symétriques.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ignore la valeur de l'option **cost threshold for parallelism** dans les conditions suivantes :  
  
    -   L'ordinateur ne dispose que d'un seul processeur logique.  
  
    -   Un seul processeur logique est disponible pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en raison de l’option de configuration **affinity mask** .  
  
    -   L'option de degré maximal de parallélisme **max degree of parallelism** a la valeur 1.  
  
Un processeur logique correspond à l'unité de base d'un processeur qui permet au système d'exploitation de distribuer une tâche ou d'exécuter un contexte de thread. Chaque processeur logique peut exécuter uniquement un contexte de thread à la fois. Le noyau du processeur désigne le circuit qui permet de décoder et d'exécuter des instructions. Un noyau de processeur peut contenir un ou plusieurs processeurs logiques. La requête [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante peut être utilisée pour obtenir les informations de l'UC pour le système.  
  
```sql  
SELECT (cpu_count / hyperthread_ratio) AS PhysicalCPUs,   
cpu_count AS logicalCPUs   
FROM sys.dm_os_sys_info  
```  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Seul un administrateur de base de données qualifié ou un spécialiste agréé doit changer cette option avancée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Dans certains cas, un plan parallèle peut être choisi, même si le plan de coût de la requête est inférieur à la valeur actuelle de l'option **Seuil de coût pour le parallélisme** . Cela s’explique par le fait que la décision d’utiliser un plan parallèle ou un plan en série est basée sur une estimation de coût fournie au début du processus d’optimisation. Pour plus d’informations, consultez le [Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md#parallel-query-processing).  

-   Même si la valeur par défaut (5) convient pour la plupart des systèmes, il est possible que vous ayez besoin de définir une valeur différente. Effectuez des tests d’application avec des valeurs supérieures et inférieures si nécessaire, afin d’optimiser les performances de l’application.
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-the-cost-threshold-for-parallelism-option"></a>Pour configurer l'option cost threshold for parallelism  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur et sélectionnez **Propriétés**.  
  
2.  Cliquez sur le nœud **Avancé** .  
  
3.  Sous **Parallélisme**, remplacez l’option **Seuil de coût pour le parallélisme** par la valeur souhaitée. Tapez ou sélectionnez une valeur comprise entre 0 et 32 767.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-the-cost-threshold-for-parallelism-option"></a>Pour configurer l'option cost threshold for parallelism  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour attribuer à l’option `cost threshold for parallelism` la valeur `10`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1 ;  
GO  
RECONFIGURE  
GO  
EXEC sp_configure 'cost threshold for parallelism', 10 ;  
GO  
RECONFIGURE  
GO  
```  
  
 Pour plus d’informations, consultez [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-cost-threshold-for-parallelism-option"></a><a name="FollowUp"></a> Suivi : Après avoir configuré l’option cost threshold for parallelism  
 Le paramètre prend effet immédiatement sans redémarrage du serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer des opérations d'index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md)   
 [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md)   
 [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md)   
 [affinity mask (option de configuration de serveur)](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
