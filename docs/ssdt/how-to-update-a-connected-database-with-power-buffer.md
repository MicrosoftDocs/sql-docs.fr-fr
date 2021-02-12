---
title: Mettre à jour une base de données connectée avec Power Buffer
description: Découvrez comment utiliser le tampon Power pour mettre à jour une base de données. Consultez comment vérifier les modifications avant de les appliquer et comment enregistrer les modifications apportées à un script en vue d’un déploiement ultérieur.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.commitpreview.dialog
ms.assetid: 4048b7f8-71a9-47ad-b812-3fc1e8066240
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2ecad870723402e33026668055368dac5dfd02c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018089"
---
# <a name="how-to-update-a-connected-database-with-power-buffer"></a>Procédure : Mettre à jour une base de données connectée avec Power Buffer

La technologie SQL Server Data Tools Power Buffer facilite l'application de modifications à votre base de données connectée en stockant toutes vos modifications dans la session active. Les erreurs dues à la modification dans la fenêtre Power Buffer (dans l'Éditeur Transact\-SQL ou le Concepteur de tables) s'affichent immédiatement dans le volet **Liste d'erreurs**, ce qui vous permet de suivre les erreurs identifiées pour la résolution. Vous pouvez vérifier vos modifications en attente jusqu'à ce que vous soyez prêt à les appliquer dans votre base de données. Au cours du processus de mise à jour, SSDT crée automatiquement un script ALTER basé sur vos modifications, et vous avertit de tout problème potentiel. Vous pouvez ensuite appliquer toutes les modifications accumulées dans les fenêtres Power Buffer ouvertes à la même base de données, ou enregistrer le script ALTER à déployer ultérieurement.  
  
SSDT tient également compte de toute modification apportée à votre schéma de base de données hors de Visual Studio. Par exemple, si vous ajoutez une nouvelle table à une base de données existante dans SQL Server Management Studio, cette modification s'affiche immédiatement dans l'Explorateur d'objets SQL Server de Visual Studio sans actualisation manuelle. La fonctionnalité de détection de dérive garantit que vous consultez toujours la définition de schéma actuelle d'une base de données dans l'Explorateur d'objets SQL Server. Notez que les objets de base de données ouverts dans le Concepteur de tables ou l'Éditeur Transact\-SQL pour modification ne sont pas actualisés pour afficher les modifications hors de Visual Studio.  
  
Les procédures suivantes utilisent les entités créées dans les procédures précédentes de la section [Développement d’une base de données connectée](../ssdt/connected-database-development.md).  
  
### <a name="to-apply-the-changes-made-in-the-previous-procedures"></a>Pour appliquer les modifications apportées au cours des procédures précédentes  
  
1.  Cliquez sur le bouton vert **Mettre à jour** dans la barre d’outils (l’info-bulle « Mettre à jour la base de données » s’affiche si vous placez le curseur sur le bouton). La barre d'outils se trouve au-dessus de la Grille Colonnes du Concepteur de tables.  
  
