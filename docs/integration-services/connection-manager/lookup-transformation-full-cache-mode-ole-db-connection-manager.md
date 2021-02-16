---
description: Transformation de recherche en mode Cache complet - Gestionnaire de connexions OLE DB
title: Transformation de recherche en mode Cache complet - Gestionnaire de connexions OLE DB | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Lookup transformation [Integration Services]
ms.assetid: c4150e1b-bdff-4f7a-af4c-3401c34def83
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7abcc50e3400e37219c73d8fbee0746557cc33da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350349"
---
# <a name="lookup-transformation-full-cache-mode---ole-db-connection-manager"></a>Transformation de recherche en mode Cache complet - Gestionnaire de connexions OLE DB

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Vous pouvez configurer la transformation de recherche afin qu'elle utilise le mode Cache complet et un gestionnaire de connexions OLE DB. Dans le mode Cache complet, le dataset de référence est chargé dans le cache avant l’exécution de la transformation de recherche.  
  
 La transformation de recherche effectue des recherches en joignant les données des colonnes d'entrée d'une source de données connectée aux colonnes d'un dataset de référence. Pour plus d’informations, voir [Lookup Transformation](../../integration-services/data-flow/transformations/lookup-transformation.md).  
  
 Lorsque vous configurez la transformation de recherche pour utiliser un gestionnaire de connexions OLE DB, vous sélectionnez une table, une vue ou une requête SQL pour générer le dataset de référence.  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-by-using-ole-db-connection-manager"></a>Pour implémenter une transformation de recherche en mode Cache complet à l'aide du gestionnaire de connexions OLE DB  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui contient le package souhaité, puis double-cliquez sur le package dans l'Explorateur de solutions.  
  
2.  Sous l’onglet **Flux de données** , faites glisser la transformation de recherche à partir de la **Boîte à outils** jusqu’à l’aire de conception.  
  
3.  Connectez la transformation de recherche au flux de données en faisant glisser un connecteur à partir d'une source ou d'une transformation précédente jusqu'à la transformation de recherche.  
  
    > [!NOTE]  
    >  Une transformation de recherche peut ne pas se valider si cette transformation se connecte à un fichier plat qui contient un champ Date vide. La validation de la transformation dépend si le gestionnaire de connexions pour le fichier plat a été configuré pour conserver des valeurs NULL. Pour que la validation de la transformation de recherche soit validée, dans **l’Éditeur de source de fichier plat**, dans la **page Gestionnaire de connexions**, sélectionnez l’option **Conserver les valeurs NULL de la source comme valeurs NULL dans le flux de données** .  
  
4.  Double-cliquez sur la transformation source ou précédente pour configurer le composant.  
  
5.  Double-cliquez sur la transformation de recherche, puis dans **l’Éditeur de transformation de recherche**, dans la page **Général** , sélectionnez **Cache complet**.  
  
6.  Dans la zone **Type de connexion** , sélectionnez **Gestionnaire de connexions OLE DB**.  
  
7.  Dans la liste **Spécifier comment gérer les lignes sans entrées correspondantes** , sélectionnez une option de gestion des erreurs pour les lignes sans entrées correspondantes.  
  
8.  Dans la page Connexion , sélectionnez un gestionnaire de connexions dans la liste **Gestionnaire de connexions OLE DB** ou cliquez sur **Nouveau** pour créer un gestionnaire de connexions. Pour plus d’informations, consultez [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
9. Exécutez l'une des tâches suivantes :  
  
    -   Cliquez sur **Utiliser une table ou une vue**, puis sélectionnez une table ou une vue, ou cliquez sur **Nouveau** pour créer une table ou une vue.  
  
         - ou -  
  
    -   Cliquez sur **Utiliser les résultats d’une requête SQL**, puis générez une requête dans la fenêtre **Commande SQL** , ou cliquez sur **Générer la requête** pour générer une requête à l’aide des outils graphiques du **Générateur de requêtes** .  
  
         - ou -  
  
    -   Vous pouvez aussi cliquer sur **Parcourir** pour importer une instruction SQL à partir d’un fichier.  
  
     Pour valider la requête SQL, cliquez sur **Analyser la requête**.  
  
     Pour afficher un échantillon des données, cliquez sur **Aperçu**.  
  
10. Cliquez sur la page **Colonnes** , puis faites glisser au moins une colonne de la liste **Colonnes d’entrée disponibles** vers une colonne de la liste **Colonnes de recherche disponibles** .  
  
    > [!NOTE]  
    >  La transformation de recherche mappe automatiquement les colonnes ayant le même nom et le même type de données.  
  
    > [!NOTE]  
    >  Les types de données des colonnes doivent correspondre pour que les colonnes puissent être mappées. Pour plus d’informations, consultez [Types de données Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
11. Incluez des colonnes de recherche dans la sortie en exécutant les tâches suivantes :  
  
    1.  Dans la liste **Colonnes de recherche disponibles** , sélectionnez des colonnes.  
  
    2.  Dans la liste **Opération de recherche** , spécifiez si les valeurs des colonnes de recherche remplacent les valeurs des colonnes d’entrée ou si elles sont écrites dans une nouvelle colonne.  
  
12. Pour configurer la sortie d’erreur, cliquez sur la page **Sortie d’erreur** et définissez les options de gestion des erreurs. Pour plus d’informations, consultez [Éditeur de transformation de recherche &#40;page Sortie d’erreur&#41;](../data-flow/transformations/lookup-transformation.md).  
  
13. Cliquez sur **OK** pour enregistrer les modifications apportées à la transformation de recherche, puis exécutez le package.  
  
## <a name="see-also"></a>Voir aussi  
 [Implémenter une transformation de recherche en mode Cache complet à l’aide du gestionnaire de connexions du cache](../../integration-services/connection-manager/lookup-transformation-full-cache-mode-cache-connection-manager.md)   
 [Implémenter une recherche en mode Aucun cache ou Cache partiel](../../integration-services/data-flow/transformations/implement-a-lookup-in-no-cache-or-partial-cache-mode.md)   
 [Transformations Integration Services](../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
