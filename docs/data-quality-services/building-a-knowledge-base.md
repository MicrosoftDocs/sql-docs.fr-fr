---
description: Construction d'une base de connaissances
title: Construction d'une base de connaissances
ms.date: 07/31/2012
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
ms.assetid: 51eff161-6ecd-4ee4-8187-1dd8ef4814bd
author: swinarko
ms.author: sawinark
ms.openlocfilehash: e5914d97a8110f9b50ecfd20da7130c564aac840
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88449973"
---
# <a name="building-a-knowledge-base"></a>Construction d'une base de connaissances

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sqlserver.md)]

  Une base de connaissances dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) est un référentiel de connaissances sur vos données qui vous permet de comprendre vos données et de préserver leur intégrité. Une base de connaissances comprend des domaines, chacun représentant les données dans un champ de données. La base de connaissances est utilisée par DQS pour effectuer le nettoyage et la duplication de données sur une base de données. Pour préparer la base de connaissances au nettoyage de données, vous pouvez exécuter une analyse assistée par ordinateur d'un exemple de données, et gérer de manière interactive les valeurs dans les domaines. DQS vous permet d'importer des connaissances, de créer des règles et des relations, de modifier directement les valeurs des données et d'exploiter une base de données par défaut.  
  
## <a name="in-this-section"></a>Dans cette section  
 Vous pouvez effectuer les opérations suivantes sur une base de connaissances :  
  
|Description de l’opération|Rubrique|  
|-|-|  
|Créer une nouvelle base de connaissances en partant de zéro, d'une base de connaissances existante ou d'un fichier de données .dqs.|[Créer une base de connaissances](../data-quality-services/create-a-knowledge-base.md)|  
|Ouvrir une base de connaissances existante afin d'effectuer une découverte des connaissances ou une gestion des domaines, ou d'ajouter une politique de correspondance.|[Ouvrir une base de connaissances](../data-quality-services/open-a-knowledge-base.md)|  
|Effectuez les actions de gestion sur une base de connaissances, comme l'ouvrir, la déverrouiller, ignorer votre travail, la renommer, la supprimer ou afficher ses propriétés.|[Gérer une base de connaissances](../data-quality-services/manage-a-knowledge-base.md)|  
|Ajouter des connaissances à une base de connaissances via la découverte des connaissances, la gestion des valeurs de domaine, l'ajout d'une stratégie de correspondance, l'importation de connaissances, de domaines ou de valeurs, ou l'utilisation de la base de connaissances par défaut, DQS Data.|[Ajout de connaissances à une base de connaissances](../data-quality-services/adding-knowledge-to-a-knowledge-base.md)|  
|Analyser un exemple de données pour les critères de qualité des données.|[Effectuer une découverte des connaissances](../data-quality-services/perform-knowledge-discovery.md)|  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Importation ou exportation de la connaissance depuis ou vers une base de connaissances.|[Importation et exportation de connaissances](../data-quality-services/importing-and-exporting-knowledge.md)|  
|Création d'un seul domaine et ajout de connaissances au domaine.|[Gestion d'un domaine](../data-quality-services/managing-a-domain.md)|  
|Création d'un domaine composite et ajout de connaissances au domaine.|[Gestion d'un domaine composite](../data-quality-services/managing-a-composite-domain.md)|  
  
  
