---
title: Ouvrir un éditeur (SQL Server Management Studio)
description: Découvrez comment ouvrir les éditeurs de requête, MDX, DMX et XML/A du Moteur de base de données dans SQL Server Management Studio.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 5d654a60-d205-49d2-a831-b3d986d60024
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ce81ec69f717f10c705b0d4e7fc1f5b1c6d6ea70
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354597"
---
# <a name="open-an-editor-sql-server-management-studio"></a>Ouvrir un éditeur (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette rubrique explique comment ouvrir l'Éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] , ainsi que les éditeurs MDX, DMX et XML/A dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Une fois ouverte, chaque fenêtre d'éditeur s'affiche sous forme d'onglet dans le volet central de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
## <a name="before-you-begin"></a>Avant de commencer  
 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] prend en charge quatre éditeurs : l’Éditeur de requête [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour la modification de scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] , les éditeurs DMX et MDX pour la modification des scripts utilisant ces langages et l’éditeur XML/A pour la modification de scripts XML/A ou de fichiers XML. Tous ces éditeurs peuvent également être utilisés pour modifier des fichiers texte.  
  
### <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Si vous partagez des fichiers avec des utilisateurs d'autres sites qui utilisent des pages de codes distinctes, vous devez enregistrer votre fichier avec le code Unicode approprié afin d'éviter les erreurs de lecture du fichier. De même, lorsque vous enregistrez des fichiers pour UNIX ou Macintosh, assurez-vous de les enregistrer au format de document approprié. Dans le menu **Fichier** , cliquez sur **Enregistrer sous**, puis sur **Enregistrer avec encodage** en cliquant sur la flèche en regard du bouton **Enregistrer** , puis choisissez **Unix** ou **Macintosh** sous **Fins de ligne**.  
  
### <a name="permissions"></a>Autorisations  
 Les opérations que vous réalisez dans un éditeur de codes sont soumises aux autorisations accordées au compte d'authentification que vous avez utilisé pour vous connecter. Par exemple, si vous ouvrez une fenêtre de l'Éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] à l'aide de l'authentification Windows, vous ne pouvez pas exécuter des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui font référence à des objets pour lesquels votre compte de connexion Windows ne dispose pas d'autorisation d'accès.  
  
## <a name="how-to-open-editors"></a>Procédure : Ouvrir les éditeurs  
 Cette section explique comment ouvrir les différents éditeurs dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
### <a name="using-the-filenew-menu"></a>Utilisation du menu Fichier/Nouveau  
 Dans le menu de **Fichier** , cliquez sur **Nouveau**, puis sélectionnez l’un des éditeurs de requête proposés en option :  
  
-   **Requête avec la connexion actuelle** - Ouvre une nouvelle fenêtre d’éditeur du type associé à la connexion active dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. La fenêtre de l'éditeur utilise les mêmes informations d'authentification que la connexion active. Par exemple, si vous sélectionnez une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] dans l’Explorateur d’objets, puis choisissez **Requête avec la connexion actuelle**, [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ouvre un Éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] qui est connecté à la même instance en utilisant les mêmes informations d’identification.  
  
