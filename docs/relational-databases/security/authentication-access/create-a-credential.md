---
title: Création des informations d’identification | Microsoft Docs
description: Découvrez comment créer des informations d’identification dans SQL Server à l’aide de SQL Server Management Studio ou de Transact-SQL. Découvrez comment respecter les limitations et restrictions.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- credentials [SQL Server], creating
- authentication [SQL Server], credentials
- logins [SQL Server], credentials
ms.assetid: c1e77e91-2a69-40d9-b8b3-97cffc710586
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 98fddc3b614cd53ad88d761365243bb81a3eca10
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250311"
---
# <a name="create-a-credential"></a>Create a Credential
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment créer des informations d'identification dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 Les informations d'identification fournissent un moyen d'autoriser les utilisateurs de l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à avoir une identité en dehors de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ceci sert principalement à exécuter du code dans des assemblys avec le jeu d'autorisations EXTERNAL_ACCESS. Les informations d'identification peuvent également être utilisées lorsqu'un utilisateur de l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a besoin d'accéder à une ressource du domaine, par exemple à un emplacement de fichier pour y stocker une sauvegarde.  
  
 Un jeu d'informations d'identification peut être mappé sur plusieurs connexions [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à la fois. Une connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne peut être mappée que sur un seul jeu d'informations d'identification à la fois. Après avoir créé des informations d’identification, utilisez **Propriétés de la connexion (page Général)** pour mapper une connexion à un jeu d’informations d’identification.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour créer des informations d'identification, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   En l'absence d'une information d'identification mappée à une connexion pour le fournisseur, l'information d'identification mappée au compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est utilisée.  
  
-   Une connexion peut avoir plusieurs informations d'identification mappées à elle, à condition qu'elles soient utilisées avec des fournisseurs distinctifs. Il ne doit y avoir qu'une seule information d'identification mappée par fournisseur par connexion. La même information d'identification peut être mappée à d'autres connexions.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l'autorisation ALTER ANY CREDENTIAL pour créer ou modifier des informations d'identification et une autorisation ALTER ANY LOGIN pour mapper une connexion à des informations d'identification.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-a-credential"></a>Pour créer les informations d'identification  
  
1.  Dans l’Explorateur d’objets, développez le dossier **Sécurité** .  
  
2.  Cliquez avec le bouton droit sur le dossier **Informations d’identification** et sélectionnez **Nouvelles informations d’identification...** .  
  
3.  Dans la boîte de dialogue **Nouvelles informations d'identification** , saisissez le nom des information d'identification dans la zone **Nom d'identification** .  
  
4.  Dans la zone **Identité** , tapez le nom du compte utilisé pour les connexions sortantes (en quittant le contexte de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]). En général, il s'agit d'un compte d'utilisateur Windows, mais l'identité peut être un autre type de compte.  
  
     Vous pouvez également cliquer sur les points de suspension **(...)** pour ouvrir la boîte de dialogue **Sélectionner un utilisateur ou un groupe**.  
  
5.  Dans les zones **Mot de passe** et **Confirmer le mot de passe** , tapez le mot de passe du compte indiqué dans la zone **Identité** . Si **Identité** correspond à un compte d'utilisateur Windows, il s'agit du mot de passe Windows. Si le mot de passe n'est pas requis, la zone **Mot de passe** peut être vide.  
  
6.  Sélectionnez **Utiliser le fournisseur de chiffrement** pour définir les informations d’identification devant être vérifiées par un fournisseur de gestion de clés extensible (EKM). Pour plus d’informations, consultez [Gestion de clés extensible &#40;EKM&#41;](../../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
7.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-a-credential"></a>Pour créer les informations d'identification  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- Creates the credential called "AlterEgo.".   
    -- The credential contains the Windows user "Mary5" and a password.  
    CREATE CREDENTIAL AlterEgo WITH IDENTITY = 'Mary5',   
        SECRET = '<EnterStrongPasswordHere>';  
    GO  
    ```  
  
 Pour plus d’informations, consultez [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md).  
  
  
