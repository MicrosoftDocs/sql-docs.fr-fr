---
title: Cloner une base de données existante
description: Découvrez comment cloner une base de données. Consultez les étapes impliquées dans la création d’une nouvelle base de données, la duplication de son schéma et la réplication de ses données.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: aad3594a-11cf-4e68-a622-071a93d43875
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 9dd341612f075c3a92cd01b5cfca1133910f2c57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018449"
---
# <a name="how-to-clone-an-existing-database"></a>Procédure : Cloner une base de données existante

Cette tâche utilise certaines des étapes que vous avez apprises aux cours des procédures précédentes pour créer une nouvelle base de données et déplacer des données existantes. En outre, il utilise les étapes abordées dans [Procédure : utiliser le schéma pour comparer différentes définitions de base de données](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md) pour synchroniser le schéma de base de données source et de projet.  
  
Ces étapes vous permettront de créer facilement une base de données de développement ou de test à partir d'une base de données de production avec un schéma et des données identiques. Vous pouvez ensuite continuer à développer la base de données de test en mode connecté, ou à créer un projet de base de données à des fins de développement et de test en mode hors connexion, et tout cela sans interrompre le fonctionnement de la base de données de production.  
  
> [!WARNING]  
> Les procédures suivantes utilisent les entités créées dans les procédures précédentes de la section [Développement d’une base de données connectée](../ssdt/connected-database-development.md).  
  
### <a name="to-create-a-development-database"></a>Pour créer une base de données de développement  
  
1.  Dans l'**Explorateur d'objets SQL Server**, sous le nœud **SQL Server**, développez l'instance de serveur connecté.  
  
2.  Cliquez avec le bouton droit sur le nœud **Bases de données** et sélectionnez **Ajouter une nouvelle base de données**.  
  
3.  Renommez la nouvelle base de données en **TradeDev**.  
  
4.  Dans l’**Explorateur d'objets SQL Server**, cliquez avec le bouton droit sur **Trade** et sélectionnez **Comparaison de schémas**. Suivez les étapes de la rubrique [Procédure : utiliser le schéma pour comparer différentes définitions de base de données](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md) en sélectionnant la base de données **Trade** d'origine comme source et la nouvelle base de données **TradeDev** comme cible. Ainsi, la base de données **TradeDev** sera mise à jour avec le schéma de la base de données **Trade**.  
  
### <a name="to-replicate-data"></a>Pour répliquer les données  
  
1.  L'étape précédente n'a dupliqué que le schéma de la base de données de production dans la base de données de développement. Au cours de cette procédure, vous allez dupliquer les données de production dans la base de données de développement.  
  
    Dans la base de données **Trade**, cliquez avec le bouton droit sur la table **Suppliers**, puis sélectionnez **Afficher les données**. L'éditeur de données s'ouvre.  
  
2.  Cliquez sur le bouton **Script** à côté de **Nombre de lignes maximal** dans la barre d'outils.  
  
3.  Lorsque la fenêtre de script s'ouvre, vérifiez que Connecté s'affiche dans la barre d'état sous le volet de script Transact\-SQL. Si Déconnecté s'affiche, cliquez sur le bouton **Connecter** (le plus à gauche dans la barre d'outils) et entrez vos informations serveur et d'identification.  
  
4.  Dans le menu déroulant **Base de données** en regard des boutons **Connecter**/**Déconnecter**, sélectionnez **TradeDev**. Cette opération est semblable à l'instruction Transact\-SQL`USE` et garantit que le script dans l'éditeur de code sera exécuté par rapport à la base de données **TradeDev**.  
  
5.  Cliquez sur le bouton **Exécuter la requête`INSERT` pour exécuter les instructions** . Ainsi, toutes les lignes de la table `Suppliers` de la base de données `Trade` sont insérées dans la table `Suppliers` de la base de données `TradeDev`.  
  
6.  Répétez les étapes ci-dessus pour toutes les tables de la base de donnés `Trade`, de façon à les répliquer dans la base de données `TradeDev`.  
  
7.  Utilisez l'Éditeur de code pour vérifier que toutes les tables de la nouvelle base de données `TradeDev` ont été remplies.  
  
## <a name="see-also"></a>Voir aussi  
[Procédure : utiliser le schéma pour comparer différentes définitions de base de données](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)  
  
