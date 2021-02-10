---
description: ActualSize, propriété (ADO)
title: ActualSize, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::ActualSize
helpviewer_keywords:
- ActualSize property [ADO]
ms.assetid: 722803d0-cef5-4d4c-b79d-3f2f58052229
author: rothja
ms.author: jroth
ms.openlocfilehash: 3fa2172e43b1c3af015b8483a8740641a0e443d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035859"
---
# <a name="actualsize-property-ado"></a>ActualSize, propriété (ADO)
Indique la longueur réelle de la valeur d’un champ, en octets.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Retourne une valeur de **type long** .  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **ActualSize** pour retourner la longueur réelle de la valeur d’un objet [Field](./field-object.md) . Pour tous les champs, la propriété **ActualSize** est en lecture seule. Si ADO ne peut pas déterminer la longueur de la valeur de l’objet **Field** , la propriété **ActualSize** retourne **adUnknown**.  
  
 Les propriétés **ActualSize** et [DefinedSize](./definedsize-property.md) sont différentes, comme illustré dans l’exemple suivant. Un objet de **champ** avec un type déclaré d' **adVarChar** et une longueur maximale de 50 caractères retourne une valeur de propriété **DefinedSize** de 50, mais la valeur de propriété **ActualSize** renvoyée correspond à la longueur des données stockées dans le champ pour l’enregistrement actif. Les **champs** avec une valeur **DefinedSize** supérieure à 255 octets sont traités comme des colonnes de longueur variable.  
  
## <a name="applies-to"></a>S'applique à  
 [Objet Field](./field-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ActualSize et DefinedSize, exemple de propriétés (VB)](./actualsize-and-definedsize-properties-example-vb.md)   
 [ActualSize et DefinedSize, exemple de propriétés (VC + +)](./actualsize-and-definedsize-properties-example-vc.md)   
 [DefinedSize, propriété](./definedsize-property.md)