---
description: Precision, propriété (ADOX)
title: Precision, propriété (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Column::put_Precision
- _Column::PutPrecision
- _Column::GetPrecision
- _Column::get_Precision
- _Column::Precision
helpviewer_keywords:
- Precision property [ADOX]
ms.assetid: 0e0ecbbf-d7de-49d4-a128-5a519ecd54ba
author: rothja
ms.author: jroth
ms.openlocfilehash: 4ecb674380de526f729246a249e2369f0cf90e4b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164116"
---
# <a name="precision-property-adox"></a>Precision, propriété (ADOX)
Indique la précision maximale des valeurs de données dans la [colonne](./column-object-adox.md).  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit et retourne une valeur de type **long** qui correspond à la précision maximale des valeurs de données dans la colonne lorsque la propriété de [type](./type-property-column-adox.md) est un type numérique. La **précision** est ignorée pour tous les autres types de données.  
  
## <a name="remarks"></a>Notes  
 La valeur par défaut est zéro (**0**).  
  
 Cette propriété est en lecture seule pour les objets de [colonne](./column-object-adox.md) déjà ajoutés à une collection.  
  
## <a name="applies-to"></a>S'applique à  
 [Column, objet (ADOX)](./column-object-adox.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de code ADOX : NumericScale et Precision, exemple de propriétés (VB)](./adox-code-example-numericscale-and-precision-properties-example-vb.md)   
 [Type, propriété (Column) (ADOX)](./type-property-column-adox.md)   
 [Column, objet (ADOX)](./column-object-adox.md)