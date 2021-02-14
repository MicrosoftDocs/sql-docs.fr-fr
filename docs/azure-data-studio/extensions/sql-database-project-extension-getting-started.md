---
title: Bien démarrer avec l’extension des projets SQL Database
description: Bien démarrer avec l’extension des projets SQL Database pour Azure Data Studio
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: ''
ms.date: 07/30/2020
ms.openlocfilehash: 397bdebe2bdea292009e0216c6b3004ac9475b18
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040313"
---
# <a name="getting-started-with-the-sql-database-projects-extension-preview"></a>Bien démarrer avec l’extension des projets SQL Database (préversion)

Cet article décrit trois façons de commencer à utiliser l’extension des projets SQL Database :

1. Créez un projet de base de données en accédant à l’icône **Projets** sous Explorer ou en recherchant **Nouveau projet de base de données** dans la palette de commandes.
2. Les projets de base de données existants peuvent être ouverts via **Ouvrir un projet de base de données** dans la palette de commandes.
3. Démarrez à partir d’une base de données existante à l’aide de **Importer un nouveau projet de base de données** à partir de la palette de commandes.

    ![Nouvelle icône](media/sql-database-projects-extension/projects-viewlet.png)

## <a name="create-an-empty-database-project"></a>Créer un projet de base de données vide

Dans le viewlet **Projets** sous **Explorer**, sélectionnez le bouton **Nouveau projet** et entrez un nom de projet dans l’entrée de texte qui s’affiche.  Dans la boîte de dialogue « Sélectionner un dossier » qui s’affiche, sélectionnez un répertoire pour le dossier du projet, le fichier. sqlproj et tout autre contenu dans lequel se trouve.
Le projet vide est ouvert et visible dans l’icône **Projets** pour modification.

## <a name="open-an-existing-project"></a>Ouvrir un projet existant

Dans le viewlet **Projets**, sélectionnez le bouton **Ouvrir le projet** et ouvrez un fichier *.sqlproj* existant à partir du sélecteur de fichiers qui s’affiche. Les projets existants peuvent provenir d’Azure Data Studio ou [Visual Studio SQL Server Data Tools](../../ssdt/sql-server-data-tools.md).

Le projet vide est ouvert et son contenu visible dans l’icône **Projets** pour modification.

## <a name="create-a-database-project-from-an-existing-database"></a>Créer un projet de base de données à partir d’une base de données existante

Dans le viewlet **Projet**, sélectionnez le bouton **Importer le projet à partir de la base de données** et connectez-vous à un SQL Server.  Une fois la connexion établie, sélectionnez une base de données dans la liste des bases de données disponibles et définissez le nom du projet.

Enfin, sélectionnez une structure cible pour l’extraction.  Le nouveau projet est ouvert et contient des scripts SQL pour le contenu de la base de données sélectionnée.

## <a name="build-and-publish"></a>Générer et publier

Le déploiement du projet de base de données est effectué dans l’extension des projets SQL Database pour Azure Data Studio en générant le projet dans un [fichier d’application de la couche Données](../../relational-databases/data-tier-applications/data-tier-applications.md) (DACPAC) et en le publiant sur une plateforme prise en charge. Pour plus d’informations sur ce processus, consultez [Générer et publier un projet](sql-database-project-extension-build.md).

## <a name="schema-compare"></a>Comparaison de schémas

L’extension de projets SQL Database interagit avec [l’extension de comparaison de schémas](schema-compare-extension.md), si elle est installée, pour comparer le contenu d’un projet à une base de données dacpac ou existante.  La comparaison de schémas résultante peut être utilisée pour visualiser et appliquer les différences entre la source et la cible.

## <a name="next-steps"></a>Étapes suivantes

- [Générer et publier un projet avec l’extension des projets SQL Database pour Azure Data Studio](sql-database-project-extension-build.md)