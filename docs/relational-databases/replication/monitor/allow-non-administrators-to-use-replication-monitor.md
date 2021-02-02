---
title: Autoriser des non-administrateurs à utiliser le moniteur de réplication
description: Découvrez comment autoriser des non-administrateurs à utiliser le moniteur de réplication dans SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Replication Monitor, non-administrators access
ms.assetid: 1cf21d9e-831d-41a1-a5a0-83ff6d22fa86
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 2fc1710ca316e1cb62373bcdde6c1ad2dcfa8f61
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077097"
---
# <a name="allow-non-administrators-to-use-replication-monitor"></a>Autoriser des non-administrateurs à utiliser le Moniteur de réplication
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique explique comment autoriser des non-administrateurs à utiliser le Moniteur de réplication dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Le moniteur de réplication est utilisable par les membres des rôles suivants :  
  
-   Rôle serveur fixe **sysadmin** .  
  
     Ces utilisateurs peuvent surveiller la réplication et avoir un contrôle total sur la modification des propriétés de réplication telles que les planifications d'agents, les profils d'agents, etc.  
  
-   Rôle de base de données **replmonitor** dans la base de données de distribution.  
  
     Ces utilisateurs peuvent surveiller la réplication mais ne peuvent pas modifier les propriétés de réplication.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour autoriser des non-administrateurs à utiliser le Moniteur de réplication à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Pour autoriser des non-administrateurs à utiliser le Moniteur de réplication, un membre du rôle serveur fixe **sysadmin** doit ajouter l'utilisateur à la base de données de distribution et attribuer à ce dernier le rôle **replmonitor** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-allow-non-administrators-to-use-replication-monitor"></a>Pour autoriser des non-administrateurs à utiliser le moniteur de réplication  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], connectez-vous au serveur de distribution, puis développez le nœud du serveur.  
  
2.  Développez **Bases de données**, puis **Bases de données système** et enfin la base de données de distribution (nommée **distribution** par défaut).  
  
3.  Développez **Sécurité**, cliquez avec le bouton droit sur **Utilisateurs**, puis cliquez sur **Nouvel utilisateur**.  
  
4.  Entrez un nom d'utilisateur et une connexion pour l'utilisateur.  
  
5.  Sélectionnez un schéma par défaut de **replmonitor**.  
  
6.  Activez la case à cocher **replmonitor** dans la grille **Appartenance au rôle de base de données** .  
  
7.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-add-a-user-to-the-replmonitor-fixed-database-role"></a>Pour ajouter un utilisateur au rôle de base de données fixe replmonitor  
  
1.  Dans la base de données de distribution sur le serveur de distribution, exécutez [sp_helpuser &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md). Si l’utilisateur n’est pas répertorié dans **UserName** dans le jeu de résultats, il doit se voir attribuer l’accès à la base de données de distribution à l’aide de l’instruction [CREATE USER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-user-transact-sql.md).  
  
2.  Dans la base de données de distribution sur le serveur de distribution, exécutez [sp_helprolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md), en spécifiant la valeur **replmonitor** pour le paramètre `@rolename`. Si l'utilisateur est répertorié dans **MemberName** dans le jeu de résultats, l'utilisateur appartient déjà à ce rôle.  
  
3.  Si l’utilisateur n’appartient pas au rôle **replmonitor**, exécutez [sp_addrolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) dans la base de données de distribution sur le serveur de distribution. Spécifiez une valeur de **replmonitor** pour `@rolename` et le nom de l’utilisateur de base de données ou la connexion Windows [!INCLUDE[msCoName](../../../includes/msconame-md.md)] ajoutée pour `@membername`.  
  
#### <a name="to-remove-a-user-from-the-replmonitor-fixed-database-role"></a>Pour supprimer un utilisateur du rôle de base de données fixe replmonitor  
  
1.  Pour vérifier que l’utilisateur appartient au rôle **replmonitor**, exécutez [sp_helprolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md) dans la base de données de distribution sur le serveur de distribution et spécifiez la valeur **replmonitor** pour `@rolename`. Si l'utilisateur n'est pas répertorié dans **MemberName** dans le jeu de résultats, l'utilisateur n'appartient pas à ce rôle.  
  
2.  Si l’utilisateur appartient au rôle **replmonitor**, exécutez [sp_droprolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md) dans la base de données de distribution sur le serveur de distribution. Spécifiez la valeur **replmonitor** pour `@rolename` et le nom de l’utilisateur de base de données ou la connexion Windows supprimée pour `@membername`. 
  
  
