---
title: Surveiller les scripts avec des rapports personnalisés
description: Utilisez des rapports personnalisés dans SQL Server Management Studio (SSMS) pour surveiller l’exécution de scripts externes (Python et R), les ressources utilisées, diagnostiquer les problèmes et optimiser les performances dans SQL Server Machine Learning Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e0c3874177691832bd6fa66f81c3fe94c726ff76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337077"
---
# <a name="monitor-python-and-r-script-execution-using-custom-reports-in-sql-server-management-studio"></a>Surveiller l’exécution de scripts Python et R à l’aide de rapports personnalisés dans SQL Server Management Studio
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Utilisez des rapports personnalisés dans [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) pour superviser l’exécution de scripts externes (Python et R), les ressources utilisées, diagnostiquer les problèmes et optimiser les performances dans [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).

Dans ces rapports, vous pouvez afficher les informations suivantes :

- Sessions Python ou R actives
- Paramètres de configuration de l’instance.
- Statistiques d’exécution pour les travaux de Machine Learning
- Événements étendus pour R Services
- Packages Python ou R installés sur l’instance actuelle

Cet article explique comment installer et utiliser les rapports personnalisés fournis pour SQL Server Machine Learning Services.

Pour plus d’informations sur les rapports dans SQL Server Management Studio, consultez [Rapports personnalisés dans Management Studio](../../ssms/object/custom-reports-in-management-studio.md).

## <a name="how-to-install-the-reports"></a>Comment installer les rapports

Les rapports sont conçus à l’aide de SQL Server Reporting Services, mais il peuvent être utilisés directement à partir de SQL Server Management Studio. Reporting Services n’a pas besoin d’être installé sur votre instance SQL Server.

Pour utiliser ces rapports, procédez comme suit :

1. Téléchargez les [rapports personnalisés SSMS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/machine-learning-services/ssms-custom-reports) pour SQL Server Machine Learning Services à partir de GitHub.

   ::: moniker range="=azuresqldb-mi-current"
   >[!NOTE]
   > Le rapport personnalisé **ML services - Configurer l’instance** n’est pas pris en charge sur Azure SQL Managed Instance.
   ::: moniker-end

2. Copier les rapports dans Management Studio

    1. Recherchez le dossier des rapports personnalisés utilisé par SQL Server Management Studio. Par défaut, les rapports personnalisés sont stockés dans ce dossier (où **user_name** est votre nom d’utilisateur Windows) :

        `C:\Users\user_name\Documents\SQL Server Management Studio\Custom Reports`

       Vous pouvez spécifier un autre dossier, ou créer des sous-dossiers.

    2. Copiez les fichiers *.RDL téléchargés dans le dossier des rapports personnalisés.

3. Exécutez les rapports dans Management Studio

    1. Dans Management Studio, cliquez avec le bouton droit sur le nœud **Bases de données** de l’instance où vous souhaitez exécuter les rapports.

    2. Cliquez sur **Rapports**, puis sur **Rapports personnalisés**.

    3. Dans la boîte de dialogue **Ouvrir un fichier** , recherchez le dossier des rapports personnalisés.

    4. Sélectionnez l’un des fichiers RDL que vous avez téléchargés, puis cliquez sur **Ouvrir**.

## <a name="reports"></a>Rapports

Le [référentiel de rapports personnalisés SSMS dans GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/machine-learning-services/ssms-custom-reports) comprend les rapports suivants :

| Rapport | Description |
|-|-|
| Sessions actives | Utilisateurs actuellement connectés à l’instance SQL Server et exécutant un script Python ou R. |
| Configuration | Paramètres d’installation de Machine Learning Services et propriétés du runtime Python ou R. |
| Configurer une instance | Configure Machine Learning Services. |
| Statistiques d’exécution | Statistiques d’exécution de Machine Learning Services. Par exemple, vous pouvez obtenir le nombre total d’exécutions de scripts externes et le nombre d’exécutions en parallèle. |
| Événements étendus | Événements étendus disponibles pour obtenir plus d’informations sur l’exécution de scripts externes. |
| . | Répertorie les packages R ou Python installés sur l’instance SQL Server et leurs propriétés, telles que la version et le nom. |
| Utilisation des ressources | Affiche des informations relatives à l’utilisation du processeur, de la mémoire, des E/S de SQL Server et l’exécution des scripts externes. Vous pouvez également afficher le paramètre de mémoire pour les pools de ressources externes. |

## <a name="next-steps"></a>Étapes suivantes

- [Surveiller SQL Server Machine Learning Services à l’aide de vues de gestion dynamique (DMV)](monitor-sql-server-machine-learning-services-using-dynamic-management-views.md)
- [Surveiller les scripts Python et R avec des événements étendus dans SQL Server Machine Learning Services](extended-events.md)
