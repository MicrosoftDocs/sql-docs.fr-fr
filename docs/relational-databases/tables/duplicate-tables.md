---
description: Créer une copie dupliquée d’une table, sans les données de ligne
title: Dupliquer des tables | Microsoft Docs
ms.custom: ''
ms.date: 10/05/2020
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- copying tables
- tables [SQL Server], duplicating
- duplicating tables
- duplicating table structures
- table copying [SQL Server]
ms.assetid: c6b07423-d1e5-4e5e-8681-5088921f5df3
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 43f0ddc7e9737fca68d888ba66caef8a3c073781
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179719"
---
# <a name="duplicate-tables"></a>Dupliquer des tables
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

Vous pouvez dupliquer une table existante dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)] en créant une table puis en copiant les informations sur les colonnes d'une table existante.  
  
> [!IMPORTANT]  
>  Cette opération permet de dupliquer uniquement la structure d'une table, elle ne duplique aucune ligne de table.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour dupliquer une table à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l'autorisation CREATE TABLE dans la base de données de destination.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-duplicate-a-table"></a>Pour dupliquer une table  
  
1.  Vérifiez que vous êtes connecté à la base de données dans laquelle vous voulez créer la table et que la base de données est sélectionnée dans l'Explorateur d'objets.  
  
2.  Dans l’Explorateur d'objets, cliquez avec le bouton droit sur **Tables** , puis cliquez sur **Nouvelle table**.  
  
3.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur la table que vous souhaitez copier et cliquez sur **Conception**.  
  
4.  Sélectionnez les colonnes dans la table existante et, dans le menu **Edition** , cliquez sur **Copier**.  
  
5.  Revenez à la nouvelle table et sélectionnez la première ligne.  
  
6.  Dans le menu **Edition** , cliquez sur **Coller**.  
  
7.  Dans le menu **Fichier** , cliquez sur **Enregistrer**_nom_table_.  
  
8.  Dans la boîte de dialogue **Choisir un nom** , tapez un nom pour la nouvelle table et cliquez sur **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-duplicate-a-table-in-query-editor"></a>Pour dupliquer une table dans l'éditeur de requête  
  
1.  Vérifiez que vous êtes connecté à la base de données dans laquelle vous voulez créer la table et que la base de données est sélectionnée dans l'Explorateur d'objets.  
  
2.  Cliquez avec le bouton droit sur la table que vous souhaitez dupliquer, pointez sur **Générer un script de la table en tant que**, puis pointez sur **CREATE To** et sélectionnez **Nouvelle fenêtre d’éditeur de requête**.  
  
3.  Modifiez le nom de la table.  
  
4.  Supprimez toutes les colonnes qui ne sont pas nécessaires dans la nouvelle table.  
  
5.  Cliquez sur **Exécuter**.  
