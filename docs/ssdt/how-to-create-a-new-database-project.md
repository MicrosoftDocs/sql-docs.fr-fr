---
title: Créer un projet de base de données
description: Découvrez comment créer un nouveau projet de base de données. Consultez comment importer le schéma à partir d’une base de données existante dans le nouveau projet.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.dbprojectwizard.importschema
- sql.data.tools.SqlProjectImportDatabaseDialog.dialog
- sql.data.tools.importscriptwizard.welcome
- sql.data.tools.importscriptwizard.summary
- sql.data.tools.SqlProjectImportDatabaseSummaryDialog.dialog
- sql.data.tools.importscriptwizard.fileselection
ms.assetid: 0b7883fa-b6e1-4ccf-b1d8-f522fd03a59d
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6039138eda38f67aae2a1f4770ebc93380087607
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018369"
---
# <a name="how-to-create-a-new-database-project"></a>Procédure : Créer un projet de base de données

Vous pouvez créer un nouveau projet de base de données et importer un schéma de base de données à partir d'une base de données existante, d'un fichier de script SQL ou d'une application de la couche Données (.dacpac). Vous pouvez ensuite appeler les mêmes outils du concepteur visuel (Éditeur Transact\-SQL, Concepteur de tables) disponibles pour le développement de base de données connectée afin d'apporter des modifications au projet de base de données en mode hors connexion, et republier les modifications dans la base de données de production. Les modifications peuvent aussi être enregistrées en tant que script pour être publiées ultérieurement. Le volet **Propriétés du projet** permet de modifier la plateforme cible vers différentes versions de SQL Server (y compris SQL Azure).  
  
Les deux procédures suivantes produisent essentiellement le même résultat en créant un nouveau projet de base de données et en important un schéma à partir d'une base de données existante. Chaque objet de base de données sera représenté en tant que fichier script SQL (.sql) dans l'**Explorateur de solutions**. Pour plus d’informations sur l’importation d’un schéma de base de données à partir d’une capture instantanée, consultez [Procédure : créer une capture instantanée d’un projet](../ssdt/how-to-create-a-snapshot-of-a-project.md).  
  
> [!WARNING]  
> Les procédures suivantes utilisent les entités créées dans les procédures précédentes de la section [Développement d’une base de données connectée](../ssdt/connected-database-development.md).  
  
### <a name="to-create-a-new-database-project-off-a-connected-database"></a>Pour créer un nouveau projet de base de données à partir d'une base de données connectée  
  
1.  Cliquez avec le bouton droit sur le nœud **TradeDev** dans l'**Explorateur d'objets SQL Server**, puis sélectionnez **Créer un nouveau projet**.  
  
2.  Dans la boîte de dialogue **Importer une base de données**, notez que les paramètres **Connexion de base de données source** ont été prédéfinis par la base de données sélectionnée dans l'**Explorateur d'objets SQL Server**. Dans le paramètre **Projet cible**, remplacez le nom du projet par **TradeDev**.  
  
3.  Dans la section **Importer les paramètres**, notez les options d'importation d'objets et paramètres spécifiques, et de création de dossiers pour chaque schéma et/ou type d'objet. Pour obtenir une hiérarchie organisée de tous les objets de base de données, acceptez tous les paramètres par défaut, puis cliquez sur **Démarrer**.  
  
4.  La boîte de dialogue **Importer une base de données** affiche une barre de progression et une liste d'objets importés par SSDT. Lorsque l'opération d'importation est terminée, cliquez sur **Terminer** pour quitter le dernier écran.  
  
5.  Examinez la hiérarchie dans l'**Explorateur de solutions**. Développez le dossier **dbo** et vous trouverez des dossiers **Fonctions**, **Tables** et **Affichages** distincts. Remarquez que les tables et fonctions sont regroupées sous leur dossier de schéma.  
  
6.  Sous **Tables**, double-cliquez sur **Products.sql**. Le **Concepteur de tables** s'ouvre, affichant l'interprétation visuelle de la table dans la Grille Colonnes, et la définition du script de la table dans le volet de script. Vous voyez la même chose dans la section [Développement de base de données connectée](../ssdt/connected-database-development.md).  
  
7.  Désactivez la case à cocher **Autoriser les valeurs NULL** correspondant à la colonne **CustomerId**. Appuyez sur CTRL+S pour enregistrer le fichier.  
  
8.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **TradeDev**, puis sélectionnez **Générer** pour générer le projet de base de données.  
  
    Les résultats de l'opération de génération peuvent être consultés dans la fenêtre Sortie.  
  
### <a name="to-create-a-new-project-and-import-existing-database-schema"></a>Pour créer un nouveau projet de base de données et importer un schéma de base de données existant  
  
1.  Cliquez sur **Fichier**, sur **Nouveau**, puis sur **Projet**. Dans la boîte de dialogue **Nouveau projet**, sélectionnez **SQL Server** dans le volet gauche. Notez qu'il existe un seul type de projet de base de données : le **Projet de base de données SQL Server**. Il n'existe pas de projet spécifique à la plateforme comme dans les versions précédentes de Visual Studio. Vous serez en mesure de définir votre plateforme cible dans la boîte de dialogue **Paramètres du projet** après la création du projet. Ces tâches seront couvertes dans la rubrique [Procédure : modifier la plateforme cible et publier un projet de base de données](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md).  
  
2.  Remplacez le nom du projet par **TradeDev** et cliquez sur **OK** pour créer le nouveau projet.  
  
3.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le nouveau projet **TradeDev**, sélectionnez **Importer**, puis **Base de données**.  
  
    La boîte de dialogue **Importer une base de données** s'ouvre. Dans la section **Connexion de base de données source**, cliquez sur **Choisir une base de données**, puis sélectionnez **TradeDev**. Si **TradeDev** ne figure pas dans la liste déroulante, utilisez le bouton **Nouvelle connexion** pour modifier les propriétés de connexion.  
  
4.  Dans la section **Importer les paramètres**, notez les options d'importation d'objets et paramètres spécifiques, et de création de dossiers pour chaque schéma et/ou type d'objet. Pour obtenir une hiérarchie organisée de tous les objets de base de données, acceptez tous les paramètres par défaut, puis cliquez sur **Démarrer**.  
  
5.  La boîte de dialogue **Importer une base de données** affiche une barre de progression et une liste d'objets importés par SSDT. Lorsque l'opération d'importation est terminée, cliquez sur **Terminer** pour quitter le dernier écran.  
  
6.  Examinez la hiérarchie dans l'**Explorateur de solutions**. Développez le dossier **dbo** et vous trouverez des dossiers **Fonctions**, **Tables** et **Affichages** distincts. Remarquez que les tables et fonctions sont regroupées sous leur dossier de schéma.  
  
7.  Sous **Tables**, double-cliquez sur **Products.sql**. Le **Concepteur de tables** s'ouvre, affichant l'interprétation visuelle de la table dans la Grille Colonnes, et la définition du script de la table dans le volet de script. Vous voyez la même chose dans la section [Développement de base de données connectée](../ssdt/connected-database-development.md).  
  
8.  Désactivez la case à cocher **Autoriser les valeurs NULL** correspondant à la colonne **CustomerId**. Appuyez sur CTRL+S pour enregistrer le fichier.  
  
9. Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **TradeDev**, puis sélectionnez **Générer** pour générer le projet de base de données.  
  
## <a name="see-also"></a>Voir aussi  
[Procédure : modifier la plateforme cible et publier un projet de base de données](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md)  
  
