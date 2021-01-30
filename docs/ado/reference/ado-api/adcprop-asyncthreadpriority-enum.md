---
description: ADCPROP_ASYNCTHREADPRIORITY_ENUM
title: ADCPROP_ASYNCTHREADPRIORITY_ENUM | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADCPROP_ASYNCTHREADPRIORITY_ENUM
helpviewer_keywords:
- ADCPROP_ASYNCTHREADPRIORITY_ENUM [ADO]
ms.assetid: f0965617-17d8-41e0-98d0-f824274735a6
author: rothja
ms.author: jroth
ms.openlocfilehash: a0e9a8fff8888268587b1a2d88fa64ee5a66dba6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161773"
---
# <a name="adcprop_asyncthreadpriority_enum"></a>ADCPROP_ASYNCTHREADPRIORITY_ENUM
Pour un objet [Recordset](./recordset-object-ado.md) RDS, spécifie la priorité d’exécution du thread asynchrone qui récupère les données.  
  
 Utilisez ces constantes avec la propriété dynamique «**priorité des threads d’arrière-plan**» du **jeu d’enregistrements** , qui est référencée dans l’index de propriété dynamique ADO-to-OLE DB et documentée dans la documentation du [Service de curseur Microsoft pour OLE DB](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) .  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adPriorityAboveNormal**|4|Définit la priorité entre le paramètre normal et le niveau le plus élevé.|  
|**adPriorityBelowNormal**|2|Définit la priorité entre le niveau le plus bas et le niveau normal.|  
|**adPriorityHighest**|5|Définit la priorité sur la valeur la plus élevée possible.|  
|**AdPriorityLowest**|1|Définit la priorité sur la valeur la plus faible possible.|  
|**adPriorityNormal**|3|Définit la priorité sur normal.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums.AdcPropAsyncThreadPriority.ABOVENORMAL|  
|AdoEnums.AdcPropAsyncThreadPriority.BELOWNORMAL|  
|AdoEnums.AdcPropAsyncThreadPriority.HIGHEST|  
|AdoEnums.AdcPropAsyncThreadPriority.LOWEST|  
|AdoEnums.AdcPropAsyncThreadPriority.NORMAL|