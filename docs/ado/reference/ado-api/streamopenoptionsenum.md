---
description: StreamOpenOptionsEnum
title: StreamOpenOptionsEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- StreamOpenOptionsEnum
helpviewer_keywords:
- StreamOpenOptionsEnum enumeration [ADO]
ms.assetid: 85b6c57f-47ed-46ba-bd92-07882ae9e9d2
author: rothja
ms.author: jroth
ms.openlocfilehash: 5da222318b827eae05d07d7cad4a121a568d6540
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166418"
---
# <a name="streamopenoptionsenum"></a>StreamOpenOptionsEnum
Spécifie les options d’ouverture d’un objet de [flux](./stream-object-ado.md) . Les valeurs peuvent être combinées avec une opération OR.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adOpenStreamAsync**|1|Ouvre l’objet de **flux** en mode asynchrone.|  
|**adOpenStreamFromRecord**|4|Identifie le contenu du paramètre *source* comme étant un objet [enregistrement](./record-object-ado.md) déjà ouvert. Le comportement par défaut consiste à traiter la *source* comme une URL qui pointe directement vers un nœud dans une arborescence. Le flux par défaut associé à ce nœud est ouvert.|  
|**adOpenStreamUnspecified**|-1|Par défaut. Spécifie l’ouverture de l’objet de **flux** avec les options par défaut.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Ces constantes n’ont pas d’équivalents ADO/WFC.  
  
## <a name="applies-to"></a>S'applique à  
 [Open, méthode (Stream ADO)](./open-method-ado-stream.md)