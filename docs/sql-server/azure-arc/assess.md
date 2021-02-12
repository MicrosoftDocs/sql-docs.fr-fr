---
title: Configurer SQL Assessment à la demande sur une instance Azure Arc enabled SQL Server
description: Configurer SQL Assessment à la demande sur une instance Azure Arc enabled SQL Server
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: 2fb25ca30a3b55bfcedb470addad8680f4914e2f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063291"
---
# <a name="configure-sql-assessment-on-an-azure-arc-enabled-sql-server-instance"></a>Configurer SQL Assessment sur une instance Azure Arc enabled SQL Server

SQL Assessment fournit un mécanisme permettant d’évaluer la configuration de SQL Server. Cet article fournit des instructions sur l’utilisation de SQL Assessment sur une instance Azure Arc enabled SQL Server

## <a name="prerequisites"></a>Prérequis

* Votre instance SQL est connectée à Azure Arc. Pour obtenir des instructions, consultez l’article [Connecter votre SQL Server à Azure Arc](connect.md).

* L’extension Microsoft Monitoring Agent (MMA) doit être installée et configurée sur l’ordinateur. Pour obtenir des instructions, affichez l’article [installer MMA](configure-advanced-data-security.md#install-microsoft-monitoring-agent-mma). Vous pouvez également obtenir plus d’informations sur l’article [Agent Log Analytics](/azure/azure-monitor/platform/log-analytics-agent).

* Le [protocole TCP/IP doit être activé](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md) sur votre SQL Instance SQL.

* Le [service du navigateur SQL Server](../../tools/configuration-manager/sql-server-browser-service.md) doit être en cours d’exécution si vous utilisez une instance nommée de SQL Server.

* Vérifiez avoir consulté les [Composants requis des évaluations à la demande des SErvices Hub](/services-hub/health/assessment-prereq-docs#on-demand-assessment-prerequisite-documents) du document SQL Server.

## <a name="run-on-demand-sql-assessment"></a>Exécuter SQL Assessment à la demande

1. Ouvrez votre ressource SQL Server - Azure Arc et sélectionnez **Intégrité de l’environnement** dans le menu de gauche.

   > [!div class="mx-imgBorder"]
   > [ ![Capture d’écran affichant l’écran Intégrité de l’environnement d’une ressource SQL Server - Azure Arc.](media/assess/sql-assessment-heading-sql-server-arc.png) ](media/assess/sql-assessment-heading-sql-server-arc.png#lightbox)

> [!IMPORTANT]
> Si l’extension MMA n’est pas installée, vous ne pourrez pas lancer SQL Assessment à la demande.

2. Sélectionnez le type de compte. Si vous disposez d’un compte de service administré, vous pouvez lancer SQL Assessment directement à partir du portail. Spécifiez le nom du compte.

> [!NOTE]
> La spécification d’un *compte de service administré* active le bouton **Configurer SQL Assessment** pour vous permettre de lancer l’évaluation à partir du portail en déployant un *CustomScriptExtension*. Étant donné qu’un seul *CustomScriptExtension* peut être déployé à la fois, l’extension de script pour SQL Assessment est automatiquement supprimée après l’exécution. Si un autre *CustomScriptExtension* est déjà déployé sur l’ordinateur d’hébergement, le bouton **Configurer SQL Assessment** n’est pas activé.

3. Spécifiez un répertoire de travail sur la machine de collecte de données si vous souhaitez changer la valeur par défaut. Par défaut, `C:\sql_assessment\work_dir` est utilisé. Pendant la collecte et l’analyse, les données sont stockées temporairement dans ce dossier. Si le dossier n’existe pas, il est créé automatiquement.

4. Si vous lancez SQL Assessment à partir du portail en cliquant sur **Configurer SQL Assessment**, une bulle de déploiement standard s’affiche.

> [!div class="mx-imgBorder"]
   > [ ![Capture d’écran montrant le déploiement de CustomScriptExtension.](media/assess/sql-assessment-custom-script-deployment.png) ](media/assess/sql-assessment-custom-script-deployment.png#lightbox)

5. Si vous préférez lancer SQL Assessment à partir de l’ordinateur cible, cliquez sur **Télécharger le script de configuration**, copiez le script téléchargé sur l’ordinateur cible, puis exécutez l’un des blocs de code suivants dans une instance d’administration de **powershell.exe** :

   * _Compte de domaine_ :  Vous êtes invité à entrer le compte d’utilisateur et le mot de passe.

      ```powershell
      Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
      & '.\AddSqlAssessment.ps1'
      ```

   * _Compte de service géré (MSA)_

      ```powershell
      Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass -Force
      & '.\AddSqlAssessment.ps1' -ManagedServiceAccountName <MSA account name>
      ```

> [!NOTE]
> Le script planifie une tâche nommée *SQLAssessment*, qui déclenche la collecte des données. Cette tâche s’exécute dans l’heure qui suit l’exécution du script. Il se répète ensuite tous les sept jours.

> [!TIP]
> Vous pouvez modifier la tâche pour qu’elle s’exécute à une date et une heure différentes, voire la forcer à s’exécuter immédiatement. Dans la bibliothèque du planificateur de tâches, recherchez **Microsoft** > **Operations Management Suite** > **AOI\*\*\***  > **Évaluations** > **SQLAssessment**.

## <a name="view-sql-assessment-results"></a>Afficher les résultats SQL Assessment

* Dans le volet _Intégrité de l’environnement_, sélectionnez le bouton **Afficher les résultats de SQL Assessment**.

   > [!NOTE]
   > Le bouton **Afficher les résultats de SQL Assessment** reste désactivé jusqu’à ce que les résultats soient prêts dans Log Analytics. Ce processus peut prendre jusqu’à deux heures après le traitement des fichiers de données sur l’ordinateur cible.

   > [!div class="mx-imgBorder"]
   > [ ![Capture d’écran montrant les résultats de SQL Assessment.](media/assess/sql-assessment-results.png) ](media/assess/sql-assessment-results.png#lightbox)

* Vous pouvez voir l’état du traitement de données sur la machine de collecte en vérifiant les fichiers dans le dossier de travail. Une fois la tâche planifiée terminée, vous devez voir plusieurs fichiers avec le préfixe _new._ préfixe dans le répertoire de travail.

   > [!div class="mx-imgBorder"]
   > [ ![Capture d’écran montrant une fenêtre Gestionnaire de fichiers affichant les nouveaux fichiers de données dans le dossier de travail.](media/assess/sql-assessment-data-files-ready.png) ](media/assess/sql-assessment-data-files-ready.png#lightbox)

* Microsoft Monitoring Agent analyse le dossier de travail toutes les 15 minutes. Il recherche les fichiers _nouveaux.*_ et envoie les données à l’espace de travail Log Analytics. Une fois que MMA a téléchargé le fichier, il modifie la modification de préfixe de _nouveau._ sur _processed._

   > [!div class="mx-imgBorder"]
   > ![Capture d’écran montrant une fenêtre Gestionnaire de fichiers affichant les nouveaux fichiers de données.](media/assess/sql-assessment-data-files-processed.png)

## <a name="next-steps"></a>Étapes suivantes

* Pour plus d’informations en affichant les documents prérequis, consultez la section [Évaluations à la demande du hub de services](/services-hub/health/assessment-prereq-docs#on-demand-assessment-prerequisite-documents).

* Pour bénéficier d’un support complet pour la fonctionnalité SQL Assessment à la demande, un abonnement au support Premier ou Unified est requis. Pour plus d’informations, consultez [Support Premier Azure](https://azure.microsoft.com/support/plans/premier).
