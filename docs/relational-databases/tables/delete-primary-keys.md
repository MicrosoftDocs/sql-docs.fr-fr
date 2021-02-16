---
description: Supprimer des clés primaires
title: Supprimer des clés primaires | Microsoft Docs
ms.custom: ''
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- removing primary keys
- deleting primary keys
- primary keys [SQL Server], deleting
ms.assetid: c472e465-7bdd-4d74-8fc9-e47fca007ccb
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: de6753c96323d19bcf31782c447cddaaf42b4cf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346683"
---
# <a name="delete-primary-keys"></a>Supprimer des clés primaires

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Vous pouvez supprimer une clé primaire dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Lorsque la clé primaire est supprimée, l'index correspondant l'est également.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour supprimer une clé primaire à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert une autorisation ALTER sur la table.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-delete-a-primary-key-constraint-using-object-explorer"></a>Pour supprimer une contrainte de clé primaire à l'aide de l'Explorateur d'objets  
  
1.  Dans l'Explorateur d'objets, développez la table qui contient la clé primaire, puis développez **Clés**.  
  
2.  Cliquez avec le bouton droit sur la clé, puis sélectionnez **Supprimer**.  
  
3.  Dans la boîte de dialogue **Supprimer un objet** , vérifiez que la clé correcte est spécifiée et cliquez sur **OK**.  
  
#### <a name="to-delete-a-primary-key-constraint-using-table-designer"></a>Pour supprimer une contrainte de clé primaire à l'aide du Concepteur de tables  
  
1.  Dans l’Explorateur d'objets, cliquez avec le bouton droit sur la table avec la clé primaire, puis cliquez sur **Conception**.  
  
2.  Dans la grille de la table, cliquez avec le bouton droit sur la ligne contenant la clé primaire et choisissez **Supprimer la clé primaire** pour désactiver le paramètre.  
  
    > [!NOTE]  
    >  Pour annuler cette action, fermez la table sans enregistrer les modifications. La suppression d'une clé primaire ne peut pas être annulée sans perdre toutes les autres modifications apportées à la table.  
  
3.  Dans le menu **Fichier**, cliquez sur **Enregistrer**_nom de la table_.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-delete-a-primary-key-constraint"></a>Pour supprimer une contrainte de clé primaire  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. L'exemple identifie d'abord le nom de la contrainte de clé primaire et supprime ensuite la contrainte.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Return the name of primary key.  
    SELECT name  
    FROM sys.key_constraints  
    WHERE type = 'PK' AND OBJECT_NAME(parent_object_id) = N'TransactionHistoryArchive';  
    GO  
    -- Delete the primary key constraint.  
    ALTER TABLE Production.TransactionHistoryArchive  
    DROP CONSTRAINT PK_TransactionHistoryArchive_TransactionID;   
    GO  
    ```  
  
 Pour plus d’informations, consultez [ALTER TABLE& #40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) et [sys.key_constraints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-key-constraints-transact-sql.md)  
  
###  <a name="TsqlExample"></a>  
