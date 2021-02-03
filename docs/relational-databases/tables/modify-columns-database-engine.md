---
description: Modifier des colonnes (moteur de base de données)
title: Modifier des colonnes (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- modifying data types
- column data types [SQL Server]
- data types [SQL Server], columns
ms.assetid: b67b95c5-61ef-4bd8-9a3e-1640eb7583ac
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 099e0d09eff857f0480ec61ae4966490cfc61bc1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195791"
---
# <a name="modify-columns-database-engine"></a>Modifier des colonnes (moteur de base de données)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Vous pouvez modifier le type de données d'une colonne dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!WARNING]  
>  La modification du type de données d'une colonne qui contient déjà des données peut entraîner la perte définitive des données lors de leur conversion en ce nouveau type. De plus, le code et les applications qui dépendent de la colonne modifiée peuvent échouer. Il peut s'agir de requêtes, de vues, de procédures stockées, de fonctions définies par l'utilisateur et d'applications clientes. Notez que ces défaillances se produisent en cascade. Par exemple, une procédure stockée qui appelle une fonction définie par l'utilisateur dépendant de la colonne modifiée peut échouer. Prenez toutes les précautions nécessaires avant de modifier une colonne.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour modifier le type de données d'une colonne à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert une autorisation ALTER sur la table.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-modify-the-data-type-of-a-column"></a>Pour modifier le type de données d'une colonne  
  
1.  Dans **l’Explorateur d’objets**, cliquez avec le bouton droit sur la table contenant les colonnes dont vous souhaitez modifier l’échelle et cliquez sur **Conception**.  
  
2.  Sélectionnez la colonne pour laquelle vous souhaitez modifier le type de données.  
  
3.  Sous l’onglet **Propriétés des colonnes** , cliquez sur la cellule de grille correspondant à la propriété **Type de données** et choisissez un nouveau type de données dans la liste déroulante.  
  
4.  Dans le menu **Fichier**, cliquez sur **Enregistrer**_nom de la table_.  
  
> [!NOTE]  
>  Lorsque vous modifiez le type de données d'une colonne, le Concepteur de tables applique la longueur par défaut du type de données que vous avez sélectionné, même si vous en avez déjà spécifié une autre. Affectez toujours la valeur souhaitée comme longueur de type de données après avoir spécifié le type de données.  
  
> [!WARNING]  
>  Si vous tentez de modifier le type de données d'une colonne associée à d'autres tables, le Concepteur de tables vous demande de confirmer que la modification concerne également les colonnes d'autres tables.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-modify-the-data-type-of-a-column"></a>Pour modifier le type de données d'une colonne  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```sql  
    CREATE TABLE dbo.doc_exy (column_a INT ) ;  
    GO  
    INSERT INTO dbo.doc_exy (column_a) VALUES (10) ;  
    GO  
    ALTER TABLE dbo.doc_exy ALTER COLUMN column_a DECIMAL (5, 2) ;  
    GO  
  
    ```  
  
 Pour plus d’informations, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
  
