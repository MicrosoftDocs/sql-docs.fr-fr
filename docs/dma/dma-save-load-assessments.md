---
title: Enregistrez et chargez les évaluations avec Assistant Migration de données
description: Découvrez comment utiliser Assistant Migration de données pour enregistrer et charger des évaluations.
ms.date: 01/10/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 9f427c7d706e3665cc5cbca092bfbc99a333a1ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030627"
---
# <a name="save-and-load-assessments-with-data-migration-assistant"></a>Enregistrez et chargez les évaluations avec Assistant Migration de données

Les instructions pas à pas suivantes vous aident à utiliser le Assistant Migration de données v 5.0 ou version ultérieure pour enregistrer une évaluation de base de données dans un fichier, puis à charger une évaluation à partir d’un fichier.

> [!NOTE]
> En plus de charger les évaluations enregistrées à l’aide de la dernière version de DMA, les utilisateurs peuvent également utiliser cette fonctionnalité pour charger des évaluations exportées en tant que fichiers. JSON à partir de versions précédentes de Assistant Migration de données pour afficher les résultats dans v 5.0 et versions ultérieures.

## <a name="saving-an-assessment-to-a-file"></a>Enregistrement d’une évaluation dans un fichier

1. Après avoir exécuté une évaluation à l’aide de Assistant Migration de données, sélectionnez **enregistrer l’évaluation**.

   ![Ouverture de la boîte de dialogue Enregistrer l’évaluation](../dma/media/dma-save-load-assessments/dma-open-save-dialog.png)

   L' **enregistrement standard...** s’affiche.

   > [!NOTE]
   > Pour plus d’informations sur l’exécution d’une évaluation dans Assistant Migration de données, consultez l’article [effectuer une évaluation de la Migration SQL Server avec Assistant Migration de données](../dma/dma-assesssqlonprem.md).

2. Spécifiez un nom pour le fichier, puis sélectionnez **Enregistrer**.

   ![Attribution d’un nom et enregistrement d’un fichier d’évaluation](../dma/media/dma-save-load-assessments/dma-name-save-assessment.png)

## <a name="loading-an-assessment-saved-to-a-file"></a>Chargement d’une évaluation enregistrée dans un fichier

1. Pour charger une évaluation que vous avez enregistrée précédemment dans un fichier, démarrez Assistant Migration de données, puis sous l’onglet **toutes les évaluations** , sélectionnez **évaluation de charge**.

   ![Ouverture de la boîte de dialogue évaluation de la charge](../dma/media/dma-save-load-assessments/dma-open-load-dialog.png)

2. Accédez au fichier d’évaluation enregistré que vous souhaitez charger, sélectionnez le fichier, puis sélectionnez **ouvrir**.

   ![Ouverture d’un fichier d’évaluation](../dma/media/dma-save-load-assessments/dma-open-assessment.png)

   Une entrée pour l’évaluation que vous avez chargée s’affiche sous l’onglet **toutes les évaluations** .

   ![Affichage de l’entrée d’évaluation](../dma/media/dma-save-load-assessments/dma-display-assessment-entry.png)

3. Sélectionnez l’entrée d’évaluation, puis sélectionnez **ouvrir l’évaluation**.

   ![Ouverture du détail de l’évaluation](../dma/media/dma-save-load-assessments/dma-open-assessment-detail.png)

   Assistant Migration de données affiche maintenant les détails de l’évaluation que vous avez exécutée précédemment.

   ![Affichage des détails de l’évaluation](../dma/media/dma-save-load-assessments/dma-display-assessment-detail.png)
