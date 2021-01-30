---
description: ADCPROP_UPDATECRITERIA_ENUM
title: ADCPROP_UPDATECRITERIA_ENUM | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADCPROP_UPDATECRITERIA_ENUM
helpviewer_keywords:
- ADCPROP_UPDATECRITERIA_ENUM [ADO]
ms.assetid: 33fd7b65-2ec8-4f62-91a7-630b5dab1aa2
author: rothja
ms.author: jroth
ms.openlocfilehash: 32f58d13583ecdb92e805f0400f88ed1b9110563
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161758"
---
# <a name="adcprop_updatecriteria_enum"></a>ADCPROP_UPDATECRITERIA_ENUM
Spécifie les champs qui peuvent être utilisés pour détecter les conflits pendant une mise à jour optimiste d’une ligne de la source de données avec un objet [Recordset](./recordset-object-ado.md) .  
  
 Utilisez ces constantes avec la propriété dynamique «**critères de mise à jour**» du **jeu d’enregistrements** , qui est référencée dans l' [index de propriété dynamique ADO](./ado-dynamic-property-index.md) et documentée dans la documentation du [service de curseur Microsoft pour OLE DB](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) .  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adCriteriaAllCols**|1|Détecte les conflits si une colonne de la ligne de la source de données a été modifiée.|  
|**adCriteriaKey**|0|Détecte les conflits si la colonne clé de la ligne de la source de données a été modifiée, ce qui signifie que la ligne a été supprimée.|  
|**adCriteriaTimeStamp**|3|Détecte les conflits si l’horodateur de la ligne de la source de données a été modifié, ce qui signifie que la ligne a fait l’objet d’un accès après l’obtention du **Recordset** .|  
|**adCriteriaUpdCols**|2|Détecte les conflits si des colonnes de la ligne de source de données qui correspondent à des champs mis à jour du **Recordset** ont été modifiées.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums.AdcPropUpdateCriteria.ALLCOLS|  
|AdoEnums. AdcPropUpdateCriteria. KEY|  
|AdoEnums.AdcPropUpdateCriteria.TIMESTAMP|  
|AdoEnums.AdcPropUpdateCriteria.UPDCOLS|