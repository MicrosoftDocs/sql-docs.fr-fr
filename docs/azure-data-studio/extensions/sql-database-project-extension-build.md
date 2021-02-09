---
title: Générer et publier un projet
description: Générer et publier un projet à l’aide de l’extension SQL Server Database Projects
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 06/25/2020
ms.openlocfilehash: 06021fc598165982156093c12c26434f06cbc422
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495707"
---
# <a name="build-and-publish-a-project"></a>Générer et publier un projet

Le processus de génération dans l’extension SQL Database Projects (préversion) pour Azure Data Studio permet la création de *dacpac* dans les environnements Windows, macOS et Linux. Le projet peut être déployé dans un environnement local ou cloud à l’aide du processus de publication.

## <a name="prerequisites"></a>Prérequis

- Installez et configurez l’[extension SQL Database Projects pour Azure Data Studio](sql-database-project-extension.md).

## <a name="build-a-database-project"></a>Générer un projet de base de données

 Dans la viewlet **Projets** sous **Explorateur**, cliquez avec le bouton droit sur le nœud racine *.sqlproj*, puis sélectionnez **Générer**.

 Le volet de sortie s’affiche automatiquement avec la sortie du processus de génération.  Le message suivant apparaît à la fin d’une génération réussie : 

 ``` ... exited with code: 0 ```

## <a name="publish-a-database-project"></a>Publier un projet de base de données

Une fois qu’un projet a été compilé via le processus de génération, la base de données peut être publiée sur une instance SQL Server. Pour publier un projet de base de données, dans la viewlet **Projets** sous **Explorateur**, cliquez avec le bouton droit sur le nœud racine *.sqlproj*, puis sélectionnez **Publier**.

Dans la boîte de dialogue **Publier la base de données** qui s’affiche, spécifiez une connexion serveur et le nom de la base de données à créer.

## <a name="next-steps"></a>Étapes suivantes

- [Extension SQL Database Projects pour Azure Data Studio](sql-database-project-extension.md)
- [Générer des projets de base de données SQL à partir de la ligne de commande](sql-database-project-extension-build-from-command-line.md)
