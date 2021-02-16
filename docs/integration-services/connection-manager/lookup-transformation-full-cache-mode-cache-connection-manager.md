---
description: Transformation de recherche en mode Cache complet - Gestionnaire de connexions du cache
title: Transformation de recherche en mode Cache complet - Gestionnaire de connexions du cache | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Lookup transformation [Integration Services]
ms.assetid: 58bc7611-5fb5-4113-9742-10959e06b94c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c6560951e789dc16c78788b278579abb906fa3e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350333"
---
# <a name="lookup-transformation-full-cache-mode---cache-connection-manager"></a>Transformation de recherche en mode Cache complet - Gestionnaire de connexions du cache

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Vous pouvez configurer la transformation de recherche afin qu'elle utilise le mode Cache complet et un gestionnaire de connexions du cache. En mode Cache complet, le dataset de référence est chargé dans le cache avant l'exécution de la transformation de recherche.  
  
> [!NOTE]  
>  Le gestionnaire de connexions du cache ne prend pas en charge les types de données de l'objet BLOB (Binary Large Object) DT_TEXT, DT_NTEXT et DT_IMAGE. Si le dataset de référence contient un type de données d'objet BLOB, le composant échoue lorsque vous exécutez le package. Vous pouvez utiliser l' **Éditeur du gestionnaire de connexions du cache** pour modifier des types de données de colonne. Pour plus d’informations, consultez [Éditeur du gestionnaire de connexions du cache](./cache-connection-manager.md).  
  
 La transformation de recherche effectue des recherches en joignant les données des colonnes d'entrée d'une source de données connectée aux colonnes d'un dataset de référence. Pour plus d’informations, voir [Lookup Transformation](../../integration-services/data-flow/transformations/lookup-transformation.md).  
  
 Vous utilisez l'un des éléments suivants pour générer un dataset de référence :  
  
-   Fichier cache (.caw)  
  
     Vous configurez le gestionnaire de connexions du cache afin de lire les données d'un fichier cache existant.  
  
-   Source de données connectée dans le flux de données  
  
     Vous utilisez la transformation du cache pour écrire des données provenant d'une source de données connectée dans le flux de données dans un gestionnaire de connexions du cache. Les données sont toujours stockées en mémoire.  
  
     Vous devez ajouter la transformation de recherche à un flux de données séparé. Cet ajout permet à la transformation du cache de renseigner le gestionnaire de connexions du cache avant l'exécution de la transformation de recherche. Les flux de données peuvent être dans le même package ou dans deux packages séparés.  
  
     Si les flux de données sont dans le même package, utilisez une contrainte de précédence pour connecter les flux de données. Cette contrainte permet à la transformation du cache de s'exécuter avant l'exécution de la transformation de recherche.  
  
     Si les flux de données sont dans des packages séparés, vous pouvez utiliser la tâche Exécuter le package pour appeler le package enfant à partir du package parent. Vous pouvez appeler plusieurs packages enfants en ajoutant plusieurs tâches Exécuter le package à une tâche Conteneur de séquences dans le package parent.  
  
 Vous pouvez partager le dataset de référence stocké dans le cache, entre plusieurs transformations de Recherche en utilisant l'une des méthodes suivantes :  
  
-   Configurez les transformations de recherche dans un seul package afin qu'elles utilisent le même gestionnaire de connexions du cache.  
  
-   Configurez les gestionnaires de connexions du cache des différents packages afin qu'ils utilisent le même fichier cache.  
  
 Pour plus d'informations, voir les rubriques suivantes :  
  
-   [Transformation du cache](../../integration-services/data-flow/transformations/cache-transform.md)  
  
-   [Gestionnaire de connexions du cache](../../integration-services/connection-manager/cache-connection-manager.md)  
  
-   [Contraintes de précédence](../../integration-services/control-flow/precedence-constraints.md)  
  
-   [Tâche d’exécution de package](../../integration-services/control-flow/execute-package-task.md)  
  