-   **Requête de moteur de base de données** - Ouvre un nouvel Éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et une boîte de dialogue dans laquelle vous devez entrer les informations nécessaires pour vous connecter à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   **Requête MDX Analysis Services** - Ouvre un nouvel Éditeur de requête MDX [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et une boîte de dialogue dans laquelle vous devez entrer les informations requises pour vous connecter à une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
-   **Requête DMX Analysis Services** - Ouvre un nouvel Éditeur de requête DMX [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et une boîte de dialogue dans laquelle vous devez entrer les informations requises pour vous connecter à une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
-   **Requête XMLA Analysis Services** - Ouvre un nouvel Éditeur de requête XML/A [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et une boîte de dialogue dans laquelle vous devez entrer les informations requises pour vous connecter à une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
### <a name="using-the-fileopen-menu"></a>Utilisation du menu Fichier/Ouvrir  
 Dans le menu **Fichier** , cliquez sur **Ouvrir**, puis naviguez jusqu’à un fichier et ouvrez-le. [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ouvre le type approprié d’éditeur pour l’extension de fichier, copie le contenu du fichier dans la fenêtre de l’éditeur et ouvre si nécessaire une boîte de dialogue de connexion. Par exemple, si vous ouvrez un fichier avec l'extension .sql, [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ouvre une fenêtre de l'Éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] , y copie le contenu du fichier .sql, et ouvre une boîte de dialogue de connexion. Si vous ouvrez un fichier avec une extension non associée à un éditeur spécifique, [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ouvre une fenêtre d'éditeur de texte et y copie le contenu du fichier.  
  
 Pour plus d’informations, consultez [Associer des extensions de fichier à un éditeur de code](./associate-file-extensions-to-a-code-editor.md).  
  
### <a name="using-the-toolbar"></a>Utilisation de la barre d'outils  
 Dans la barre d’outils **Standard** , cliquez sur l’un des boutons suivants :  
  
-   **Nouvelle requête** - Ouvre une nouvelle fenêtre d’éditeur du type associé à la connexion active dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. La fenêtre de l'éditeur utilise les mêmes informations d'authentification que la connexion active. Par exemple, si vous sélectionnez une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] dans l’Explorateur d’objets, puis cliquez sur **Nouvelle requête** , [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ouvre un Éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] qui est connecté à la même instance en utilisant les mêmes informations d’identification.  
  
-   **Requête de moteur de base de données** - Ouvre un nouvel Éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et une boîte de dialogue dans laquelle vous devez entrer les informations nécessaires pour vous connecter à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   **Requête MDX Analysis Services** - Ouvre un nouvel Éditeur de requête MDX [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et une boîte de dialogue dans laquelle vous devez entrer les informations requises pour vous connecter à une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
-   **Requête DMX Analysis Services** - Ouvre un nouvel Éditeur de requête DMX [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et une boîte de dialogue dans laquelle vous devez entrer les informations requises pour vous connecter à une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
-   **Requête XMLA Analysis Services** - Ouvre un nouvel Éditeur de requête XML/A [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et une boîte de dialogue dans laquelle vous devez entrer les informations requises pour vous connecter à une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
### <a name="using-object-explorer"></a>Utilisation de l'Explorateur d'objets  
 Dans l’ **Explorateur d’objets**:  
  
-   Cliquez avec le bouton droit sur le nœud serveur connecté à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)], puis sélectionnez **Nouvelle requête**. Cela ouvre une fenêtre de l'Éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] connectée à la même instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et définit le contexte de base de données de la fenêtre sur la base de données par défaut pour la connexion.  
  
-   Cliquez avec le bouton droit sur un nœud de base de données, puis sélectionnez **Nouvelle requête**. Cela ouvre une fenêtre de l'Éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] connectée à la même instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et définit le contexte de base de données de la fenêtre sur la même base de données.  
  
### <a name="using-solution-explorer"></a>Utilisation de l'Explorateur de solutions  
 Dans l’ **Explorateur de solutions**, développez un dossier, cliquez avec le bouton droit sur un élément du dossier, puis cliquez sur **Ouvrir** ou double-cliquez sur l’élément ou le fichier.  
  
### <a name="using-template-browser-to-open-the-database-engine-query-editor"></a>Utilisation de l'Explorateur de modèles pour ouvrir l'Éditeur de requête du moteur de base de données  
  
-   Dans le menu **Affichage** , cliquez sur **Explorateur de modèles**.  
  
-   La fenêtre **Explorateur de modèles** s’affiche dans le volet droit.  
  
-   Double-cliquez sur un modèle pour ouvrir une fenêtre Requête de moteur de base de données avec le texte du modèle. Par exemple, pour ouvrir un modèle CREATE DATABASE, ouvrez le dossier **Modèles SQL Server** , ouvrez le dossier **Bases de données** et double-cliquez sur **Créer la base de données**.