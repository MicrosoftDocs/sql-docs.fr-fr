---
title: Importer dans un projet de base de données
description: Découvrez comment importer des objets dans un projet de base de données à partir d’une base de données active, d’une application de la couche Données et d’un script. En savoir plus sur l’importation d’objets chiffrés.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- SQL.DATA.TOOLS.SQLPROJECTIMPORTSNAPSHOTSUMMARYDIALOG.DIALOG
- SQL.DATA.TOOLS.SQLPROJECTIMPORTDATABASESUMMARYDIALOG.DIALOG
- SQL.DATA.TOOLS.IMPORTSCRIPTWIZARD.SUMMARY
ms.assetid: d0a0a394-6cb6-416a-a25f-9babf8ba294a
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0820f680382d7a126d2837c653ab7870defeb0ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017929"
---
# <a name="import-into-a-database-project"></a>Importer dans un projet de base de données

Vous pouvez utiliser la fonction Importer pour remplir un projet avec de nouveaux objets provenant d'une base de données active ou d'un fichier .dacpac, ou pour mettre à jour des objets existants dans votre projet avec une nouvelle définition d'un script. Il convient de noter plusieurs différences de comportement entre ces trois choix, comme décrit ci-dessous.  
  
**Menu Importer**  
  
![Menu Importer de SSDT](../ssdt/media/ssdt-import.gif "Menu Importer de SSDT")  
  
**Sections de cette rubrique**  
  
[Source d’importation : base de données ou application de la couche Données (*.dacpac)](#bkmk_import_source_db)  
  
[Source d’importation : script (*.sql)](#bkmk_import_source_script)  
  
[Importer des objets chiffrés](#bkmk_import_encrypted)  
  
## <a name="import-source-database-or-data-tier-application-dacpac"></a><a name="bkmk_import_source_db"></a>Source d’importation : base de données ou application de la couche Données (*.dacpac)  
La possibilité d'importer un schéma depuis une base de données ou un fichier .dacpac n'est disponible que si aucun objet de schéma n'est encore défini dans le projet. Cela n'inclut pas les fichiers RefactorLogs ni les scripts de prédéploiement et de post-déploiement.  
  
Lors de l’importation, les définitions d’objet sont écrites via un script dans les fichiers de projet à l’aide des valeurs organisationnelles par défaut de SSDT pour les nouveaux objets : nouveaux fichiers pour les objets de niveau supérieur, enfants hiérarchiques définis dans le même fichier que le parent, contraintes de table/colonne définies en ligne lorsque cela est possible. Pour disposer d'un meilleur contrôle et d'une visibilité ciblée pour chaque objet, utilisez Comparaison de schémas au lieu d'Importer.  
  
Si la source d'importation contient des scripts de prédéploiement et de post-déploiement, des fichiers RefactorLogs ou des définitions de variable SQLCMD, ils seront importés dans le projet. Si le projet contient déjà l’un de ces artefacts, les fichiers importés seront ajoutés à un dossier **Ignoré lors de l’importation** dans le projet.  
  
Dossier **Ignoré lors de l’importation**  
  
![Dossier des éléments ignorés lors de l'importation dans SSDT](../ssdt/media/ssdt-ignoredonimport.gif "Dossier des éléments ignorés lors de l'importation dans SSDT")  
  
## <a name="import-source-script-sql"></a><a name="bkmk_import_source_script"></a>Source d’importation : script (*.sql)  
Tous les objets de la source d’importation qui n’existent *pas* encore dans le projet seront ajoutés, et tous les ceux qui existent *déjà* remplaceront la définition des objets dans le projet.  
  
> [!NOTE]  
> Il existe deux bogues connues dans cette méthode. Elles seront corrigées dans une prochaine version :  
>   
> -   Si les contraintes de table/colonne sont définies à l’extérieur de l’instruction CREATE TABLE dans la définition de la table du projet, l’importation remplace la définition de la table de sorte que cette contrainte soit inline. Toutefois, elle laisse la contrainte hors ligne, ce qui se traduit par des contraintes en double dans le projet.  
> -   Toutes les clés principales ou les clés de chiffrement de base de données de votre script source qui existent déjà dans le projet sont dupliquées lors de l'importation. Supprimez ces doublons afin de pouvoir générer le projet.  
  
Le processus d'importation à partir d'un script ne comprend pas les scripts de prédéploiement et de post-déploiement, les variables SQLCMD ni les fichiers RefactorLog. Ces éléments, ainsi que toutes les autres constructions non prises en charge qui sont détectées lors de l’importation, seront placés dans un fichier **ScriptsIgnoredOnImport.sql** au sein d’un dossier **Scripts** dans votre projet.  
  
 
## <a name="import-encrypted-objects"></a><a name="bkmk_import_encrypted"></a>Importer des objets chiffrés  
Lors de l'importation d'objets chiffrés dans un projet de base de données, le corps entier de la définition d'objet ne peut pas toujours être récupéré depuis le serveur. De ce fait, le comportement d'importation peut varier lorsque cette classe d'objets doit être gérée.  
  
Lorsqu'il n'est pas possible de récupérer la définition du corps entier, l'en-tête/le pied de page de l'objet, ainsi qu'un corps factice, sont écrits dans un script. Attendez-vous à rencontrer ce comportement lorsque vous effectuez une importation ou utilisez une comparaison de schémas dans laquelle la source est une base de données active ou un fichier .dacpac extrait d'une base de données.  
  
**Script avec un corps factice**  
  
![Script avec un corps factice](../ssdt/media/ssdt-procwithencryption.gif "Script avec un corps factice")  
  
Dans le cas où la définition complète de l'objet est disponible et peut être récupérée, les processus d'importation et de comparaison de schémas permettent de la rétablir dans le projet dans son intégralité. Cela se produit lors de la mise à jour du projet à partir d'un script, d'un fichier .dacpac créé à partir d'un projet de base de données ou d'un autre projet de base de données.  
  
