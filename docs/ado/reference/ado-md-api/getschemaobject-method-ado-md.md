---
description: GetSchemaObject, méthode (ADO MD)
title: Méthode GetSchemaObject (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- GetSchemaObject
- Cellset::GetSchemaObject
helpviewer_keywords:
- GetSchemaObject method [ADO MD]
ms.assetid: 36b754b4-6b17-4dd1-a925-bca46938b7c4
author: rothja
ms.author: jroth
ms.openlocfilehash: 4031b9dbe100df7e73c64354888ffa97cf38c30c
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88986690"
---
# <a name="getschemaobject-method-ado-md"></a>GetSchemaObject, méthode (ADO MD)
Récupère un objet de schéma ADO MD ([dimension](./dimension-object-ado-md.md), [hiérarchie](./hierarchy-object-ado-md.md), [niveau](./level-object-ado-md.md)ou [membre](./member-object-ado-md.md)) par son [UniqueName](./uniquename-property-ado-md.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Set object = CubeDef.GetSchemaObject (ObjType, UniqueName)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *ObjType*  
 Valeur [SchemaObjectTypeEnum](./schemaobjecttypeenum.md) spécifiant le type d’objet de schéma (dimension, hiérarchie, niveau ou membre) à récupérer.  
  
 *UniqueName*  
 **Chaîne** spécifiant la valeur de la propriété **UniqueName** de l’objet à récupérer.  
  
## <a name="remarks"></a>Notes  
 **GetSchemaObject** récupère les objets à l’aide de leurs noms uniques, comme spécifié par la propriété **UniqueName** . Les noms des objets parents n’ont pas besoin d’être connus, et les collections parentes n’ont pas besoin d’être remplies pour récupérer un objet de schéma.  
  
## <a name="applies-to"></a>S'applique à  
 [CubeDef, objet (ADO MD)](./cubedef-object-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [CubeDef, objet (ADO MD)](./cubedef-object-ado-md.md)