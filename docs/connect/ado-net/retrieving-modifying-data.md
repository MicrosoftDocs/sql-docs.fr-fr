---
title: Extraction et modification de données
description: Dans .NET, le fournisseur de données Microsoft SqlClient pour SQL Server sert de pont entre une application et une source de données pour lire et mettre à jour des données.
ms.date: 11/30/2020
ms.assetid: 722e7f87-3691-46c6-87e8-7d159722d675
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 4275b7de0f31d03aa36ef31d8801fcdc0e9ec853
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771538"
---
# <a name="retrieving-and-modifying-data-in-adonet"></a>Extraction et modification de données dans ADO.NET

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Une fonction principale de toute application de base de données consiste à se connecter à une source de données et extraire les données qu'elle contient. Le fournisseur de données SqlClient fait office de passerelle entre une application et une source de données, ce qui vous permet d'exécuter des commandes et d'extraire des données à l'aide d'un **DataReader** ou d'un **DataAdapter**. Une fonction clé de toute application de base de données est la capacité à mettre à jour les données stockées dans la base de données. Dans le Fournisseur de données Microsoft SqlClient pour SQL Server, la mise à jour de données implique l’utilisation des objets **DataAdapter** et <xref:System.Data.DataSet> et **Command** ; cela peut également impliquer l’utilisation de transactions.

## <a name="in-this-section"></a>Dans cette section

[Connexion à une source de données](connecting-to-data-source.md)  
Décrit comment établir une connexion à une source de données et comment travailler avec des événements de connexion.

[Chaînes de connexion](connection-strings.md)  
Contient des rubriques qui décrivent divers aspects de l'utilisation de chaînes de connexion, y compris des mots clés de chaîne de connexion, des informations de sécurité, ainsi que de leur stockage et leur extraction.

[Regroupement de connexions](connection-pooling.md)  
Décrit le regroupement de connexions pour le Fournisseur de données Microsoft SqlClient pour SQL Server.

[Commandes et paramètres](commands-parameters.md)  
Contient des rubriques qui décrivent comment créer des commandes et des générateurs de commande, configurer des paramètres et exécuter des commandes pour extraire et modifier des données.

[DataAdapters et DataReaders](dataadapters-datareaders.md)  
Contient des rubriques qui décrivent les objets DataReader et DataAdapter, les paramètres, la gestion des événements DataAdapter et l'exécution d'opérations par lots.

[Transactions et accès simultané](transactions-and-concurrency.md)  
Contient des rubriques qui décrivent comment effectuer des transactions locales, des transactions distribuées et travailler avec l’accès concurrentiel optimiste.

[Extraction des informations de schéma de base de données](retrieving-database-schema-information.md)  
Décrit la manière d'obtenir des bases de données ou des catalogues disponibles, des tables et des vues dans une base de données, des contraintes existantes pour des tables et d'autres informations de schéma à partir d'une source de données.

[DbProviderFactories](dbproviderfactories.md)  
Décrit le modèle fabrique de fournisseurs et illustre l'utilisation des classes de base dans l'espace de noms `System.Data.Common`.  

[Extraction de l’identité ou de valeurs à numérotation automatique](retrieve-identity-or-autonumber-values.md)  
Propose un exemple de mappage des valeurs générées pour une colonne **identity** d’une table SQL Server à une colonne d’une ligne insérée dans une table. Traite de la fusion de valeurs d'identité dans un `DataTable`.  
  
[Extraction de données binaires](retrieve-binary-data.md)  
Décrit comment extraire des données binaires ou de grosses structures de données à l’aide de `CommandBehavior`.`SequentialAccess` pour modifier le comportement par défaut d’un `DataReader`.  
  
[Modification de données avec des procédures stockées](modify-data-with-stored-procedures.md)  
Décrit comment utiliser des paramètres d'entrée et sortie de procédure stockée afin d'insérer une ligne dans une base de données et retourner une nouvelle valeur d'identité.  

[Suivi de données dans SqlClient](data-tracing.md)  
Décrit comment le fournisseur de données Microsoft SqlClient pour SQL Server fournit des fonctionnalités de traçage de données intégrées.  
  
[Compteurs de performances dans SqlClient](performance-counters.md)  
Décrit les compteurs de performances disponibles pour le fournisseur de données Microsoft SqlClient pour SQL Server.  
  
[Programmation asynchrone](asynchronous-programming.md)  
Décrit le fournisseur de données Microsoft SqlClient pour la prise en charge de la programmation asynchrone par SQL Server.  
  
[Prise en charge de la diffusion en continu SqlClient](sqlclient-streaming-support.md)  
Explique comment écrire des applications qui diffusent en continu les données provenant de SQL Server sans avoir à les charger complètement en mémoire.  

## <a name="see-also"></a>Voir aussi

- [Mappages des types de données dans ADO.NET](data-type-mappings-ado-net.md)
- [SQL Server et ADO.NET](./sql/index.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
