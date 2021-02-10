---
description: FieldEnum
title: FieldEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FieldEnum
helpviewer_keywords:
- FieldEnum enumeration [ADO]
ms.assetid: be4eda13-d4e4-4d6b-bb0d-3310b0a96fc2
author: rothja
ms.author: jroth
ms.openlocfilehash: d111bac142126b8d5c4d9ec14616c91a18e70d4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024625"
---
# <a name="fieldenum"></a>FieldEnum
Spécifie les champs spéciaux référencés dans la collection de [champs](./fields-collection-ado.md) d’un objet [enregistrement](./record-object-ado.md) .  
  
## <a name="remarks"></a>Notes  
 Ces constantes fournissent un « raccourci » pour accéder à des champs spéciaux associés à un **enregistrement**. Récupérez l’objet de [champ](./field-object.md) à partir de la collection de **champs** , puis obtenez son contenu avec la propriété [value](./value-property-ado.md) de l’objet **Field** .  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adDefaultStream**|-1|Fait référence au champ contenant l’objet de [flux](./stream-object-ado.md) par défaut associé à un **enregistrement**.|  
|**adRecordURL**|-2|Fait référence au champ contenant la chaîne d’URL absolue pour l' **enregistrement** en cours.|