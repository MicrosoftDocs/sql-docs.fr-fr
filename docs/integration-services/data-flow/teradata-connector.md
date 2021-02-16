---
description: Connecteur Microsoft pour Teradata
title: Connecteur Microsoft pour Teradata | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5f82833ec5498c107155a5284d7a419ba6289239
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346543"
---
# <a name="microsoft-connector-for-teradata"></a>Connecteur Microsoft pour Teradata

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Le connecteur Microsoft pour Teradata permet d’exporter des données à partir de bases de données Teradata et de charger des données dans ces bases dans un package SSIS.

Ce nouveau connecteur prend en charge les bases de données avec des tables de 1 Mo.

## <a name="version-support"></a>Prise en charge de la version

Les produits Microsoft SQL Server suivants sont pris en charge par le connecteur Microsoft pour Teradata :

- Microsoft SQL Server 2019
- Microsoft SQL Server Data Tools (SSDT) 15.8.1 ou ultérieure pour Visual Studio 2017
- Microsoft SQL Server Data Tools (SSDT) pour Visual Studio 2019

Le connecteur Microsoft pour Teradata utilise l’interface de langage de programmation d’application Teradata Parallel Transporter pour charger des données dans la base de données Teradata et exporter des données de cette base. Les versions suivantes sont prises en charge :

- Teradata Parallel Transporter (Teradata PT) 16.10
- Teradata Parallel Transporter (Teradata PT) 16.20

Les versions de base de données Teradata suivantes sont prises en charge pour la source de données :

- Base de données Teradata 16.20
- Base de données Teradata 16.10
- Base de données Teradata 15.10
- Base de données Teradata 15.00

Reportez-vous à la [documentation Teradata](https://docs.teradata.com/) pour consulter le guide du programmeur et obtenir plus d’informations sur l’interface de programmation d’application Teradata Parallel Transporter.

## <a name="installation"></a>Installation

Sur les ordinateurs 32 bits, installez les pilotes suivants à partir de la page [Teradata Tools and Utilities - Windows Installation Package](https://downloads.teradata.com/download/tools/teradata-tools-and-utilities-windows-installation-package) (Outils et utilitaires Teradata - package d’installation Windows) :

- Pilote ODBC Teradata (32 bits)
- API Teradata PT (32 bits)

Sur les ordinateurs 64 bits, installez les pilotes suivants :

- Pilote ODBC Teradata (64 bits)
- API Teradata PT (64 bits)

Pour installer le connecteur pour base de données Teradata, téléchargez et exécutez le programme d’installation de la [dernière version du connecteur Microsoft pour Teradata](https://www.microsoft.com/download/details.aspx?id=100599). Suivez les instructions dans l'assistant d'installation.

Après avoir installé le connecteur, vous devez redémarrer le service d’intégration SQL Server pour vous assurer que la source et la destination Teradata fonctionnent correctement.

## <a name="design-and-execute-ssis-packages"></a>Concevoir et exécuter des packages SSIS

Le connecteur Microsoft pour Teradata offre une expérience utilisateur similaire avec le connecteur Attunity Teradata. L’utilisateur peut concevoir de nouveaux packages en fonction de l’expérience précédente, à l’aide de SSDT pour VS 2017 ou VS 2019, avec *ciblant SQL Server 2019*.

La source et la destination Teradata se trouvent sous la catégorie Common.

![Le composant Teradata](media/teradata-component.png)

Le gestionnaire de connexions Teradata est affiché en tant que « TERADATA ».

![Le type de gestionnaire de connexions Teradata](media/teradata-connection-manager-type.png)

Les packages SSIS existants qui ont été conçus avec le connecteur Attunity Teradata seront automatiquement mis à niveau pour utiliser le connecteur Microsoft pour Teradata. Les icônes seront également modifiées.

Pour exécuter un package SSIS *ciblant SQL Server 2017 (et les versions antérieures)* , vous devez installer le **connecteur Microsoft pour Teradata par Attunity** avec la version correspondante à partir du lien suivant :

- [SQL Server 2017 : Connecteur Microsoft version 5.0 pour Teradata par Attunity](https://www.microsoft.com/download/details.aspx?id=55179)
- [SQL Server 2016 : Connecteur Microsoft version 4.0 pour Teradata par Attunity](https://www.microsoft.com/download/details.aspx?id=52950)
- [SQL Server 2014 : Connecteur Microsoft version 3.0 pour Teradata par Attunity](https://www.microsoft.com/download/details.aspx?id=44582)
- [SQL Server 2012 : Connecteur Microsoft version 2.0 pour Teradata par Attunity](https://www.microsoft.com/download/details.aspx?id=29283)

Pour concevoir un package SSIS dans SSDT *ciblant SQL Server 2017 (et les versions antérieures)* , vous devez installer le **connecteur Microsoft pour Teradata** et le **connecteur Microsoft pour Teradata par Attunity** avec la version correspondante.

## <a name="limitations-and-known-issues"></a>Limitations et problèmes connus

- Éditeur de la source/destination Teradata, la propriété de la **base de données par défaut** ne prend pas effet.  Pour contourner le problème, tapez le nom de la base de données dans la zone déroulante pour filtrer la table ou la vue.

- Éditeur de la source/destination Teradata, l’étape de mappage ne fonctionne pas quand vous tapez \<database>.<table/vue>. Pour contourner le problème, tapez \<database>.<table/vue>, puis cliquez sur le bouton déroulant.

- Éditeur de source Teradata, la vue ne peut pas être affichée lorsque le mode d’accès aux données est « Nom de la table – Exportation TPT ». Pour contourner le problème, utilisez l’Éditeur avancé de la source Teradata.

- Destination Teradata, l’attribut ’PackMaximum’ ne peut pas être défini sur ’True'.  Sinon, une erreur se produit.

## <a name="uninstallation"></a>Désinstallation

Vous pouvez exécuter l’Assistant de désinstallation pour supprimer le **connecteur Microsoft pour Teradata**.

## <a name="next-steps"></a>Étapes suivantes

- Configurer le [gestionnaire de connexions Teradata](teradata-connection-manager.md)
- Configurer la [source Teradata](teradata-source.md)
- Configurer la [destination Teradata](teradata-destination.md)
- Si vous avez des questions, rendez-vous sur [Tech Community](https://aka.ms/AA6iwdw).
