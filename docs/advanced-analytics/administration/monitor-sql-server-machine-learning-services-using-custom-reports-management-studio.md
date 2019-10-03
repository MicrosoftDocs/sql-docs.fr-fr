---
title: Surveiller l’exécution de scripts Python et R à l’aide de rapports personnalisés dans SSMS
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/17/2019
ms.topic: conceptual
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 549cdcd35b939b2247b14817271e3d1063fab1e0
ms.sourcegitcommit: fd3e81c55745da5497858abccf8e1f26e3a7ea7d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71714393"
---
# <a name="monitor-python-and-r-script-execution-using-custom-reports-in-sql-server-management-studio"></a>Surveiller l’exécution de scripts Python et R à l’aide de rapports personnalisés dans SQL Server Management Studio
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

Utilisez des rapports personnalisés dans SQL Server Management Studio (SSMS) pour surveiller l’exécution de scripts externes (Python et R), les ressources utilisées, diagnostiquer les problèmes et optimiser les performances dans SQL Server Machine Learning Services.

Dans ces rapports, vous pouvez afficher des détails tels que:

- Sessions python ou R actives
- Paramètres de configuration de l’instance
- Statistiques d’exécution pour les travaux de Machine Learning
- Événements étendus pour R services
- Packages python ou R installés sur l’instance actuelle

Cet article explique comment installer et utiliser les rapports personnalisés fournis pour SQL Server Machine Learning Services.

Pour plus d’informations sur les rapports dans SQL Server Management Studio, consultez [rapports personnalisés dans Management Studio](../../ssms/object/custom-reports-in-management-studio.md).

## <a name="how-to-install-the-reports"></a>Comment installer les rapports

Les rapports sont conçus à l’aide de SQL Server Reporting Services, mais peuvent être utilisés directement à partir de SQL Server Management Studio. Reporting Services n’a pas besoin d’être installé sur votre instance SQL Server.

Pour utiliser ces rapports, procédez comme suit :

1. Téléchargez les [rapports personnalisés SSMS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/machine-learning-services/ssms-custom-reports) pour SQL Server machine learning services à partir de github.

2. Copier les rapports dans Management Studio

    1. Recherchez le dossier des rapports personnalisés utilisé par SQL Server Management Studio. Par défaut, les rapports personnalisés sont stockés dans ce dossier (où **user_name** est votre nom d’utilisateur Windows) :

        `C:\Users\user_name\Documents\SQL Server Management Studio\Custom Reports`

       Vous pouvez également spécifier un dossier différent ou créer des sous-dossiers.

    2. Copiez le *. Fichiers RDL que vous avez téléchargés dans le dossier des rapports personnalisés.

3. Exécuter les rapports dans Management Studio

    1. Dans Management Studio, cliquez avec le bouton droit sur le nœud **Bases de données** de l’instance où vous souhaitez exécuter les rapports.

    2. Cliquez sur **Rapports**, puis sur **Rapports personnalisés**.

    3. Dans la boîte de dialogue **Ouvrir un fichier** , recherchez le dossier des rapports personnalisés.

    4. Sélectionnez l’un des fichiers RDL que vous avez téléchargés, puis cliquez sur **Ouvrir**.

## <a name="reports"></a>Rapports

Le [référentiel de rapports personnalisés SSMS dans GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/machine-learning-services/ssms-custom-reports) comprend les rapports suivants :

| Rapport | Description |
|-|-|
| Sessions actives | Les utilisateurs qui sont actuellement connectés à l’instance SQL Server et qui exécutent un script Python ou R. |
| Configuration | Paramètres d’installation de Machine Learning Services et propriétés du runtime python ou R. |
| Configurer l’instance | Configurez Machine Learning Services. |
| Statistiques d’exécution | Statistiques d’exécution des services Machine Learning. Par exemple, vous pouvez obtenir le nombre total d’exécutions de scripts externes et le nombre d’exécutions en parallèle. |
| Événements étendus | Événements étendus disponibles pour obtenir plus d’informations sur l’exécution de scripts externes. |
| . | Répertoriez les packages R ou python installés sur l’instance SQL Server et leurs propriétés, telles que la version et le nom. |
| Utilisation des ressources | Affichez le processeur, la mémoire, la consommation d’e/s des SQL Server et l’exécution des scripts externes. Vous pouvez également afficher le paramètre de mémoire pour les pools de ressources externes. |

## <a name="next-steps"></a>Étapes suivantes

- [Surveiller SQL Server Machine Learning Services à l’aide de vues de gestion dynamique (DMV)](monitor-sql-server-machine-learning-services-using-dynamic-management-views.md)
- [Événements étendus pour R Services](../r/extended-events-for-sql-server-r-services.md)