---
description: Ordinal, propriété (objet Cell d’ADO MD)
title: Propriété ordinale (cellule ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cell::Ordinal
- Ordinal
helpviewer_keywords:
- Ordinal property [ADO MD]
ms.assetid: a6001168-b954-47f0-ba0d-c05c4cc40c58
author: rothja
ms.author: jroth
ms.openlocfilehash: 4d76ed341fe71f7a5927f6a7e84a63322adcf13f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051040"
---
# <a name="ordinal-property-ado-md-cell"></a>Ordinal, propriété (objet Cell d’ADO MD)
Identifie de façon unique une [cellule](./cell-object-ado-md.md) en fonction de sa position dans un CellSet.  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne un entier **long** et est en lecture seule.  
  
## <a name="remarks"></a>Notes  
 La valeur ordinale de la cellule identifie de façon unique la cellule dans un CellSet. Conceptuellement, les cellules sont numérotées dans un ensemble de cellules comme si l’ensemble de cellules était un tableau de dimensions *p*, où *p* est le nombre d' [axes](./axes-collection-ado-md.md). Les cellules sont numérotées à partir de zéro dans l’ordre ligne-principal. Voici la formule permettant de calculer le nombre ordinal d’une cellule :  
  
 La valeur ordinale de la cellule peut être utilisée avec la propriété [Item](./item-property-ado-md-cellset.md) de l’objet [Cellset](./cellset-object-ado-md.md) pour récupérer rapidement la [cellule](./cell-object-ado-md.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Cell, objet (ADO MD)](./cell-object-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [AXIS, exemple (VBScript)](./axis-example-vbscript.md)   
 [CellSet, objet (ADO MD)](./cellset-object-ado-md.md)   
 [Item, propriété (ADO MD CellSet)](./item-property-ado-md-cellset.md)   
 [Ordinal, propriété (objet Position d’ADO MD)](./ordinal-property-ado-md-position.md)