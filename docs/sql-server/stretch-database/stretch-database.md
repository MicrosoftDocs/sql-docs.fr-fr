---
description: Stretch Database
title: Stretch Database
ms.date: 06/27/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database
ms.assetid: ce6db775-21a5-40bc-95a1-f560376d4ee2
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: c2d2acc7ba0ac0f57038b3a72f62e6b67383b1bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077390"
---
# <a name="stretch-database"></a>Stretch Database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Stretch Database migre vos données à froid dans le cloud Microsoft Azure de façon sécurisée et fluide.  
  
 Si vous souhaitez juste commencer à utiliser Stretch Database, consultez [Bien démarrer en exécutant l’Assistant Activer la base de données pour Stretch](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md).  
  
## <a name="what-are-the-benefits-of-stretch-database"></a>Quels sont les avantages de Stretch Database ?  
 Database Stretch offre les avantages suivants :  
  
 **Mise à disposition à bas coût des données à froid**  
 Étendez les données transactionnelles à chaud et à froid dynamiquement de SQL Server vers Microsoft Azure avec SQL Server Stretch Database. Vos données sont toujours en ligne et disponibles pour les requêtes, ce qui n’est pas le cas avec le stockage de données à froid habituel. Vous pouvez sans vous ruiner fournir des durées de conservation de données plus longues pour des tables volumineuses telles que l’historique des commandes client. Tirez parti du faible coût d’Azure plutôt que d’essayer de mettre à l’échelle un stockage local coûteux. Vous pouvez choisir le niveau de tarification et configurer les paramètres dans le portail Azure afin de garder le contrôle des prix et des coûts. Augmentation ou réduction d’échelle, selon le cas. Pour plus d’informations, consultez [Tarification de SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) .  
  
 **Aucune modification des requêtes ou des applications nécessaire**  
 Accédez facilement à vos données SQL Server, que celles-ci soient locales ou étendues au cloud.  Vous définissez la stratégie qui détermine l’endroit où les données sont stockées, et SQL Server gère le déplacement des données en arrière-plan. La table entière est toujours en ligne et peut toujours être interrogée. De plus, Stretch Database ne nécessite aucune modification des requêtes ou applications existantes : l’emplacement des données est totalement transparent pour l’application.  
  
 **Simplification de la maintenance locale des données**  
 Réduisez la maintenance et le stockage locaux de vos données. Les sauvegardes de vos données locales sur site s’exécutent plus vite et se terminent au sein de la fenêtre de maintenance. Les sauvegardes de la partie de vos données stockées dans le cloud sont exécutées automatiquement. Vos besoins en stockage local sont considérablement réduits. Le stockage Azure peut vous coûter 80 % de moins que l’extension du SSD local.  
  
 **Protection des données même pendant la migration**  
 Étant donné que vos applications les plus importantes sont étendues en toute sécurité vers le cloud, vous n’avez aucune inquiétude à vous faire. SQL Server Always Encrypted assure le chiffrement de vos données en mouvement. De plus, vous pouvez également utiliser la sécurité au niveau des lignes et d’autres fonctionnalités avancées de sécurité de SQL Server avec Stretch Database pour protéger vos données.  
  
## <a name="what-does-stretch-database-do"></a>Que fait la Stretch Database ?  
 Une fois que vous avez activé Stretch Database pour une instance SQL Server et une base de données, et sélectionné au moins une table, Stretch Database commence à migrer vos données à froid vers Azure en mode silencieux.  
  
-   Si vous stockez des données à froid dans une table distincte, vous pouvez migrer la table entière.  
  
-   Si votre table contient des données actuelles et anciennes, vous pouvez spécifier une fonction de filtre pour sélectionner les lignes à migrer.

**Vous n’avez pas besoin de modifier les requêtes et les applications clientes existantes.** Vous pouvez toujours accéder de façon transparente aux données locales et distantes, même pendant leur migration. Un certain degré de latence se produit pour les requêtes à distance, mais uniquement quand vous interrogez les données à froid.

