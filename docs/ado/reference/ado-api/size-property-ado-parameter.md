---
description: Size, propriété (paramètre ADO)
title: Size, propriété (paramètre ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- _Parameter::Size
helpviewer_keywords:
- Size property [ADO Parameter]
ms.assetid: e6bad449-ebdb-4dd3-886a-9e6f1e7ee5d2
author: rothja
ms.author: jroth
ms.openlocfilehash: 2bede13c00304565a0e3b7819be7c8e6a5b5ca48
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88777478"
---
# <a name="size-property-ado-parameter"></a>Size, propriété (paramètre ADO)
Indique la taille maximale, en octets ou en caractères, d’un objet de [paramètre](./parameter-object.md) .  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur de **type long** qui indique la taille maximale en octets ou en caractères d’une valeur dans un objet **Parameter** .  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **Size** pour déterminer la taille maximale des valeurs écrites ou lues à partir de la propriété [value](./value-property-ado.md) d’un objet **Parameter** .  
  
 Si vous spécifiez un type de données de longueur variable pour un objet de **paramètre** (par exemple, n’importe quel type de **chaîne** , tel que **adVarChar**), vous devez définir la propriété **Size** de l’objet avant de l’ajouter à la collection [Parameters](./parameters-collection-ado.md) . dans le cas contraire, une erreur se produit.  
  
 Si vous avez déjà ajouté l’objet de **paramètre** à la collection de **paramètres** d’un objet de [commande](./command-object-ado.md) et que vous modifiez son type en type de données de longueur variable, vous devez définir la propriété **Size** de l’objet **Parameter** avant d’exécuter l’objet **Command** . dans le cas contraire, une erreur se produit.  
  
 Si vous utilisez la méthode [Refresh](./refresh-method-ado.md) pour obtenir des informations sur les paramètres du fournisseur et qu’elle retourne un ou plusieurs objets de **paramètre** de type de données de longueur variable, ADO peut allouer de la mémoire pour les paramètres en fonction de leur taille potentielle maximale, ce qui peut provoquer une erreur pendant l’exécution. Pour éviter une erreur, vous devez définir explicitement la propriété **Size** pour ces paramètres avant d’exécuter la commande.  
  
 La propriété **Size** est en lecture/écriture.  
  
## <a name="applies-to"></a>S'applique à  
 [Objet Parameter](./parameter-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ActiveConnection, CommandText, CommandTimeout, CommandType, size et direction, exemple de propriétés (VB)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [ActiveConnection, CommandText, CommandTimeout, CommandType, size et direction, exemple de propriétés (VC + +)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [ActiveConnection, CommandText, CommandTimeout, CommandType, size et direction, exemple de propriétés (JScript)](./activeconnection-commandtext-timeout-type-size-example-jscript.md)   
 [Size, propriété (objet Stream ADO)](./size-property-ado-stream.md)