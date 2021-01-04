---
title: Configurer la sécurité avancée des données
titleSuffix: Azure Arc
description: Configurer Advanced Data Security pour votre instance de serveur SQL doté d’Azure Arc
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: e0c68752d21de5945c06433af8c08f6208322f7c
ms.sourcegitcommit: 18e2f0706e03d0b2b6324845244fbafaa077a8dd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97103167"
---
# <a name="configure-advanced-data-security-for-azure-arc-enabled-sql-server-instance"></a>Configurer Advanced Data Security pour votre instance de serveur SQL doté d’Azure Arc

Vous pouvez activer la sécurité avancée des données pour vos instances SQL Server sur site en procédant comme suit.

## <a name="prerequisites"></a>Prérequis

* Votre instance SQL Server est intégrée au serveur SQL doté d’Azure Arc. Suivez ces instructions pour [intégrer votre instance SQL Server à un serveur SQL doté d’Azure Arc](connect.md).

* Votre compte d’utilisateur se voit attribuer l’un des rôles [Security Center (RBAC)](/azure/security-center/security-center-permissions)

## <a name="create-a-log-analytics-workspace"></a>Créer un espace de travail Log Analytics

1. Recherchez le type de ressource __espace de travail Log Analytics__ et ajoutez-en un nouveau par le biais du panneau de création.

   ![Créer un espace de travail](media/configure-advanced-data-security/create-new-log-analytics-workspace.png)

   > [!NOTE]
   > Vous pouvez utiliser un espace de travail Log Analytics dans n’importe quelle région. Par conséquent, si vous en avez déjà un, vous pouvez l’utiliser. Toutefois, nous vous recommandons de le créer dans la région dans laquelle votre ressource __Machine - Azure Arc__ a été créée.

1. Accédez à la page de présentation de la ressource de l’espace de travail Log Analytics et sélectionnez « Windows, Linux et d’autres sources ». Copiez et collez l’ID de l’espace de travail et la clé primaire pour pouvoir les réutiliser ultérieurement.

   ![Panneau de l’espace de travail Log Analytics](media/configure-advanced-data-security/log-analytics-workspace-blade.png)

## <a name="install-microsoft-monitoring-agent-mma"></a>Installer Microsoft Monitoring Agent (MMA)

La prochaine étape est nécessaire uniquement si vous n’avez pas encore configuré l’agent MMA sur l’ordinateur distant.

1. Sélectionnez la ressource __Machine - Azure Arc__ pour le serveur virtuel ou physique sur lequel l’instance SQL Server est installée et ajoutez l’extension __Microsoft Monitoring Agent - Azure Arc__ à l’aide de la fonctionnalité **Extensions**. Lorsque vous êtes invité à configurer l’espace de travail Log Analytics, utilisez l’ID de l’espace de travail et le principal que vous avez enregistrés à l’étape précédente.

   ![Installer MMA](media/configure-advanced-data-security/install-mma-extension.png)

1. Une fois la validation réussie, cliquez sur **Créer** pour démarrer le flux de travail de déploiement de l’extension MMA Arc. Une fois le déploiement terminé, l’état **Réussite** s’affiche.

1. Pour plus d’informations, consultez [Gestion des extensions avec Azure Arc](/azure/azure-arc/servers/manage-vm-extensions)

## <a name="enable-azure-defender"></a>Activer Azure Defender

Ensuite, vous devez activer Azure Defender pour l’instance SQL Server.

1. Accédez à Security Center, puis, à partir de la barre latérale, ouvrez **Tarification et paramètres**.

1. Sélectionner l’espace de travail que vous avez configuré pour l’extension MMA à l’étape précédente

1. Sélectionnez **Azure Defender activé**. Vérifiez que l’option **Serveurs SQL sur les machines** est activée.

   ![Mettre à niveau l’espace de travail](media/configure-advanced-data-security/enable-azure-defender.png)

 > [!NOTE]
   > La première analyse évaluant la vulnérabilité aura lieu dans les 24 heures qui suivent l’activation d’Advanced Data Security. Ensuite, des analyses automatiques sont effectuées chaque dimanche.

## <a name="explore"></a>Explorer

Examinez les anomalies et les menaces de sécurité dans Azure Security Center.

1. Ouvrez votre ressource SQL Server - Azure Arc et sélectionnez **Sécurité** dans le menu de gauche pour afficher les recommandations et les alertes pour cette instance.

   ![Sélectionner l’en-tête de sécurité](media/configure-advanced-data-security/security-heading-sql-server-arc.png)

1. Cliquez sur l’une des recommandations pour afficher les détails de la vulnérabilité dans __Security Center__.

   ![Rapport de vulnérabilité](media/configure-advanced-data-security/vulnerabilities-report.png)

1. Cliquez sur une alerte de sécurité pour obtenir des détails complets et examiner l’attaque dans [Azure Sentinel](/azure/sentinel/overview). Le diagramme suivant est un exemple de l’alerte de force brute.

   ![Alerte de force brute](media/configure-advanced-data-security/brute-force-alert.png)

1. Cliquez sur **Agir** pour supprimer l’alerte.

   ![Suppression des alertes](media/configure-advanced-data-security/brute-force-alert-mitigation.png)

> [!NOTE]
> Le lien __Security Center__ général situé en haut de la page n’utilise pas l’URL du portail en préversion. Les ressources __SQL Server - Azure arc__ ne seront donc pas visibles ici. Nous vous recommandons de suivre les liens pour connaître les différentes recommandations ou alertes.

## <a name="next-steps"></a>Étapes suivantes

Vous pouvez approfondir vos investigations sur les alertes et les attaques de sécurité à l’aide d’[Azure Sentinel](/azure/sentinel/overview). Pour [intégrer Azure Sentinel](/azure/sentinel/connect-data-sources), suivez ces instructions.