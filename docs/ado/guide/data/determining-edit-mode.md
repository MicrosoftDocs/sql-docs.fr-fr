---
description: Détermination du mode d’édition
title: Détermination du mode d’édition | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- editing data [ADO], edit mode
- ADO, editing data
ms.assetid: 4c7e010d-08cd-4e22-9b32-23c36f02f88c
author: rothja
ms.author: jroth
ms.openlocfilehash: 1b0a6d6642e17841b8a974efa8d497cdc12203fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033249"
---
# <a name="determining-edit-mode"></a>Détermination du mode d’édition
ADO gère un tampon d’édition associé à l’enregistrement actif. La propriété **EditMode** indique si des modifications ont été apportées à cette mémoire tampon ou si un nouvel enregistrement a été créé. Utilisez **EditMode** pour déterminer l’état de modification de l’enregistrement en cours. Vous pouvez tester les modifications en attente si un processus de modification a été interrompu et déterminer si vous devez utiliser la méthode **Update** ou **CancelUpdate** .  
  
 **EditMode** retourne l’une des constantes **EditModeEnum** , qui sont répertoriées dans le tableau suivant.  
  
|Constante|Description|  
|--------------|-----------------|  
|**adEditNone**|Indique qu’aucune opération de modification n’est en cours.|  
|**adEditInProgress**|Indique que les données de l’enregistrement en cours ont été modifiées mais pas enregistrées.|  
|**adEditAdd**|Indique que la méthode **AddNew** a été appelée et que l’enregistrement actif dans la mémoire tampon de copie est un nouvel enregistrement qui n’a pas été enregistré dans la base de données.|  
|**adEditDelete**|Indique que l’enregistrement en cours a été supprimé.|  
  
 **EditMode** peut retourner une valeur valide uniquement s’il existe un enregistrement en cours. **EditMode** retourne une erreur si **BOF** ou **EOF** a la **valeur true** ou si l’enregistrement en cours a été supprimé.
