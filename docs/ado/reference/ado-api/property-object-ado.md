---
description: Property, objet (ADO)
title: Property, objet (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Property
helpviewer_keywords:
- Property object [ADO]
ms.assetid: b2a4767c-03c7-4935-a3bc-df3e1a38a009
author: rothja
ms.author: jroth
ms.openlocfilehash: a24503889e8bd7deab3bca6072da9114f0a0efdd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170527"
---
# <a name="property-object-ado"></a>Property, objet (ADO)
Représente une caractéristique dynamique d’un objet ADO qui est défini par le fournisseur.  
  
## <a name="remarks"></a>Notes  
 Les objets ADO ont deux types de propriétés : intégré et dynamique.  
  
 Les propriétés intégrées sont les propriétés implémentées dans ADO et immédiatement disponibles pour tout nouvel objet, à l’aide de la `MyObject.Property` syntaxe. Ils n’apparaissent pas en tant qu’objets de **propriété** dans la collection de [Propriétés](./properties-collection-ado.md) d’un objet. par conséquent, bien que vous puissiez modifier leurs valeurs, vous ne pouvez pas modifier leurs caractéristiques.  
  
 Les propriétés dynamiques sont définies par le fournisseur de données sous-jacent et s’affichent dans la collection **Properties** pour l’objet ADO approprié. Par exemple, une propriété spécifique au fournisseur peut indiquer si un objet [Recordset](./recordset-object-ado.md) prend en charge les transactions ou la mise à jour. Ces propriétés supplémentaires s’affichent sous la forme d’objets de **propriété** dans la collection de **Propriétés** de cet objet **Recordset** . Les propriétés dynamiques peuvent être référencées uniquement par l’intermédiaire de la collection, à l’aide de la `MyObject.Properties(0)` `MyObject.Properties("Name")` syntaxe ou.  
  
 Vous ne pouvez pas supprimer un type de propriété.  
  
 Un objet de **propriété** dynamique a quatre propriétés intégrées :  
  
-   La propriété [Name](./name-property-ado.md) est une chaîne qui identifie la propriété.  
  
-   La propriété de [type](./type-property-ado.md) est un entier qui spécifie le type de données de la propriété.  
  
-   La propriété [valeur](./value-property-ado.md) est un variant qui contient le paramètre de propriété. La **valeur** est la propriété par défaut d’un objet de **propriété** .  
  
-   La propriété [attributes](./attributes-property-ado.md) est une valeur de type long qui indique les caractéristiques de la propriété spécifique au fournisseur.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de l’objet Property](./property-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Command, objet (ADO)](./command-object-ado.md)   
 [Connection, objet (ADO)](./connection-object-ado.md)   
 [Field, objet](./field-object.md)   
 [Properties, collection (ADO)](./properties-collection-ado.md)   
 [Recordset, objet (ADO)](./recordset-object-ado.md)