**Stretch Database garantit qu’aucune donnée n’est perdue** en cas de défaillance pendant la migration. Elle est également dotée d’une logique de nouvelle tentative pour gérer les problèmes de connexion susceptibles de se produire pendant la migration. Une vue de gestion dynamique fournit l’état de la migration.

**Vous pouvez suspendre la migration des données** pour résoudre les problèmes sur le serveur local ou optimiser la bande passante réseau disponible.  
  
 ![Vue d’ensemble de Stretch Database](../../sql-server/stretch-database/media/stretch-overview.png "Vue d’ensemble de Stretch Database")  
  
## <a name="is-stretch-database-for-you"></a>Stretch Database, est-ce pour vous ?  
 Si vous répondez oui aux affirmations suivantes, Stretch Database peut vous aider à satisfaire vos besoins et à résoudre vos problèmes.  
  
|Si vous êtes décideur|Si vous êtes administrateur de bases de données|  
|--------------------------------|---------------------|  
|Je dois conserver les données transactionnelles longtemps.|Je n’arrive plus à gérer la taille de mes tables.|  
|Je dois parfois interroger les données à froid.|Mes utilisateurs disent qu’ils veulent pouvoir accéder aux données à froid, mais ils les utilisent rarement.|  
|Je dispose d’applications, y compris d’applications anciennes, que je ne souhaite pas mettre à jour.|Je dois constamment engager des fonds pour augmenter la capacité de stockage.|  
|Je souhaite trouver un moyen de faire des économies sur le stockage.|Je ne peux pas sauvegarder ni restaurer des tables d’un tel volume dans le cadre du contrat SLA.|  
  
## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>Quels types de bases de données ou de tables sont des candidats à Stretch Database ?  
 Stretch Database cible les bases de données transactionnelles comportant de grands volumes de données à froid généralement stockées dans un petit nombre de tables. Ces tables peuvent contenir plus d’un milliard de lignes.  
  
 Si vous vous servez de la fonctionnalité de table temporelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez utiliser Stretch Database pour migrer tout ou partie de la table de l’historique associée vers le stockage économique d’Azure. Pour plus d’informations, consultez [Gérer la rétention des données d’historique dans les tables temporelles avec version gérée par le système](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md).  
  
 Utilisez Stretch Database Advisor, fonctionnalité de conseiller de mise à niveau de SQL Server 2016 pour identifier des bases de données et des tables pour Stretch Database. Pour plus d’informations, consultez [Identifier des bases de données et tables pour Stretch Database en exécutant Stretch Database Advisor](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md). Pour en savoir plus sur les problèmes de blocages potentiels, consultez [Limitations concernant Stretch Database](../../sql-server/stretch-database/limitations-for-stretch-database.md).  

## <a name="test-drive-stretch-database"></a>Tester Stretch Database  
 **Tester Stretch Database à l’aide de l’exemple de base de données AdventureWorks.** Pour obtenir l’exemple de base de données AdventureWorks, téléchargez au minimum le fichier de base de données et le fichier d’exemples et de scripts [ici](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks). Après avoir restauré l’exemple de base de données dans une instance de SQL Server 2016, décompressez le fichier d’exemples et ouvrez le fichier d’exemples Stretch Database à partir du dossier Stretch DB. Exécutez les scripts de ce fichier pour vérifier l’espace utilisé par vos données avant et après l’activation de Stretch Database, pour suivre la progression de la migration des données et pour confirmer que vous pouvez continuer à interroger les données existantes et insérer de nouvelles données à la fois pendant et après la migration des données.  
  
## <a name="next-step"></a>Étape suivante  
 **Identifier les bases de données et les tables qui peuvent être utilisées avec Stretch Database.** Téléchargez l’Assistant Migration de données et exécutez une évaluation pour identifier les bases de données et les tables éligibles pour Stretch Database. Pour plus d’informations, consultez [Identifier des bases de données et tables pour Stretch Database en exécutant Stretch Database Advisor](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md).  
  
  
