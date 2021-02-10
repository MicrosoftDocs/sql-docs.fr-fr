---
description: Size, propriété (objet Stream ADO)
title: Size, propriété (objet Stream ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::Size
helpviewer_keywords:
- Size property [ADO Stream]
ms.assetid: a487c241-d953-4c31-ae7e-6358d5cf6733
author: rothja
ms.author: jroth
ms.openlocfilehash: eefb2271372a6746c2df9c3f052760bb35d7f8ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040589"
---
# <a name="size-property-ado-stream"></a>Size, propriété (objet Stream ADO)
Indique la taille du flux en nombre d’octets.  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne une valeur de **type long** qui spécifie la taille du flux en nombre d’octets. La valeur par défaut est la taille du flux, ou-1 si la taille du flux n’est pas connue.  
  
## <a name="remarks"></a>Notes  
 La **taille** ne peut être utilisée qu’avec des objets de [flux](./stream-object-ado.md) ouverts.  
  
> [!NOTE]
>  Un nombre quelconque de bits peut être stocké dans un objet de **flux** , limité uniquement par les ressources système. Si le **flux** contient plus de bits que ce qui peut être représenté par une valeur **long** , la **taille** est tronquée et ne représente donc pas avec précision la longueur du **flux**.  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Size, propriété (paramètre ADO)](./size-property-ado-parameter.md)