---
description: DefinedSize, propriété
title: DefinedSize, propriété | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::DefinedSize
helpviewer_keywords:
- DefinedSize property [ADO]
ms.assetid: 3ee27314-a305-4fbc-8433-9ee9a909afd6
author: rothja
ms.author: jroth
ms.openlocfilehash: 756fe40bf7916dfa3e56a3e559be2874b5e8cd85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171305"
---
# <a name="definedsize-property"></a>DefinedSize, propriété
Indique la capacité de données d’un objet de [champ](../../../ado/reference/ado-api/field-object.md) .  
  
## <a name="return-value"></a>Valeur renvoyée  
 Retourne une valeur de **type long** qui reflète la taille définie d’un champ, qui dépend du type de données de l’objet Field ; Pour plus d’informations, consultez [type](../../../ado/reference/ado-api/type-property-ado.md) . Pour un champ qui utilise un type de données de longueur fixe, la valeur de retour correspond à la taille du type de données en octets. Pour un champ qui utilise un type de données de longueur variable, il s’agit de l’un des éléments suivants :  
  
1.  Longueur maximale du champ en caractères (pour **adVarChar** et **adVarWChar**) ou en octets (pour **adVarBinary** et **adVarNumeric**) si le champ a une longueur définie. Par exemple, le champ **adVarChar (5)** a une longueur maximale de 5.  
  
2.  Longueur maximale du type de données en caractères (pour **adChar** et **adWChar**) ou en octets (pour **adBinary** et **adNumeric**) si le champ n’a pas de longueur définie.  
  
3.  ~ 0 (au niveau du bit, la valeur n’est pas 0 ; tous les bits sont définis sur 1) si le champ et le type de données n’ont pas une longueur maximale définie.  
  
4.  Pour les types de données qui n’ont pas de longueur, cette valeur est définie sur ~ 0 (au niveau du bit, la valeur n’est pas 0 ; tous les bits sont définis sur 1).  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **DefinedSize** pour déterminer la capacité de données d’un objet de **champ** .  
  
 Les propriétés **DefinedSize** et [ActualSize](../../../ado/reference/ado-api/actualsize-property-ado.md) sont différentes. Par exemple, considérez un objet de **champ** avec un type déclaré de **adVarChar** et une valeur de propriété **DefinedSize** de 50, contenant un caractère unique. La valeur de la propriété **ActualSize** renvoyée correspond à la longueur en octets du caractère unique.  
  
## <a name="applies-to"></a>S'applique à  
 [Objet Field](../../../ado/reference/ado-api/field-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ActualSize et DefinedSize, exemple de propriétés (VB)](../../../ado/reference/ado-api/actualsize-and-definedsize-properties-example-vb.md)   
 [ActualSize et DefinedSize, exemple de propriétés (VC + +)](../../../ado/reference/ado-api/actualsize-and-definedsize-properties-example-vc.md)   
 [ActualSize, propriété (ADO)](../../../ado/reference/ado-api/actualsize-property-ado.md)
