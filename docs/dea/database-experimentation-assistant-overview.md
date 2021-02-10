---
title: Vue d’ensemble de l’Assistant Expérimentation de base de données
description: Découvrez plus d’informations sur les Assistant Expérimentation de base de données (DEA), par exemple comment évaluer une version ciblée de SQL Server pour une charge de travail spécifique.
ms.date: 12/12/2019
ms.prod: sql
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 09d334d7cfcc313cd36d3b59a6b47c64d441f3ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017069"
---
# <a name="overview-of-database-experimentation-assistant"></a>Vue d’ensemble de Assistant Expérimentation de base de données

Assistant Expérimentation de base de données (DEA) est une solution d’expérimentation pour les mises à niveau SQL Server. La DEA peut vous aider à évaluer une version ciblée de SQL Server pour une charge de travail spécifique. Les clients effectuant une mise à niveau à partir de versions antérieures de SQL Server (à partir de 2005) vers des versions plus récentes de SQL Server peuvent utiliser les métriques d’analyse fournies par l’outil.

Les métriques d’analyse de DEA sont les suivantes :

- Requêtes qui présentent des erreurs de compatibilité.
- Les requêtes et les plans de requête dégradés.
- Autres données de comparaison de la charge de travail.

Les données de comparaison peuvent entraîner une plus grande confiance et garantir une expérience de mise à niveau réussie.

## <a name="get-dea"></a>Bénéficier de DEA

