---
description: Modifier des relations de clé étrangère
title: Modifier des relations de clé étrangère | Microsoft Docs
ms.custom: ''
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- vdtsql.chm:65538
- vdt.ppg.relationships
helpviewer_keywords:
- foreign keys [SQL Server], modifying
- modifying foreign keys
ms.assetid: 0c9ca80d-d79b-44c4-a21e-0fce39c398ec
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 457f4db2126b314237ccc09678718e14ffc25069
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354123"
---
# <a name="modify-foreign-key-relationships"></a>Modifier des relations de clé étrangère

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Vous pouvez modifier le côté clé étrangère d'une relation dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. La modification de la clé étrangère d’une table modifie les colonnes liées aux colonnes figurant dans la table de clé primaire.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour modifier une clé étrangère, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
 Le type de données et la taille de la nouvelle colonne clé étrangère doivent correspondre à ceux de la colonne clé primaire à laquelle elle est associée, à ceci près :  
  
-   Une colonne **char** ou **sysname** peut être en relation avec une colonne **varchar** .  
  
-   Une colonne **binary** peut être en relation avec une colonne **varbinary** .  
  
-   Un type de données alias peut être en relation avec son type de base.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert une autorisation ALTER sur la table.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-modify-a-foreign-key"></a>Pour modifier une clé étrangère  
  
1.  Dans l' **Explorateur d'objets**, développez la table avec la clé étrangère, puis développez **Clés**.  
  
2.  Cliquez avec le bouton droit sur la clé étrangère à modifier et sélectionnez **Modifier**.  
  
3.  Dans la boîte de dialogue **Relations de clé étrangère** , vous pouvez apporter les modifications suivantes.  
  
     **Relation sélectionnée**  
     Répertorie les relations existantes. Sélectionnez une relation pour afficher ses propriétés dans la partie droite de la grille. Si la liste est vide, aucune relation n'est définie pour la table.  
  
     **Ajouter**  
     Crée une nouvelle relation. Une relation valide exige que la **Spécification de tables et colonnes** soit définie.  
  
     **Supprimer**  
     Supprime la relation sélectionnée dans la liste **Relation sélectionnée** . Pour annuler l'ajout d'une relation, supprimez la relation à l'aide de ce bouton.  
  
     **Catégorie Général**  
     Se développe pour afficher **Vérifier les données existantes à la création ou à la réactivation** et **Spécification de tables et colonnes**.  
  
     **Vérifier les données existantes à la création ou à la réactivation**  
     Vérifie en fonction de la contrainte, toutes les données qui existaient dans la table avant la création ou la réactivation de la contrainte.  
  
     **Catégorie Spécification de tables et colonnes**  
     Se développe pour afficher les colonnes des tables jouant le rôle de clé étrangère et de clé primaire (ou unique) dans la relation. Pour modifier ou définir ces valeurs, cliquez sur le bouton de sélection ( **...** ) à droite du champ de propriété.  
  
     **Table de base de clé étrangère**  
     Affiche la table qui contient la colonne jouant le rôle de clé étrangère dans la relation sélectionnée.  
  
     **Colonnes clés étrangères**  
     Affiche la colonne qui joue le rôle de clé étrangère dans la relation sélectionnée.  
  
     **Table de base de clé Primary/Unique**  
     Affiche la table qui contient la colonne jouant le rôle de clé primaire (ou unique) dans la relation sélectionnée.  
  
     **Colonnes clés Primary/Unique**  
     Affiche la colonne jouant le rôle de clé primaire (ou unique) dans la relation sélectionnée.  
  
     **Catégorie Identité**  
     Se développe pour afficher les champs de propriété pour le **Nom** et la **Description**.  
  
     **Nom**  
     Indique le nom de la relation. Lorsqu'une nouvelle relation est créée, elle obtient un nom par défaut basé sur la table affichée dans la fenêtre active du **Concepteur de tables**. Vous pouvez modifier le nom à tout moment.  
  
     **Description**  
     Décrit la relation. Pour écrire une description plus détaillée, cliquez sur **Description** , puis sur le bouton de sélection **(...)** qui apparaît à droite du champ de propriété. Vous obtiendrez une zone d'écriture plus large.  
  
     **Catégorie Concepteur de tables**  
     Se développe pour afficher des informations relatives aux options **Vérifier les données existantes à la création ou à la réactivation** et **Appliquer la réplication**.  
  
     **Appliquer la réplication**  
     Indique si la contrainte doit être appliquée lorsqu'un Agent de réplication effectue une requête Insert, Update ou Delete sur cette table.  
  
     **Appliquer la contrainte de clé étrangère**  
     Spécifie si les modifications apportées aux données des colonnes dans la relation sont autorisées lorsqu'elles annulent l'intégrité de la relation de clé étrangère. Choisissez **Oui** si vous ne souhaitez pas autoriser de telles modifications et **Non** si vous souhaitez les autoriser.  
  
     **Catégorie Spécification INSERT et UPDATE**  
     Se développe pour afficher des informations relatives aux options **Règle de suppression** et **Règle de mise à jour** pour la relation.  
  
     **Règle de suppression**  
     Spécifie ce qui se produit si un utilisateur tente de supprimer une ligne contenant des données impliquées dans une relation de clé étrangère :  
  
    -   **Aucune action** Un message d'erreur indique à l'utilisateur que la suppression n'est pas autorisée et la commande DELETE est annulée.  
  
    -   **Cascade** Supprime toutes les lignes contenant des données qui interviennent dans la relation de clé étrangère. Ne spécifiez pas CASCADE si la table est incluse dans une publication de fusion qui utilise des enregistrements logiques.  
  
    -   **Définir Null** Affecte la valeur Null si toutes les colonnes clés étrangères de la table acceptent des valeurs Null.  
  
    -   **Définir la valeur par défaut** Définit la valeur par défaut définie pour la colonne si toutes les colonnes clés étrangères de la table ont des valeurs par défaut définies.  
  
     **Règle de mise à jour**  
     Spécifie ce qui se produit si un utilisateur tente de mettre à jour une ligne contenant des données impliquées dans une relation de clé étrangère :  
  
    -   **Aucune action** Un message d'erreur indique à l'utilisateur que la mise à jour n'est pas autorisée et la commande UPDATE est annulée.  
  
    -   **Cascade** Met à jour toutes les lignes contenant des données qui interviennent dans la relation de clé étrangère. Ne spécifiez pas CASCADE si la table est incluse dans une publication de fusion qui utilise des enregistrements logiques.  
  
    -   **Définir Null** Affecte la valeur Null si toutes les colonnes clés étrangères de la table acceptent des valeurs Null.  
  
    -   **Définir la valeur par défaut** Affecte la valeur par défaut définie pour la colonne si toutes les colonnes clés étrangères de la table ont des valeurs par défaut définies.  
  
4.  Dans le menu **Fichier**, cliquez sur **Enregistrer**_nom de la table_.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour modifier une clé étrangère**  
  
 Pour modifier une contrainte FOREIGN KEY à l'aide de Transact-SQL, vous devez d'abord supprimer la contrainte FOREIGN KEY existante, puis la recréer avec sa nouvelle définition. Pour plus d'informations, consultez [Delete Foreign Key Relationships](../../relational-databases/tables/delete-foreign-key-relationships.md) et [Create Foreign Key Relationships](../../relational-databases/tables/create-foreign-key-relationships.md).  
  
###  <a name="TsqlExample"></a>  
