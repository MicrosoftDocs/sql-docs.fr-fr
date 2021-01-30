---
description: SearchDirectionEnum
title: SearchDirectionEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- SearchDirectionEnum
helpviewer_keywords:
- SearchDirectionEnum enumeration [ADO]
ms.assetid: 81272ae3-2165-4f4e-adfe-9ede0368cb17
author: rothja
ms.author: jroth
ms.openlocfilehash: 957e84b3a98f0248e41a83301ef2c9a9c1943b11
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166575"
---
# <a name="searchdirectionenum"></a>SearchDirectionEnum
Spécifie la direction d’une recherche d’enregistrement dans un [Recordset](./recordset-object-ado.md).  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adSearchBackward**|-1|Recherche vers le haut, en s’arrêtant au début du **jeu d’enregistrements**. Si aucune correspondance n’est trouvée, le pointeur d’enregistrement est positionné sur [BOF](./bof-eof-properties-ado.md).|  
|**adSearchForward**|1|Recherche en avant et en arrêt à la fin de l’ensemble d' **enregistrements**. Si aucune correspondance n’est trouvée, le pointeur d’enregistrement est positionné à [EOF](./bof-eof-properties-ado.md).|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums.SearchDirection.BACKWARD|  
|AdoEnums.SearchDirection.FORWARD|  
  
## <a name="applies-to"></a>S'applique à  
 [Find, méthode (ADO)](./find-method-ado.md)