2.  La boîte de dialogue **Aperçu des mises à jour de la base de données** apparaît. Un script de déploiement basé sur vos modifications est généré en arrière-plan. La boîte de dialogue affiche ensuite un résumé des actions qui seront effectuées par SSDT (par exemple, création ou suppression d'entités de base de données), ainsi que les problèmes potentiels identifiés (cela ne s'applique pas à notre procédure, mais est pratique lorsque votre définition de base de données contient des erreurs qui empêchent une mise à jour tant qu'elles ne sont pas résolues).  
  
3.  Si vous ne souhaitez pas mettre à jour la base de données pour l'instant, cliquez sur le bouton **Annuler** pour quitter la boîte de dialogue **Aperçu des mises à jour de la base de données**.  
  
4.  Si vous maîtrisez les modifications jusqu'à présent, cliquez sur le bouton **Mettre à jour la base de données** dans la boîte de dialogue **Aperçu des mises à jour de la base de données**. Le script de déploiement s'exécute en votre nom, et les modifications accumulées sont à présent appliquées à la base de données.  
  
5.  Si vous souhaitez consulter le script de déploiement à des fins de vérification, ou apporter des modifications avant la mise à jour, cliquez sur le bouton **Générer un script** dans la boîte de dialogue **Aperçu des mises à jour de la base de données**. Le script généré s'ouvre dans une nouvelle fenêtre de l'Éditeur Transact\-SQL. Vous pouvez cliquer sur le bouton **Exécuter la requête** dans la barre d'outil de l'Éditeur Transact\-SQL pour exécuter cette requête. Cette opération est similaire à l'action du bouton **Mettre à jour la base de données** utilisé à l'étape 4.  
  
    > [!WARNING]  
    > Si vous apportez des modifications au script de déploiement et l'exécutez, ces modifications ne s'afficheront pas dans les entités de base de données ouvertes. Par exemple, si vous renommez une colonne de la table `Customers` dans le script de déploiement et l'exécutez pour mettre à jour la base de données, et si la table `Customers` est ouverte dans le Concepteur de tables, le nom de la colonne sera encore l'ancien nom utilisé lorsque vous avez cliqué sur le bouton **Mettre à jour la base de données**. Vous devez fermer manuellement le Concepteur de tables sans l'enregistrer localement en tant que script. Lorsque vous rouvrirez la table dans l'**Explorateur d'objets SQL Server**, vous noterez que la base de données a été mise à jour avec les modifications apportées dans le script de déploiement.  
  
6.  Dans le volet **Sortie** de l'Éditeur Transact\-SQL (ou le volet **Message** si vous exécutez vous-même le script de déploiement), notez les messages suivants qui indiquent que la mise à jour a réussi.  
  
**Création de [dbo].[Customers]...Création de [dbo].[Products]...Création de [dbo].[Suppliers]...Création de FK_Products_SupplierId...Création de FK_Products_CustomerId...Création de CK_Products_ShelfLife La partie des transactions de la mise à jour de la base de données a réussi. Vérification des données existantes par rapport au nouveau constraintsUpdate créé terminée.**  
  
7.  Dans l'**Explorateur d'objets SQL Server**, notez que les nouvelles tables sont affichées sous le nœud **Tables** de la base de données **Trade**.  
  
### <a name="to-view-changes-made-to-a-database-outside-visual-studio"></a>Pour afficher les modifications apportées à une base de données hors de Visual Studio  
  
1.  Ouvrez SQL Server Management Studio. Dans la boîte de dialogue **Se connecter au serveur**, entrez le nom du serveur de base de données auquel vous étiez connecté dans Visual Studio et cliquez sur **Se connecter**.  
  
2.  Dans l’**Explorateur d'objets SQL Server**, développez **Bases de données** et accédez à la base de données **Trade**.  
  
3.  Cliquez avec le bouton droit sur **Tables** sous **Trade** et sélectionnez **Nouvelle table**. Dans le Concepteur de tables, entrez **id** comme Nom de colonne et **int** comme Type de données.  
  
4.  Pour enregistrer la table, cliquez sur l'icône **Enregistrer** dans la barre d'outils. Acceptez le nom par défaut, puis cliquez sur **OK**.  
  
    Revenez à Visual Studio. Examinez le nœud **Tables** sous la base de données **Trade** dans l’**Explorateur d'objets SQL Server**. Notez l'apparence de la nouvelle table **Table_1** créée.  
  
5.  Cliquez avec le bouton droit sur **Table_1**, puis sélectionnez **Supprimer**. Dans la boîte de dialogue **Aperçu des mises à jour de la base de données**, cliquez sur **Mettre à jour la base de données**.  
  
## <a name="see-also"></a>Voir aussi  
[Procédure : corriger les erreurs](../ssdt/how-to-fix-errors.md)  
  
