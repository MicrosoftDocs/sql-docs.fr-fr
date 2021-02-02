---
description: Activer les index et contraintes
title: Activer les index et contraintes | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- indexes [SQL Server], enabling
- nonclustered indexes [SQL Server], enabling a disabled index
- index enabling [SQL Server]
- disabled indexes [SQL Server], how to enable
- constraints [SQL Server], enabling
- clustered indexes, enabling disabled indexes
ms.assetid: c55c8865-322e-4ab0-ba04-ea1f56735353
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0d309a308e71057f71fd20b0d0652d14a6011ce5
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048902"
---
# <a name="enable-indexes-and-constraints"></a>Activer les index et contraintes
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Cette rubrique explique comment activer un index désactivé dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Lorsqu'un index est désactivé, il reste à l'état désactivé tant qu'il n'est pas reconstruit ou supprimé  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour activer un index désactivé, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Après la reconstruction de l'index, toutes les contraintes qui étaient désactivées du fait de la désactivation de l'index doivent être réactivées manuellement. Les contraintes PRIMARY KEY et UNIQUE sont activées par la reconstruction de l'index associé. Cet index doit être reconstruit (activé) avant que les contraintes FOREIGN KEY qui font référence à la contrainte PRIMARY KEY ou UNIQUE puissent être activées. Les contraintes FOREIGN KEY sont activées à l'aide de l'instruction ALTER TABLE CHECK CONSTRAINT.  
  
-   La reconstruction d'un index cluster désactivé n'est pas possible lorsque l'option ONLINE a la valeur ON.  
  
-   Lorsque l'index cluster est désactivé ou activé et que l'index non-cluster est désactivé, l'action sur l'index cluster produit les résultats ci-dessous sur l'index non-cluster désactivé.  
  
    |Action sur l'index cluster|Index non cluster désactivé…|  
    |----------------------------|-----------------------------------|  
    |ALTER INDEX REBUILD|Reste désactivé.|  
    |ALTER INDEX ALL REBUILD|Est reconstruit et activé.|  
    |DROP INDEX|Reste désactivé.|  
    |CREATE INDEX WITH DROP_EXISTING|Reste désactivé.|  
  
     La création d'un nouvel index cluster suit le même comportement que ALTER INDEX ALL REBUILD.  
  
-   Les actions autorisées sur les index non-cluster associés à un index cluster dépendent de l'état, désactivé ou activé, des deux types d'index. Le tableau ci-dessous récapitule les actions autorisées sur les index non-cluster.  
  
    |Action sur l'index non cluster|Lorsque les index cluster et non cluster sont désactivés.|Lorsque l'index cluster est activé et que l'index non cluster est activé ou désactivé.|  
    |-------------------------------|--------------------------------------------------------------------|----------------------------------------------------------------------------------------|  
    |ALTER INDEX REBUILD|L'action échoue.|L'action réussit.|  
    |DROP INDEX|L'action réussit.|L'action réussit.|  
    |CREATE INDEX WITH DROP_EXISTING|L'action échoue.|L'action réussit.|  

-   Lors de la regénération d’index non-cluster compressés désactivés, data_compression est défini sur 'none' par défaut, ce qui signifie que les index seront décompressés. Cela est dû à la perte des métadonnées des paramètres de compression quand les index non-cluster sont désactivés. Pour contourner ce problème, spécifiez la compression de données explicite dans une instruction rebuild.

###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite une autorisation ALTER sur la table ou la vue. S’il utilise DBCC DBREINDEX, l’utilisateur doit être propriétaire de la table ou être membre du rôle serveur fixe **sysadmin** ou des rôles de base de données fixes **db_ddladmin** et **db_owner**.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-enable-a-disabled-index"></a>Pour activer un index désactivé  
  
1.  Dans l'Explorateur d'objets, cliquez sur le signe plus (+) pour développer la base de données qui contient la table sur laquelle vous souhaitez activer un index.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Tables** .  
  
3.  Cliquez sur le signe plus (+) pour développer la table sur laquelle vous souhaitez activer un index.  
  
4.  Cliquez sur le signe plus (+) pour développer le dossier **Index** .  
  
5.  Cliquez avec le bouton droit sur l’index que vous souhaitez activer et sélectionnez **Reconstruire**.  
  
6.  Dans la boîte de dialogue **Reconstruire les index** , vérifiez que l'index correct figure dans la grille **Index à reconstruire** , puis cliquez sur **OK**.  
  
#### <a name="to-enable-all-indexes-on-a-table"></a>Pour activer tous les index d'une table  
  
1.  Dans l'Explorateur d'objets, cliquez sur le signe plus (+) pour développer la base de données qui contient la table sur laquelle vous souhaitez activer les index.  
  
2.  Cliquez sur le signe plus (+) pour développer le dossier **Tables** .  
  
3.  Cliquez sur le signe plus (+) pour développer la table sur laquelle vous souhaitez activer les index.  
  
4.  Cliquez avec le bouton droit sur le dossier **Index** et sélectionnez **Tout reconstruire**.  
  
5.  Dans la boîte de dialogue **Reconstruire les index** , vérifiez que les index corrects figurent dans la grille **Index à reconstruire** , puis cliquez sur **OK**. Pour supprimer un index de la grille **Index à reconstruire** , sélectionnez l'index et appuyez sur la touche SUPPR.  
  
 Les informations suivantes sont disponibles dans la boîte de dialogue **Reconstruire les index** :  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-enable-a-disabled-index-using-alter-index"></a>Pour activer un index désactivé à l'aide de ALTER INDEX  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Enables the IX_Employee_OrganizationLevel_OrganizationNode index  
    -- on the HumanResources.Employee table.  
  
    ALTER INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
    REBUILD;   
    GO  
    ```  
  
#### <a name="to-enable-a-disabled-index-using-create-index"></a>Pour activer un index désactivé à l'aide de CREATE INDEX  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- re-creates the IX_Employee_OrganizationLevel_OrganizationNode index  
    -- on the HumanResources.Employee table  
    -- using the OrganizationLevel and OrganizationNode columns  
    -- and then deletes the existing IX_Employee_OrganizationLevel_OrganizationNode index  
    CREATE INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
       (OrganizationLevel, OrganizationNode)  
    WITH (DROP_EXISTING = ON);  
    GO  
    ```  
  
#### <a name="to-enable-a-disabled-index-using-dbcc-dbreindex"></a>Pour activer un index désactivé à l'aide de DBCC DBREINDEX  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    -- enables the IX_Employee_OrganizationLevel_OrganizationNode index  
    -- on the HumanResources.Employee table  
    DBCC DBREINDEX ("HumanResources.Employee", IX_Employee_OrganizationLevel_OrganizationNode);  
    GO  
    ```  
  
#### <a name="to-enable-all-indexes-on-a-table-using-alter-index"></a>Pour activer tous les index d'une table à l'aide de ALTER INDEX  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- enables all indexes  
    -- on the HumanResources.Employee table  
    ALTER INDEX ALL ON HumanResources.Employee  
    REBUILD;  
    GO  
    ```  
  
#### <a name="to-enable-all-indexes-on-a-table-using-dbcc-dbreindex"></a>Pour activer tous les index d'une table à l'aide de DBCC DBREINDEX  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    -- enables all indexes  
    -- on the HumanResources.Employee table  
    DBCC DBREINDEX ("HumanResources.Employee", " ");  
    GO  
    ```  
  
 Pour plus d’informations, consultez [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md), [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md) et [DBCC DBREINDEX &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md).  
  
  
