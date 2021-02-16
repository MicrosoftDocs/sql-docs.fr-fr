---
description: Connecteur Microsoft pour Oracle
title: Connecteur Microsoft pour Oracle | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 70acf129fa608c1058e808d2193cf4c901198d6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343222"
---
# <a name="microsoft-connector-for-oracle"></a>Connecteur Microsoft pour Oracle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Le connecteur Microsoft pour Oracle offre la possibilité d’exporter et de charger des données dans une source de données Oracle dans un package SSIS.

## <a name="version-support"></a>Prise en charge de la version

Les produits Microsoft SQL Server suivants sont pris en charge par le connecteur Microsoft pour Oracle :

- À compter de SQL Server 2019 CU1
- SQL Server Data Tools (SSDT) 15.9.3 ou ultérieure pour Visual Studio 2017
- Microsoft SQL Server Data Tools (SSDT) pour Visual Studio 2019

Les versions de base de données Oracle suivantes sont prises en charge pour la source de données :

- Oracle 10.x
- Oracle 11.x
- Oracle 12c
- Oracle 18c (sans prise en charge de l’authentification Windows)
- Oracle 19c (sans prise en charge de l’authentification Windows)

La base de données Oracle est prise en charge sur tous les systèmes d’exploitation et plateformes.
> [!NOTE]
>
> Le client Oracle n’est pas nécessaire pour le connecteur Microsoft pour base de données Oracle dans SQL Server 2019.

## <a name="installation"></a>Installation

Pour installer le connecteur pour base de données Oracle, téléchargez et exécutez le programme d’installation de la [dernière version du connecteur Microsoft pour Oracle](https://www.microsoft.com/download/details.aspx?id=58228). Suivez les instructions dans l'assistant d'installation.

Après avoir installé le connecteur, vous devez redémarrer le service d’intégration SQL Server pour vous assurer que la source et la destination Oracle peuvent fonctionnent correctement.

Pour exécuter un package SSIS ciblant SQL Server 2017 (et les versions antérieures), vous devez non seulement installer le **connecteur Microsoft pour Oracle**, mais également le **client Oracle** et le **connecteur Microsoft pour Oracle par Attunity** avec la version correspondante à partir des liens suivants :

- [SQL Server 2017 : Connecteur Microsoft Version 5.0 pour Oracle par Attunity](https://www.microsoft.com/download/details.aspx?id=55179)
- [SQL Server 2016 : Connecteur Microsoft Version 4.0 pour Oracle par Attunity](https://www.microsoft.com/download/details.aspx?id=52950)
- [SQL Server 2014 : Connecteur Microsoft Version 3.0 pour Oracle par Attunity](https://www.microsoft.com/download/details.aspx?id=44582)
- [SQL Server 2012 : Connecteur Microsoft Version 2.0 pour Oracle par Attunity](https://www.microsoft.com/download/details.aspx?id=29283)

## <a name="limitations-and-known-issues"></a>Limitations et problèmes connus

- Les vues ne sont pas répertoriées sous la source Oracle *Nom de la table ou de la vue*. Pour contourner le problème, utilisez la commande SQL et effectuez une sélection * à partir de la vue, ou définissez le nom de la vue sur la propriété [Oracle Source].[TableName] dans Éditeur avancé.

## <a name="uninstallation"></a>Désinstallation

Vous pouvez exécuter l’Assistant de désinstallation pour supprimer le connecteur Microsoft pour base de données Oracle de SQL Server.

## <a name="next-steps"></a>Étapes suivantes

- Configurer le [Gestionnaire de connexions Oracle](oracle-connection-manager.md).
- Configurer la [Source Oracle](oracle-source.md).
- Configurer la [Destination Oracle](oracle-destination.md).
- Si vous avez des questions, rendez-vous sur [TechCommunity](https://aka.ms/AA5u35j).
