---
title: 'Didacticiel R : Développer un modèle dans SQL'
description: Découvrez comment créer une solution de bout en bout pour la modélisation prédictive basée sur la prise en charge des fonctionnalités R dans SQL Server 2016 ou SQL Server 2017.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/11/2019
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: d9bfe9ec4f81ca61bacbb4dc3526ea5b7a1a87f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272960"
---
# <a name="tutorial-sql-development-for-r-data-scientists"></a>Tutoriel : Développement SQL pour les scientifiques des données R
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

Dans ce didacticiel pour les scientifiques des données, découvrez comment créer une solution de bout en bout pour la modélisation prédictive basée sur la prise en charge des fonctionnalités R dans SQL Server 2016 ou SQL Server 2017. Ce didacticiel utilise une base de données [NYCTaxi_sample](demo-data-nyctaxi-in-sql.md) sur SQL Server. 

Vous utilisez une combinaison de code R, de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de fonctions SQL personnalisées pour générer un modèle de classification qui indique la probabilité que le chauffeur obtienne un pourboire sur un trajet en taxi particulier. Vous déployez également votre modèle R sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et utilisez des données de serveur pour générer des scores basés sur le modèle.

Cet exemple peut être étendu à tous les types de problèmes réels, tels que la prédiction des réponses des clients aux campagnes de vente ou la prédiction des dépenses ou de la participation lors d’un événement. En outre, le modèle pouvant être appelé à partir d’une procédure stockée, vous pouvez facilement l’incorporer dans une application.

La procédure pas à pas étant conçue pour présenter [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] aux développeurs R, les tâches sont effectuées à l’aide de R, dans la mesure du possible. Toutefois, cela ne signifie pas que R est le meilleur outil pour toutes les tâches. Dans de nombreux cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut offrir de meilleures performances, en particulier pour des tâches telles que l’agrégation de données et l’ingénierie de caractéristiques.  Ces tâches peuvent notamment profiter de nouvelles fonctionnalités dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], telles que les index columnstore optimisés en mémoire. Nous essayons d’indiquer les optimisations possibles au cours de la procédure.

## <a name="prerequisites"></a>Prérequis

+ [SQL Server Machine Learning Services avec l’intégration R ](../install/sql-machine-learning-services-windows-install.md#verify-installation) ou [SQL Server 2016 R Services](../install/sql-r-services-windows-install.md)

+ [Autorisations de base de données](../security/user-permission.md) et une connexion d’utilisateur de base de données SQL Server

+ [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)

+ [Base de données de démonstration Taxis de New York](demo-data-nyctaxi-in-sql.md)

+ Un IDE R tel que RStudio ou l’outil RGUI intégré fourni avec R

Nous vous recommandons d’effectuer cette procédure pas à pas sur une station de travail cliente. Vous devez être en mesure de vous connecter, sur le même réseau, à un ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec SQL Server et le langage R activé. Pour obtenir des instructions sur la configuration de la station de travail, consultez [Configurer un client de science des données pour le développement R sur SQL Server](../r/set-up-a-data-science-client.md).

Vous pouvez également exécuter la procédure pas à pas sur un ordinateur qui possède à la fois [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et un environnement de développement R, mais nous ne recommandons pas cette configuration pour un environnement de production. Si vous devez placer le client et le serveur sur le même ordinateur, veillez à installer un deuxième ensemble de bibliothèques Microsoft R pour envoyer un script R depuis un client « distant ». N’utilisez pas les bibliothèques R qui sont installées dans les fichiers programme de l’instance SQL Server. Plus précisément, si vous utilisez un ordinateur, vous avez besoin de la bibliothèque RevoScaleR dans ces deux emplacements pour prendre en charge les opérations du client et du serveur.

+ C:\Program Files\Microsoft\R Client\R_SERVER\library\RevoScaleR 
+ C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR

> [!NOTE]
> Si vous utilisez [Machine Learning Server](/machine-learning-server/) ou [Data Science Virtual Machine](/azure/machine-learning/data-science-virtual-machine/), au lieu de R Client, le chemin d’accès à RevoScaleR est C:\Program Files\Microsoft\ML Server\R_SERVER\library\RevoScaleR

<a name="add-packages"></a>

## <a name="additional-r-packages"></a>Packages R supplémentaires

Cette procédure pas à pas nécessite certaines bibliothèques R qui ne sont pas installées par défaut dans le cadre de [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)]. Vous devez installer les packages à la fois sur le client où vous développez la solution et sur l’ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où vous déployez la solution.

### <a name="on-a-client-workstation"></a>Sur une station de travail cliente

Dans votre environnement R, copiez les lignes suivantes et exécutez le code dans une fenêtre de console (Rgui ou un IDE). Certains packages installent également les packages nécessaires. En tout, environ 32 packages sont installées. Vous devez avoir une connexion Internet pour terminer cette étape.
    
  ```R
  # Install required R libraries, if they are not already installed.
  if (!('ggmap' %in% rownames(installed.packages()))){install.packages('ggmap')}
  if (!('mapproj' %in% rownames(installed.packages()))){install.packages('mapproj')}
  if (!('ROCR' %in% rownames(installed.packages()))){install.packages('ROCR')}
  if (!('RODBC' %in% rownames(installed.packages()))){install.packages('RODBC')}
  ```

### <a name="on-the-server"></a>Sur le serveur

Plusieurs options s’offrent à vous pour installer des packages sur SQL Server. Par exemple, SQL Server fournit une fonctionnalité de [gestion des packages R](../package-management/install-additional-r-packages-on-sql-server.md) qui permet aux administrateurs de base de données de créer un référentiel de packages et d’affecter aux utilisateurs les droits nécessaires pour installer leurs propres packages. Toutefois, si vous êtes administrateur de l’ordinateur, vous pouvez installer de nouveaux packages à l’aide de R, à condition que vous les installiez dans la bibliothèque appropriée.

> [!NOTE]
> **N’installez pas** les packages dans une bibliothèque utilisateur du serveur, même si vous y êtes invité. Si vous les installez dans une bibliothèque utilisateur, l’instance SQL Server ne peut pas trouver ni exécuter les packages. Pour plus d’informations, consultez [Installer de nouveaux packages R avec sqlmlutils](../package-management/install-additional-r-packages-on-sql-server.md).

1. Sur l’ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ouvrez RGui.exe **en tant qu’administrateur**.  Si vous avez installé SQL Server R Services à l’aide des paramètres par défaut, vous trouverez Rgui.exe dans C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64).

2. À l’invite R, exécutez les commandes R suivantes :
  
  ```R
  install.packages("ggmap", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  install.packages("mapproj", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  install.packages("ROCR", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  install.packages("RODBC", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  ```
  Cet exemple utilise la fonction grep R pour rechercher le vecteur de chemins disponibles et trouver celui qui comprend « Program Files ». Pour plus d’informations, consultez [https://www.rdocumentation.org/packages/base/functions/grep](https://www.rdocumentation.org/packages/base/functions/grep).

  Si vous pensez que les packages sont déjà installés, vérifiez la liste des packages installés en exécutant `installed.packages()`.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Explorer et synthétiser des données](walkthrough-view-and-summarize-data-using-r.md)