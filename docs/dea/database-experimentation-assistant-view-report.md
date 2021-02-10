---
title: Afficher les rapports d’analyse pour les mises à niveau de SQL Server
description: Découvrez comment afficher et comprendre les rapports d’analyse pour obtenir des informations sur les performances dans Assistant Expérimentation de base de données (DEA).
ms.custom: seo-lt-2019
ms.date: 02/04/2020
ms.prod: sql
ms.prod_service: dea
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.openlocfilehash: d1666d4ba2279623065dd3c0d8faf5e0653de82e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065944"
---
# <a name="view-analysis-reports-in-database-experimentation-assistant"></a>Afficher les rapports d’analyse dans Assistant Expérimentation de base de données

Une fois que vous avez utilisé Assistant Expérimentation de base de données (DEA) pour [créer un rapport d’analyse](database-experimentation-assistant-create-report.md), vous pouvez consulter le rapport pour obtenir des informations sur les performances en fonction du test A/B que vous avez effectué.

## <a name="open-an-existing-analysis-report"></a>Ouvrir un rapport d’analyse existant

1. Dans DEA, sélectionnez l’icône de liste, spécifiez le nom du serveur et le type d’authentification, activez ou désactivez les cases à cocher **chiffrer la connexion** et **approuver le certificat du serveur** en fonction de votre scénario, puis sélectionnez **se connecter**.

   ![Se connecter au serveur avec le rapport](./media/database-experimentation-assistant-view-report/dea-connect-to-server-with-report-files.png)

2. Dans l’écran **rapports d’analyse** , sur le côté gauche, sélectionnez l’entrée pour le rapport que vous souhaitez afficher.

   ![Ouvrir un fichier de rapport existant](./media/database-experimentation-assistant-view-report/dea-select-report-to-view.png)

## <a name="view-and-understand-the-analysis-report"></a>Afficher et comprendre le rapport d’analyse

Cette section vous guide tout au long du rapport d’analyse.

Sur la première page de votre rapport, des informations sur la version et les informations de build des serveurs cibles sur lesquels l’expérimentation a été exécutée s’affichent. Seuil vous permet d’ajuster la sensibilité ou la tolérance de votre analyse de test A/B. Par défaut, le seuil est défini sur 5%; toute amélioration des performances >= 5% est catégorisée comme « améliorée ».  La liste déroulante vous permet d’évaluer le rapport avec différents seuils de performances.

Vous pouvez exporter les données du rapport dans un fichier CSV en sélectionnant le bouton **Exporter** .  Sur n’importe quelle page du rapport d’analyse, vous pouvez sélectionner **Imprimer** pour imprimer ce qui est visible à l’écran à ce moment-là.

### <a name="query-distribution"></a>Distribution des requêtes

- Sélectionnez les différents secteurs des graphiques à secteurs pour afficher uniquement les requêtes qui appartiennent à cette catégorie.

   ![Catégories de rapports sous forme de secteurs](./media/database-experimentation-assistant-view-report/dea-view-report-pie-slices.png)

  - **Détérioré**: requêtes exécutées plus loin sur la cible 2 que sur la cible 1.
  - **Erreurs**: requêtes qui ont montré des erreurs au moins une fois sur au moins l’une des cibles.
  - **Amélioration**: requêtes qui ont été plus performantes sur la cible 2 que sur la cible 1.
  - **Impossible d’évaluer**: requêtes dont la taille d’échantillon est trop petite pour l’analyse statistique. Pour l’analyse de test A/B, la DEA requiert que les mêmes requêtes aient au moins 15 exécutions sur chaque cible.
  - **Identique**: les requêtes qui n’ont pas de différence statistique entre Target 1 et Target 2.

  Les requêtes d’erreur, le cas échéant, sont affichées dans des graphiques séparés ; graphique à barres qui classe les erreurs par type et un graphique à secteurs qui classe les erreurs par ID d’erreur.

   ![Graphiques de requête d’erreur](./media/database-experimentation-assistant-view-report/dea-error-query-charts.png)

  Il existe quatre types d’erreurs possibles :

  - **Erreurs existantes**: erreurs qui existent sur les cibles 1 et 2.
  - **Nouvelles Erreurs**: erreurs nouvelles sur la cible 2.
  - **Erreurs résolues**: erreurs qui existent sur la cible 1 mais qui sont résolues sur la cible 2.
  - **Bloqueurs de mise à niveau**: erreurs qui bloquent la mise à niveau vers le serveur cible.

  Cliquer sur une section de barres ou de secteurs dans les graphiques permet de descendre dans la catégorie et d’afficher les métriques de performances, même pour la catégorie **ne peut pas évaluer** .

  En outre, le tableau de bord affiche les cinq principales requêtes améliorées et dégradées pour fournir une vue d’ensemble rapide des performances.

### <a name="individual-query-drill-down"></a>Exploration des requêtes individuelles

Vous pouvez sélectionner des liens de modèle de requête pour obtenir des informations plus détaillées sur des requêtes spécifiques.

![Exploration d’une requête spécifique](./media/database-experimentation-assistant-view-report/dea-query-drill-down-report.png)

- Sélectionnez une requête spécifique pour ouvrir le résumé de la comparaison connexe.

   ![Résumé de la comparaison](./media/database-experimentation-assistant-view-report/dea-view-report-comparison-summary.png)

   Vous trouverez des statistiques récapitulatives pour cette requête, telles que le nombre d’exécutions, la durée moyenne, l’UC moyenne, les lectures/écritures moyennes et le nombre d’erreurs.  Si la requête est une requête d’erreur, l’onglet informations sur l' **erreur** affiche plus de détails sur l’erreur.  Sous l’onglet informations sur le **plan de requête** , vous pouvez trouver des informations sur les plans de requête utilisés pour la requête sur Target 1 et Target 2.

   > [!NOTE]
   > Si vous analysez un événement étendu (. XEL), les informations de plan de requête ne sont pas collectées pour limiter la sollicitation de la mémoire sur l’ordinateur de l’utilisateur.

## <a name="see-also"></a>Voir également

- Pour savoir comment générer un rapport d’analyse à partir d’une invite de commandes, consultez [exécuter à l’invite de commandes](database-experimentation-assistant-run-command-prompt.md).
