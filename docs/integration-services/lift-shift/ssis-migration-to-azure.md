---
title: Vue d’ensemble de la migration de SQL Server Integration Services vers Azure | Microsoft Docs
description: Cet article présente les processus et les outils permettant de migrer SQL Server Integration Services vers Azure.
ms.date: 04/10/2020
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f443bc4209365253cf6e4e4005d19afe9ee11268
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061464"
---
# <a name="migrate-on-premises-ssis-workloads-to-ssis-in-adf"></a>Migrer des charges de travail SSIS locales vers SSIS dans ADF

Cet article liste les processus et les outils qui peuvent vous aider à migrer des charges de travail SQL Server Integration Services (SSIS) vers SSIS dans ADF.

La [vue d’ensemble de la migration](/azure/data-factory/scenario-ssis-migration-overview) présente le processus de migration de vos charges de travail ETL dans SSIS au niveau local vers SSIS dans ADF.

Le processus de migration se déroule en deux phases : l’[évaluation](/azure/data-factory/scenario-ssis-migration-overview#assessment) et la [migration](/azure/data-factory/scenario-ssis-migration-overview#migration).

## <a name="assessment"></a>Évaluation

À cette fin, vous pouvez utiliser l’Assistant Migration de données (DMA), un outil disponible en téléchargement gratuit qui peut être installé et exécuté localement. Vous pouvez créer un projet d’évaluation DMA de type Services d’intégration pour évaluer des packages SSIS par lots et identifier les problèmes de compatibilité.

Obtenez l’[Assistant Migration de base de données](../../dma/dma-overview.md) et [procédez à l’évaluation des packages](../../dma/dma-assess-ssis.md).

## <a name="migration"></a>Migration

Les étapes de migration des packages SSIS et des travaux de SQL Server Agent qui planifient les exécutions de packages SSIS peuvent varier selon les types de stockage des packages SSIS sources et la destination de la migration des charges de travail de base de données. Pour plus d’informations, consultez [cette page](/azure/data-factory/scenario-ssis-migration-overview#migration).

## <a name="next-steps"></a>Étapes suivantes

- [Migrer des packages SSIS vers Azure SQL Managed Instance](/azure/dms/how-to-migrate-ssis-packages-managed-instance).
- [Migrez les travaux SSIS vers Azure Data Factory (ADF) avec SQL Server Management Studio (SSMS)](/azure/data-factory/how-to-migrate-ssis-job-ssms).