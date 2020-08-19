---
description: Mise à jour des données
title: Mise à jour des données | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data updates [ADO], about data updates
- updating data [ADO], about updating data
ms.assetid: 6508e4e9-e33a-4dad-b340-5d632fd78a91
author: rothja
ms.author: jroth
ms.openlocfilehash: 5ea884479e7353b822e8b679a0fff34d70c3fd93
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88452641"
---
# <a name="updating-data"></a>Mise à jour des données
Le comportement et les fonctionnalités de mise à jour dépendent en grande partie du mode de mise à jour (type de verrou), du type de curseur et de l’emplacement du curseur.  
  
 Utilisez la méthode **Update** pour enregistrer toutes les modifications que vous avez apportées à l’enregistrement actuel d’un objet **Recordset** depuis l’appel de la méthode **AddNew** ou depuis la modification des valeurs de champ dans un enregistrement existant. L’objet **Recordset** doit prendre en charge les mises à jour.  
  
 Si l’objet **Recordset** prend en charge la mise à jour par lot, vous pouvez mettre en cache plusieurs modifications dans un ou plusieurs enregistrements localement jusqu’à ce que vous appeliez la méthode **UpdateBatch** . Si vous modifiez l’enregistrement en cours ou si vous ajoutez un nouvel enregistrement lorsque vous appelez la méthode **UpdateBatch** , ADO appelle automatiquement la méthode **Update** pour enregistrer toutes les modifications en attente dans l’enregistrement en cours avant de transmettre les modifications par lot au fournisseur.  
  
 L’enregistrement actif reste actif après l’appel des méthodes **Update** ou **UpdateBatch** .  
  
 Cette section contient les rubriques suivantes :  
  
-   [Mode immédiat](../../../ado/guide/data/immediate-mode.md)  
  
-   [Mode Batch](../../../ado/guide/data/batch-mode.md)  
  
-   [Traitement transactionnel](../../../ado/guide/data/transaction-processing.md)
