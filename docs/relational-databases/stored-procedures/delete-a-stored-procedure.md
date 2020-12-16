---
title: Supprimer une procédure stockée | Microsoft Docs
description: Découvrez comment supprimer une procédure stockée dans SQL Server 2019 (15.x) à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: conceptual
helpviewer_keywords:
- removing stored procedures
- stored procedures [SQL Server], deleting
- deleting stored procedures
ms.assetid: 232dbf4d-392a-406f-af3a-579518cd8e46
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 742b0e7f3631f5cf262ff6e20bdc64f1ba7bea7f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475310"
---
# <a name="delete-a-stored-procedure"></a>Supprimer une procédure stockée
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
    
##  <a name="this-topic-describes-how-to-delete-a-stored-procedure-in-sscurrent-by-using-ssmanstudiofull-or-tsql"></a><a name="Top"></a> Cette rubrique explique comment supprimer une procédure stockée dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   **Avant de commencer :**  [Limitations et restrictions](#Restrictions), [sécurité](#Security)  
  
-   **Pour supprimer une procédure avec :**  [SQL Server Management Studio](#SSMSProcedure), [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
 La suppression d'une procédure peut entraîner l'échec des scripts et des objets dépendants quand ceux-ci n'ont pas été mis à jour pour refléter la suppresion de la procédure. Cependant, si vous créez une nouvelle procédure ayant le même nom et les mêmes paramètres pour remplacer celle qui a été supprimée, les autres objets qui y font référence pourront s'exécuter correctement. Pour plus d’informations, consultez [Afficher les dépendances d’une procédure stockée](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Exige l'autorisation ALTER sur le schéma auquel appartient la procédure ou l'autorisation CONTROL sur la procédure.  
  
##  <a name="how-to-delete-a-stored-procedure"></a><a name="Procedures"></a> Pour supprimer une procédure stockée  
 Vous pouvez utiliser l'un des éléments suivants :  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour supprimer une procédure dans l'Explorateur d'objets**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)] et développez-la.  
  
2.  Développez **Bases de données**, développez la base de données à laquelle appartient la procédure, puis développez **Programmabilité**.  
  
3.  Développez **Procédures stockées**, cliquez avec le bouton droit sur la procédure à supprimer, puis sélectionnez **Supprimer**.  
  
4.  Pour afficher les objets qui dépendent de la procédure, cliquez sur **Afficher les dépendances**.  
  
5.  Vérifiez que la procédure correcte est sélectionnée, puis cliquez sur **OK**.  
  
6.  Supprimez les références à la procédure à partir de tous les objets et scripts dépendants.  

###  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour supprimer une procédure dans l'Éditeur de requête**  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et développez-la.  
  
2.  Développez **Bases de données**, développez la base de données à laquelle appartient la procédure, ou, dans la barre d'outils, sélectionnez la base de données dans la liste des bases de données disponibles.  
  
3.  Dans le menu Fichier, cliquez sur **Nouvelle requête**.  
  
4.  Obtient le nom de la procédure stockée à supprimer dans la base de données active. Dans l'Explorateur d'objets, développez **Programmabilité** , puis **Procédures stockées**. Sinon, dans l'éditeur de requête, exécutez l'instruction suivante.  
  
    ```sql  
    SELECT name AS procedure_name   
        ,SCHEMA_NAME(schema_id) AS schema_name  
        ,type_desc  
        ,create_date  
        ,modify_date  
    FROM sys.procedures;  
    ```  
  
5.  Copiez et collez l'exemple suivant dans l'éditeur de requête et insérez un nom de procédure stockée à supprimer de la base de données active.  
  
    ```sql  
    DROP PROCEDURE <stored procedure name>;  
    GO  
    ```  
  
6.  Supprimez les références à la procédure à partir de tous les objets et scripts dépendants.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer une procédure stockée](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [Modifier une procédure stockée](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)   
 [Renommer une procédure stockée](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)   
 [Afficher la définition d'une procédure stockée](../../relational-databases/stored-procedures/view-the-definition-of-a-stored-procedure.md)   
 [Afficher les dépendances d’une procédure stockée](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md)   
 [DROP PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-procedure-transact-sql.md)  
  
  
