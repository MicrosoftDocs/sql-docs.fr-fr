---
description: RuleEnum
title: RuleEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RuleEnum
helpviewer_keywords:
- RuleEnum enumeration [ADOX]
ms.assetid: 738fd3ff-3daf-483d-a0b9-88bef1be54c1
author: rothja
ms.author: jroth
ms.openlocfilehash: b1c00531161702fb15b55d0f8112c2424f38e52f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049769"
---
# <a name="ruleenum"></a>RuleEnum
Spécifie la règle à suivre lorsqu’une [clé](./key-object-adox.md) est supprimée.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adRICascade**|1|Modifications en cascade.|  
|**adRINone**|0|Par défaut. Aucune action n'est effectuée.|  
|**adRISetDefault**|3|La valeur de clé étrangère est définie sur la valeur par défaut.|  
|**adRISetNull**|2|La valeur de clé étrangère est définie sur null.|  
  
## <a name="applies-to"></a>S'applique à  
 [DeleteRule, propriété (ADOX)](./deleterule-property-adox.md)