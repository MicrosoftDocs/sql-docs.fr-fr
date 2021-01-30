---
description: InheritTypeEnum
title: InheritTypeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- InheritTypeEnum
helpviewer_keywords:
- InheritTypeEnum enumeration [ADOX]
ms.assetid: c2f6ce79-c4b3-4d40-ac95-21025208f991
author: rothja
ms.author: jroth
ms.openlocfilehash: d758c78b8778d5a00c0105c8c9dcebfe40da22e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164192"
---
# <a name="inherittypeenum"></a>InheritTypeEnum
Spécifie la manière dont les objets héritent des autorisations définies avec [SetPermissions](./setpermissions-method-adox.md).  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adInheritBoth**|3|Les objets et les autres conteneurs contenus dans l’objet principal héritent de l’entrée.|  
|**adInheritContainers**|2|Les autres conteneurs qui sont contenus par l’objet principal héritent de l’entrée.|  
|**adInheritNone**|0|Par défaut. Aucun héritage n’a lieu.|  
|**adInheritNoPropagate**|4|Les indicateurs **adInheritObjects** et **adInheritContainers** ne sont pas propagés à une entrée héritée.|  
|**adInheritObjects**|1|Les objets qui ne sont pas des conteneurs dans le conteneur héritent des autorisations.|  
  
## <a name="applies-to"></a>S'applique à  
 [SetPermissions, méthode (ADOX)](./setpermissions-method-adox.md)