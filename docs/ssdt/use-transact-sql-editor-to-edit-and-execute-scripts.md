---
title: Utiliser l'Éditeur Transact-SQL pour modifier et exécuter des scripts
description: Familiarisez-vous avec l’Éditeur Transact-SQL. Découvrez comment ouvrir l’éditeur, consulter les informations affichées sur ses volets et afficher les ressources sur ses fonctionnalités.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- SQL.DATA.TOOLS.SQLEDITOR
- sql.data.tools.sqleditor
ms.assetid: fa78e2cf-3c64-49f5-93cc-a3d50b1e7d05
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6edd17773c3624e181659962cd3c7a4e2b9f6d4b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066834"
---
# <a name="use-transact-sql-editor-to-edit-and-execute-scripts"></a>Utiliser l'Éditeur Transact-SQL pour modifier et exécuter des scripts

L'Éditeur Transact\-SQL vous offre une expérience d'édition et de débogage enrichie lorsque vous utilisez des scripts. Il est appelé lorsque vous utilisez le menu contextuel **Afficher le code** pour ouvrir une entité de base de données dans une base de données connectée ou un projet. Il s'ouvre aussi automatiquement lorsque vous utilisez le menu contextuel **Nouvelle requête** de l'Explorateur d'objets SQL Server, ou lorsque vous ajoutez un nouvel objet de script à un projet de base de données.  
  
Si vous n'êtes pas connecté à une base de données, mais que vous souhaitez exécuter une requête par rapport à une base de données, vous pouvez aussi utiliser la boîte de dialogue **Nouvelle connexion à la requête** de l'option de menu Éditeur **SQL** -> **Transact\-SQL** pour vous connecter à une base de données et démarrer l'Éditeur Transact\-SQL.  
  
L'Éditeur Transact\-SQL comprend un volet **T-SQL** principal où vous pouvez écrire et modifier des scripts Transact\-SQL. L'éditeur prend en charge IntelliSense, ainsi que le codage en couleurs de la syntaxe facilitant la lisibilité des instructions complexes. Il prend aussi en charge la fonction de recherche et remplacement, les commentaires en bloc, les polices et les couleurs personnalisées et la numérotation des lignes. Vous pouvez aussi modifier la base de données par rapport à laquelle le script de l'éditeur sera exécuté. Pour plus d’informations, consultez [Procédure : cloner une base de données existante](../ssdt/how-to-clone-an-existing-database.md). Le volet de **résultats** affiche les résultats de la requête dans une grille ou dans le texte. Vous pouvez également rediriger les résultats de la requête vers un fichier. Le volet de **message** affiche des erreurs, des avertissements et des messages d'information retournés lors de l'exécution d'un script. Lorsque les statistiques du client sont activées, le volet **Statistiques** affiche des informations regroupées par catégorie sur l'exécution de la requête. Le volet **Plan d'exécution** affiche les méthodes d'extraction de données choisies par SQL Server, ainsi que le coût d'exécution de requêtes et d'instructions spécifiques.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|---------|---------------|  
|[Procédure : structurer et ajouter des extraits de code à un script Transact-SQL](../ssdt/how-to-outline-and-add-snippets-to-transact-sql-script.md)|Utilisez le Sélecteur d'extraits pour insérer du code Transact\-SQL prêt à l'emploi dans votre requête.|  
|[Procédure : naviguer entre des scripts](../ssdt/how-to-navigate-between-scripts.md)|Utilisez Atteindre la définition et Rechercher toutes les références pour naviguer entre les scripts.|  
|[Procédure : utiliser Renommer et Refactorisation pour apporter des modifications à vos objets de base de données](../ssdt/how-to-use-rename-and-refactoring-to-make-changes-to-your-database-objects.md)|Renommez un objet dans tous les scripts et affichez un aperçu des modifications.|  
|[Procédure : exécuter une requête partielle](../ssdt/how-to-execute-a-partial-query.md)|Mettez en surbrillance un segment spécifique du script et exécutez-le en tant que requête unique.|  
|[Procédure : déboguer des procédures stockées](../ssdt/how-to-debug-stored-procedures.md)|Créez et déboguez une procédure stockée Transact\-SQL pas à pas.|  
|[Analyser les performances de script](../ssdt/analyze-script-performance.md)|Utilisez les plans d'exécution, les statistiques du client et l'analyse du code pour déterminer si vous pouvez améliorer les performances de vos requêtes, procédures stockées ou scripts.|  
  
## <a name="see-also"></a>Voir aussi

[Procédure : créer de nouveaux objets de base de données à l’aide de requêtes](../ssdt/how-to-create-new-database-objects-using-queries.md)