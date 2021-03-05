---
title: Vue d’ensemble d’Azure Synapse Pathway Preview
description: Azure Synapse Pathway est un outil qui permet de migrer un entrepôt de données vers Azure Synapse Analytics.
author: anshul82-ms
ms.author: anrampal
ms.topic: overview
ms.date: 03/02/2021
ms.prod: sql
ms.technology: tools-other
monikerRange: =azure-sqldw-latest
ms.custom: template-overview
ms.openlocfilehash: 5e3844f6e63fafca5137a646ff4c02edbc7105b8
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102185919"
---
# <a name="azure-synapse-pathway-preview-overview"></a>Vue d’ensemble d’Azure Synapse Pathway Preview
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Quand des clients envisagent de moderniser leurs systèmes d’entrepôts de données, un des facteurs critiques bloquants auxquels ils sont confrontés est la traduction de leur code SQL. Le code existant est écrit et optimisé pour leur système actuel, mais doit être optimisé pour le nouveau système vers lequel ils migrent.

Les organisations du monde entier veulent moderniser leur plateforme d’analytique pour bénéficier à la fois d’un meilleur coût total de possession (TCO) et des avantages de l’innovation. Cependant, des clients ont investi des milliers d’heures de travail et des millions d’euros, et ont écrit des dizaines de milliers de lignes de code pour leur entrepôt de données.
 
Pour traduire ce code SQL critique, les clients doivent réécrire manuellement le code SQL existant, ou bien investir une grande partie de leur budget pour faire réécrire ou convertir leur code par un prestataire externe.

> [!IMPORTANT]
> Azure Synapse Pathway est actuellement en préversion publique.
> Cette préversion est fournie sans contrat de niveau de service et n’est pas recommandée pour les charges de travail de production. Certaines fonctionnalités peuvent être limitées ou non prises en charge.
 
> Pour plus d’informations, consultez [Conditions d’Utilisation Supplémentaires relatives aux Évaluations Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

**Azure Synapse Pathway** vous aide à effectuer une mise à niveau vers une plateforme d’entrepôt de données moderne en automatisant la traduction du code de votre entrepôt de données existant. Il s’agit d’un outil gratuit, intuitif et facile à utiliser, qui automatise la traduction du code, ce qui permet une migration plus rapide vers Azure Synapse Analytics.

 ![Vue d’ensemble d’Azure Synapse Pathway.](./media/azure-synapse-pathway-overview/pathway-overview.png) 

Synapse Pathway traduit les instructions DDL (Data Definition Language) et DML (Data Manipulation Language) en un langage compatible T-SQL compatible avec Azure Synapse SQL.

## <a name="behind-the-scenes"></a>Dans les coulisses

Pour plus d’informations sur Azure Synapse Pathway, reportez-vous à [Dans les coulisses](synapse-pathway-behind-the-scenes.md) pour un processus pas à pas expliquant le fonctionnement d’Azure Synapse Pathway.

## <a name="get-azure-synapse-pathway"></a>Obtenir Azure Synapse Pathway

Pour installer Synapse Pathway, consultez [Téléchargement d’Azure Synapse Pathway](synapse-pathway-download.md) pour les prérequis et le lien permettant de télécharger la version la plus récente.

## <a name="supported-sources"></a>Sources prises en charge

Azure Synapse Pathway prend en charge la conversion de code des bases de données, des schémas et des tables pour les sources suivantes :
- **IBM Netezza**
- **Microsoft SQL Server**
- **Snowflake**

## <a name="frequently-asked-questions"></a>Forum aux questions

Pour plus d’informations sur Azure Synapse Pathway, consultez la [page Forum aux questions](pathway-faq.md).

## <a name="next-steps"></a>Étapes suivantes

- [Exécuter votre première traduction avec Azure Synapse Pathway](synapse-pathway-assessment.md)
- Blog des annonces - [Announcing Azure Synapse Pathway: Turbocharge your data warehouse migration - Microsoft Tech Community](https://techcommunity.microsoft.com/t5/azure-synapse-analytics/announcing-azure-synapse-pathway-turbocharge-your-data-warehouse/ba-p/2176630)