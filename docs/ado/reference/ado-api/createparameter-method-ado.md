---
description: CreateParameter, méthode (ADO)
title: CreateParameter, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::raw_CreateParameter
- Command15::CreateParameter
helpviewer_keywords:
- CreateParameter method [RDS]
ms.assetid: 9666fdcc-0544-4ed7-a97b-c415f2a56d7e
author: rothja
ms.author: jroth
ms.openlocfilehash: c5156f48f17d2d389646f4f752033cf88faf83f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025963"
---
# <a name="createparameter-method-ado"></a>CreateParameter, méthode (ADO)
Crée un nouvel objet de [paramètre](./parameter-object.md) avec les propriétés spécifiées.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Set parameter = command.CreateParameter (Name, Type, Direction, Size, Value)  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne un objet de **paramètre** .  
  
#### <a name="parameters"></a>Paramètres  
 *Nom*  
 facultatif. Valeur de **chaîne** qui contient le nom de l’objet de **paramètre** .  
  
 *Type*  
 facultatif. Valeur [DataTypeEnum](./datatypeenum.md) qui spécifie le type de données de l’objet de **paramètre** .  
  
 *Sens*  
 facultatif. Valeur [ParameterDirectionEnum](./parameterdirectionenum.md) qui spécifie le type d’objet de **paramètre** .  
  
 *Taille*  
 facultatif. Valeur de **type long** qui spécifie la longueur maximale de la valeur de paramètre en caractères ou en octets.  
  
 *Valeur*  
 facultatif. **Variant** qui spécifie la valeur de l’objet **Parameter** .  
  
## <a name="remarks"></a>Notes  
 Utilisez la méthode **CreateParameter** pour créer un nouvel objet de **paramètre** avec un nom, un type, une direction, une taille et une valeur spécifiés. Toutes les valeurs que vous transmettez dans les arguments sont écrites dans les propriétés de **paramètre** correspondantes.  
  
 Cette méthode n’ajoute pas automatiquement l’objet de **paramètre** à la collection **Parameters** d’un objet [Command](./command-object-ado.md) . Cela vous permet de définir des propriétés supplémentaires dont les valeurs sont validées par ADO lorsque vous ajoutez l’objet de **paramètre** à la collection.  
  
 Si vous spécifiez un type de données de longueur variable dans l’argument de *type* , vous devez soit passer un argument de *taille* , soit définir la propriété de [taille](./size-property-ado-parameter.md) de l’objet de **paramètre** avant de l’ajouter à la collection de **paramètres** ; dans le cas contraire, une erreur se produit.  
  
 Si vous spécifiez un type de données numérique (**adNumeric** ou **adDecimal**) dans l’argument de *type* , vous devez également définir les propriétés [NumericScale](./numericscale-property-ado.md) et [PRECISION](./precision-property-ado.md) .  
  
## <a name="applies-to"></a>S'applique à  
 [Command, objet (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Append et CreateParameter, exemple de méthodes (VB)](./append-and-createparameter-methods-example-vb.md)   
 [Append et CreateParameter, exemples de méthodes (VC + +)](./append-and-createparameter-methods-example-vc.md)   
 [Append, méthode (ADO)](./append-method-ado.md)   
 [Parameter (objet)](./parameter-object.md)   
 [Parameters, collection (ADO)](./parameters-collection-ado.md)