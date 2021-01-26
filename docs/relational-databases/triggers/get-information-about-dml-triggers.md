---
description: Obtenir des informations sur les déclencheurs DML
title: Obtenir des informations sur les déclencheurs DML | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- metadata [SQL Server], triggers
- viewing DML triggers
- DML triggers, metadata
- displaying DML triggers
- status information [SQL Server], triggers
- DML triggers, viewing
ms.assetid: 37574aac-181d-4aca-a2cc-8abff64237dc
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7a5e8f14728e5239ab4e0b1d71c78b713f2c27c2
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765586"
---
# <a name="get-information-about-dml-triggers"></a>Obtenir des informations sur les déclencheurs DML
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Cette rubrique explique comment obtenir des informations sur les déclencheurs DML dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ces informations peuvent inclure les types de déclencheurs existant sur une table, ainsi que leur nom, leur propriétaire et la date de leur création ou modification. Si le déclencheur n'a pas été chiffré lors de sa création, vous obtenez la définition du déclencheur. Vous pouvez utiliser la définition pour vous aider à comprendre comment un déclencheur affecte la table sur laquelle il est défini. En outre, vous pouvez déterminer les objets qu'un déclencheur spécifique utilise. Avec ces informations, vous pouvez identifier les objets dont l'éventuelle suppression ou modification aurait une répercussion sur le déclencheur.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour obtenir des informations sur les déclencheurs DML, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 **sys.sql.modules**, **sys.object**, **sys.triggers**, **sys.events**, **sys.trigger_events**  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
 OBJECT_DEFINITION, OBJECTPROPERTY, **sp_helptext**  
 Nécessite l'appartenance au rôle **public** . La définition des objets utilisateur est visible par le propriétaire de l’objet ou les bénéficiaires de l’une des autorisations suivantes : ALTER, CONTROL, TAKE OWNERSHIP ou VIEW DEFINITION. Ces autorisations sont implicitement possédées par des membres des rôles de base de données fixes **db_owner**, **db_ddladmin** et **db_securityadmin** .  
  
 **sys.sql_expression_dependencies**  
 Requiert l’autorisation VIEW DEFINITION sur la base de données et l’autorisation SELECT sur **sys.sql_expression_dependencies** pour la base de données. Par défaut, l’autorisation SELECT est accordée uniquement aux membres du rôle de base de données fixe **db_owner** . Lorsque les autorisations SELECT et VIEW DEFINITION sont accordées à un autre utilisateur, le bénéficiaire peut consulter toutes les dépendances dans la base de données.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-view-the-definition-of-a-dml-trigger"></a>Pour voir la définition d'un déclencheur DML  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et développez-la.  
  
2.  Développez la base de données choisie, développez **Tables**, puis développez la table qui contient le déclencheur dont vous souhaitez consulter la définition.  
  
3.  Développez **Déclencheurs**, cliquez avec le bouton droit sur le déclencheur de votre choix, puis cliquez sur **Modifier**. La définition du déclencheur DML s'affiche dans la fenêtre de requête.  

#### <a name="to-view-the-dependencies-of-a-dml-trigger"></a>Pour afficher les dépendances d'un déclencheur DML  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et développez-la.  
  
2.  Développez la base de données choisie, développez **Tables**, puis développez la table qui contient le déclencheur et ses dépendances que vous souhaitez afficher.  
  
3.  Développez **Déclencheurs**, cliquez avec le bouton droit sur le déclencheur de votre choix, puis cliquez sur **Afficher les dépendances**.  
  
4.  Dans la fenêtre **Dépendances d’objets**, sélectionnez **Objets dépendants de \<DML trigger name>** pour afficher les objets qui dépendent du déclencheur DML. Les objets apparaissent dans la zone **Dépendances** .  
  
     Pour afficher les objets dont dépend le déclencheur DML, sélectionnez **Objets dont dépend \<DML trigger name>** . Les objets apparaissent dans la zone **Dépendances** . Développez chaque nœud pour voir tous les objets.  
  
