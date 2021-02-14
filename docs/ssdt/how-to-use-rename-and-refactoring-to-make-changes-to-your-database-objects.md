---
title: Renommer et Refactorisation pour apporter des modifications à vos objets de base de données
description: En savoir plus sur les menus Renommer et Refactoriser dans l’éditeur Transact-SQL. Découvrez comment renommer des types, basculer des schémas d’objet, développer des caractères génériques et qualifier des noms complets.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.dbrefactoring.previewdialog
- sql.data.tools.editor.howto.refactoring
- sql.data.tools.dbrefactoring.renamedialog
- sql.data.tools.dbrefactoring.moveschemadialog
- sql.data.tools.dbrefactoring.renameserverdatabasedialog
ms.assetid: f35520e6-8e6e-47b1-87a3-22c0cf2cabdb
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 8fd846301467dbbcbf08aa6bf4a02e52fb467c4b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018019"
---
# <a name="how-to-use-rename-and-refactoring-to-make-changes-to-your-database-objects"></a>Procédure : Utiliser Renommer et Refactorisation pour apporter des modifications à vos objets de base de données

Le menu contextuel Refactoriser de l'Éditeur Transact\-SQL vous permet de renommer ou déplacer un objet vers un autre schéma, et d'afficher un aperçu de toutes les zones affectées avant de valider les modifications. Vous pouvez aussi utiliser le menu Refactoriser pour effectuer une qualification complète de toutes les références à des objets de base de données, ou pour développer les caractères génériques utilisés dans les instructions `SELECT` de votre projet de base de données.  
  
> [!NOTE]  
> Les procédures suivantes utilisent les entités créées dans les procédures précédentes des sections [Développement de base de données connectée](../ssdt/connected-database-development.md) et [Développement de base de données hors connexion orienté projet](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-rename-a-type"></a>Pour renommer un type  
  
1.  Cliquez avec le bouton droit sur la table **Products** (Products.sql) dans l'**Explorateur de solutions**, puis sélectionnez **Afficher le code** pour ouvrir le script dans l'Éditeur Transact\-SQL.  
  
2.  Cliquez avec le bouton droit sur `[Products]` dans le script, sélectionnez **Refactoriser**, puis **Renommer**.  
  
3.  Dans le champ **Nouveau nom**, modifiez-le en **Product**. Conservez la case à cocher **Aperçu des modifications** activée, puis cliquez sur **OK**.  
  
4.  Dans l'écran suivant, vous serez en mesure d'afficher un aperçu d'une liste de scripts qui seront affectés par cette opération d'attribution d'un nouveau nom. Plus précisément, tous les emplacements qui font référence à `Products` seront mis en surbrillance. Cette opération est très similaire à la tâche Rechercher toutes les références de la procédure précédente. Cliquez n'importe où dans le volet supérieur et consultez la modification réelle dans les scripts (en vert) dans le volet inférieur.  
  
5.  Cliquez sur **Appliquer**.  
  
6.  Pour les fichiers de script qui sont déjà ouverts dans le Concepteur de tables ou l'Éditeur Transact\-SQL, notez que l'Éditeur Transact\-SQL a mis en surbrillance les emplacements où les modifications ont été apportées avec une barre verte à gauche.  
  
7.  Notez l'ajout de **TradeDev.refactorlog** dans l'**Explorateur de solutions**. Double-cliquez sur ce fichier pour l'ouvrir. Il contient une représentation XML de toutes les modifications de cette session.  
  
8.  Appuyez sur F5 pour générer et déployer le projet dans la base de données locale.  
  
9. Dans l’**Explorateur d'objets SQL Server**, **sous Local**, cliquez avec le bouton droit sur **TradeDev** et sélectionnez **Actualiser**.  
  
10. Développez **Tables**, et notez que la table **Products** a été renommée.  
  
11. Cliquez avec le bouton droit sur la table **Product** et sélectionnez **Afficher les données**. Notez que les données existantes sont intactes indépendamment du changement de nom.  
  
### <a name="to-expand-wildcards"></a>Pour développer des caractères génériques  
  
1.  Dans l'**Explorateur de solutions**, développez le nœud **Fonctions** et double-cliquez sur le fichier **GetProductsBySupplier.sql**.  
  
2.  Positionnez le curseur sur l'astérisque dans cette ligne et cliquez avec le bouton droit. Sélectionnez **Refactoriser**, puis **Développer les caractères génériques**.  
  
    ```  
    SELECT * from Product p  
    ```  
  
3.  Dans la boîte de dialogue **Aperçu des modifications**, cliquez sur `SELECT * from Product p` dans le volet supérieur pour le mettre en surbrillance.  
  
4.  Dans le volet **Aperçu des modifications** en dessous, notez que `*` a été développé comme suit dans le script.  
  
    ```  
    [Id], [Name], [ShelfLife], [SupplierId], [CustomerId]  
    ```  
  
5.  Cliquez sur le bouton **Appliquer**.  Notez que la ligne qui contient les modifications provenant de l'opération d'expansion est de nouveau mise en surbrillance avec une barre verte à gauche.  
  
### <a name="to-fully-qualify-database-object-names"></a>Pour qualifier complètement le nom d'objets de base de données  
  
1.  Vérifiez que **GetProductsBySupplier.sql** est toujours ouvert dans l'Éditeur Transact\-SQL.  
  
2.  Positionnez le curseur sur `Product` dans cette ligne et cliquez avec le bouton droit. Sélectionnez **Refactoriser**, puis **Noms qualifiés complets**.  
  
    ```  
    SELECT [Id], [Name], [ShelfLife], [SupplierId], [CustomerId] from Product p  
    ```  
  
3.  Dans la boîte de dialogue **Aperçu des modifications**, cliquez sur le bouton **Appliquer**.  Notez que toutes les références d'objet ont été mises à jour pour inclure le nom du schéma de l'objet et, si l'objet a un parent, le nom du parent.  
  
    ```  
    SELECT [p].[Id], [p].[Name], [p].[ShelfLife], [p].[SupplierId], [p].[CustomerId] from [dbo].[Product] p  
    ```  
  
### <a name="to-move-schema"></a>Pour déplacer le schéma  
  
1.  Cliquez avec le bouton droit sur l'objet que vous souhaitez déplacer. Sélectionnez **Refactoriser**, puis **Déplacer le schéma**.  
  
2.  Dans la liste **Nouveau schéma**, cliquez sur le nom du schéma vers lequel vous souhaitez déplacer l'objet. Cliquez sur OK.  
  
    Si vous avez sélectionné la case à cocher **Aperçu des modifications**, la boîte de dialogue **Aperçu des modifications** s'affiche. Dans le cas contraire, le nom de l'objet est mis à jour et l'objet est déplacé vers le nouveau schéma.  
  
