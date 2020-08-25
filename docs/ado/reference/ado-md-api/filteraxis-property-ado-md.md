---
description: FilterAxis, propriété (ADO MD)
title: FilterAxis, propriété (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Cellset::FilterAxis
- FilterAxis
helpviewer_keywords:
- FilterAxis property [ADO MD]
ms.assetid: 9c656963-531e-4cd1-b698-d5f42a9b7ba3
author: rothja
ms.author: jroth
ms.openlocfilehash: 0100363a013dc3ea3435ca205822a4f88b56735b
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88778138"
---
# <a name="filteraxis-property-ado-md"></a>FilterAxis, propriété (ADO MD)
Indique les informations de filtre relatives à l' [Cellset](./cellset-object-ado-md.md)actuel.  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne un objet [Axis](./axis-object-ado-md.md) , et est en lecture seule.  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **FilterAxis** pour retourner des informations sur les dimensions qui ont été utilisées pour découper les données. La propriété [DimensionCount](./dimensioncount-property-ado-md.md) de l' **axe** retourne le nombre de dimensions de découpage. Cet axe n’a généralement qu’une seule ligne.  
  
 L' **axe** retourné par **FilterAxis** n’est pas contenu dans la collection [axes](./axes-collection-ado-md.md) d’un objet [Cellset](./cellset-object-ado-md.md) .  
  
## <a name="applies-to"></a>S'applique à  
 [Cellset, objet (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [AXIS, objet (ADO MD)](./axis-object-ado-md.md)   
 [Objet dimension (ADO MD)](./dimension-object-ado-md.md)   
 [DimensionCount, propriété (ADO MD)](./dimensioncount-property-ado-md.md)