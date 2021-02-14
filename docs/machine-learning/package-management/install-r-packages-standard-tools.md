---
title: Installer des packages avec les outils R
description: Découvrez comment utiliser des outils R standard pour installer de nouveaux packages R sur une instance de SQL Server Machine Learning Services ou SQL Server R Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: e54df6240cf1bf4dcaee7813eaf614a84c31a0f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082620"
---
# <a name="install-packages-with-r-tools"></a>Installer des packages avec les outils R

[!INCLUDE [SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

Cet article décrit comment utiliser des outils R standard pour installer de nouveaux packages R sur une instance de SQL Server Machine Learning Services ou SQL Server R Services. Vous pouvez installer des packages sur un SQL Server disposant d’une connexion Internet, ainsi que d’un package isolé d’Internet.

Outre les outils R standard, vous pouvez installer des packages R à l’aide de :

+ [RevoScaleR](install-r-packages-with-revoscaler.md)
::: moniker range="=sql-server-2017"
+ [T-SQL](install-r-packages-with-tsql.md) (CREATE EXTERNAL LIBRARY)
::: moniker-end

## <a name="general-considerations"></a>Considérations d’ordre général

+ Le code R s’exécutant dans SQL Server peut utiliser uniquement des packages installés dans la bibliothèque d’instances par défaut. SQL Server ne peut pas charger les packages à partir de bibliothèques externes, même si cette bibliothèque est sur le même ordinateur.
Cela comprend les bibliothèques R installées avec d’autres produits Microsoft.

+ La bibliothèque de packages R se trouve dans le dossier de Fichiers programmes de votre instance et, par défaut, l’installation dans ce dossier requiert des autorisations d’administrateur. Pour plus d’informations, consultez [Emplacement de la bibliothèque de packages](../package-management/r-package-information.md#default-r-library-location).

  ::: moniker range="=sql-server-2017"
  Les non-administrateurs peuvent installer des packages à l’aide de RevoScaleR 9.0.1 et versions ultérieures ou à l’aide de CREATE EXTERNAL LIBRARY. L’utilisateur **dbo_owner** ou un utilisateur disposant de l’autorisation CREATE EXTERNAL LIBRARY, peut installer des packages R dans la base de données actuelle. Pour plus d'informations, consultez les pages suivantes :
  + [Utiliser RevoScaleR pour installer des packages R](install-r-packages-with-revoscaler.md)
  + [Utiliser T-SQL (CREATE EXTERNAL LIBRARY) pour installer des packages R sur SQL Server](install-r-packages-with-tsql.md)
  ::: moniker-end

  ::: moniker range="=sql-server-2016"
  Les non-administrateurs peuvent installer des packages à l’aide de RevoScaleR 9.0.1 et versions ultérieures. L’utilisateur **dbo_owner** peut installer des packages R dans la base de données actuelle. Pour plus d’informations, consultez [Utiliser RevoScaleR pour installer des packages R](install-r-packages-with-revoscaler.md).
  ::: moniker-end

+ Dans un environnement SQL Server renforcé, vous souhaiterez peut-être éviter ce qui suit :
  + Les packages qui nécessitent un accès réseau
  + Les packages qui nécessitent un accès au système de fichiers élevé
  + Les packages utilisés pour le développement Web ou d’autres tâches qui ne bénéficient pas de l’exécution dans SQL Server

## <a name="online-installation-with-internet-access"></a>Installation en ligne (avec accès à Internet)

Si le SQL Server a accès à Internet, vous pouvez utiliser les outils d’installation de packages standard pour installer des packages R.

1. Déterminez l’emplacement de la bibliothèque d’instances (consultez [Obtenir des informations sur le package R](../package-management/r-package-information.md)) et accédez au dossier dans lequel les outils R sont installés.

   ::: moniker range="=sql-server-2016"
   Par exemple, le chemin d’accès par défaut d’une instance par défaut est le suivant :

   `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   Par exemple, le chemin d’accès par défaut d’une instance par défaut est le suivant :

   `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

1. Exécutez **R** ou **Rgui** en tant qu’administrateur à partir de ce dossier.

1. Exécutez la commande R `install.packages` et spécifiez le nom du package. Si le package a des dépendances, le programme d’installation télécharge automatiquement les dépendances et les installe.

Si vous disposez de plusieurs instances côte à côte de SQL Server, exécutez l’installation séparément pour chaque instance dans laquelle vous souhaitez utiliser le package. Il n’est pas possible de partager des packages entre les instances.

## <a name="offline-installation-no-internet-access"></a><a name = "bkmk_offlineInstall"></a> Installation hors connexion (aucun accès à Internet)

Souvent, les serveurs qui hébergent des bases de données de production n’ont pas de connexion Internet. Pour installer des packages R dans cet environnement, vous devez télécharger et préparer des packages et des dépendances à l’avance (sous forme de fichiers zippés), puis copier les fichiers dans un dossier sur le serveur. Une fois les fichiers en place, vous pouvez installer les packages hors connexion.

L’identification de toutes les dépendances devient compliquée. Pour R, nous vous recommandons d’utiliser [**miniCRAN**](https://andrie.github.io/miniCRAN/) pour créer un référentiel local.
**miniCRAN** s’appuie sur une liste des packages que vous souhaitez installer, analyse les dépendances et récupère tous les fichiers zippés pour vous. Il crée ensuite un référentiel unique que vous pouvez copier sur l’instance isolée. Le package [**igraph**](https://igraph.org/r/) est également utile pour l’analyse des dépendances du package.

Pour plus d’informations, consultez [Créer un référentiel de packages R local à l’aide de miniCRAN](create-a-local-package-repository-using-minicran.md).

Une fois que le fichier zip se trouve sur l’instance, vous pouvez l’installer à l’aide des outils R standard sur le serveur.

1. Déterminez l’emplacement de la bibliothèque d’instances (consultez [Obtenir des informations sur le package R](../package-management/r-package-information.md)) et accédez au dossier dans lequel les outils R sont installés. 

   ::: moniker range="=sql-server-2016"
   Par exemple, le chemin d’accès par défaut d’une instance par défaut est le suivant :

   `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   Par exemple, le chemin d’accès par défaut d’une instance par défaut est le suivant :

   `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

1. Exécutez **R** ou **Rgui** en tant qu’administrateur à partir de ce dossier.

1. Exécutez la commande R `install.packages` et spécifiez le nom du package ou du référentiel, ainsi que l’emplacement des fichiers zip. Par exemple :

   ```R
   install.packages("C:\\Temp\\Downloaded packages\\mynewpackage.zip", repos=NULL)
   ```

   Cette commande extrait le package R `mynewpackage` à partir de son fichier zippé local et installe le package. Si le package a des dépendances, le programme d’installation recherche les packages existants dans la bibliothèque. Si vous avez créé un référentiel qui inclut les dépendances, le programme d’installation installe également les packages requis.

   > [!NOTE]
   > Si certains packages requis ne sont pas présents dans la bibliothèque d’instances et sont introuvables dans les fichiers zip, l’installation du package cible échoue.

Comme alternative à **miniCRAN**, vous pouvez effectuer ces étapes manuellement :

1. Identifiez toutes les dépendances des packages.
1. Vérifiez si des packages requis sont déjà installés sur le serveur. Si le package est installé, vérifiez que la version est correcte.
1. Téléchargez le package et toutes les dépendances sur un ordinateur distinct disposant d’un accès à Internet.
1. Placez le package et les dépendances dans une archive de package unique.
1. Zippez l’archive si elle n’est pas déjà au format zippé.
1. Déplacez les fichiers vers un dossier accessible par le serveur.
1. Exécutez une commande d’installation prise en charge ou une instruction DDL pour installer le package dans la bibliothèque d’instances.

## <a name="see-also"></a>Voir aussi

+ [Obtenir des informations sur les packages R](r-package-information.md)
+ [Conseils pour l’utilisation de packages R](tips-for-using-r-packages.md)
+ [Tutoriels sur le langage R SQL Server](../tutorials/r-tutorials.md)