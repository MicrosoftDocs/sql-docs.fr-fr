---
description: Charger des données à l'aide de la destination OLE DB
title: Charger des données à l’aide de la destination OLE DB | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- loading data
- OLE DB destination [Integration Services]
- destinations [Integration Services], OLE DB
ms.assetid: 78899498-725e-4300-a7af-f983f4ea384b
author: chugugrace
ms.author: chugu
ms.openlocfilehash: bb8f2f1b0991a7141fb3b601f6ce3adb625d031f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343262"
---
# <a name="load-data-by-using-the-ole-db-destination"></a>Charger des données à l'aide de la destination OLE DB

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Pour pouvoir ajouter et configurer une destination OLE DB, le package doit inclure au moins une tâche de flux de données et une source.  
  
### <a name="to-load-data-using-an-ole-db-destination"></a>Pour charger des données à l'aide d'une destination OLE DB  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l’onglet **Flux de données** puis, dans la **Boîte à outils**, faites glisser la destination OLE DB vers l’aire de conception.  
  
4.  Connectez la destination OLE DB au flux de données en faisant glisser le connecteur à partir d'une source de données ou d'une transformation précédente vers la destination.  
  
5.  Double-cliquez sur la destination OLE DB.  
  
6.  Dans la boîte de dialogue **Éditeur de destination OLE DB** , dans la page **Gestionnaire de connexions** , sélectionnez un gestionnaire de connexions OLE DB existant ou cliquez sur **Nouveau** pour créer un gestionnaire de connexions. Pour plus d’informations, consultez [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
7.  Sélectionnez la méthode d'accès aux données :  
  
    -   **Table ou vue** : sélectionnez une table ou une vue dans la base de données qui contient les données.  
  
    -   **Table ou vue - chargement rapide** : sélectionnez une table ou une vue dans la base de données qui contient les données, puis définissez les options de chargement rapide : **Conserver l’identité**, **Conserver les valeurs NULL**, **Verrou de table**, **Contrainte de validation**, **Lignes par lot** ou **Taille de validation d’insertion maximale**.  
  
    -   **Variable de nom de table ou de vue** : sélectionnez la variable définie par l’utilisateur qui contient le nom d’une table ou d’une vue dans la base de données.  
  
    -   **Variable de nom de table ou de vue - chargement rapide** : sélectionnez une variable définie par l’utilisateur qui contient le nom d’une table ou d’une vue dans la base de données qui contient les données, puis définissez les options de chargement rapide.  
  
    -   **Commande SQL** : tapez une commande SQL ou cliquez sur **Générer la requête** pour écrire une commande SQL à l’aide du **Générateur de requêtes**.  
  
8.  Cliquez sur **Mappages** , puis mappez les colonnes de la liste **Colonnes d'entrée disponibles** aux colonnes de la liste **Colonnes de destination disponibles** en faisant glisser les colonnes d'une liste à l'autre.  
  
    > [!NOTE]  
    >  La destination OLE DB mappe automatiquement les colonnes portant le même nom.  
  
9. Pour configurer l'affichage des erreurs, cliquez sur **Sortie d'erreur**. Pour plus d’informations, consultez [Débogage d’un flux de données](../../integration-services/troubleshooting/debugging-data-flow.md).  
  
10. Cliquez sur **OK**.  
  
11. Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
## <a name="see-also"></a>Voir aussi  
 [Destination OLE DB](../../integration-services/data-flow/ole-db-destination.md)   
 [Transformations Integration Services](../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Chemins Integration Services](../../integration-services/data-flow/integration-services-paths.md)   
 [tâche de flux de données](../../integration-services/control-flow/data-flow-task.md)  
  
  
