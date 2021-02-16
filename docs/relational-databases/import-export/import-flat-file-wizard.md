---
title: Importer un fichier plat dans SQL | Microsoft Docs
description: L’Assistant Importation de fichier plat offre un moyen simple de copier les données d’un fichier .csv ou .txt dans une nouvelle table de base de données. Cet article vous montre comment et quand utiliser l’Assistant.
ms.custom: ''
ms.date: 09/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: data-movement
ms.topic: conceptual
f1_keywords:
- sql13.swb.importflatfile.f1
author: yualan
ms.author: alayu
ms.reviewer: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 444489bc74106ca43d645e26564543b4452ac8ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351356"
---
# <a name="import-flat-file-to-sql-wizard"></a>Assistant Importation d’un fichier plat dans SQL
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
> Pour tout contenu associé à l’Assistant Importation et exportation, consultez [Assistant Importation et exportation SQL Server](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

L’Assistant Importation de fichier plat permet de copier les données d’un fichier plat (.csv ou .txt) dans une nouvelle table d’une base de données.  L’Assistant Importation de fichier plat prend en charge les formats de fichiers de largeur fixe et séparés par des virgules. Cette présentation décrit les raisons d’utiliser cet Assistant et où le trouver, puis fournit un exemple simple à suivre.

## <a name="why-would-i-use-this-wizard"></a>Pourquoi utiliser cet Assistant ?
Cet Assistant a été créé pour améliorer l’expérience d’importation actuelle en tirant parti d’un framework intelligent appelé [PROSE](https://microsoft.github.io/prose/) (Program Synthesis using Examples). Pour un utilisateur sans connaissance technique du domaine, l’importation de données peut souvent être une tâche complexe, sujette aux erreurs et fastidieuse. Avec cet Assistant, le processus d’importation se résume simplement à la sélection d’un fichier d’entrée et d’un nom unique de table, le framework PROSE s’occupe du reste.

PROSE analyse les modèles de données dans votre fichier d’entrée pour déduire les noms de colonnes, les types, les séparateurs, etc. Ce framework apprend la structure du fichier et gère tout le travail ingrat à la place des utilisateurs.

Pour mieux comprendre l’amélioration apportée à l’expérience utilisateur de l’Assistant Importation de fichier plat, regardez cette vidéo :

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Introducing-the-new-Import-Flat-File-Wizard-in-SSMS-173/player?WT.mc_id=dataexposed-c9-niner]

## <a name="prerequisites"></a>Prérequis
Cette fonctionnalité est disponible sur SQL Server Management Studio (SSMS) 17.3 ou version ultérieure. Veillez à utiliser la version la plus récente. Vous trouverez la dernière version [ici.](../../ssms/download-sql-server-management-studio-ssms.md)
 
## <a name="getting-started"></a><a id="started"></a>Commencer
Pour accéder à l’Assistant Importation d’un fichier plat, procédez comme suit :

1. Ouvrez **SQL Server Management Studio**.
2. Connectez-vous à une instance du Moteur de base de données SQL Server ou à un hôte local.
3. Développez **Bases de données**, cliquez avec le bouton droit sur une base de données (test dans l’exemple ci-dessous), pointez sur **Tâches** et cliquez sur **Importer un fichier plat** au-dessus de « Importer des données ».

![Menu de l’Assistant](media/import-flat-file-wizard/import-flat-file-menu.png)

Pour en savoir plus sur les différentes fonctions de l’Assistant, suivez le tutoriel ci-dessous :

## <a name="tutorial"></a>Didacticiel
Pour les besoins de ce didacticiel, n’hésitez pas à utiliser votre propre fichier plat. Sinon, ce didacticiel utilise le fichier CSV Excel suivant que vous êtes libre de copier. Si vous utilisez ce fichier CSV, nommez-le **example.csv** et enregistrez-le au format csv à un emplacement pratique comme votre Bureau.

![Assistant - Excel](media/import-flat-file-wizard/import-flat-file-example.png)

Présentation :
1. [Assistant Accès](#step-1-access-wizard-and-intro-page)
2. [Spécifier le fichier d’entrée](#step-2-specify-input-file)
3. [Aperçu des données](#step-3-preview-data)
4. [Modifier les colonnes](#step-4-modify-columns)
5. [Résumé](#step-5-summary)
6. [Résultats](#step-6-results)

### <a name="step-1-access-wizard-and-intro-page"></a>Étape 1 : Accéder à l’Assistant et à la page d’introduction
Accédez à l’Assistant, comme décrit [ici](#started).

La première page de l’Assistant est la page d’accueil. Si vous ne souhaitez plus voir cette page, n’hésitez pas à cliquer sur **Ne plus afficher cette page de démarrage.**

![Assistant - Introduction](media/import-flat-file-wizard/import-flat-file-intro.png)

### <a name="step-2-specify-input-file"></a>Étape 2 : Spécifier le fichier d’entrée
Cliquez sur Parcourir pour sélectionner votre fichier d’entrée. Par défaut, l’Assistant recherche les fichiers .csv et .txt. PROSE détecte s’il s’agit d’un format de fichier de largeur fixe ou séparé par des virgules, quelle que soit l’extension du fichier.

Le nouveau nom de table doit être unique. S’il ne l’est pas, l’Assistant ne vous autorise pas à aller plus loin.

![Assistant - Spécification](media/import-flat-file-wizard/import-flat-file-specify.png)

### <a name="step-3-preview-data"></a>Étape 3 : Aperçu des données
L’Assistant génère un aperçu où vous pouvez voir les 50 premières lignes. Si vous rencontrez des problèmes, cliquez sur Annuler, sinon passez à la page suivante.

![Assistant - Aperçu](media/import-flat-file-wizard/import-flat-file-preview.png)

### <a name="step-4-modify-columns"></a>Étape 4 : Modifier les colonnes
L’Assistant identifie ce qu’il pense être les bons noms de colonnes, de types de données, etc. Voici où vous pouvez modifier les champs s’ils sont incorrects (par exemple, le type de données doit être une valeur float et non une valeur int).

Les colonnes où sont détectées des valeurs vides ont l’option « Autoriser les valeurs NULL » cochée. Cependant, si vous attendiez des valeurs NULL dans une colonne et que l’option « Autoriser les valeurs NULL » n’est pas cochée, c’est ici que pouvez mettre à jour la définition de la table de façon à d’autoriser les valeurs NULL dans une ou toutes les colonnes.

Quand vous êtes prêt, poursuivez.

![Assistant - Modification](media/import-flat-file-wizard/import-flat-file-modify.png)

### <a name="step-5-summary"></a>Étape 5 : Résumé
Il s’agit simplement d’une page qui résume votre configuration actuelle. Si vous voyez des problèmes, vous pouvez revenir à des sections précédentes. Sinon, cliquez sur Terminer pour tenter le processus d’importation.

![Assistant - Résumé](media/import-flat-file-wizard/import-flat-file-summary.png)

### <a name="step-6-results"></a>Étape 6 : Résultats
Cette page indique si l’importation a réussi. Si une coche verte s’affiche, elle a réussi. Dans le cas contraire, vous devrez peut-être rechercher des erreurs dans votre configuration ou votre fichier d’entrée.

![Assistant - Résultats](media/import-flat-file-wizard/import-flat-file-results.png)

## <a name="troubleshooting"></a>Résolution des problèmes
L’Assistant Importation de fichier plat détecte les types de données en fonction des 200 premières lignes.  Si d’autres données du fichier plat ne sont pas conformes aux types de données détectés automatiquement, une erreur se produit lors de l’importation. Le message d'erreur qui s'affiche est semblable au suivant :
```
Error inserting data into table. (Microsoft.SqlServer.Prose.Import)
The given value of type String from the data source cannot be converted to type nvarchar of the specified target column. (System.Data)
String or binary data would be truncated. (System.Data)
```
Solutions pour corriger cette erreur :
- Le développement de la ou des tailles de types de données à l’étape [Modifier les colonnes](#step-4-modify-columns), par exemple la longueur d’une colonne nvarchar, peut compenser les variations de données par rapport au reste du fichier plat.
- La création de rapports d’erreurs à l’étape [Modifier les colonnes](#step-4-modify-columns), en particulier par un nombre plus petit, indique la ou les lignes du fichier plat qui contiennent des données qui ne correspondent pas aux types de données sélectionnés. Par exemple, dans un fichier plat où la deuxième ligne contient une erreur, l’exécution de l’importation avec une création de rapports d’erreurs avec une plage de 1 génère un message d’erreur spécifique.  L’examen du fichier directement à l’emplacement peut fournir des modifications plus ciblées sur les types de données en fonction des données contenues dans les lignes identifiées.

![Résultats des rapports d’erreurs](media/import-flat-file-wizard/import-flat-file-error.png)

```
Error inserting data into table occured while inserting rows 1 - 2. (Microsoft.SqlServer.Prose.Import)
The given value of type String from the data source cannot be converted to type float of the specified target column. (System.Data)
Failed to convert parameter value from a String to a Double. (System.Data)
```


## <a name="learn-more"></a>En savoir plus

Découvrez-en plus sur l’Assistant.
 
- **En savoir plus sur l’importation d’autres sources.** Si vous souhaitez importer plusieurs fichiers plats, consultez [Assistant Importation et exportation SQL Server](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).
- **En savoir plus sur la connexion à des sources de fichiers plats.** Si vous recherchez plus d’informations sur la connexion à des sources de fichiers plats, consultez [Se connecter à une source de données de fichiers plats](../../integration-services/import-export-data/connect-to-a-flat-file-data-source-sql-server-import-and-export-wizard.md).
- **En savoir plus sur PROSE.** Si vous recherchez une vue d’ensemble du framework intelligent utilisé par cet Assistant, consultez [PROSE SDK](https://microsoft.github.io/prose/).