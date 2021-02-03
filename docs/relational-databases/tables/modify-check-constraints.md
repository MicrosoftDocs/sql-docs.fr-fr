---
description: Modifier des contraintes de validation
title: Modifier des contraintes de validation | Microsoft Docs
ms.custom: ''
ms.date: 06/28/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- CHECK constraints, modifying
- modifying constraints
- constraints [SQL Server], check
- constraints [SQL Server], modifying
ms.assetid: f22daef8-e350-40ef-8ff0-b5f87d1d9e56
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dcb4e032fad3582caf9f7e6f7aaa77dad38a69b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195799"
---
# <a name="modify-check-constraints"></a>Modifier des contraintes de validation
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Vous pouvez modifier une contrainte de validation dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)] lorsque vous voulez changer l'expression de contrainte ou les options qui activent ou désactivent la contrainte pour des conditions spécifiques.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour modifier une contrainte de validation à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert une autorisation ALTER sur la table.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-modify-a-check-constraint"></a>Pour modifier une contrainte de validation  
  
1.  Dans l’ **Explorateur d’objets**, cliquez avec le bouton droit sur la table contenant la contrainte de validation, puis sélectionnez **Conception**.  
  
2.  Dans le menu **Concepteur de tables** , cliquez sur **Vérifier les contraintes...**.  
  
3.  Sélectionnez dans la liste **Contrainte de validation sélectionnée** de la boîte de dialogue **Contraintes de validation** la la contrainte que vous souhaitez modifier.  
  
4.  Effectuez l'une des actions décrites dans le tableau suivant :  
  
    |À|Procédez comme suit|  
    |--------|------------------------|  
    |Modifier l'expression de contrainte|Tapez la nouvelle expression dans le champ **Expression** .|  
    |Renommer la contrainte|Tapez un nouveau nom dans le champ **Nom** .|  
    |Appliquer la contrainte à des données existantes|Choisissez Oui pour l'option **Vérifier les données existantes à la création ou à la réactivation** .|  
    |Désactiver la contrainte lorsque de nouvelles données sont ajoutées à la table ou lorsque les données existantes sont mises à jour dans la table|Désactivez la case à cocher **Appliquer la contrainte pour INSERT et UPDATE** .|  
    |Désactiver la contrainte lorsque l'Agent de réplication insère ou met à jour les données dans votre table.|Désactivez la case à cocher **Appliquer la réplication** .|  
  
    > [!NOTE]  
    >  Certaines bases de données offrent des fonctionnalités différentes pour les contraintes de validation.  
  
5.  Cliquez sur **Fermer**.  
  
6.  Dans le menu **Fichier**, cliquez sur **Enregistrer**_nom de la table_.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour modifier une contrainte de validation**  
  
 Pour modifier une contrainte `CHECK` à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)], vous devez d'abord supprimer la contrainte `CHECK` existante, puis la recréer avec la nouvelle définition. Pour plus d’informations, consultez [Supprimer des contraintes de validation](../../relational-databases/tables/delete-check-constraints.md) et [Créer des contraintes de validation](../../relational-databases/tables/create-check-constraints.md).  
  
###  <a name="TsqlExample"></a>  
