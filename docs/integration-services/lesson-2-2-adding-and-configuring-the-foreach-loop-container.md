---
description: 'Leçon 2-2 : Ajouter et configurer le conteneur de boucles Foreach'
title: 'Étape 2 : Ajouter et configurer le conteneur de boucles Foreach | Microsoft Docs'
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 88a973cc-0f23-4ecf-adb6-5b06279c2df6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8c63288019fa6938f4481bbded2e5b29c29198d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354312"
---
# <a name="lesson-2-2-add-and-configure-the-foreach-loop-container"></a>Leçon 2-2 : Ajouter et configurer le conteneur de boucles Foreach

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Dans cette tâche, vous activez la fonction qui permet d'effectuer des boucles dans un dossier de fichiers plats et d'appliquer la transformation de flux de données de la leçon 1 à chacun de ces fichiers plats. Pour activer cette fonction, vous allez ajouter et configurer un conteneur de boucles Foreach dans le flux de contrôle.  
  
Le conteneur de boucles Foreach que vous allez ajouter doit pouvoir se connecter à chaque fichier plat dans le dossier. Étant donné que tous les fichiers du dossier ont le même format, le conteneur de boucles Foreach peut utiliser le même Gestionnaire de connexions de fichiers plats pour se connecter à chacun de ces fichiers. Le Gestionnaire de connexions de fichiers plats qu’utilise le conteneur est celui que vous avez créé à la leçon 1.  
  
Pour l'instant, le Gestionnaire de connexions de fichiers plats créé au cours de la leçon 1 se connecte uniquement à un seul fichier plat spécifique. Pour que la connexion réitère et s'établisse à chaque fichier plat dans le dossier, vous devez configurer le conteneur de boucles Foreach et le Gestionnaire de connexions de fichiers plats comme suit :  
  
-   **Conteneur de boucles Foreach :** vous mappez la valeur énumérée du conteneur à une variable de package définie par l’utilisateur. Le conteneur utilise ensuite cette variable pour modifier dynamiquement la propriété **ConnectionString** du Gestionnaire de connexions de fichiers plats et se connecter de façon itérative à chaque fichier plat du dossier.  
  
-   **Gestionnaire de connexions de fichiers plats :** vous modifiez le Gestionnaire de connexions qui a été créé dans la leçon 1 en utilisant une variable définie par l’utilisateur pour remplir la propriété **ConnectionString** du Gestionnaire de connexions.  
  
Les procédures de cette tâche montrent comment créer et modifier le conteneur de boucles Foreach pour utiliser une variable de package définie par l'utilisateur et comment ajouter la tâche de flux de données à la boucle. Au cours de la tâche suivante, vous allez apprendre à modifier le Gestionnaire de connexions de fichiers plats pour qu'il utilise cette variable définie par l'utilisateur.  
  
Une fois ces modifications apportées au package et le package exécuté, le conteneur de boucles Foreach effectue une itération dans la collection de tous les fichiers dans le dossier Sample Data. Chaque fois qu’un fichier répondant aux critères est trouvé, le conteneur de boucles Foreach remplit la nouvelle variable définie par l’utilisateur avec le nom du fichier, mappe cette variable à la propriété **ConnectionString** du Gestionnaire de connexions de fichiers plats Sample Currency Data, puis exécute le flux de données par rapport à ce fichier. Ainsi, à chaque itération de la boucle Foreach, la tâche de flux de données utilise un fichier plat différent.  
  
> [!NOTE]  
> Étant donné que [!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] sépare le flux de contrôle du flux de données, les boucles que vous ajoutez au flux de contrôle ne nécessiteront pas la modification du flux de données. Par conséquent, il n'est pas nécessaire de modifier le flux de données de la leçon 1.  
  
## <a name="add-a-foreach-loop-container"></a>Ajouter un conteneur de boucles Foreach  
  
1.  Dans **SQL Server Data Tools**, sélectionnez l’onglet **Flux de contrôle**.  
  
2.  Dans la **Boîte à outils SSIS**, développez **Conteneurs**, puis faites glisser un **conteneur de boucles Foreach** dans l’aire de conception de l’onglet **Flux de contrôle** .  
  
3.  Cliquez avec le bouton droit sur le **conteneur de boucles Foreach**, puis sélectionnez **Modifier**.  
  
4.  Dans la boîte de dialogue **Éditeur de boucle Foreach**, dans la page **Général**, pour **Nom**, entrez **Foreach File in Folder**. Sélectionnez **OK**.  
  
5.  Cliquez avec le bouton droit sur le conteneur de boucles Foreach, sélectionnez **Propriétés** puis, dans la fenêtre **Propriétés**, vérifiez que la propriété **LocaleID** est définie sur **Anglais (États-Unis)** .  
  
## <a name="configure-the-enumerator-for-the-foreach-loop-container"></a>Configurer l'énumérateur pour le conteneur de boucles Foreach  
  
1.  Double-cliquez sur **Foreach File in Folder** pour rouvrir l’**Éditeur de boucle Foreach**.  
  
2.  Sélectionnez **Collection**.  
  
3.  Dans la page **Collection** , cliquez sur **Énumérateur Foreach File**.  
  
4.  Dans le groupe **Configuration de l’énumérateur** , sélectionnez **Parcourir**.  
  
5.  Dans la boîte de dialogue **Rechercher un dossier**, recherchez dans votre ordinateur le dossier qui contient les fichiers Currency_*.txt inclus avec les exemples de données.

6.  Dans la zone **Fichiers**, entrez **Currency_\*.txt**.  
  
## <a name="map-the-enumerator-to-a-user-defined-variable"></a>Mapper l'énumérateur à une variable définie par l'utilisateur  
  
1.  Sélectionnez **Mappages de variables**.  
  
2.  Dans la page **Mappages de variables**, dans la colonne **Variable**, cliquez sur la cellule vide et sélectionnez **\<New Variable...>** .  
  
3.  Dans la boîte de dialogue **Ajouter une variable**, pour **Nom** entrez **varFileName**.  
  
    > [!NOTE]  
    > Les noms de variable respectent la casse.  
  
4.  Sélectionnez **OK**.  
  
5.  Sélectionnez **OK** pour quitter la boîte de dialogue **Éditeur de boucle Foreach**.  
  
## <a name="add-the-data-flow-task-to-the-loop"></a>Ajouter la tâche de flux de données à la boucle  
  
-   Faites glisser la tâche de flux de données **Extract Sample Currency Data** vers le conteneur de boucles Foreach **Foreach File in Folder**.  
  
## <a name="go-to-next-task"></a>Passer à la tâche suivante  
[Étape 3 : Modifier le gestionnaire de connexions de fichiers plats](../integration-services/lesson-2-3-modifying-the-flat-file-connection-manager.md)  
  
## <a name="see-also"></a>Voir aussi  
[Configurer un conteneur de boucles Foreach](./control-flow/foreach-loop-container.md)  
[Utiliser des variables dans des packages](./integration-services-ssis-variables.md)  
  
  
