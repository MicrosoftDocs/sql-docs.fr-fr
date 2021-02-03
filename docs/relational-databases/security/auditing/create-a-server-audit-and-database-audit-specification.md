---
title: Créer une spécification pour l’audit de serveur et l’audit de base de données
description: Découvrez comment créer une spécification pour l’audit SQL Server et l’audit de base de données en utilisant SQL Server Management Studio ou Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.sqlaudit.dbaudit.general.f1
helpviewer_keywords:
- audits [SQL Server], creating database specification
- database audit [SQL Server]
ms.assetid: 26ee85de-6e97-4318-b526-900924d96e62
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d015a1adf0bdd5e5f47b49a8dabb6783b8538b7e
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250078"
---
# <a name="create-a-server-audit-and-database-audit-specification"></a>Créer une spécification pour l’audit de serveur et l’audit de base de données
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cet article explique comment créer une spécification d’audit de serveur et d’audit de base de données dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] en utilisant [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 L’audit d’une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou d’une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] implique le suivi et la journalisation des événements qui se produisent sur le système. L’objet *Audit SQL Server* collecte une seule instance des actions et des groupes d’actions au niveau du serveur ou au niveau de la base de données à superviser. L'audit s'effectue au niveau de l'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Vous pouvez exécuter plusieurs audits par instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . L’objet *Spécification d’audit de niveau base de données* appartient à un audit. Vous pouvez créer une spécification d'audit de base de données par base de données SQL Server et par audit. Pour plus d’informations, consultez [Audit SQL Server &#40;Moteur de base de données&#41;](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 ##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
 Les spécifications d'audit de base de données sont des objets non sécurisables qui résident dans une base de données spécifiée. Lorsqu’une spécification d’audit de base de données est créée, elle se trouve dans un état désactivé.  
  
 Lorsque vous créez ou modifiez une spécification d’audit de base de données dans une base de données utilisateur, n’incluez pas d’actions d’audit sur les objets situés dans l’étendue du serveur, tels que les vues système. Si vous le faites, l’audit sera créé. Toutefois, les objets situés dans l’étendue du serveur ne seront pas inclus, et aucune erreur ne sera retournée. Pour auditer les objets situés dans l’étendue du serveur, utilisez une spécification d’audit de base de données dans la base de données MASTER.  
  
 Les spécifications d’audit de base de données résident dans la base de données où elles sont créées, à l’exception de la base de données système **TempDB**.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
  
-   Les utilisateurs disposant de l’autorisation ALTER ANY DATABASE AUDIT peuvent créer des spécifications d’audit de base de données et les lier à n’importe quel audit.  
  
-   Une fois qu’une spécification d’audit de base de données est créée, les principaux qui disposent des autorisations CONTROL SERVER ou ALTER ANY DATABASE AUDIT peuvent la voir. Le compte sysadmin peut également la voir.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-a-server-audit"></a>Pour créer un audit de serveur  
  
1.  Dans l'Explorateur d'objets, développez le dossier **Sécurité** .  
  
2.  Cliquez avec le bouton droit sur le dossier **Audits**, puis sélectionnez **Nouvel audit**. Pour plus d’informations, consultez [Créer un audit du serveur et une spécification d’audit du serveur](../../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md).  
  
3.  Lorsque vous avez sélectionné les options, sélectionnez **OK**.  

#### <a name="to-create-a-database-level-audit-specification"></a>Pour créer une spécification d'audit de niveau base de données  
  
1.  Dans l’Explorateur d’objets, développez la base de données dans laquelle vous souhaitez créer la spécification d’audit.  
  
2.  Développez le dossier **Sécurité** .  
  
3.  Cliquez avec le bouton droit sur le dossier **Spécifications de l’audit de la base de données**, puis sélectionnez **Nouvelle spécification de l’audit de la base de données**.  
  
     Ces options sont disponibles dans la boîte de dialogue **Créer la spécification de l’audit de la base de données** :  
  
     **Nom**  
     Nom de la spécification de l'audit de la base de données. Un nom est généré automatiquement lorsque vous créez une spécification d’audit de serveur. Le nom est modifiable.  
  
     **Audit**  
     Nom d’un objet d’audit de serveur existant. Tapez le nom de l'audit ou sélectionnez-le dans la liste.  
  
     **Type d'action de l'audit**  
     Spécifie les groupes d'actions d'audit de niveau base de données et les actions d'audit à capturer. Pour obtenir la liste d’actions d’audit et de groupes d’actions d’audit au niveau de la base de données, ainsi qu’une description des événements qu’ils contiennent, consultez [Actions et groupes d’actions SQL Server Audit](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
     **Schéma d'objet**  
     Affiche le schéma du **Nom de l’objet** spécifié.  
  
     **Nom de l’objet**  
     Nom de l'objet à auditer. Cette option est disponible uniquement pour les actions d’audit. Elle ne s’applique pas aux groupes d’audit.  
  
     **Points de suspension (...)**  
     Ouvre la boîte de dialogue **Sélectionner des objets** permettant de rechercher et de sélectionner un objet disponible, en fonction du **Type d’action de l’audit** spécifié.  
  
     **Nom principal**  
     Compte par lequel filtrer l'audit pour l'objet audité.  
  
     **Points de suspension (...)**  
     Ouvre la boîte de dialogue **Sélectionner des objets** permettant de rechercher et de sélectionner un objet disponible, en fonction du **Nom d’objet** spécifié.  
  
4.  Lorsque vous avez sélectionné les options, sélectionnez **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-a-server-audit"></a>Pour créer un audit de serveur  
  
1.  Dans l'Explorateur d'objets, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Dans la barre d’outils standard, sélectionnez **Nouvelle requête**.  
  
3.  Collez l’exemple suivant dans la fenêtre de requête, puis sélectionnez **Exécuter**.  
  
    ```  
    USE master ;  
    GO  
    -- Create the server audit.   
    CREATE SERVER AUDIT Payrole_Security_Audit  
        TO FILE ( FILEPATH =   
    'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA' ) ;   
    GO  
    -- Enable the server audit.   
    ALTER SERVER AUDIT Payrole_Security_Audit   
    WITH (STATE = ON) ;  
    ```  
  
#### <a name="to-create-a-database-level-audit-specification"></a>Pour créer une spécification d'audit de niveau base de données  
  
1.  Dans l'Explorateur d'objets, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Dans la barre d’outils standard, sélectionnez **Nouvelle requête**.  
  
3.  Collez l’exemple suivant dans la fenêtre de requête, puis sélectionnez **Exécuter**. Cet exemple crée une spécification d’audit de serveur nommée `Audit_Pay_Tables`. Il audite les instructions SELECT et INSERT de l’utilisateur `dbo` pour la table `HumanResources.EmployeePayHistory`, en fonction de l’audit de serveur défini dans la section précédente.  
  
    ```  
    USE AdventureWorks2012 ;   
    GO  
    -- Create the database audit specification.   
    CREATE DATABASE AUDIT SPECIFICATION Audit_Pay_Tables  
    FOR SERVER AUDIT Payrole_Security_Audit  
    ADD (SELECT , INSERT  
         ON HumanResources.EmployeePayHistory BY dbo )   
    WITH (STATE = ON) ;   
    GO  
  
    ```  
  
 Pour plus d’informations, consultez [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-audit-transact-sql.md) et [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/create-database-audit-specification-transact-sql.md).  
  
  
