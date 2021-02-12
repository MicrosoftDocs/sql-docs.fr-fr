---
title: Installer et gérer des extensions de fonctionnalités
description: Découvrez comment installer des extensions de fonctionnalités afin de pouvoir augmenter les fonctionnalités de SQL Server Data Tools. Consultez l’emplacement d’installation de différents types d’extensions.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 9cdc8cd5-c36f-4bee-a191-87ed457803e7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 04/26/2017
ms.openlocfilehash: c469770ab64401498b35038f7b20e6f245793804
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020742"
---
# <a name="how-to-install-and-manage-feature-extensions"></a>Procédure : Installer et gérer des extensions de fonctionnalités

Il est possible d’ajouter des règles d’analyse du code de la base de données, des conditions pour les tests unitaires de base de données et des contributeurs Build and Deployment pour améliorer les fonctionnalités offertes par les différentes éditions de Visual Studio, y compris SQL Server Data Tools. Cependant, vous devrez tout d’abord installer l’extension de fonctionnalité pour pouvoir l’utiliser, qu’elle ait été créée par vous ou par quelqu’un d’autre.  
  
L'endroit où vous installerez votre extension dépend du type d'extension et de l'emplacement d'utilisation. Dans les dernières éditions de Visual Studio, l’emplacement d’installation de certains composants n’est plus le répertoire d’installation de SQL Server, mais le répertoire Visual Studio. Grâce à cette configuration, il est plus facile d’exécuter côte à côte différentes versions du logiciel, mais cela signifie aussi qu’il peut être nécessaire d’installer l’extension à plusieurs emplacements pour pouvoir l’utiliser dans différentes versions de SQL Server Data Tools et en ligne de commande.  
  
### <a name="installing-extensions-for-use-inside-visual-studio"></a>Installer des extensions pour une utilisation dans Visual Studio  
  
|Type d'extension|Emplacement d’installation|  
|------------------|--------------------|  
|Condition de test personnalisée pour les tests unitaires SQL Server|<Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\TestConditions|  
|Collaborateurs de build<br /><br />Collaborateurs de déploiement<br /><br />Règles d'analyse statique du code|<Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120\Extensions|  
  
<Visual Studio Install Dir> dépend de la version de Visual Studio utilisée et de l’emplacement d’installation choisi. Pour Visual Studio 2012, il s’agit généralement de C:\Program Files (x86)\\MicrosoftVisual Studio 11.0. Pour Visual Studio 2013, il s’agit généralement de C:\Program Files (x86)\\MicrosoftVisual Studio 12.0.  
  
Les extensions peuvent être exécutées dans le cadre de nos services de ligne de commande :  
  
|Type d'extension|Service de ligne de commande|Dossier d'installation|  
|------------------|------------------------|------------------|  
|Condition de test personnalisée pour les tests unitaires SQL Server|MSBuild/MSTest peut être utilisé pour exécuter des tests unitaires dans l’Invite de commandes développeur de Visual Studio 2013 et avec des outils en ligne de commande similaires.|Le même qu’en cas d’exécution dans Visual Studio.|  
|Collaborateurs de build<br /><br />Collaborateurs de déploiement|[SqlPackage.exe](../tools/sqlpackage/sqlpackage.md), ou à l’aide de cibles de déploiement ou de publication MSBuild lors de la génération d’un projet de base de données.|MSBuild : Le même qu’en cas d’exécution dans Visual Studio.<br /><br />[SqlPackage.exe](../tools/sqlpackage/sqlpackage.md) : s’il se trouve dans le répertoire Visual Studio, le même qu’avant.<br /><br />Si SqlPackage.exe et d’autres DLL DacFx se trouvent en dehors de ce répertoire, les extensions doivent être placées dans le même répertoire ou dans C:\Program Files (x86)\\MicrosoftSQL Server\120\DAC\bin\Extensions.|  
|Règles d'analyse statique du code|Vous pouvez utiliser MSBuild pour générer le projet et exécuter l'analyse statique du code.<br /><br />Vous pouvez de plus exécuter l’analyse du code avec une API CodeAnalysisService dans vos propres applications. Dans ce cas, les règles de recherche d'extension fonctionnent comme lors de l'utilisation de SqlPackage.exe.|Le même que pour les collaborateurs de déploiement et de génération.|  
  
> [!NOTE]  
> Vous devez avoir des autorisations d'administrateur sur votre ordinateur pour accéder aux répertoires d'installation sous le dossier Program Files. Si vous n'avez pas les autorisations appropriées, contactez votre administrateur réseau.  
  
**Security Considerations**  
  
Avant d’installer une extension dont vous n’êtes pas l’auteur, prenez connaissance des risques suivants :  
  
-   Le programme d’installation de l’extension peut être malveillant et, selon vos autorisations d’installation, accéder à des ressources protégées.  
  
-   L’extension peut être elle-même malveillante et prendre le contrôle de ressources protégées si l’utilisateur qui s’en sert dispose des autorisations nécessaires.  
  
Pour réduire le risque, n’installez l’extension que si elle provient d’une source connue. Si elle est issue d’une source non approuvée, examinez son code source et son programme d’installation (le cas échéant) avant de l’installer et de l’utiliser.  
  
## <a name="to-install-a-custom-feature-extension"></a>Pour installer une extension de fonctionnalité personnalisée  
Copiez l'assembly signé (.dll) dans le dossier d'installation correct. Fermez et rouvrez Visual Studio. L'extension doit maintenant être disponible.  
  
## <a name="see-also"></a>Voir aussi  
[Extension des fonctionnalités de base de données](../ssdt/extending-the-database-features.md)  
  