-   [conteneur de séquences](../../integration-services/control-flow/sequence-container.md)  
  
 Pour savoir comment implémenter la transformation de recherche en mode Cache complet à l’aide du gestionnaire de connexions du cache, consultez la vidéo [Procédure : implémenter une transformation de recherche en mode Cache complet (Vidéo liée à SQL Server)](/previous-versions/sql/sql-server-2008/cc952929(v=sql.100)).  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-in-one-package-by-using-cache-connection-manager-and-a-data-source-in-the-data-flow"></a>Pour implémenter une transformation de recherche en mode Cache complet dans un seul package en utilisant le gestionnaire de connexions du cache et une source de données dans le flux de données  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , puis un package.  
  
2.  Sous l’onglet **Flux de contrôle** , ajoutez deux tâches de flux de données, puis connectez les tâches à l’aide d’un connecteur vert :  
  
3.  Dans le premier flux de données, ajoutez une transformation du cache, puis connectez la transformation à une source de données.  
  
     Configurez la source de données selon vos besoins.  
  
4.  Double-cliquez sur la transformation du cache puis, dans **l’Éditeur de transformation du cache**, dans la page **Gestionnaire de connexions** , cliquez sur **Nouveau** pour créer un gestionnaire de connexions du cache.  
  
5.  Cliquez sur l’onglet **Colonnes** de la boîte de dialogue **Éditeur du gestionnaire de connexions du cache** , puis désignez les colonnes d’index à l’aide de l’option **Position d’index** .  
  
     Pour les colonnes qui ne sont pas des index, la position d'index est 0. Pour les colonnes d'index, la position d'index est un nombre séquentiel, positif.  
  
    > [!NOTE]  
    >  Lorsque la transformation de recherche est configurée pour utiliser un gestionnaire de connexions du cache, seules les colonnes d'index dans le dataset de référence peuvent être mappées avec des colonnes d'entrée. En outre, toutes les colonnes d'index doivent être mappées. Pour plus d’informations, consultez [Éditeur du gestionnaire de connexions du cache](./cache-connection-manager.md).  
  
6.  Pour enregistrer le cache dans un fichier, dans **l’Éditeur du gestionnaire de connexions du cache**, sous l’onglet **Général** , configurez le gestionnaire de connexions du cache en définissant les options suivantes :  
  
    -   Sélectionnez **Utiliser le cache de fichier**.  
  
    -   Pour **Nom de fichier**, tapez le chemin du fichier ou cliquez sur **Parcourir** pour sélectionner le fichier.  
  
         Si vous tapez un chemin d'accès de fichier qui n'existe pas, le système crée le fichier lorsque vous exécutez le package.  
  
    > [!NOTE]  
    >  Le niveau de protection du package ne s'applique pas au fichier cache. Si le fichier cache contient des informations sensibles, utilisez une liste de contrôle d'accès (ACL) pour restreindre l'accès à l'emplacement ou au dossier dans lequel vous stockez le fichier. Vous devez autoriser l'accès à certains comptes uniquement. Pour plus d’informations, consultez [Accéder aux fichiers utilisés par des packages](../../integration-services/security/security-overview-integration-services.md#files).  
  
