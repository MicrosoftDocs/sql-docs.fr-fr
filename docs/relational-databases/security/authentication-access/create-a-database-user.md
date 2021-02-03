---
title: Créer un utilisateur de base de données | Microsoft Docs
description: Découvrez comment créer les types les plus courants d’utilisateurs de base de données à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 04/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.user.securables.f1
- SQL13.SWB.DATABASEUSER.GENERAL.F1
helpviewer_keywords:
- database users, creating
- creating users with Management Studio
- mapping users
- users [SQL Server], creating
- database user additions [SQL Server]
- database users, mapping
- CREATE USER [Management Studio]
- users [SQL Server], adding
- mapping database users
ms.assetid: 782798d3-9552-4514-9f58-e87be4b264e4
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ce4d006d0df8f17b487a515e3179d43fefd501d5
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236438"
---
# <a name="create-a-database-user"></a>Créer un utilisateur de base de données
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Cette rubrique décrit comment créer les types courants d'utilisateurs de base de données. Il existe onze types d’utilisateurs. La liste complète est fournie dans la rubrique [CREATE USER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-user-transact-sql.md). Tous les types de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prennent en charge les utilisateurs de base de données, mais pas nécessairement tous les types d'utilisateurs.  
  
 Vous pouvez créer un utilisateur de base de données à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
##  <a name="understanding-the-types-of-users"></a><a name="Understanding"></a> Présentation des types d'utilisateurs  
 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] présente 6 options de création d'un utilisateur de base de données. L’image suivante montre les 6 options dans la zone verte et indique ce qu'elles représentent.  
  
 ![Diagramme décrivant les différents types d’utilisateurs](../../../relational-databases/security/authentication-access/media/typesofusers.png "TypesOfUsers")  
  
