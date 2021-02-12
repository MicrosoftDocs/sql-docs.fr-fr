---
title: Résolution des problèmes liés à Azure Data Studio
description: Découvrez comment obtenir des journaux et résoudre les problèmes d’Azure Data Studio, ce qui est utile pour la création de rapports de bogues.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: hanqin, maghan
ms.custom: seodec18
ms.date: 11/24/2020
ms.openlocfilehash: 81fb4957672fa2008582b0ab2397322fb2b30043
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039969"
---
# <a name="azure-data-studio-troubleshooting"></a>Résolution des problèmes liés à Azure Data Studio
Azure Data Studio effectue le suivi des problèmes et des requêtes de fonctionnalités à l’aide d’un [système de suivi des problèmes du référentiel GitHub](https://github.com/Microsoft/azuredatastudio/issues) pour le référentiel `azuredatastudio`. 

## <a name="if-youve-experienced-any-issue"></a>Si vous rencontrez des problèmes

Signalez les problèmes au [Système de suivi des problèmes GitHub](https://github.com/Microsoft/azuredatastudio/issues) et indiquez-nous les détails qui nous aideront à reproduire l’erreur. Incluez toutes les [informations de journalisation](#how-to-set-the-logging-level) à partir du fichier journal.

## <a name="writing-good-bug-reports-and-feature-requests"></a>Écriture de rapports de bogues et de requêtes de fonctionnalités corrects

Enregistrez dans le fichier un seul sujet par problème et requête de fonctionnalité.

* N’énumérez pas plusieurs bogues ou requêtes de fonctionnalités dans le même problème.
* N’ajoutez pas votre problème en tant que commentaire à un problème existant, sauf s’il s’agit d’une entrée identique. De nombreux problèmes semblent similaires, mais ont des causes différentes.

Plus vous pouvez fournir d’informations, plus il sera facile de reproduire le problème et de trouver un correctif. 

Incluez les informations suivantes dans chaque problème :

* Version d’Azure Data Studio
* Étapes reproductibles (1... 2... 3...) et ce que vous avez attendu par rapport aux faits réels constatés. 
* Des images, des animations ou un lien vers une vidéo. Les images et les animations illustrent les étapes de reproduction, mais ne les remplacent pas.
* Un extrait de code qui illustre le problème ou un lien vers un référentiel de code que nous pouvons facilement extraire sur notre ordinateur pour recréer le problème. 

> [!NOTE]
>  Étant donné que nous devons copier et coller l’extrait de code, y compris un extrait de code comme un fichier média (.gif), cela ne suffit pas. 

* Erreurs dans la console des outils de développement (Aide | Basculer Outils de développement)

N’oubliez pas d’effectuer les opérations suivantes :

* Vérifiez le référentiel des problèmes à la recherche d’un doublon. 
* Simplifiez votre code autour du problème afin de pouvoir mieux isoler le problème. 

Ne vous inquiétez pas si nous ne pouvons pas reproduire le problème et demandons plus d’informations !

## <a name="how-to-set-the-logging-level"></a>Comment définir le niveau de journalisation

### <a name="azure-data-studio"></a>Azure Data Studio
Exécutez la commande `Developer: Set Log Level...` pour sélectionner le niveau de journalisation pour la session active. Cette valeur n’est PAS conservée sur plusieurs sessions. Par conséquent, si vous redémarrez Azure Data Studio le niveau de `Info` par défaut est rétabli. 

Si vous souhaitez activer la journalisation du débogage pour le démarrage, définissez le niveau de journalisation sur `Debug` et exécutez la commande `Developer: Reload Window`

### <a name="mssql-built-in-extension"></a>MSSQL (extension intégrée)

Si le paramètre utilisateur `Mssql: Log Debug Info` a la valeur true, les informations du journal de débogage sont envoyées au canal de sortie `MSSQL`.

Le paramètre utilisateur `Mssql: Tracing Level` est utilisé pour contrôler la verbosité de la journalisation.

## <a name="debug-log-location"></a>Emplacement du journal de débogage
À partir d’Azure Data Studio, exécutez la commande `Developer: Open Logs Folder` pour ouvrir le chemin d’accès aux journaux. Il existe de nombreux types de fichiers journaux qui y écrivent, les plus couramment utilisés sont les suivants :

1. `renderer#.log` (par exemple, renderer1.log) : ce fichier est le fichier journal du processus principal.
1. `telemetry.log` - lorsque le niveau de journal est défini sur `Trace` ce fichier contiendra les événements de télémétrie envoyés par Azure Data Studio
1. `exthost#/exthost.log` - fichier journal pour le processus hôte d’extension (il s’agit uniquement du processus lui-même et non des extensions qui s’y trouvent)
1. `exthost#/Microsoft.mssql` - journaux pour l’extension mssql, qui contient une grande partie de la logique de base pour les fonctionnalités MSSQL
   * sqltools.log est le journal du Service des outils SQL
1. `exthost#/output_logging_#######` - ces dossiers contiennent les messages affichés dans le panneau `Output` d’Azure Data Studio. Chaque fichier est nommé `#-<Channel Name>`, par exemple le canal de sortie `Notebooks` peut être généré dans un fichier nommé `3-Notebooks.log`.

Si vous êtes invité à fournir des journaux, compressez le dossier entier pour vous assurer que les journaux corrects sont inclus. 

## <a name="next-steps"></a>Étapes suivantes
- [Signaler un problème](https://github.com/Microsoft/azuredatastudio/issues)
- [Qu’est-ce qu’Azure Data Studio](what-is-azure-data-studio.md)