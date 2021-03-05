---
title: Génération de rapports dans Azure Synapse Pathway Preview
description: Azure Synapse Pathway fournit des rapports complets sur les scripts traduits.
author: anshul82-ms
ms.author: anrampal
ms.topic: tutorial
ms.prod: sql
ms.technology: tools-other
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-tutorial
ms.openlocfilehash: 26701c33f713a1b82e3bab604a03e7dca054a36b
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186334"
---
# <a name="report-generation-for-azure-synapse-pathway-preview"></a>Génération de rapports pour Azure Synapse Pathway Preview
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Azure Synapse Pathway fournit un rapport complet sur le nombre de scripts qui ont été traduits. Le rapport montre aussi le nombre d’erreurs et d’avertissements sur les instructions qui n’ont pas été traduites.

## <a name="generate-report"></a>Générer un rapport

Quand vous sélectionnez **Traduire**, vous voyez un rapport comme celui-ci :

![Rapport Azure Synapse Pathway.](./media/report-generaration/report-overview.png)

## <a name="report-summary"></a>Récapitulatif du rapport

Réussite de la conversion indique le pourcentage de scripts traduits avec succès.

![Azure Synapse Pathway.](./media/report-generaration/conversion-success.png)

La section Distribution des instructions détaille le nombre d’instructions DDL et DML qui ont été traduites, avec le nombre de schémas, de tables et de bases de données créés.

![Azure Synapse - Rapport 1.](./media/report-generaration/statement-distribution.png)

Les économies liées à la migration sont calculées en fonction du nombre et de la complexité (simple, élevée ou moyenne) des objets traduits. Synapse Pathway prend comme hypothèse 30 minutes de travail manuel pour chaque traduction d’objet.

![Azure Synapse - Rapport 2.](./media/report-generaration/migration-savings.png)

## <a name="next-steps"></a>Étapes suivantes

[Télécharger Azure Synapse Pathway](synapse-pathway-download.md)