### <a name="selecting-the-type-of-user"></a>Sélectionnez le type d'utilisateur  
 **Connexion ou utilisateur non mappé à une connexion**  
  
 Si vous ne connaissez pas encore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], il peut être difficile de déterminer quel type d'utilisateur vous souhaitez créer. Tout d'abord, posez-vous la question suivante : la personne ou le groupe qui doit accéder à la base de données dispose-t-elle d’une connexion ? Dans la base de données MASTER, les connexions sont courantes pour les personnes qui gèrent le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et celles qui doivent accéder à plusieurs ou l'ensemble des bases de données sur l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Dans ce cas, vous allez créer un **utilisateur SQL avec connexion**. L'utilisateur de base de données est l'identité du compte de connexion lorsqu'il est connecté à la base de données. Il peut utiliser le même nom que celui du compte de connexion, mais cela n'est pas obligatoire. Cette rubrique part du principe qu'il existe déjà un compte de connexion dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d’informations sur la création d’un compte de connexion, consultez [Créer un compte de connexion](../../../relational-databases/security/authentication-access/create-a-login.md).  
  
 Si la personne ou le groupe qui doit accéder à la base de données ne dispose pas d'une connexion et n’a besoin d’accéder qu’à une ou un petit nombre de bases de données, créez un **utilisateur Windows** ou un **utilisateur SQL avec mot de passe**. Également appelé utilisateur de base de données autonome, il n'est pas associé à une connexion dans la base de données MASTER. Cela constitue un excellent choix lorsque vous souhaitez pouvoir déplacer facilement votre base de données entre des instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour utiliser cette option sur [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)], un administrateur doit d’abord activer les bases de données autonomes pour le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]et activer l’autonomie pour la base de données. Pour plus d’informations, voir [Utilisateurs de base de données autonome - Rendre votre base de données portable](../../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
> **IMPORTANT !** Lorsque vous vous connectez en tant qu'utilisateur de base de données autonome, vous devez fournir le nom de la base de données dans la chaîne de connexion. Pour spécifier la base de données dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], dans la boîte de dialogue **Se connecter à** , cliquez sur **Options**, puis sur l'onglet **Propriétés de connexion** .  
  
 Sélectionnez **utilisateur SQL avec mot de passe** ou **utilisateur SQL avec connexion** avec **connexion d'authentification SQL Server**, lorsque la personne qui se connecte ne peut pas s'authentifier avec Windows. Ceci est courant lorsque des personnes extérieures à votre organisation (par exemple des clients) se connectent à votre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
> **ASTUCE** Pour les personnes internes à votre organisation, l'authentification Windows est un meilleur choix, parce qu’elles n'auront pas à mémoriser un mot de passe supplémentaire et que l'authentification Windows offre des fonctionnalités de sécurité supplémentaires, notamment Kerberos.  
  
##  <a name="background"></a><a name="Restrictions"></a> Arrière-plan  
 Un utilisateur est un principal de sécurité au niveau de la base de données. Les comptes de connexion doivent être mappés à un utilisateur de base de données pour permettre la connexion à une base de données. Un compte de connexion peut être mappé à différentes bases de données en tant qu'utilisateurs différents, mais il ne peut être mappé que comme utilisateur unique dans chaque base de données. Dans une base de données partiellement autonome, il est possible de créer un utilisateur qui ne dispose pas de compte de connexion. Pour plus d’informations sur les utilisateurs de base de données autonome, consultez [CREATE USER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-user-transact-sql.md). Lorsque l'utilisateur invité d'une base de données est activé, un compte de connexion non mappé à un utilisateur de base de données peut accéder à la base de données en tant qu'utilisateur invité.  
  
> **IMPORTANT !** L'utilisateur invité est habituellement désactivé. Ne l'activez que lorsque cela est nécessaire.  
  
 En tant que principal de sécurité, il est possible d'accorder des autorisations à des utilisateurs. L'étendue d'un utilisateur est la base de données. Pour se connecter à une base de données spécifique sur l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], un compte de connexion doit être mappé à un utilisateur de base de données. Les autorisations dans la base de données sont accordées et refusées à l'utilisateur de la base de données, pas au compte de connexion.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l'autorisation **ALTER ANY USER** sur la base de données.  
  
##  <a name="create-a-user-with-ssms"></a><a name="SSMSProcedure"></a> Créer un utilisateur avec SSMS  
  
 
1.  Dans l'Explorateur d'objets, développez le dossier **Bases de données** .  
  
2.  Développez la base de données où créer le nouvel utilisateur de base de données.  
  
3.  Cliquez avec le bouton droit sur le dossier **Sécurité**, pointez sur **Nouveau**, puis sélectionnez **Utilisateur...** .  
  
4.  Dans la boîte de dialogue **Nouvel utilisateur de base de données**, dans la page **Général**, sélectionnez un des types d’utilisateurs suivants à partir de la liste **Type d’utilisateur** :  
  
    -   **utilisateur SQL avec connexion**  
  
    -   **utilisateur SQL avec mot de passe**  
  
    -   **Utilisateur SQL sans connexion**  
  
    -   **Utilisateur mappé à un certificat**  
  
    -   **Utilisateur mappé à une clé asymétrique**  
  
    -   **utilisateur Windows**  
  
5.  Lorsque vous sélectionnez une option, les options restantes dans la boîte de dialogue peuvent changer. Certaines options s'appliquent uniquement à des types spécifiques d'utilisateurs de base de données. Certaines options peuvent être vides et utiliser la valeur par défaut.  
  
     **Nom d'utilisateur**  
     Entrez le nom du nouvel utilisateur. Si vous avez choisi **Utilisateur Windows** dans la liste **Type d’utilisateur**, vous pouvez également cliquer sur les points de suspension **(...)** pour ouvrir la boîte de dialogue **Sélectionner un utilisateur ou un groupe**.  
  
     **Nom d'accès**  
     Entrez le nom de connexion de l'utilisateur. Vous pouvez également cliquer sur les points de suspension **(…)** pour ouvrir la boîte de dialogue **Sélectionner la connexion**. **Nom de connexion** est disponible si vous sélectionnez **Utilisateur SQL avec connexion** ou **Utilisateur Windows** dans la liste **Type d'utilisateur** .  
  
     **Mot de passe** et **Confirmer le mot de passe**  
     Entrez un mot de passe pour les utilisateurs qui s'authentifient auprès de la base de données.  
  
     **Langue par défaut**  
     Entrez la langue par défaut de l’utilisateur.  
  
     **Schéma par défaut**  
     Entrez le schéma qui possédera les objets créés par cet utilisateur. Vous pouvez également cliquer sur les points de suspension **(…)** pour ouvrir la boîte de dialogue **Sélectionner le schéma**. **Schéma par défaut** est disponible si vous sélectionnez **Utilisateur SQL avec connexion**, **Utilisateur SQL sans connexion** ou **Utilisateur Windows** dans la liste **Type d'utilisateur** .  
  
     **Nom du certificat**  
     Entrez le certificat à utiliser pour l'utilisateur de base de données. Vous pouvez également cliquer sur les points de suspension **(…)** pour ouvrir la boîte de dialogue **Sélectionner le certificat**. **Nom du certificat** est disponible si vous sélectionnez **Utilisateur mappé à un certificat** dans la liste **Type d'utilisateur** .  
  
     **Nom de la clé asymétrique**  
     Entrez la clé à utiliser pour l'utilisateur de base de données. Vous pouvez également cliquer sur les points de suspension **(…)** pour ouvrir la boîte de dialogue **Sélectionner la clé asymétrique**. **Nom de la clé asymétrique** est disponible si vous sélectionnez **Utilisateur mappé à une clé asymétrique** dans la liste **Type d'utilisateur** .  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="additional-options"></a>Options supplémentaires  
 La boîte de dialogue **Utilisateur de base de données - Nouveau** offre également des options sur quatre pages supplémentaires : **schémas détenus**, **appartenance**, **éléments sécurisables** et **propriétés étendues**.  
  
-   La page **Schémas appartenant à un rôle** répertorie tous les schémas possibles qui peuvent être détenus par le nouvel utilisateur de base de données. Pour ajouter des schémas à un utilisateur de base de données ou lui en supprimer, sous **Schémas appartenant à cet utilisateur**, activez ou désactivez les cases à cocher en regard de ces schémas.  
  
-   La page **Appartenance** répertorie tous les rôles possibles d'appartenance de base de données qui peuvent être détenus par le nouvel utilisateur de base de données. Pour ajouter des rôles à un utilisateur de base de données ou lui en supprimer, sous **Appartenance au rôle de base de données**, activez ou désactivez les cases à cocher en regard de ces rôles.  
  
-   La page **Éléments sécurisables** répertorie tous les éléments sécurisables possibles et les autorisations sur ces éléments sécurisables qui peuvent être accordées à la connexion.  
  
-   La page **Propriétés étendues** vous permet d'ajouter des propriétés personnalisées aux utilisateurs de base de données. Les options supplémentaires suivantes sont disponibles sur cette page.  
  
     **Sauvegarde de la base de données**  
     Affiche le nom de la base de données sélectionnée. Ce champ est en lecture seule.  
  
     **Classement**  
     Affiche le classement utilisé pour la base de données sélectionnée. Ce champ est en lecture seule.  
  
     **Propriétés**  
     Affiche ou spécifie les propriétés étendues de l'objet. Chaque propriété étendue est constituée d'une paire nom/valeur de métadonnées associées à l'objet.  
  
     **Points de suspension (...)**  
     Cliquez sur les points de suspension **(…)** figurant après **Valeur** pour ouvrir la boîte de dialogue **Valeur de la propriété étendue**. Tapez ou affichez la valeur de la propriété étendue dans cet emplacement de plus grande taille. Pour plus d'informations, consultez [Boîte de dialogue Valeur de la propriété étendue](../../databases/value-for-extended-property-dialog-box.md).  
  
     **Supprimer**  
     Supprime la propriété étendue sélectionnée.  
  
##  <a name="create-a-user-using-t-sql"></a><a name="TsqlProcedure"></a> Créer un utilisateur à l’aide de T-SQL  
    
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils **Standard** , cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- Creates the login AbolrousHazem with password '340$Uuxwp7Mcxo7Khy'.  
    CREATE LOGIN AbolrousHazem   
        WITH PASSWORD = '340$Uuxwp7Mcxo7Khy';  
    GO  
  
    -- Creates a database user for the login created above.  
    CREATE USER AbolrousHazem FOR LOGIN AbolrousHazem;  
    GO  
    ```  
  
 Pour plus d’informations, consultez [CREATE USER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-user-transact-sql.md) qui contient beaucoup d’autres exemples [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [Principaux &#40;moteur de base de données&#41;](../../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Créer un compte de connexion](../../../relational-databases/security/authentication-access/create-a-login.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/create-login-transact-sql.md)  
  
  
