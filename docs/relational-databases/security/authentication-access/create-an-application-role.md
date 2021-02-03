---
title: Créer un rôle d’application | Microsoft Docs
description: Créez un rôle d’application dans SQL Server à l’aide de SQL Server Management Studio ou de Transact-SQL pour restreindre l’accès à une base de données, sauf par le biais d’une application.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.approle.general.f1
helpviewer_keywords:
- application roles [SQL Server], creating
ms.assetid: 6b8da1f5-3d8e-4f88-b111-b915788b06f1
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b9d77743864b3b5425379ec7b5c570629b470e04
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250742"
---
# <a name="create-an-application-role"></a>Créer un rôle d'application
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Cette rubrique explique comment créer un rôle d'application dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Les rôles d'application limitent l'accès utilisateur à une base de données sauf via des applications spécifiques. Les rôles d'application ne possèdent pas d'utilisateurs, de sorte que la liste **Membres du rôle** n'est pas affichée lorsque l'option **Rôle d'application** est sélectionnée.  
  
> [!IMPORTANT]  
>  La complexité des mots de passe est vérifiée lors de la définition des mots de passe des rôles d'application. Les applications qui appellent des rôles d'application doivent stocker leurs mots de passe. Les mots de passe des rôles d'application doivent toujours être stockés sous forme chiffrée.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour créer un rôle d'application, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l'autorisation ALTER ANY APPLICATION ROLE sur la base de données.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
##### <a name="to-create-an-application-role"></a>Pour créer un rôle d'application  
  
1.  Dans l'Explorateur d'objets, développez la base de données dans laquelle vous souhaitez créer un rôle d'application.  
  
2.  Développez le dossier **Sécurité** .  
  
3.  Développez le dossier **Rôles** .  
  
4.  Cliquez avec le bouton droit sur le dossier **Rôles d’application** et sélectionnez **Nouveau rôle d’application...** .  
  
5.  Dans la boîte de dialogue **Rôle d’application - Nouveau**, dans la **page Général**, entrez le nouveau nom du nouveau rôle d’application dans la zone **Nom du rôle**.  
  
6.  Dans la zone **Schéma par défaut** , spécifiez le schéma qui possédera les objets créés par ce rôle en entrant les noms d'objet. Vous pouvez également cliquer sur les points de suspension **(…)** pour ouvrir la boîte de dialogue **Localiser le schéma**.  
  
7.  Dans la zone **Mot de passe** , entrez un mot de passe pour le nouveau rôle. Entrez de nouveau ce mot de passe dans la zone **Confirmer le mot de passe** .  
  
8.  Sous **Schémas appartenant à ce rôle**, sélectionnez ou affichez les schémas qui appartiendront à ce rôle. Un schéma ne peut appartenir qu'à un seul schéma ou rôle.  
  
9. [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

### <a name="additional-options"></a>Options supplémentaires  
 La boîte de dialogue **Rôle d'application - Nouveau** offre également des options sur deux pages supplémentaires : **Sécurisables** et **Propriétés étendues**.  
  
-   La page **Éléments sécurisables** répertorie tous les éléments sécurisables possibles et les autorisations sur ces éléments sécurisables qui peuvent être accordées à la connexion.  
  
-   La page **Propriétés étendues** vous permet d'ajouter des propriétés personnalisées aux utilisateurs de base de données.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-an-application-role"></a>Pour créer un rôle d'application  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- Creates an application role called "weekly_receipts" that has the password "987Gbv876sPYY5m23" and "Sales" as its default schema.  
  
    CREATE APPLICATION ROLE weekly_receipts   
        WITH PASSWORD = '987G^bv876sPY)Y5m23'   
        , DEFAULT_SCHEMA = Sales;  
    GO  
    ```  
  
 Pour plus d’informations, consultez [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-application-role-transact-sql.md).  
  
  
