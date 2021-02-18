---
description: Inscrire un serveur connecté (SQL Server Management Studio)
title: Inscrire un serveur connecté
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.registerserver.f1
helpviewer_keywords:
- Registered Servers [SQL Server], register connected servers
- connected server registrations [SQL Server]
ms.assetid: 77deb5f5-0f80-484f-8b8b-29afa67ec18f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 07/28/2016
ms.openlocfilehash: 4a5780c0d945f3ac44c6936bdc2a2d9a4e7f9fe5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350479"
---
# <a name="register-a-connected-server-sql-server-management-studio"></a>Inscrire un serveur connecté (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette rubrique explique comment inscrire un serveur connecté dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS). Lors de l'inscription du serveur, vous pouvez enregistrer les informations de connexion relatives aux serveurs auxquels vous accédez fréquemment. Un serveur peut être inscrit avant la connexion ou au moment de la connexion depuis l'Explorateur d'objets.  Vous pouvez afficher les serveurs inscrits dans SSMS en accédant à **Afficher**\\**Serveurs inscrits** à partir du menu.
  
 **Dans cette rubrique**  
  
-   **Pour inscrire un serveur, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-register-a-connected-server"></a>Pour inscrire un serveur connecté  
  
Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur auquel vous êtes déjà connecté, puis cliquez sur **Inscrire**.
  
**Nom du serveur**  
La valeur par défaut de ce champ est le nom du serveur auquel vous êtes connecté.  Si vous le souhaitez, vous pouvez taper un nom de serveur ou en choisir un dans la liste déroulante.

**Authentification**  
Deux modes d'authentification sont disponibles lors de la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

-    **Authentification Windows**  
Le mode d'authentification Windows permet à l'utilisateur de se connecter au moyen d'un compte d'utilisateur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. 

-    **Authentification SQL Server**   
Quand un utilisateur se connecte avec un nom de connexion et un mot de passe spécifiés à partir d’une connexion non approuvée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] réalise lui-même l’authentification en vérifiant si un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été défini et si le mot de passe spécifié correspond à celui enregistré précédemment. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne possède pas de compte de connexion, l'authentification échoue et un message d'erreur est envoyé à l'utilisateur.

     > [!IMPORTANT]  
     > [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)] Pour plus d’informations, consultez [Choisir un mode d’authentification](../../relational-databases/security/choose-an-authentication-mode.md).  

     -    **Nom d'utilisateur**  
Spécifie le nom d'utilisateur actuel avec lequel vous vous connectez. Cette option en lecture seule est disponible uniquement si vous avez choisi de vous connecter via l'authentification Windows. Pour modifier les **Noms d'utilisateurs**, ouvrez une session sur l'ordinateur en tant qu'utilisateur différent. 

     -    **Connexion**  
Entrez le nom d'accès avec lequel se connecter. Cette option est disponible uniquement si vous avez choisi la connexion avec l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

     -    **Mot de passe**  
Entrez le mot de passe utilisé avec la connexion. Cette option peut être modifiée uniquement si vous avez choisi de vous connecter via l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . 

     -    **Mémoriser le mot de passe**  
Sélectionnez cette option pour que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] chiffre et stocke le mot de passe entré. Cette option n'est affichée que si vous avez choisi la connexion avec l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

          > [!NOTE]  
          > Si vous avez stocké le mot de passe et ne voulez plus le conserver en mémoire, décochez la case, puis cliquez sur **Enregistrer**.  

**Nom du serveur inscrit**  
Le nom qui doit apparaître dans Serveurs inscrits. Ce nom ne doit pas correspondre à celui de la zone **Nom du serveur** .  
  
**Description du serveur inscrit**  
Entrez une description facultative du serveur.  
  
**Test**  
Cliquez sur cette option pour tester la connexion au serveur sélectionné dans la zone **Nom du serveur**.  
  
**Save**  
Cliquez sur ce bouton pour enregistrer les paramètres des serveurs inscrits. 

## <a name="see-also"></a>Voir aussi

[Créer un nouveau serveur inscrit (SQL Server Management Studio)](./create-a-new-registered-server-sql-server-management-studio.md)