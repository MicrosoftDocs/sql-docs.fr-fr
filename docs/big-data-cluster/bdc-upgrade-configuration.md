---
title: Effectuer une mise à niveau vers un cluster Big Data avec gestion de la configuration
titleSuffix: SQL Server big data clusters
description: Effectuer une mise à niveau vers un cluster Big Data avec gestion de la configuration
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 0a9f6addcf73e0c50d67f75f19fedd51b2f3dbc9
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100344009"
---
# <a name="upgrading-to-a-configuration-management-enabled-cluster-cu8-or-lower-to-cu9"></a>Mise à niveau vers un cluster avec gestion de la configuration (CU8 ou version inférieure vers CU9+)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
À partir de CU9, les clusters Big Data incluent une fonctionnalité de gestion de la configuration qui permet aux administrateurs de modifier ou d’ajuster diverses parties des clusters Big Data après leur déploiement afin d’obtenir des insights plus approfondis sur leurs configurations en cours d’exécution. Avant CU9, les configurations de clusters Big Data étaient généralement modifiables uniquement au moment du déploiement et une solution de contournement permettait de définir certaines configurations SQL par le biais d’un fichier `mssql-custom.conf` personnalisé. Cette solution de contournement est à présent résolue puisque ces paramètres sont configurables par le biais de la fonctionnalité de gestion de la configuration.

## <a name="migrating-sql-configurations-in-mssql-customconf-to-the-configuration-management-system"></a>Migration des configurations SQL dans mssql-custom.conf vers le système de gestion de la configuration
Si vous avez créé un fichier `mssql-custom.conf` personnalisé pour vos instances maîtres de SQL Server, suivez les instructions ponctuelles ci-dessous pour gérer les paramètres par le biais du système de configuration, et non du fichier. Si vous ne suivez pas ces étapes, la fonctionnalité de gestion de la configuration ne va pas gérer ces configurations SQL et les paramètres `mssql-custom.conf` remplaceront toutes les modifications apportées à ces paramètres par le biais de la fonctionnalité de gestion de la configuration.

Étapes :
1. Mettre à niveau le cluster Big Data vers CU9
> [!NOTE]
> Les paramètres définis par le biais du fichier `mssql-custom.conf` ne sont pas modifiés ni supprimés. Ils ne sont tout simplement pas reflétés ni gérés par le framework de configuration.

2. Définissez et appliquez tous les paramètres précédemment définis dans le fichier `mssql-custom.conf` à l’aide de la nouvelle fonctionnalité de configuration. Consultez [Vue d’ensemble de la configuration post-déploiement de clusters Big Data SQL Server](configure-bdc-postdeployment.md) pour obtenir des instructions pas à pas sur la modification des paramètres. Consultez [Propriétés de configuration des clusters Big Data SQL Server](reference-config-bdc-overview.md) pour obtenir la liste complète des paramètres disponibles pour chaque étendue. Notez que certains paramètres comme customerFeedback ont peut-être changé d’étendue, mais restent disponibles.
3. Renommez le fichier `mssql-custom.conf` `deprecated-mssql-custom.conf` dans le conteneur `mssql-server` de chaque pod maître. Si vous avez un seul maître, `master-0`. Si un passage à une version antérieure ou une restauration d’un cluster sans gestion de la configuration (CU8 ou version inférieure) est nécessaire, vous pouvez réutiliser ce fichier pour appliquer ces configurations SQL personnalisées.

## <a name="downgrading-from-a-configuration-management-enabled-cluster-to-a-non-configuration-management-enabled-cluster-cu9-to-cu8-or-lower"></a>Passage à une version antérieure depuis un cluster avec gestion de la configuration vers un cluster sans gestion de la configuration (CU9+ vers CU8 ou version antérieure)
Le passage à une version antérieure depuis un cluster avec gestion de la configuration (CU9+) vers un cluster sans gestion de la configuration (CU8 ou version antérieure) va supprimer la possibilité d’ajuster le cluster Big Data après le déploiement. Il va aussi nécessiter l’utilisation du fichier `mssql-custom.conf` facultatif pour définir les configurations SQL. Si vous avez renommé `deprecated-mssql-custom.conf` ce fichier au moment de la mise à niveau vers CU9+, renommez-le `mssql-custom.conf`. Si vous avez supprimé le fichier ou si vous ne l’aviez pas créé auparavant et que vous avez maintenant besoin de définir ces configurations SQL spéciales, créez-le en suivant ces instructions : [Propriétés de configuration de l’instance maître SQL Server - Version antérieure à CU9](reference-config-master-instance.md). Tous les paramètres définis et modifiés par le biais de l’expérience de gestion de la configuration seront rétablis à vos configurations précédentes ou aux valeurs système par défaut. 

Une fois le cluster passé à une version antérieure, les paramètres sont rétablis à leurs valeurs par défaut ou aux valeurs spécifiées dans le fichier bdc.json du déploiement. Aucune autre étape n’est nécessaire après le passage à une version antérieure.