7.  Configurez la transformation du cache selon vos besoins. Pour plus d’informations, consultez [Éditeur de transformation du cache &#40;page Gestionnaire de connexions&#41;](../data-flow/transformations/cache-transform.md) et [Éditeur de transformation du cache &#40;page Mappages&#41;](../data-flow/transformations/cache-transform.md).  
  
8.  Dans le second flux de données, ajoutez une transformation de recherche, puis configurez la transformation en effectuant les tâches suivantes :  
  
    1.  Connectez la transformation de recherche au flux de données en faisant glisser un connecteur à partir d'une source ou d'une transformation précédente jusqu'à la transformation de recherche.  
  
        > [!NOTE]  
        >  Une transformation de recherche peut ne pas se valider si cette transformation se connecte à un fichier plat qui contient un champ Date vide. La validation de la transformation dépend si le gestionnaire de connexions pour le fichier plat a été configuré pour conserver des valeurs NULL. Pour que la validation de la transformation de recherche soit validée, dans **l’Éditeur de source de fichier plat**, dans la **page Gestionnaire de connexions**, sélectionnez l’option **Conserver les valeurs NULL de la source comme valeurs NULL dans le flux de données** .  
  
    2.  Double-cliquez sur la transformation source ou précédente pour configurer le composant.  
  
    3.  Double-cliquez sur la transformation de recherche, puis dans **l’Éditeur de transformation de recherche**, dans la page **Général** , sélectionnez **Cache complet**.  
  
    4.  Dans la zone **Type de connexion** , sélectionnez **Gestionnaire de connexions du cache**.  
  
    5.  Dans la liste **Spécifier comment gérer les lignes sans entrées correspondantes** , sélectionnez une option de gestion des erreurs.  
  
    6.  Dans la page **Connexion** , dans la liste **Gestionnaire de connexions du cache** , sélectionnez un gestionnaire de connexions du cache.  
  
    7.  Cliquez sur la page **Colonnes** , puis faites glisser au moins une colonne de la liste **Colonnes d’entrée disponibles** vers une colonne de la liste **Colonnes de recherche disponibles** .  
  
        > [!NOTE]  
        >  La transformation de recherche mappe automatiquement les colonnes ayant le même nom et le même type de données.  
  
        > [!NOTE]  
        >  Les types de données des colonnes doivent correspondre pour que les colonnes puissent être mappées. Pour plus d’informations, consultez [Types de données Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
    8.  Dans la liste **Colonnes de recherche disponibles** , sélectionnez des colonnes. Ensuite, dans la liste **Opération de recherche** , indiquez si les valeurs des colonnes de recherche doivent remplacer les valeurs des colonnes d’entrée ou si elles sont écrites dans une nouvelle colonne.  
  
    9. Pour configurer la sortie d’erreur, cliquez sur la page **Sortie d’erreur** et définissez les options de gestion des erreurs. Pour plus d’informations, consultez [Éditeur de transformation de recherche &#40;page Sortie d’erreur&#41;](../data-flow/transformations/lookup-transformation.md).  
  
    10. Cliquez sur **OK** pour enregistrer les modifications que vous avez apportées à la transformation de recherche.  
  
9. Exécutez le package.  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-in-two-packages-by-using-cache-connection-manager-and-a-data-source-in-the-data-flow"></a>Pour implémenter une transformation de recherche en mode Cache complet dans deux packages en utilisant un gestionnaire de connexions du cache et une source de données dans le flux de données  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , puis deux packages.  
  
2.  Sous l'onglet Flux de contrôle dans chaque package, ajoutez une tâche de flux de données.  
  
3.  Dans le package parent, ajoutez une transformation du cache au flux de données, puis connectez la transformation à une source de données.  
  
     Configurez la source de données selon vos besoins.  
  
4.  Double-cliquez sur la transformation du cache puis, dans **l’Éditeur de transformation du cache**, dans la page **Gestionnaire de connexions** , cliquez sur **Nouveau** pour créer un gestionnaire de connexions du cache.  
  
5.  Dans **l’Éditeur du gestionnaire de connexions du cache**, sous l’onglet **Général** , configurez le gestionnaire de connexions du cache en définissant les options suivantes :  
  
    -   Sélectionnez **Utiliser le cache de fichier**.  
  
    -   Pour **Nom de fichier**, tapez le chemin du fichier ou cliquez sur **Parcourir** pour sélectionner le fichier.  
  
         Si vous tapez un chemin d'accès de fichier qui n'existe pas, le système crée le fichier lorsque vous exécutez le package.  
  
    > [!NOTE]  
    >  Le niveau de protection du package ne s'applique pas au fichier cache. Si le fichier cache contient des informations sensibles, utilisez une liste de contrôle d'accès (ACL) pour restreindre l'accès à l'emplacement ou au dossier dans lequel vous stockez le fichier. Vous devez autoriser l'accès à certains comptes uniquement. Pour plus d’informations, consultez [Accéder aux fichiers utilisés par des packages](../../integration-services/security/security-overview-integration-services.md#files).  
  
6.  Cliquez sur l’onglet **Colonnes** , puis spécifiez quelles colonnes sont les colonnes d’index en utilisant l’option **Position d’index** .  
  
     Pour les colonnes qui ne sont pas des index, la position d'index est 0. Pour les colonnes d'index, la position d'index est un nombre séquentiel, positif.  
  
    > [!NOTE]  
    >  Lorsque la transformation de recherche est configurée pour utiliser un gestionnaire de connexions du cache, seules les colonnes d'index dans le dataset de référence peuvent être mappées avec des colonnes d'entrée. En outre, toutes les colonnes d'index doivent être mappées. Pour plus d’informations, consultez [Éditeur du gestionnaire de connexions du cache](./cache-connection-manager.md).  
  
7.  Configurez la transformation du cache selon vos besoins. Pour plus d’informations, consultez [Éditeur de transformation du cache &#40;page Gestionnaire de connexions&#41;](../data-flow/transformations/cache-transform.md) et [Éditeur de transformation du cache &#40;page Mappages&#41;](../data-flow/transformations/cache-transform.md).  
  
8.  Effectuez l'une des opérations suivantes pour renseigner le gestionnaire de connexions du cache utilisé dans le second package :  
  
    -   Exécutez le package parent.  
  
    -   Double-cliquez sur le gestionnaire de connexions du cache que vous avez créé à l’étape 4, cliquez sur **Colonnes**, sélectionnez les lignes, puis appuyez sur Ctrl+C pour copier les métadonnées de colonne.  
  
9. Dans le package enfant, pour créer un gestionnaire de connexions du cache, cliquez avec le bouton droit dans la zone **Gestionnaires de connexions** , cliquez sur **Nouvelle connexion**, sélectionnez **CACHE** dans la boîte de dialogue **Ajout d’un gestionnaire de connexions SSIS** , puis cliquez sur **Ajouter**.  
  
     La zone **Gestionnaires de connexions** apparaît au bas des onglets **Flux de contrôle**, **Flux de données** et **Gestionnaires d’événements** du concepteur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
10. Dans **l’Éditeur du gestionnaire de connexions du cache**, sous l’onglet **Général** , configurez le gestionnaire de connexions du cache pour lire les données à partir du fichier cache que vous avez sélectionné en définissant les options suivantes :  
  
    -   Sélectionnez **Utiliser le cache de fichier**.  
  
    -   Pour **Nom de fichier**, tapez le chemin du fichier ou cliquez sur **Parcourir** pour sélectionner le fichier.  
  
    > [!NOTE]  
    >  Le niveau de protection du package ne s'applique pas au fichier cache. Si le fichier cache contient des informations sensibles, utilisez une liste de contrôle d'accès (ACL) pour restreindre l'accès à l'emplacement ou au dossier dans lequel vous stockez le fichier. Vous devez autoriser l'accès à certains comptes uniquement. Pour plus d’informations, consultez [Accéder aux fichiers utilisés par des packages](../../integration-services/security/security-overview-integration-services.md#files).  
  
11. Si vous avez copié les métadonnées de colonne à l’étape 8, cliquez sur **Colonnes**, sélectionnez la ligne vide, puis appuyez sur Ctrl+V pour coller les métadonnées de colonne.  
  
12. Ajoutez une transformation de recherche, puis configurez la transformation en effectuant les tâches suivantes :  
  
    1.  Connectez la transformation de recherche au flux de données en faisant glisser un connecteur à partir d'une source ou d'une transformation précédente jusqu'à la transformation de recherche.  
  
        > [!NOTE]  
        >  Une transformation de recherche peut ne pas se valider si cette transformation se connecte à un fichier plat qui contient un champ Date vide. La validation de la transformation dépend si le gestionnaire de connexions pour le fichier plat a été configuré pour conserver des valeurs NULL. Pour que la validation de la transformation de recherche soit validée, dans **l’Éditeur de source de fichier plat**, dans la **page Gestionnaire de connexions**, sélectionnez l’option **Conserver les valeurs NULL de la source comme valeurs NULL dans le flux de données** .  
  
    2.  Double-cliquez sur la transformation source ou précédente pour configurer le composant.  
  
    3.  Double-cliquez sur la transformation de recherche, puis sélectionnez **Cache complet** dans la page **Général** de **l’Éditeur de transformation de recherche**.  
  
    4.  Sélectionnez **Gestionnaire de connexions du cache** dans la zone **Type de connexion** .  
  
    5.  Sélectionnez une option de gestion des erreurs pour les lignes sans entrées correspondantes dans la liste **Spécifier comment gérer les lignes sans entrées correspondantes** .  
  
    6.  Dans la page **Connexion** , dans la liste **Gestionnaire de connexions du cache** , sélectionnez le gestionnaire de connexions du cache que vous avez ajouté.  
  
    7.  Cliquez sur la page **Colonnes** , puis faites glisser au moins une colonne de la liste **Colonnes d’entrée disponibles** vers une colonne de la liste **Colonnes de recherche disponibles** .  
  
        > [!NOTE]  
        >  La transformation de recherche mappe automatiquement les colonnes ayant le même nom et le même type de données.  
  
        > [!NOTE]  
        >  Les types de données des colonnes doivent correspondre pour que les colonnes puissent être mappées. Pour plus d’informations, consultez [Types de données Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
    8.  Dans la liste **Colonnes de recherche disponibles** , sélectionnez des colonnes. Ensuite, dans la liste **Opération de recherche** , indiquez si les valeurs des colonnes de recherche doivent remplacer les valeurs des colonnes d’entrée ou si elles sont écrites dans une nouvelle colonne.  
  
    9. Pour configurer la sortie d’erreur, cliquez sur la page **Sortie d’erreur** et définissez les options de gestion des erreurs. Pour plus d’informations, consultez [Éditeur de transformation de recherche &#40;page Sortie d’erreur&#41;](../data-flow/transformations/lookup-transformation.md).  
  
    10. Cliquez sur **OK** pour enregistrer les modifications que vous avez apportées à la transformation de recherche.  
  
13. Ouvrez le package parent, ajoutez une tâche Exécuter le package au flux de contrôle, puis configurez la tâche afin d'appeler le package enfant. Pour plus d’informations, consultez [Tâche d’exécution de package](../../integration-services/control-flow/execute-package-task.md).  
  
14. Exécutez les packages.  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-by-using-cache-connection-manager-and-an-existing-cache-file"></a>Pour implémenter une transformation de recherche en mode Cache complet à l'aide d'un gestionnaire de connexions du cache et d'un fichier cache existant  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , puis un package.  
  
2.  Cliquez avec le bouton droit dans la zone **Gestionnaires de connexions** , puis cliquez sur **Nouvelle connexion**.  
  
     La zone **Gestionnaires de connexions** apparaît au bas des onglets **Flux de contrôle**, **Flux de données** et **Gestionnaires d’événements** du concepteur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
3.  Dans la boîte de dialogue **Ajout d’un gestionnaire de connexions SSIS** , sélectionnez **CACHE**, puis cliquez sur **Ajouter** pour ajouter un gestionnaire de connexions du cache.  
  
4.  Double-cliquez sur le gestionnaire de connexions du cache pour ouvrir **l’Éditeur du gestionnaire de connexions du cache**.  
  
5.  Dans **l’Éditeur du gestionnaire de connexions du cache**, sous l’onglet **Général** , configurez le gestionnaire de connexions du cache en définissant les options suivantes :  
  
    -   Sélectionnez **Utiliser le cache de fichier**.  
  
    -   Pour **Nom de fichier**, tapez le chemin du fichier ou cliquez sur **Parcourir** pour sélectionner le fichier.  
  
    > [!NOTE]  
    >  Le niveau de protection du package ne s'applique pas au fichier cache. Si le fichier cache contient des informations sensibles, utilisez une liste de contrôle d'accès (ACL) pour restreindre l'accès à l'emplacement ou au dossier dans lequel vous stockez le fichier. Vous devez autoriser l'accès à certains comptes uniquement. Pour plus d’informations, consultez [Accéder aux fichiers utilisés par des packages](../../integration-services/security/security-overview-integration-services.md#files).  
  
6.  Cliquez sur l’onglet **Colonnes** , puis spécifiez quelles colonnes sont les colonnes d’index en utilisant l’option **Position d’index** .  
  
     Pour les colonnes qui ne sont pas des index, la position d'index est 0. Pour les colonnes d'index, la position d'index est un nombre séquentiel, positif.  
  
    > [!NOTE]  
    >  Lorsque la transformation de recherche est configurée pour utiliser un gestionnaire de connexions du cache, seules les colonnes d'index dans le dataset de référence peuvent être mappées avec des colonnes d'entrée. En outre, toutes les colonnes d'index doivent être mappées. Pour plus d’informations, consultez [Éditeur du gestionnaire de connexions du cache](./cache-connection-manager.md).  
  
7.  Sous l’onglet **Flux de contrôle** , ajoutez une tâche de flux de données au package, puis ajoutez une transformation de recherche au flux de données.  
  
8.  Configurez la transformation de recherche en procédant comme suit :  
  
    1.  Connectez la transformation de recherche au flux de données en faisant glisser un connecteur à partir d'une source ou d'une transformation précédente jusqu'à la transformation de recherche.  
  
        > [!NOTE]  
        >  Une transformation de recherche peut ne pas se valider si cette transformation se connecte à un fichier plat qui contient un champ Date vide. La validation de la transformation dépend si le gestionnaire de connexions pour le fichier plat a été configuré pour conserver des valeurs NULL. Pour que la validation de la transformation de recherche soit validée, dans **l’Éditeur de source de fichier plat**, dans la **page Gestionnaire de connexions**, sélectionnez l’option **Conserver les valeurs NULL de la source comme valeurs NULL dans le flux de données** .  
  
    2.  Double-cliquez sur la transformation source ou précédente pour configurer le composant.  
  
    3.  Double-cliquez sur la transformation de recherche, puis dans **l’Éditeur de transformation de recherche**, dans la page **Général** , sélectionnez **Cache complet**.  
  
    4.  Sélectionnez **Gestionnaire de connexions du cache** dans la zone **Type de connexion** .  
  
    5.  Sélectionnez une option de gestion des erreurs pour les lignes sans entrées correspondantes dans la liste **Spécifier comment gérer les lignes sans entrées correspondantes** .  
  
    6.  Dans la page **Connexion** , dans la liste **Gestionnaire de connexions du cache** , sélectionnez le gestionnaire de connexions du cache que vous avez ajouté.  
  
    7.  Cliquez sur la page **Colonnes** , puis faites glisser au moins une colonne de la liste **Colonnes d’entrée disponibles** vers une colonne de la liste **Colonnes de recherche disponibles** .  
  
        > [!NOTE]  
        >  La transformation de recherche mappe automatiquement les colonnes ayant le même nom et le même type de données.  
  
        > [!NOTE]  
        >  Les types de données des colonnes doivent correspondre pour que les colonnes puissent être mappées. Pour plus d’informations, consultez [Types de données Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
    8.  Dans la liste **Colonnes de recherche disponibles** , sélectionnez des colonnes. Ensuite, dans la liste **Opération de recherche** , indiquez si les valeurs des colonnes de recherche doivent remplacer les valeurs des colonnes d’entrée ou si elles sont écrites dans une nouvelle colonne.  
  
    9. Pour configurer la sortie d’erreur, cliquez sur la page **Sortie d’erreur** et définissez les options de gestion des erreurs. Pour plus d’informations, consultez [Éditeur de transformation de recherche &#40;page Sortie d’erreur&#41;](../data-flow/transformations/lookup-transformation.md).  
  
    10. Cliquez sur **OK** pour enregistrer les modifications que vous avez apportées à la transformation de recherche.  
  
9. Exécutez le package.  
  
## <a name="see-also"></a>Voir aussi  
 [Implémenter une transformation de recherche en mode Cache complet à l’aide du gestionnaire de connexions OLE DB](../../integration-services/connection-manager/lookup-transformation-full-cache-mode-ole-db-connection-manager.md)   
 [Implémenter une recherche en mode Aucun cache ou Cache partiel](../../integration-services/data-flow/transformations/implement-a-lookup-in-no-cache-or-partial-cache-mode.md)   
 [Transformations Integration Services](../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