Pour installer la DEA, [Téléchargez](https://www.microsoft.com/download/details.aspx?id=54090) la dernière version de l’outil. Ensuite, exécutez le fichier **DatabaseExperimentationAssistant.exe** .

## <a name="solution-architecture-for-comparing-workloads"></a>Architecture de la solution pour la comparaison des charges de travail

Le diagramme suivant illustre l’architecture de la solution pour une comparaison de charges de travail. La comparaison des charges de travail utilise la DEA et les Distributed Replay lors d’une mise à niveau de SQL Server 2008 à SQL Server 2016.

![Architecture de la solution de comparaison de charge de travail](./media/database-experimentation-assistant-overview/dea-overview-compare-solution-architecture.png)

## <a name="dea-prerequisites"></a>Conditions requises pour la DEA

Voici quelques conditions préalables à l’exécution de la DEA :

- Configuration matérielle minimale requise : un ordinateur à cœur unique avec 3,5 Go de RAM.
- Configuration matérielle idéale idéale : un processeur à huit cœurs (avec 3,5 Go de RAM ou plus). Les processeurs avec plus de huit cœurs n’améliorent pas les durées d’exécution de DEA.
- 33% de la taille de trace de performances supplémentaire est nécessaire pour stocker les bases de données A, B et d’analyse de rapport.

## <a name="configure-dea"></a>Configurer la DEA

Dans l’architecture d’environnement requise, nous vous recommandons d’installer la DEA *sur le même ordinateur que le contrôleur Distributed Replay*. Cette pratique évite les appels entre ordinateurs et simplifie la configuration.

### <a name="required-configuration-for-workload-comparison-using-dea"></a>Configuration requise pour la comparaison des charges de travail à l’aide de DEA

La DEA se connecte aux serveurs de base de données à l’aide de l’authentification Windows. Assurez-vous que l’utilisateur exécutant la DEA peut se connecter aux serveurs de base de données (source, cible et analyse) à l’aide de l’authentification Windows.

**Configuration requise pour la capture**

Pour capturer une trace, l’utilisateur doit exécuter la DEA :

- Peut se connecter au serveur de base de données source à l’aide de l’authentification Windows.
- Dispose de droits d’administrateur système sur le serveur de base de données source.

En outre, le compte de service qui exécute le serveur de base de données source requiert un accès en écriture au chemin d’accès au dossier de trace.

Pour plus d’informations, consultez le [Forum aux questions sur la capture de trace](database-experimentation-assistant-capture-trace.md#frequently-asked-questions-about-trace-capture).

**Configuration requise pour la relecture**

La relecture d’une trace nécessite que l’utilisateur exécutant la DEA :

- Peut se connecter au serveur de base de données cible à l’aide de l’authentification Windows.
- Dispose de droits d’administrateur système sur le serveur de base de données cible.

En outre, la relecture d’une trace requiert les éléments suivants :

- Le compte de service exécutant les serveurs de base de données cible dispose d’un accès en écriture au chemin d’accès au dossier de trace.
- Le compte de service qui exécute Distributed Replay clients peut se connecter au serveur de base de données cible à l’aide de l’authentification Windows.
- Les ports TCP sont ouverts pour les demandes entrantes sur le contrôleur de Distributed Replay. La DEA communique avec le contrôleur Distributed Replay à l’aide d’interfaces COM.

Pour plus d’informations, consultez le [Forum aux questions sur la relecture de trace](database-experimentation-assistant-replay-trace.md#frequently-asked-questions-about-trace-replay).

**Configuration requise pour l’analyse**

Pour effectuer l’analyse, l’utilisateur doit exécuter la DEA :

- Peut se connecter au serveur de base de données d’analyse à l’aide de l’authentification Windows.
- Dispose de droits d’administrateur système sur le serveur de base de données source.

Pour plus d’informations, consultez le [Forum aux questions sur les rapports d’analyse](database-experimentation-assistant-create-report.md#frequently-asked-questions-about-analysis-reports).

## <a name="set-up-telemetry"></a>Configurer la télémétrie

La DEA dispose d’une fonctionnalité Internet qui peut envoyer des informations de télémétrie à Microsoft pour une utilisation dans l’amélioration de l’expérience du produit. Les informations collectées sont également enregistrées sur votre ordinateur pour un audit local. vous pouvez donc toujours voir ce qui est collecté. Tous les fichiers journaux de DEA sont enregistrés dans le dossier% Temp% \\ DEA.

Les données de télémétrie peuvent être collectées sur quatre types d’événements :

- **TraceEvent**: événements d’utilisation de l’application (par exemple, « arrêter la capture »).
- **Exception**: exception levée pendant l’utilisation de l’application.
- **DiagnosticEvent**: journal des événements pour faciliter le diagnostic lorsque des problèmes se produisent (*non* envoyés à Microsoft).
- **FeedbackEvent**: commentaires des utilisateurs qui sont envoyés par le biais de l’application.

La collecte et l’envoi de données de télémétrie sont facultatifs. Pour spécifier les événements qui sont collectés et si les événements collectés sont envoyés à Microsoft, procédez comme suit :

1. Accédez à l’emplacement dans lequel la DEA est installée (par exemple, C : \\ Program Files (x86) \\ Microsoft Corporation \\ Assistant expérimentation de base de données).
2. Ouvrez et modifiez les fichiers. config **DEA.exe.config** (pour l’application) et **DEACmd.exe.config** (pour l’interface CLI) pour traiter votre scénario comme il convient :
    - Pour arrêter la collecte d’un type d’événement, définissez la valeur de l' *événement* (par exemple, **TraceEvent**) sur **false**. Pour recommencer la collecte de l’événement, définissez la valeur sur **true**.
    - Pour arrêter l’enregistrement des copies locales des événements, affectez la valeur **false** à **TraceLoggerEnabled** . Pour recommencer à enregistrer les copies locales, définissez la valeur sur **true**.
    - Pour arrêter l’envoi d’événements à Microsoft, affectez la valeur **false** à **AppInsightsLoggerEnabled** . Pour commencer à envoyer des événements à Microsoft, définissez la valeur sur **true**.

La DEA est régie par la [déclaration de confidentialité de Microsoft](https://aka.ms/dea-privacy).

## <a name="see-also"></a>Voir également

- L’article [vue d’ensemble du processus de comparaison des charges de travail](database-experimentation-assistant-get-started.md), qui explique le processus impliqué dans la comparaison des charges de travail dans deux environnements.
