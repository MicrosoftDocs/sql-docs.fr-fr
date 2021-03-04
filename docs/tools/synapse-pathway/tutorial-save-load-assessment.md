---
title: Enregistrer et charger des évaluations avec Azure Synapse Pathway Preview
description: Enregistrer et charger des évaluations d’entrepôt de données avec Azure Synapse Pathway Preview
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: Azure Synapse Pathway
ms.topic: tutorial
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: c2893272c3bc2cebf21b85378565af8ba0383bc8
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873043"
---
# <a name="save-and-load-assessments-with-azure-synapse-pathway-preview"></a>Enregistrer et charger des évaluations avec Azure Synapse Pathway Preview
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Les instructions pas à pas suivantes montrent comment enregistrer et charger une évaluation d’entrepôt de données à partir d’un fichier en utilisant Azure Synapse Pathway.

Dans ce tutoriel, vous allez apprendre à :

> [!div class="checklist"]
> * Enregistrer une évaluation dans un fichier
> * Charger l’évaluation depuis un fichier

## <a name="prerequisites"></a>Prérequis

Pour suivre ce tutoriel, vérifiez que vous avez installé [Azure Synapse Pathway](synapse-pathway-download.md). Pour plus d’informations sur l’outil, consultez [Vue d’ensemble d’Azure Synapse Pathway](azure-synapse-pathway-overview.md).

## <a name="saving-an-assessment-to-a-file"></a>Enregistrement d’une évaluation dans un fichier
 
1. Une fois que vous avez effectué la traduction, il est recommandé de consulter le rapport récapitulant l’![évaluation Azure Synapse Pathway](./media/save-load-assessment/report-overview.png) de la traduction du code.
3. Sélectionnez le bouton **Enregistrer l’évaluation**, spécifiez le nom du fichier, puis sélectionnez **Enregistrer**.
![Évaluation Azure Synapse Pathway.](./media/save-load-assessment/save-assessment.png)

4. Un fichier .asmprj est créé sur la destination spécifiée.

## <a name="loading-an-assessment-from-a-file"></a>Chargement d’une évaluation à partir d’un fichier

1. Pour ouvrir la même évaluation, sélectionnez **Charger une évaluation**, puis spécifiez le nom du fichier .asmprj de l’![évaluation Azure Synapse Pathway.](./media/save-load-assessment/browse-location.png)

1. Les dossiers source, d’entrée et de sortie sont renseignés en fonction de l’évaluation sélectionnée.
![Évaluation Azure Synapse Pathway.](./media/save-load-assessment/load-assessment.png)
1. Sélectionnez **Traduire** pour réexécuter la traduction du code.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Génération de rapports](report-generation.md)
