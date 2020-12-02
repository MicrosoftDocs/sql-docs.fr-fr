---
description: 'Leçon 1-5 : Ajouter et configurer la source du fichier plat'
title: 'Étape 5 : Ajouter et configurer la source de fichier plat | Microsoft Docs'
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 5c95ce51-e0fe-4fc5-95eb-2945929f2b13
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cfc6ffa21bf3f6c1205b1c9c667cc6e75868cece
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2020
ms.locfileid: "88390865"
---
# <a name="lesson-1-5-add-and-configure-the-flat-file-source"></a>Leçon 1-5 : Ajouter et configurer la source du fichier plat

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Au cours de cette tâche, vous allez ajouter et configurer une source de fichier plat à votre package. Une source de fichier plat est un composant de flux de données qui utilise des métadonnées définies par un gestionnaire de connexions de fichiers plats. Ces métadonnées spécifient le format et la structure des données à extraire du fichier plat par un processus de transformation. La source de fichier plat extrait les données d’un seul fichier plat, en utilisant les définitions de format indiquées dans le gestionnaire de connexions de fichiers plats.  
  
Au cours de cette tâche, vous configurez la source de fichier plat pour qu’elle utilise le gestionnaire de connexions de l’**exemple de données sources de fichier plat** que vous avez préalablement créé.  
  
## <a name="add-a-flat-file-source-component"></a>Ajouter un composant source de fichier plat  
  
1.  Pour ouvrir le concepteur **Flux de données**, double-cliquez sur la tâche de flux de données **Extract Sample Currency Data** ou sélectionnez l’onglet **Flux de données**.  
  
2.  Dans la **Boîte à outils SSIS**, développez **Autres sources**, puis faites glisser une **source de fichier plat** sur l’aire de conception de l’onglet **Flux de données** .  
  
3.  Dans l’aire de conception **Flux de données**, cliquez avec le bouton droit sur la **source du fichier plat** que vous venez d’ajouter, sélectionnez **Renommer** et remplacez le nom par **Extract Sample Currency Data**.  
  
4.  Double-cliquez sur la source de fichier plat pour ouvrir la boîte de dialogue **Éditeur de source de fichier plat**.  
  
5.  Dans le champ **Gestionnaire de connexions de fichiers plats**, sélectionnez **Sample Flat File Source Data**.  
  
6.  Sélectionnez **Colonnes** et vérifiez si les noms des colonnes sont corrects.  
  
7.  Sélectionnez **OK**.  
  
8.  Cliquez avec le bouton droit sur la source de fichier plat, puis sélectionnez **Propriétés**.  
  
9. Dans la fenêtre **Propriétés**, vérifiez que la propriété **LocaleID** a la valeur **Anglais (États-Unis)** .  
  
## <a name="go-to-next-task"></a>Passer à la tâche suivante
[Étape 6 : Ajouter et configurer les transformations de recherche](../integration-services/lesson-1-6-adding-and-configuring-the-lookup-transformations.md)  
  
## <a name="see-also"></a>Voir aussi  
[Source de fichier plat](../integration-services/data-flow/flat-file-source.md)  
[Gestionnaire de connexions de fichiers plats](../integration-services/connection-manager/flat-file-connection-manager.md)  
  
  
  
