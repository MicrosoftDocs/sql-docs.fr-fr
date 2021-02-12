---
title: SandDance pour Azure Data Studio
description: Découvrez comment utiliser une extension Azure Data Studio pour créer rapidement des visualisations de vos données, autrement dit, des visualisations qui fournissent des insights.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: seodec18
ms.date: 07/03/2019
ms.openlocfilehash: d031ddea065f035bf4d1f4ffc70bd65d90f1bd0f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040334"
---
# <a name="sanddance-for-azure-data-studio-preview"></a>SandDance pour Azure Data Studio (préversion)

Azure Data Studio offre désormais un moyen de créer des visualisations rapides pour vos données. Cette extension est utile quand vous essayez d’examiner les données et de comprendre ce qui se passe. Nous utilisons une technologie appelée SandDance de Microsoft Research, qui peut générer des visualisations sur place des données.

![sanddance-animation](https://user-images.githubusercontent.com/11507384/54236654-52d42800-44d1-11e9-859e-6c5d297a46d2.gif)

Grâce à des vues faciles à comprendre, SandDance vous aide à trouver des informations sur vos données, qui à leur tour vous aideront à faire parler vos données, à créer des cas basés sur des preuves, à tester des idées, à approfondir les explications de surface, à prendre des décisions d’achat ou à lier les données dans un contexte de monde réel plus vaste.

SandDance utilise des visualisations d’unité, qui appliquent un mappage un-à-un entre les lignes de votre base de données et les marques sur l’écran.
Les transitions fluides animées entre les vues vous aident à maintenir le contexte lorsque vous interagissez avec vos données.

## <a name="usage"></a>Usage

### <a name="view-csv-or-tsv-files"></a>Afficher les fichiers .csv ou .tsv
Cela comprend les fichiers locaux ou les fichiers sur HDFS dans votre cluster Big Data SQL Server 2019.
 
À partir du menu Fichier, utilisez Ouvrir dossier ou [Ctrl+K Ctrl+O] pour ouvrir le répertoire contenant le fichier .CSV.  Ensuite, à partir du volet de l’explorateur, cliquez avec le bouton droit sur le fichier .csv ou .tsv, puis choisissez *Afficher dans SandDance*.

Cliquez avec le bouton droit sur un fichier. csv ou .tsv dans HDFS si vous êtes connecté à un cluster Big Data SQL Server 2019, puis choisissez *Afficher dans SandDance*.

### <a name="view-sql-query-results"></a>Afficher les résultats d’une requête SQL

À partir d’une fenêtre de requête SQL, exécutez une requête pour obtenir une grille de résultats. Cliquez sur l’icône Visualiseur sur le côté droit du volet Résultats.

## <a name="known-issues"></a>Problèmes connus

Actuellement, nous ne limitons pas le nombre de lignes visualisées. Toutefois, la consommation de mémoire augmente proportionnellement au nombre de lignes ; nous vous recommandons donc de limiter le jeu de données ou la vue à environ 100 000 lignes.

Consulter les [problèmes connus](https://microsoft.github.io/SandDance/#known-issues)

## <a name="release-notes"></a>Notes de publication

### <a name="100"></a>1.0.0

Version initiale d’azdata-sanddance

## <a name="next-steps"></a>Étapes suivantes
Pour en savoir plus, [visitez le référentiel GitHub.](https://github.com/Microsoft/SandDance)
