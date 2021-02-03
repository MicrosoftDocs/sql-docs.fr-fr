---
title: Joindre un rôle | Microsoft Docs
description: Découvrez comment attribuer des rôles aux connexions et aux utilisateurs de la base de données dans SQL Server à l’aide de SQL Server Management Studio ou de Transact-SQL. Utilisez des rôles pour gérer les autorisations.
ms.custom: ''
ms.date: 07/14/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.DATABASEUSER.MEMBERSHIP.F1
helpviewer_keywords:
- adding a member to a role
- join a role
ms.assetid: 05c8d10d-5823-46c6-8b1a-81722da6a42b
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 88d884d92685b943649006caa894d78a152c7667
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250740"
---
# <a name="join-a-role"></a>joindre un rôle
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Cette rubrique décrit comment affecter des rôles aux utilisateurs des connexions et des bases de données dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Utilisez les rôles dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour gérer efficacement les autorisations. Affectez des autorisations aux rôles, puis ajoutez et supprimez des utilisateurs et des connexions aux rôles. L'utilisation de rôles évite de devoir maintenir individuellement les autorisations pour chaque utilisateur.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend en charge quatre types de rôle :  
  
-   Rôles serveur fixes  
  
-   Rôles de serveur définis par l'utilisateur  
  
-   Rôles de base de données fixes  
  
-   Rôles de base de données définis par l'utilisateur  
  
 Les rôles fixes sont automatiquement disponibles dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les rôles fixes ont les autorisations nécessaires pour accomplir des tâches courantes. Pour plus d'informations sur les rôles fixes, consultez les liens suivants. Les rôles définis par l'utilisateur sont créés par vous-même et peuvent être personnalisés à l'aide des autorisations que vous sélectionnez. Pour plus d'informations sur les rôles définis par l'utilisateur, consultez les liens suivants.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour assigner des rôles aux utilisateurs des connexions et des bases de données, à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   La modification du nom d'un rôle de base de données ne modifie pas le numéro d'identification, le propriétaire ou les autorisations du rôle.  
  
-   Les rôles de base de données sont visibles dans les vues de catalogue sys.database_role_members et sys.database_principals.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l’autorisation **ALTER ANY ROLE** sur la base de données, ou l’autorisation **ALTER** sur le rôle, ou l’appartenance au rôle **db_securityadmin**.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-add-a-member-to-a-fixed-server-role"></a>Pour ajouter un membre à un rôle serveur fixe  
  
1.  Dans l'Explorateur d'objets, développez le serveur sur lequel vous souhaitez modifier un rôle serveur fixe.  
  
2.  Développez le dossier **Sécurité** .  
  
3.  Développez le dossier **Rôles serveur** .  
  
4.  Cliquez avec le bouton droit sur le rôle à modifier, puis sélectionnez **Propriétés**.  
  
5.  Dans la boîte de dialogue **Propriétés du rôle de serveur -** _nom\_rôle\_serveur_, dans la page **Membres**, cliquez sur **Ajouter**.  
  
6.  Dans la boîte de dialogue **Sélectionner la connexion au serveur ou le rôle de serveur** , sous **Entrez les noms des objets à sélectionner (exemples)** , entrez la connexion ou le rôle serveur à ajouter à ce rôle serveur. Vous pouvez également cliquer sur **Parcourir…** et sélectionner l’ensemble ou certains des objets disponibles dans la boîte de dialogue **Rechercher des objets**. Cliquez sur **OK** pour revenir à la boîte de dialogue **Propriétés du rôle de serveur -** _nom\_rôle\_serveur_.  
  
7.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-add-a-member-to-a-user-defined-database-role"></a>Pour ajouter un membre à un rôle de base de données défini par l'utilisateur  
  
1.  Dans l'Explorateur d'objets, développez le serveur sur lequel vous souhaitez modifier un rôle de base de données défini par l'utilisateur.  
  
2.  Développez le dossier **Bases de données** .  
  
3.  Développez la base de données dans laquelle vous souhaitez modifier un rôle de base de données défini par l'utilisateur.  
  
4.  Développez le dossier **Sécurité** .  
  
5.  Développez le dossier **Rôles** .  
  
6.  Développez le dossier **Rôles serveur** .  
  
7.  Cliquez avec le bouton droit sur le rôle à modifier, puis sélectionnez **Propriétés**.  
  
8.  Dans la boîte de dialogue **Propriétés du rôle de base de données -** _nom\_rôle\_base de données_, dans la page **Général**, cliquez sur **Ajouter**.  
  
9. Dans la boîte de dialogue **Sélectionner l’utilisateur ou le rôle de la base de données** , sous **Entrez les noms des objets à sélectionner (exemples)** , entrez la connexion ou le rôle de base de données à ajouter à ce rôle de base de données. Vous pouvez également cliquer sur **Parcourir…** et sélectionner l’ensemble ou certains des objets disponibles dans la boîte de dialogue **Rechercher des objets**. Cliquez sur **OK** pour revenir à la boîte de dialogue **Propriétés du rôle de base de données-** _nom\_rôle\_base de données_.  
  
10. [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-add-a-member-to-a-fixed-server-role"></a>Pour ajouter un membre à un rôle serveur fixe  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    ALTER SERVER ROLE diskadmin ADD MEMBER [Domain\Juan] ;  
    GO  
    ```  
  
 Pour plus d’informations, consultez [ALTER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-role-transact-sql.md).  
  
#### <a name="to-add-a-member-to-a-user-defined-database-role"></a>Pour ajouter un membre à un rôle de base de données défini par l'utilisateur  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    ALTER ROLE Marketing ADD MEMBER [Domain\Juan] ;  
    GO  
    ```  
  
 Pour plus d’informations, consultez [sp_addrolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Rôles de niveau serveur](../../../relational-databases/security/authentication-access/server-level-roles.md)   
 [Rôles au niveau de la base de données](../../../relational-databases/security/authentication-access/database-level-roles.md)   
 [Rôles d’application](../../../relational-databases/security/authentication-access/application-roles.md)  
  
  
