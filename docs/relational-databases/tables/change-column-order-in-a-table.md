---
description: Modifier l'ordre des colonnes dans une table
title: Modifier l’ordre des colonnes dans une table | Microsoft Docs
ms.custom: ''
ms.date: 06/15/2018
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], change order in a table
- column order, change
ms.assetid: cd99ef56-9085-431a-a0fc-58e7add5399f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d8064366c25925bf5ba6781e0afa0168a8df04fe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181520"
---
# <a name="change-column-order-in-a-table"></a>Modifier l'ordre des colonnes dans une table

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Vous pouvez changer l'ordre des colonnes dans le Concepteur de tables dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
> [!CAUTION]  
>  Changer l'ordre des colonnes d'une table peut avoir une incidence sur le code et les applications qui dépendent de l'ordre spécifique des colonnes. Il peut s'agir de requêtes, de vues, de procédures stockées, de fonctions définies par l'utilisateur et d'applications clientes. Prenez toutes les précautions nécessaires avant de changer l'ordre des colonnes. La recommandation est de spécifier l'ordre dans lequel les colonnes sont renvoyées au niveau de l'application et de la requête. Vous ne pouvez pas compter sur l'utilisation de SELECT * pour retourner toutes les colonnes dans une commande prévue d'après l'ordre dans lequel elles sont définies dans la table. Spécifiez toujours le nom des colonnes dans vos requêtes et applications dans l'ordre dans lequel vous souhaitez qu'elles apparaissent.  
  
 **Dans cette rubrique**  
  
-   **Pour changer l'ordre des colonnes à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-change-the-column-order"></a>Pour changer l'ordre des colonnes  
  
1.  Dans l’ **Explorateur d’objets**, cliquez avec le bouton droit sur la table dont vous souhaitez réorganiser les colonnes et cliquez sur **Conception**.  
  
2.  Sélectionnez la case à gauche du nom de la colonne à réordonner.  
  
3.  Faites glisser la colonne vers un autre emplacement dans la table.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour changer l'ordre des colonnes**  
  
 Cette tâche n’est pas prise en charge à l’aide d’instructions Transact-SQL.  
  
###  <a name="TsqlExample"></a>  
