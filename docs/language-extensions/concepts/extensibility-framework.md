---
title: Architecture d’extensibilité dans les extensions de langage SQL Server
titleSuffix: ''
description: Découvrez des informations sur l’architecture d’extensibilité utilisée pour les extensions de langage SQL Server, qui vous permet d’exécuter du code externe dans SQL Server. Dans SQL Server 2019, Java est pris en charge. Le code s’exécute dans un environnement d’exécution de langage en tant qu’extension du moteur de base de données principal.
author: dphansen
ms.author: davidph
ms.date: 11/05/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 51780bbb0184bdd950e36eef45877da576cd2576
ms.sourcegitcommit: 346a37242f889d76cd783f55aeed98023c693610
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91765687"
---
# <a name="extensibility-architecture-in-sql-server-language-extensions"></a>Architecture d’extensibilité dans les extensions de langage SQL Server

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Découvrez des informations sur l’architecture d’extensibilité utilisée pour les extensions de langage SQL Server, qui vous permet d’exécuter du code externe dans SQL Server. Dans SQL Server 2019, Java est pris en charge. Le code s’exécute dans un environnement d’exécution de langage en tant qu’extension du moteur de base de données principal.

## <a name="background"></a>Arrière-plan

Le framework d’extensibilité a pour objectif de fournir une interface entre SQL Server et des langages externes tels que Java. En exécutant un langage approuvé dans un framework sécurisé géré par SQL Server, les administrateurs de base de données peuvent préserver la sécurité tout en permettant aux scientifiques des données d’accéder aux données de l’entreprise.

<!-- We need to get a diagram like the one below.
The following diagram visually describes opportunities and benefits of the extensible architecture.

  ![Goals of integration with SQL Server](../media/ml-service-value-add.png "Machine Learning Services Value Add")
-->

Vous pouvez exécuter tous les langages externes pris en charge en appelant une procédure stockée. Les résultats sont retournés sous forme de résultats tabulaires directement à SQL Server. Cela facilite l’utilisation du langage externe à partir de n’importe quelle application pouvant envoyer une requête SQL et gérer les résultats.

## <a name="architecture-diagrams"></a>Diagrammes d’architecture

L’architecture est conçue pour que le code externe s’exécute dans un processus distinct de SQL Server, mais avec des composants qui gèrent de manière interne la chaîne des requêtes relatives aux données et aux opérations sur SQL Server. 
  
  ***Architecture des composants dans Windows :***

  ![Architecture des composants sur Windows](../media/generic-architecture-windows.png "Architecture des composants sur Windows")
  
  ***Architecture des composants dans Linux :***
  
  ![Architecture des composants sur Linux](../media/generic-architecture-linux.png "Architecture des composants sur Windows/Linux")
  
Les composants incluent un service **Launchpad**, qui permet d’appeler les runtimes externes (par exemple Java), et une logique spécifique à la bibliothèque pour le chargement des interpréteurs et des bibliothèques.

<a name="launchpad"></a>

## <a name="launchpad"></a>Launchpad

[!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] est un service qui gère la durée de vie, les ressources et les limites de sécurité du processus externe responsable de l’exécution des scripts. Cela ressemble à la façon dont le service de requête et d’indexation de recherche en texte intégral lance un hôte distinct pour le traitement des requêtes de texte intégral. Le service Launchpad peut démarrer uniquement les lanceurs approuvés qui sont publiés par Microsoft, ou qui sont certifiés par Microsoft comme étant conformes aux exigences de gestion des performances et des ressources.

| Lanceurs approuvés | Extension | Versions de SQL Server |
|-------------------|-----------|---------------------|
| JavaLauncher.dll pour Java | Extension Java | SQL Server 2019 |

Le service [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] s’exécute sous **SQLRUserGroup**, qui utilise [AppContainers](/windows/desktop/secauthz/appcontainer-isolation) pour l’isolation d’exécution.

Un service [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] distinct est créé pour chaque instance de moteur de base de données à laquelle vous avez ajouté des extensions de langage machine SQL Server. Il existe un service Launchpad pour chaque instance de moteur de base de données. Ainsi, si vous avez plusieurs instances prenant en charge des scripts externes, vous disposez d’un service Launchpad pour chacune d’entre elles. Une instance de moteur de base de données est liée au service Launchpad créé pour celle-ci. À chaque appel de script externe dans une procédure stockée ou dans du code T-SQL, le service SQL Server appelle le service Launchpad créé pour la même instance.

Pour exécuter des tâches dans un langage spécifique pris en charge, le service Launchpad récupère un compte Worker sécurisé à partir du pool, puis démarre un processus satellite pour gérer le runtime externe. Chaque processus satellite hérite du compte d’utilisateur du Launchpad et utilise ce compte Worker pour toute la durée de l’exécution du script. Si le script utilise des processus parallèles, ils sont créés sous le même compte Worker unique.

## <a name="communication-channels-between-components"></a>Canaux de communication entre les composants

Les protocoles de communication entre les composants et les plateformes de données sont décrits dans cette section.

+ **TCP/IP**

  Par défaut, les communications internes entre SQL Server et SQL Satellite utilisent TCP/IP.

+ **ODBC**

  Les communications entre les clients de science des données externes et une instance de SQL Server distante utilisent ODBC. Le compte qui envoie les travaux de script à SQL Server doit avoir l’autorisation de se connecter à l’instance et l’autorisation d’exécuter des scripts externes.

  De plus, en fonction de la tâche, le compte peut avoir besoin des autorisations suivantes :

  + Lire les données utilisées par le travail
  + Écrire les données dans des tables : par exemple, durant l’enregistrement des résultats dans une table
  + Créer des objets de base de données : par exemple, si vous enregistrez un script externe dans le cadre d’une nouvelle procédure stockée

  Quand SQL Server sert de contexte de calcul pour un script exécuté à partir d’un client distant et que l’exécutable doit récupérer des données à partir d’une source externe, ODBC est utilisé pour l’écriture différée. SQL Server mappe l’identité de l’utilisateur qui émet la commande distante à l’identité de l’utilisateur sur l’instance actuelle, puis exécute la commande ODBC à l’aide des informations d’identification de cet utilisateur. La chaîne de connexion nécessaire pour effectuer cet appel ODBC est obtenue à partir du code client.

+ **Autres protocoles**

  Les processus qui peuvent avoir besoin de travailler dans des « blocs » ou de transférer des données en retour à un client distant peuvent aussi utiliser le [format de fichier XDF](/machine-learning-server/r/concept-what-is-xdf). Le transfert de données proprement dit s’effectue via des objets blob encodés.

## <a name="next-steps"></a>Étapes suivantes

+ [Que sont les extensions de langage ?](../language-extensions-overview.md)