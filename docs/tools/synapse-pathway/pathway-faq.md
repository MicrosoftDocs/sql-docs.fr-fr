---
title: Forum aux questions d’Azure Synapse Pathway Preview
description: Forum aux questions pour Azure Synapse Pathway
author: anshul82-ms
ms.author: anrampal
ms.topic: overview
ms.date: 03/02/2021
ms.prod: sql
ms.technology: tools-other
monikerRange: =azure-sqldw-latest
ms.custom: template-overview
ms.openlocfilehash: e97425f682a3f6b9f2f4e955d8476d40d6cf4312
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770507"
---
# <a name="azure-synapse-pathway-preview-faq"></a>Forum aux questions d’Azure Synapse Pathway Preview
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Ce guide recense les questions les plus fréquemment posées sur Azure Synapse Pathway Preview.

## <a name="general"></a>Général

### <a name="q-what-is-azure-synapse-pathway"></a>Q. Qu’est-ce qu’Azure Synapse Pathway ?

R. Azure Synapse Pathway est un outil de traduction de code qui vous permet d’effectuer une mise à niveau vers une plateforme d’entrepôt de données moderne, en automatisant la traduction du code de votre entrepôt de données existant pour le rendre conforme à Azure Synapse SQL, qui est basé sur T-SQL.

### <a name="q-how-can-i-download-azure-synapse-pathway"></a>Q. Comment puis-je télécharger Azure Synapse Pathway ?

R. Synapse Pathway est disponible au téléchargement auprès du [Centre de téléchargement Microsoft](https://aka.ms/synapse-pathway-download).

### <a name="q-how-much-does-azure-synapse-pathway-cost"></a>Q. Combien coûte Azure Synapse Pathway ?

R. Il n’y a aucun coût associé au téléchargement et à l’exécution de vos traductions de code en utilisant Synapse Pathway.

### <a name="q-what-sourcetarget-pairs-does-azure-synapse-pathway-currently-support"></a>Q. Quelles sont les paires source/cible actuellement prises en charge par Azure Synapse Pathway ?

R. Dans cette préversion de Synapse Pathway, les entrepôts de données suivants sont inclus en tant que sources :
- Microsoft SQL Server
- Snowflake
- Netezza

### <a name="q-what-is-included-as-part-of-the-code-conversion"></a>Q. Qu’est-ce qui est inclus dans la conversion du code ?

R. La préversion de Synapse Pathway prend en charge la traduction de code des tables, schémas et vues.

| Plateforme source| Types d’instructions pris en charge | 
|:-------------------:|:------------------|
| IBM Netezza  | Create/Alter/Drop Database<br /> Create/Alter/Drop Schema <br /> Create/Alter/Drop Table |
|Microsoft SQL Server  | Create/Alter/Drop Database<br /> Create/Alter/Drop Schema <br /> Create/Alter/Drop Table | 
| Snowflake |  Create/Alter/Drop Database<br /> Create/Alter/Drop Schema <br /> Create/Alter/Drop Table |                       

  
### <a name="q-can-it-also-scan-my-environment-and-provide-an-assessment-report-of-all-the-objects-that-need-to-be-convertedtranslated"></a>Q. Peut-il également analyser mon environnement et fournir un rapport d’évaluation de tous les objets qui doivent être convertis/traduits ?

R. Dans cette préversion de Synapse Pathway, vous devrez fournir le lien vers les scripts DDL/DML qui doivent être traduits. Synapse Pathway n’analyse pas votre environnement actuel pour identifier les objets qui doivent être traduits.

### <a name="q-what-information-does-azure-synapse-pathway-capture-about-my-current-data-warehouse-instance"></a>Q. Quelles sont les informations capturées par Azure Synapse Pathway sur mon instance actuelle de l’entrepôt de données ?

R. Comme vous pouvez exécuter Synapse Pathway dans votre environnement local, aucune donnée n’est capturée à l’exception de votre nom et de votre organisation, qui sont nécessaires quand vous téléchargez le fichier MSI à partir du Centre de téléchargement Microsoft.

### <a name="q-where-can-i-raise-issues-encountered-in-azure-synapse-pathway"></a>Q. Où puis-je signaler les problèmes rencontrés dans Azure Synapse Pathway ?

R. Azure Synapse Pathway est actuellement en **préversion**.   Le support pour Synapse Pathway est disponible via le canal de support Microsoft. Vous pouvez ouvrir le ticket dans le portail Azure ou dans les portails standard (généralement pour un support local).


> [!NOTE] 
> À l’instar de tous les autres services Azure, tous les services en préversion sont éligibles au support, simplement sans contrat SLA en place.

<!-- ### Troubleshooting and optimization

#### Q. Why do I see slow performance while running the code conversion?

#### Q. Translation of errors or unexpected results? -->

## <a name="next-steps"></a>Étapes suivantes

[Télécharger Azure Synapse Pathway](synapse-pathway-download.md)