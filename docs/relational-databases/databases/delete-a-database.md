---
title: Supprimer une base de données | Microsoft Docs
description: Découvrez comment supprimer une base de données définie par l’utilisateur dans SQL Server Management Studio dans SQL Server à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- database removal [SQL Server], SQL Server Management Studio
- removing databases
- deleting databases
- dropping databases
- databases [SQL Server], dropping
- database removal [SQL Server]
ms.assetid: 1fd8c0f5-03e1-449a-af45-b8cacb479d9c
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e6835865e78147a11878b12ae8ce9c465b66f0e7
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489423"
---
# <a name="delete-a-database"></a>Supprimer une base de données
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Cette rubrique explique comment supprimer une base de données définie par l’utilisateur dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] en utilisant [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Composants requis](#Prerequisites)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour supprimer une base de données, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après la suppression d’une base de données](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Les bases de données système ne peuvent pas être supprimées.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Supprimez les instantanés de la base de données qui existent. Pour plus d’informations, consultez [Supprimer un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md).  
  
-   Si la base de données intervient dans l'envoi de journaux, supprimez l'envoi de journaux.  
  
-   Si la base de données est publiée pour une réplication transactionnelle, ou encore publiée ou souscrite pour une réplication de fusion, supprimez la réplication.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Effectuez une sauvegarde complète de la base de données. Vous ne pouvez recréer une base de données supprimée qu'en restaurant une copie de sauvegarde.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Pour exécuter DROP DATABASE, un utilisateur doit au moins disposer de l'autorisation CONTROL sur la base de données.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-delete-a-database"></a>Pour supprimer une base de données  
  
1.  Dans **l’Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], puis développez-la.  
  
2.  Développez le dossier **Bases de données**, cliquez avec le bouton droit sur la base de données à supprimer, puis cliquez sur **Supprimer**.  
  
3.  Vérifiez que la base de données correcte est sélectionnée, puis cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-delete-a-database"></a>Pour supprimer une base de données  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L'exemple suivant supprime les bases de données `Sales` et `NewSales` .  
  
```sql  
USE master ;  
GO  
DROP DATABASE Sales, NewSales ;  
GO  
```  
  
##  <a name="follow-up-after-deleting-a-database"></a><a name="FollowUp"></a> Suivi : Après la suppression d’une base de données  
 Sauvegardez la base de données **master** . Si vous devez restaurer la base de données **master** , toutes les bases de données supprimées depuis la dernière sauvegarde de **master** seront encore référencées dans les vues du catalogue système, ce qui risque de générer des messages d'erreur.  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
  
