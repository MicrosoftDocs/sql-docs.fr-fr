---
title: Amélioration des performances avec la fonction de profilage de code R
description: Recueillez des informations utiles pour améliorer les performances et bénéficier de résultats de calcul R plus rapides sur SQL Server avec des fonctions de profilage R. La fonction *rprof* collecte et retourne des informations sur les appels de fonction internes.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/30/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 511a0d73bbc0e8df5c19ee563dca9c7eef8e53eb
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597044"
---
# <a name="use-r-code-profiling-functions-to-improve-performance"></a>Utiliser les fonctions de profilage de code R pour améliorer les performances
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Cet article décrit les outils d’analyse des performances fournis par les packages R pour obtenir des informations sur les appels de fonction internes. Vous pouvez utiliser ces informations pour améliorer le niveau de performance de votre code.

> [!TIP]
> Cet article fournit des ressources de base pour vous aider à démarrer. Pour obtenir des conseils d’expert, nous vous recommandons de lire la section *Performance* de l’ouvrage de [Hadley Wickham, « Advanced R »](http://adv-r.had.co.nz).

## <a name="using-rprof"></a>Utilisation de RPROF

[*rprof*](https://www.rdocumentation.org/packages/utils/versions/3.5.1/topics/Rprof) est une fonction intégrée au package de base [**utils**](https://www.rdocumentation.org/packages/utils/versions/3.5.1), qui est chargé par défaut. 

En général, la fonction *rprof* opère en écrivant la pile d’appels dans un fichier selon des intervalles définis. Vous pouvez ensuite utiliser la fonction [*summaryRprof*](https://www.rdocumentation.org/packages/utils/versions/3.5.1/topics/summaryRprof) pour traiter le fichier de sortie. L’un des avantages de la fonction *rprof* est qu’elle effectue un échantillonnage, ce qui atténue la charge de performances de la surveillance.

Pour utiliser le profilage R dans votre code, vous devez appeler cette fonction et spécifier ses paramètres, notamment le nom de l’emplacement du fichier journal qui sera écrit. Le profilage peut être activé et désactivé dans le code. La syntaxe suivante illustre l’utilisation de base : 

```R
# Specify profiling output file.
varOutputFile <- "C:/TEMP/run001.log")
Rprof(varOutputFile)

# Turn off profiling
Rprof(NULL)
    
# Restart profiling
Rprof(append=TRUE)
```

> [!NOTE]
> Pour pouvoir utiliser cette fonction, vous devez installer Windows Perl sur l’ordinateur appelé à exécuter le code. Par conséquent, nous recommandons de profiler le code pendant la phase de développement dans un environnement R, puis de déployer le code débogué sur SQL Server.  


## <a name="r-system-functions"></a>Fonctions du système R

Le langage R intègre la plupart des fonctions du package de base qui retournent le contenu des variables système. Par exemple, dans votre code R, vous pouvez utiliser `Sys.timezone` pour obtenir le fuseau horaire actuel ou `Sys.Time` pour obtenir l’heure système à partir de R. 

Pour obtenir des informations sur les différentes fonctions du système R, tapez le nom de la fonction comme argument de la fonction `help()` R à partir d’une invite de commandes R.

```R
help("Sys.time")
```

## <a name="debugging-and-profiling-in-r"></a>Débogage et profilage dans R

Dans la documentation de Microsoft R Open, qui est installée par défaut, vous trouverez un manuel sur le développement d’extensions pour le langage R qui traite en détail [du profilage et du débogage](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Debugging).

## <a name="next-steps"></a>Étapes suivantes

+ Pour plus d’informations sur l’optimisation des scripts R dans SQL Server, consultez [Réglage du niveau de performance et optimisation des données pour R](r-and-data-optimization-r-services.md).
+ Pour plus d’informations sur le réglage du niveau de performance dans SQL Server, consultez [Centre de performances pour le Moteur de base de données SQL Server et Azure SQL Database](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md).
+ Pour plus d’informations sur le package utils, consultez [Package R.utils](https://www.rdocumentation.org/packages/utils/versions/3.5.1).
+ Pour des explications approfondies sur la programmation R, consultez [« Advanced R » de Hadley Wickham](http://adv-r.had.co.nz).