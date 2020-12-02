---
description: 'Leçon 1-7 : Ajouter et configurer la destination OLE DB'
title: 'Étape 7 : Ajouter et configurer la destination OLE DB | Microsoft Docs'
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 442c841d-d528-4bf0-8724-7156f909ee50
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8c30d9c27550913f5c83334ff33ff3a1cc08e1bc
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2020
ms.locfileid: "90990408"
---
# <a name="lesson-1-7-add-and-configure-the-ole-db-destination"></a>Leçon 1-7 : Ajouter et configurer la destination OLE DB

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Votre package peut maintenant extraire des données de la source de fichier plat pour les transformer dans un format compatible avec la destination. La tâche suivante consiste à charger les données transformées dans la destination. Pour charger les données, vous ajoutez une destination OLE DB au flux de données. La destination OLE DB peut utiliser une table de base de données, une vue ou une commande SQL pour charger les données dans plusieurs bases de données compatibles OLE DB.  
  
Au cours de cette tâche, vous allez ajouter et configurer une destination OLE DB pour utiliser le gestionnaire de connexions OLE DB que vous avez créé précédemment.  
  
## <a name="add-and-configure-the-sample-ole-db-destination"></a>Ajouter et configurer la destination OLE DB fournie en exemple  
  
1.  Dans la **Boîte à outils SSIS**, développez **Autres destinations**, puis faites glisser **Destination OLE DB** dans la zone de conception de l’onglet **Flux de données** . Placez la **destination OLE DB** directement sous la transformation **Lookup Date Key**.  
  
2.  Sélectionnez la transformation **Lookup Date Key** et faites glisser sa flèche bleue vers la nouvelle **destination OLE DB** pour connecter ensemble les deux composants.  
  
3.  Dans la boîte de dialogue **Sélection entrée et sortie**, dans la zone de liste **Sortie**, sélectionnez **Sortie de recherche avec correspondance**, puis **OK**.  
  
4.  Dans l’aire de conception **Flux de données**, sélectionnez le nom **Destination OLE DB** dans le nouveau composant **Destination OLE DB**, puis remplacez ce nom par **Sample OLE DB Destination**.  
  
5.  Double-cliquez sur **Sample OLE DB Destination**.  
  
6.  Dans la boîte de dialogue **Éditeur de destination OLE DB**, vérifiez que **localhost.AdventureWorksDW2012** est sélectionné dans la zone **Gestionnaire de connexions OLE DB**.  
  
7.  Dans la zone **Nom de la table ou de la vue**, entrez ou sélectionnez **[dbo].[FactCurrencyRate]**.  
 
8.  Si une table nommée **NewFactCurrencyRate** existe déjà, supprimez-la maintenant. Vous allez créer une table dans l’étape suivante.
 
9.  Sélectionnez le bouton **Nouveau** pour créer une table.  Dans le script, remplacez le nom de la table **Sample OLE DB Destination** par **NewFactCurrencyRate**.  Sélectionnez **OK**.  
 
10. Quand vous sélectionnez **OK**, la boîte de dialogue se ferme et le **nom de la table ou de la vue** devient automatiquement **NewFactCurrencyRate**.  
  
11. Sélectionnez **Mappages**.  
  
12. Vérifiez que les colonnes d’entrée **AverageRate**, **CurrencyKey**, **EndOfDayRate** et **DateKey** sont correctement mappées aux colonnes de destination. Si des colonnes aux noms identiques sont mappées, le mappage est correct.  
  
13. Sélectionnez **OK**.  
  
14. Cliquez avec le bouton droit sur la destination **Sample OLE DB Destination**, puis sélectionnez **Propriétés**.  
  
15. Dans la fenêtre **Propriétés**, vérifiez que la propriété **LocaleID** a la valeur **Anglais (États-Unis)** et la propriété **DefaultCodePage** la valeur **1252**.  
  
## <a name="go-to-next-task"></a>Passer à la tâche suivante
[Étape 8 : Annoter et formater le package de la leçon 1](../integration-services/lesson-1-8-making-the-lesson-1-package-easier-to-understand.md)  
  
## <a name="see-also"></a>Voir aussi  
[Destination OLE DB](../integration-services/data-flow/ole-db-destination.md)  
  
  
  
