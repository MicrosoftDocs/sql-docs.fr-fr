---
title: Grand World Importers-exemple de base de données pour SQL | Microsoft Docs
description: Découvrez comment les exemples de bases de données WideWorldImporters prennent en charge les flux de travail de la société fictive WideWorldImporters.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f844299a392f9521afe3af7d72bc72272304d3a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354040"
---
# <a name="wide-world-importers-sample-databases-for-microsoft-sql"></a>Exemples de bases de données larges World Importers pour Microsoft SQL
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Il s’agit d’une vue d’ensemble des importateurs de l’entreprise fictive et des flux de travail qui sont traités dans les exemples de bases de données WideWorldImporters pour SQL Server et Azure SQL Database.  

WWI (World World Importers) est un importateur de marchandises et un serveur de distribution de grande envergure opérant dans la région de la baie de San Francisco.

En tant que grossiste, les clients de WWI sont principalement des entreprises qui revendent à des individus. WWI vend aux détaillants sur le États-Unis, notamment les magasins de spécialité, les supermarchés, les magasins informatiques, les magasins d’attractions touristiques et certains individus. WWI se vend également à d’autres grossistes via un réseau d’agents qui promeuvent les produits au nom de WWI. Alors que tous les clients de WWI sont actuellement basés sur le États-Unis, l’entreprise a l’intention de pousser son développement dans d’autres pays.

WWI achète des marchandises auprès des fournisseurs, y compris les fabricants de fantaisie et de jouets, ainsi que d’autres grossistes de fantaisie. Ils stockent les marchandises dans leur entrepôt WWI et recommandent auprès des fournisseurs en fonction des besoins pour répondre aux commandes des clients. Ils achètent également de gros volumes de matériaux de Packaging et les vendent en quantités réduites pour des raisons de commodité pour les clients.

Récemment, WWI a commencé à vendre une variété de Novelties comestibles comme le chocolat à piment.  La société n’a jamais eu à gérer les éléments réfrigérés. À présent, pour répondre aux exigences de gestion des denrées alimentaires, celles-ci doivent surveiller la température dans leur salle de réfrigération et les camions qui ont des sections réfrigérants.

## <a name="workflow-for-warehouse-stock-items"></a>Flux de travail pour les éléments d’inventaire d’entrepôt

Le déroulement classique de l’inventaire et de la distribution des éléments est le suivant :
- WWI crée des bons de commande et envoie les commandes aux fournisseurs.
- Les fournisseurs envoient les articles, WWI les reçoivent et les stocke dans leur entrepôt.
- Les clients commandent des éléments à partir de WWI
- WWI remplit la commande du client avec des Articles de stock dans l’entrepôt, et lorsqu’ils ne disposent pas de stock suffisant, ils ordonnent les actions supplémentaires des fournisseurs.
- Certains clients ne souhaitent pas attendre les articles qui ne sont pas en stock. S’ils indiquent cinq éléments boursiers différents et que quatre sont disponibles, ils souhaitent recevoir les quatre articles et différer l’élément restant. L’élément est ensuite envoyé plus tard dans une livraison distincte.
- WWI facture les clients pour les articles boursiers, en général en convertissant la commande en facture.
- Les clients peuvent commander des articles qui ne sont pas en stock. Ces éléments sont en souffrance.
- WWI fournit des éléments de stock aux clients par le biais de leurs propres camionnettes, ou par le biais d’autres méthodes de transport ou de courrier.
- Les clients paient des factures sur WWI.
- Périodiquement, WWI paie des fournisseurs pour les articles qui étaient sur les bons de commande. C’est souvent le cas lorsqu’ils ont reçu les marchandises.

## <a name="data-warehouse-and-analysis-workflow"></a>Flux de travail de Data Warehouse et d’analyse

Alors que l’équipe de WWI utilise SQL Server Reporting Services pour générer des rapports opérationnels à partir de la base de données WideWorldImporters, elle doit également effectuer des analyses sur leurs données et avoir besoin de générer des rapports stratégiques. L’équipe a créé un modèle de données dimensionnelles dans une base de données WideWorldImportersDW. Cette base de données est remplie par un package Integration Services.

SQL Server Analysis Services est utilisé pour créer des modèles de données analytiques à partir des données du modèle de données dimensionnelles. SQL Server Reporting Services est utilisé pour générer des rapports stratégiques directement à partir du modèle de données dimensionnelles, et également à partir du modèle analytique. Power BI est utilisé pour créer des tableaux de bord à partir des mêmes données. Les tableaux de bord sont utilisés sur les sites Web, ainsi que sur les téléphones et les tablettes. *Remarque : ces modèles de données et rapports ne sont pas encore disponibles*

## <a name="additional-workflows"></a>Flux de travail supplémentaires

Il s’agit de flux de travail supplémentaires.
- WWI émet des notes de crédit quand un client ne reçoit pas le bon pour une raison quelconque, ou lorsque les marchandises sont défectueuses. Celles-ci sont traitées comme des factures négatives.
- WWI compte régulièrement les quantités disponibles d’articles en stock pour s’assurer que les quantités de stock indiquées comme disponibles sur leur système sont exactes. (Le processus de cette opération est appelé Stocktake).
- Températures de la chambre froide. Les marchandises périssables sont stockées dans des salles réfrigérées. Les données de capteur de ces salles sont ingérées dans la base de données à des fins de surveillance et d’analyse.
- Suivi de l’emplacement des véhicules. Les véhicules qui transportent des marchandises pour WWI incluent des capteurs qui suivent l’emplacement. Cet emplacement est de nouveau ingéré dans la base de données à des fins de surveillance et d’analyse.

## <a name="fiscal-year"></a>Année fiscale

La société travaille avec un exercice qui commence le 1er novembre.

## <a name="terms-of-use"></a>Conditions d’utilisation

La licence de l’exemple de base de données et de l’exemple de code est décrite ici : [license.txt](https://github.com/Microsoft/sql-server-samples/blob/master/license.txt)

L’exemple de base de données inclut des données publiques qui ont été chargées à partir de data.gov et de EarthData naturel. Les conditions d’utilisation sont les suivantes : [https://www.naturalearthdata.com/about/terms-of-use/](https://www.naturalearthdata.com/about/terms-of-use/)
