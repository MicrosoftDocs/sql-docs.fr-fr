---
title: Extension SQL Server Profiler
description: Découvrez comment installer et utiliser l’extension de SQL Server Profiler. Solution de suivi de SQL Server facile à utiliser, similaire au Profiler SSMS.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: f2a6672f3b73a5be8077229019473758b2bf1e95
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040249"
---
# <a name="sql-server-profiler-extension-preview"></a>Extension SQL Server Profiler (préversion)

L’extension SQL Server Profiler (préversion) fournit une solution de suivi de SQL Server simple semblable à SQL Server Management Studio (SSMS) Profiler, à l’exception de l’utilisation d’événements étendus. SQL Server Profiler est très facile à utiliser et possède de bonnes valeurs par défaut pour les configurations de suivi les plus courantes. L’expérience utilisateur est optimisée pour parcourir les événements et afficher le texte Transact-SQL (T-SQL) associé. SQL Server Profiler pour Azure Data Studio suppose également de bonnes valeurs par défaut pour la collecte d’activités d’exécution de T-SQL avec une expérience utilisateur simple d’utilisation. Cette extension est actuellement en préversion.

**Cas d’utilisation courants de SQL Server Profiler :**

- Exécuter pas à pas des requêtes posant problème afin d'en déterminer la cause.
- Détecter les requêtes s'exécutant lentement et diagnostiquer la cause du problème.
- Capturer la série d'instructions Transact-SQL conduisant à un problème.
- Surveiller les performances de SQL Server en vue de paramétrer les charges de travail.
- Mise en corrélation des compteurs de performances pour diagnostiquer des problèmes.

## <a name="install-the-sql-server-profiler-extension"></a>Installer l’extension SQL Server Profiler

1. Pour ouvrir le gestionnaire d’extensions et accéder aux extensions disponibles, sélectionnez l’icône d’extensions ou sélectionnez **Extensions** dans le menu **Affichage**.
2. Sélectionnez une extension disponible pour afficher ses détails.

    ![Gestionnaire d’extension du Profiler](media/sql-server-profiler-extension/profiler-extension.png)

3. Sélectionnez l’extension de votre choix et **installez-la**.
4. Sélectionnez **Recharger** pour activer l’extension (nécessaire uniquement la première fois que vous installez une extension).

## <a name="start-profiler"></a>Démarrer le profileur

1. Pour démarrer le profileur, commencez par établir une connexion à un serveur sous l’onglet Serveurs.
2. Une fois que vous avez créé une connexion, appuyez sur **Alt + P** pour lancer le profileur.
3. Pour démarrer le profileur, appuyez sur **Alt + S**. Vous pouvez maintenant commencer à voir les événements étendus.

    ![Afficher le profileur](media/sql-server-profiler-extension/view-profiler.png)

4. Pour arrêter le profileur, appuyez sur **Alt + S**. Cette touche de raccourci est un bouton bascule.

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur le profileur et les événements étendus, consultez [Événements étendus](../../relational-databases/extended-events/extended-events.md).