---
title: Mettre à niveau un projet de test antérieur contenant des tests unitaires de base de données
description: Découvrez comment mettre à niveau les projets de test Visual Studio 2010 qui contiennent des tests unitaires de base de données. Découvrez comment utiliser SQL Server Data Tools avec ces projets.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 42782ff3-e8cf-4c9d-8dac-a95b236edfc4
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2830890ecb1fc9983ce4ccfac61874d4e529f07b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066854"
---
# <a name="upgrade-an-older-test-project-containing-database-unit-tests"></a>Mettre à niveau un projet de test antérieur contenant des tests unitaires de base de données

Mettez à niveau un ancien projet de test créé dans Visual Studio 2010 et contenant des tests unitaires de base de données pour utiliser les nouveaux outils et runtime de test unitaire de base de données SQL Server Data Tools. Après avoir mis à niveau un projet antérieur, ajoutez des tests unitaires SQL Server au projet (pour plus d’informations, consultez [Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)).  
  
> [!TIP]  
> Si vous utilisez Visual Studio 2010, après avoir ajouté des tests unitaires SQL Server à un projet de test, n'ajoutez pas de tests unitaires utilisant le modèle de test unitaire de base de données antérieur. Le cas échéant, vous devrez reconvertir le projet avant de pouvoir exécuter les tests.  
  
Si vous disposez d'un projet de base de données de test créé dans une version de Visual Studio antérieure à Visual Studio 2010, utilisez les informations de la rubrique [Procédure : mettre à niveau des tests unitaires de base de données de versions antérieures de Visual Studio](/previous-versions/visualstudio/visual-studio-2010/dd193412(v=vs.100)) pour mettre à niveau votre projet de base de données vers Visual Studio 2010 avant de mettre à niveau le projet vers SQL Server Data Tools.  
  
### <a name="initiating-an-upgrade"></a>Initialisation d'une mise à niveau.  
  
-   Lancez une mise à niveau de projet à partir du menu contextuel d'un projet de test.  
  
    Dans certains cas, une boîte de dialogue s'affiche dans SQL Server Data Tools, à partir de laquelle vous pouvez lancer une mise à niveau de projet de test.  
  
-   Le fait de mettre à niveau le projet supprime la référence d'assembly à l'ancienne infrastructure de test de base de données et ajoute une référence à la nouvelle infrastructure et un assembly d'adaptateur. Le fichier app.config est également mis à jour.  
  
    > [!NOTE]  
    > Si votre projet de test contient les fichiers de code DatabaseSetup et SQLDatabaseSetup, la mise à niveau du projet vers SQL Server Data Tools exclut le fichier DatabaseSetup de la build. Vous pouvez supprimer le fichier DatabaseSetup s'il est exclut de la build.  
  
-   Après conversion, les tests unitaires de base de données existants créés avec le modèle antérieur utiliseront les types de l'assembly d'adaptateur pour accéder à la nouvelle infrastructure. L'utilisation d'un assembly d'adaptateur signifie que la procédure de mise à niveau n'a pas modifié les scripts de test et le code. Si vous ajoutez un test unitaire SQL Server au projet, le nouveau test référencera directement la nouvelle infrastructure et non pas au moyen d'un adaptateur. Vous pouvez choisir de mettre à jour manuellement le code existant de façon à utiliser la nouvelle infrastructure pour des raisons de cohérence avec les nouveaux tests, mais cela n'est pas nécessaire.  
  
## <a name="see-also"></a>Voir aussi  
[Vérifier le code de la base de données à l’aide de tests unitaires SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
