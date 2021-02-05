---
description: Méthode compareTo (DateTimeOffset)
title: Méthode compareTo (DateTimeOffset) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e4cf2ea4-0fe9-40ce-ba79-f2a2b616997e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 02e36e2e64a6a628fd088fab448b30ea0c238450
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176234"
---
# <a name="compareto-method-datetimeoffset"></a>Méthode compareTo (DateTimeOffset)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Compare cet objet **DateTimeOffset** à un autre objet **DateTimeOffset** en fonction de l’heure GMT.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public int compareTo(DateTimeOffset other)  
```  
  
#### <a name="parameters"></a>Paramètres  
 Valeur [DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md) à comparer à l’instance actuelle.  
  
## <a name="return-value"></a>Valeur de retour  
 Le tableau suivant décrit la valeur renvoyée par cette méthode :  
  
|Valeur retournée|Description|  
|------------------|-----------------|  
|0|Les deux objets **DateTimeOffset** représente le même point dans le temps.|  
|nombre négatif|Cet objet **DateTimeOffset** représente un point dans le temps antérieur à *other*.|  
|nombre positif|Cet objet **DateTimeOffset** représente un point dans le temps postérieur à *other*.|  
  
## <a name="remarks"></a>Notes  
 Lorsque deux objets **DateTimeOffset** ont la même heure GMT, ils ne sont pas triés en fonction du décalage.  
  
## <a name="see-also"></a>Voir aussi  
 [DateTimeOffset, classe](../../../connect/jdbc/reference/datetimeoffset-class.md)   
 [Membres de DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-members.md)  
  
  