5.  Pour obtenir des informations sur un objet qui apparaît dans la zone **Dépendances** , cliquez sur l'objet. Dans le champ **Objet sélectionné** , les informations sont fournies dans les zones **Nom**, **Type** et **Type de dépendance** .  
  
6.  Pour fermer la fenêtre **Dépendances d'objet** , cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-view-the-definition-of-a-dml-trigger"></a>Pour voir la définition d'un déclencheur DML  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'un des exemples suivants dans la fenêtre de requête, puis cliquez sur **Exécuter**. Chaque exemple montre comment afficher la définition du déclencheur `iuPerson` .  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT definition   
FROM sys.sql_modules  
WHERE object_id = OBJECT_ID(N'Person.iuPerson');   
GO  
```  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT OBJECT_DEFINITION (OBJECT_ID(N'Person.iuPerson')) AS ObjectDefinition;   
GO  
  
```  
  
```sql  
USE AdventureWorks2012;   
GO  
EXEC sp_helptext 'Person.iuPerson'  
GO  
  
```  
  
#### <a name="to-view-the-dependencies-of-a-dml-trigger"></a>Pour afficher les dépendances d'un déclencheur DML  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'un des exemples suivants dans la fenêtre de requête, puis cliquez sur **Exécuter**. Chaque exemple montre comment afficher les dépendances du déclencheur `iuPerson` .  
  
```  
USE AdventureWorks2012;   
GO  
SELECT OBJECT_NAME(referencing_id) AS referencing_entity_name,   
    o.type_desc AS referencing_desciption,   
    COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
    referencing_class_desc, referenced_class_desc,   
    referenced_server_name, referenced_database_name, referenced_schema_name,   
    referenced_entity_name,   
    COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,   
    is_caller_dependent, is_ambiguous  
FROM sys.sql_expression_dependencies AS sed  
INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
WHERE referencing_id = OBJECT_ID(N'Person.iuPerson');   
GO  
  
```  
  
#### <a name="to-view-information-about-dml-triggers-in-the-database"></a>Pour afficher les informations sur les déclencheurs DML dans la base de données  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'un des exemples suivants dans la fenêtre de requête, puis cliquez sur **Exécuter**. Chaque exemple montre comment afficher des informations sur les déclencheurs DML (`TR`) dans la base de données.  
  
```  
USE AdventureWorks2012;   
GO  
SELECT  name, parent_id, create_date, modify_date, is_instead_of_trigger  
FROM sys.triggers  
WHERE type = 'TR';   
GO  
  
```  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT  name, object_id, schema_id, parent_object_id, type_desc, create_date, modify_date, is_published  
FROM sys.objects  
WHERE type = 'TR';   
GO  
  
```  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT OBJECTPROPERTY(OBJECT_ID(N'Person.iuPerson'), 'ExecIsInsteadOfTrigger');   
GO  
  
```  
  
#### <a name="to-view-information-about-events-that-fire-a-dml-trigger"></a>Pour afficher les informations sur les événements qui activent un déclencheur DML  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'un des exemples suivants dans la fenêtre de requête, puis cliquez sur **Exécuter**. Chaque exemple montre comment afficher les événements qui déclenchent le déclencheur `iuPerson` .  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT object_id, type, type_desc, is_trigger_event, event_group_type, event_group_type_desc   
FROM sys.events  
WHERE object_id = OBJECT_ID('Person.iuPerson');   
GO  
```  
  
```sql  
USE AdventureWorks2012;   
GO   
SELECT object_id, type,is_first, is_last  
FROM sys.trigger_events  
WHERE object_id = OBJECT_ID('Person.iuPerson');   
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [ENABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [DISABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptrigger &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptrigger-transact-sql.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)   
 [sys.trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trigger-events-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)   
 [sys.server_triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md)   
 [sys.server_trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-trigger-events-transact-sql.md)   
 [sys.server_sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-sql-modules-transact-sql.md)   
 [sys.server_assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-assembly-modules-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md)  
  
  
