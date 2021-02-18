---
title: Conseils pour l’utilisation de packages R
titleSuffix: SQL machine learning
description: Obtenez des conseils utiles sur l’utilisation des packages R dans SQL Server pour ceux qui débutent avec R ou SQL Server.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/06/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e3dd91e29d88fc2ab6f7216f0bac38632caa7b25
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352513"
---
# <a name="tips-for-using-r-packages"></a>Conseils pour l’utilisation de packages R

[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Cet article fournit des conseils utiles sur l’utilisation des packages R dans SQL Server. Ces conseils sont destinés aux DBA qui ne maîtrisent pas R et aux développeurs R expérimentés qui ne sont pas habitués à accéder aux packages dans une instance SQL Server.

## <a name="if-youre-new-to-r"></a>Si vous débutez avec R

En tant qu’administrateur qui installe les packages R pour la première fois, il pourrait vous être utile de connaître quelques notions de base sur la gestion des packages R pour commencer.

### <a name="package-dependencies"></a>Dépendances de package

Les packages R dépendent souvent de plusieurs autres packages et certains d’entre eux peuvent ne pas être accessibles dans la bibliothèque R par défaut utilisée par l’instance. Parfois, un package nécessite l’utilisation d’une version d’un package dépendant autre que celle déjà installée. Les dépendances de package sont indiquées dans un fichier de DESCRIPTION incorporé dans le package, mais sont parfois incomplètes. Vous pouvez utiliser un package appelé [iGraph](https://igraph.org/r/) pour articuler complètement le graphique de dépendance.

Si vous devez installer plusieurs packages ou si vous souhaitez vous assurer que tous les membres de votre organisation disposent du bon type de package et de la bonne version, nous vous recommandons d’utiliser le package [miniCRAN](https://mran.microsoft.com/package/miniCRAN) pour analyser la chaîne de dépendance complète. minicRAN crée un référentiel local qui peut être partagé entre plusieurs utilisateurs ou ordinateurs. Pour plus d’informations, consultez la section [Créer un référentiel de packages local à l’aide de miniCRAN](create-a-local-package-repository-using-minicran.md).

### <a name="package-sources-versions-and-formats"></a>Sources, versions et formats des packages

Il existe plusieurs sources de packages R, notamment [CRAN](https://cran.r-project.org/) et [Bioconductor](https://www.bioconductor.org/). Le site officiel pour le langage R (<https://www.r-project.org/>) répertorie la plupart de ces ressources. Microsoft offre [MRAN](https://mran.microsoft.com/) pour sa distribution de R open source ([MRO](https://mran.microsoft.com/open)) et d’autres packages. De nombreux packages sont publiés sur GitHub, où les développeurs peuvent obtenir le code source.

::: moniker range=">=sql-server-2016"
Les packages R s’exécutent sur plusieurs plateformes informatiques. Assurez-vous que les versions que vous installez sont des binaires Windows.
::: moniker-end

::: moniker range=">=sql-server-linux-ver15"
Les packages R s’exécutent sur plusieurs plateformes informatiques. Assurez-vous que les versions que vous installez sont des binaires Linux.
::: moniker-end

### <a name="know-which-library-youre-installing-to-and-which-packages-are-already-installed"></a>Assurez-vous de savoir sur quelle bibliothèque vous effectuez l’installation et quels sont les packages déjà installés

Si vous avez déjà modifié l’environnement R sur l’ordinateur, vérifiez que la variable d’environnement R `.libPath` utilise un seul chemin d’accès avant d’installer quoi que ce soit.

Ce chemin d’accès doit pointer vers le dossier R_SERVICES de l’instance. Pour en savoir plus, notamment sur la manière de déterminer quels packages sont déjà installés, consultez la section [Obtenir des informations sur les packages R](../package-management/r-package-information.md).

## <a name="if-youre-new-to-sql-server"></a>Si vous débutez avec SQL Server

En tant que développeur R travaillant sur des codes s’exécutant sur SQL Server, les stratégies de sécurité protégeant le serveur limitent votre capacité à contrôler l’environnement R. Les conseils suivants décrivent des situations typiques et fournissent des suggestions pour travailler dans cet environnement.

### <a name="r-user-libraries-not-supported-on-sql-server"></a>Bibliothèques utilisateur R : non prises en charge sur SQL Server

Les développeurs R devant installer de nouveaux packages R sont habitués à installer des packages à tout moment à l’aide d’une bibliothèque utilisateur privée à chaque fois que la bibliothèque par défaut n’est pas disponible ou lorsque le développeur n’est pas un administrateur sur l’ordinateur. Par exemple, dans un environnement de développement R standard, l’utilisateur pourrait ajouter l’emplacement du package à la variable d’environnement R `libPath` ou référencer le chemin d’accès complet du package, comme ceci :

```R
library("c:/Users/<username>/R/win-library/packagename")
```

Cela ne fonctionne pas lors de l’exécution de solutions R dans SQL Server, car les packages R doivent être installés dans une bibliothèque par défaut spécifique associée à l’instance. S’ils ne le sont pas, vous risquez d’obtenir ce type d’erreur quand vous essayez d’appeler le package :

*Erreur dans la bibliothèque (xxx) : aucun package nommé « package-name » trouvé*

Pour plus d’informations sur l’installation de packages R dans SQL Server, consultez la section [Installer de nouveaux packages R sur SQL Server Machine Learning Services ou SQL Server R Services](install-additional-r-packages-on-sql-server.md).

### <a name="how-to-avoid-package-not-found-errors"></a>Comment éviter les erreurs « package introuvable »

Consultez les instructions suivantes pour éviter de recevoir des erreurs « package introuvable ».

+ Éliminez les dépendances sur les bibliothèques utilisateur.

    C’est une mauvaise pratique de développement que d’installer les packages R requis dans une bibliothèque utilisateur personnalisée. Cela peut entraîner des erreurs si une solution est exécutée par un autre utilisateur qui n’a pas accès à l’emplacement de la bibliothèque.

    De plus, si un package est installé dans la bibliothèque par défaut, le runtime R charge ce package à partir de la bibliothèque par défaut, même si une autre version est spécifiée dans le code R.

+ Assurez-vous que votre code peut s’exécuter dans un environnement partagé.

+ Évitez d’installer des packages dans le cadre d’une solution. Si vous n’avez pas les autorisations nécessaires pour installer des packages, le code échoue. Même si vous disposez des autorisations nécessaires pour installer des packages, vous devez le faire séparément du code que vous souhaitez exécuter.

+ Vérifiez votre code pour vous assurer qu’il ne contient pas d’appel à des packages désinstallés.

+ Mettez à jour votre code pour supprimer les références directes aux chemins d’accès des packages R ou des bibliothèques R.

+ Sachez quelle bibliothèque de packages est associée à l’instance. Pour plus d’informations, consultez la section [Obtenir des informations sur le package R](../package-management/r-package-information.md).

## <a name="see-also"></a>Voir aussi

::: moniker range="<=sql-server-2017"
+ [Installer des packages avec les outils R](install-r-packages-standard-tools.md)
::: moniker-end
::: moniker range=">sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current"
+ [Installer de nouveaux packages R avec sqlmlutils](install-additional-r-packages-on-sql-server.md)
::: moniker-end
