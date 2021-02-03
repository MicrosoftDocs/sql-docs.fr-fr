---
description: Modifier ou renommer les déclencheurs DML
title: Modifier ou renommer les déclencheurs DML | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- renaming triggers
- modifying triggers
- DML triggers, modifying
ms.assetid: c7317eec-c0e9-479e-a4a7-83b6b6c58d59
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 34947ff934ead8d308c6da1df2f1d0a6bd1b07c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187279"
---
# <a name="modify-or-rename-dml-triggers"></a>Modifier ou renommer les déclencheurs DML
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Cette rubrique explique comment modifier ou renommer un déclencheur DML dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour modifier ou renommer un déclencheur DML à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Lorsque vous renommez un déclencheur, celui-ci doit se trouver dans la base de données actuelle et le nouveau nom doit respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Nous vous recommandons de ne pas utiliser la procédure stockée [sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md) pour renommer un déclencheur. La modification d'une partie du nom de l'objet peut provoquer des problèmes dans des scripts et des procédures stockées. Le fait de renommer un déclencheur ne modifie pas le nom de l’objet correspondant dans la colonne de définition de l’affichage catalogue [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) . Nous vous recommandons plutôt de supprimer et de recréer le déclencheur.  
  
-   Si vous changez le nom d'un objet référencé par un déclencheur DML, vous devez modifier le déclencheur pour que sa définition se réfère au nouveau nom de l'objet. Par conséquent, avant de renommer un objet, affichez les dépendances de l'objet pour savoir si des déclencheurs peuvent être concernés par la modification projetée.  
  
-   Un déclencheur DML peut aussi être modifié pour en chiffrer la définition.  
  
-   Pour afficher les dépendances d'un déclencheur, vous pouvez utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou la fonction et les affichages catalogue suivants :  
  
    -   [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
    -   [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)  
  
    -   [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 La modification d'un déclencheur DML nécessite une autorisation ALTER sur la table ou la vue sur laquelle le déclencheur est défini.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-modify-a-dml-trigger"></a>Pour modifier un déclencheur DML  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et développez-la.  
  
2.  Développez la base de données choisie, développez **Tables**, puis développez la table qui contient le déclencheur que vous souhaitez modifier.  
  
3.  Développez **Déclencheurs**, cliquez avec le bouton droit sur le déclencheur à modifier, puis cliquez sur **Modifier**.  
  
4.  Modifiez le déclencheur, puis sélectionnez **Exécuter**.  
  
#### <a name="to-rename-a-dml-trigger"></a>Pour renommer un déclencheur DML  
  
1.  [Supprimez le déclencheur](../../relational-databases/triggers/delete-or-disable-dml-triggers.md) que vous souhaitez renommer.  
  
2.  [Recréez le déclencheur](../../relational-databases/triggers/create-dml-triggers.md)en spécifiant un nouveau nom.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-modify-a-trigger-using-alter-trigger"></a>Pour modifier un déclencheur à l'aide de ALTER TRIGGER  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez les exemples suivants dans le volet de requête. Exécutez le premier exemple pour créer un déclencheur DML qui envoie un message défini par l'utilisateur au client lorsqu'un utilisateur tente d'ajouter ou de modifier les données de la table `SalesPersonQuotaHistory` . Exécutez l'instruction [ALTER TRIGGER](../../t-sql/statements/alter-trigger-transact-sql.md) pour modifier le déclencheur afin qu'il s'active uniquement sur des activités `INSERT` . Ce déclencheur est utile car il rappelle à l'utilisateur qui met à jour ou insère des lignes dans cette table de notifier également le département `Compensation` .  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID(N'Sales.bonus_reminder', N'TR') IS NOT NULL  
    DROP TRIGGER Sales.bonus_reminder;  
GO  
CREATE TRIGGER Sales.bonus_reminder  
ON Sales.SalesPersonQuotaHistory  
WITH ENCRYPTION  
AFTER INSERT, UPDATE   
AS RAISERROR ('Notify Compensation', 16, 10);  
GO  
  
```  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER TRIGGER Sales.bonus_reminder  
ON Sales.SalesPersonQuotaHistory  
AFTER INSERT  
AS RAISERROR ('Notify Compensation', 16, 10);  
GO  
  
```  
  
#### <a name="to-rename-a-trigger-using-drop-trigger-and-alter-trigger"></a>Pour renommer un déclencheur à l'aide de DROP TRIGGER et ALTER TRIGGER  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple utilise les instructions [DROP TRIGGER](../../t-sql/statements/drop-trigger-transact-sql.md) et [ALTER TRIGGER](../../t-sql/statements/alter-trigger-transact-sql.md) pour renommer le déclencheur `Sales.bonus_reminder` en `Sales.bonus_reminder_2`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID(N'Sales.bonus_reminder', N'TR') IS NOT NULL  
    DROP TRIGGER Sales.bonus_reminder;  
GO  
CREATE TRIGGER Sales.bonus_reminder_2  
ON Sales.SalesPersonQuotaHistory  
WITH ENCRYPTION  
AFTER INSERT, UPDATE   
AS RAISERROR ('Notify Compensation', 16, 10);  
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
 [Obtenir des informations sur les déclencheurs DML](../../relational-databases/triggers/get-information-about-dml-triggers.md)   
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
  
  
