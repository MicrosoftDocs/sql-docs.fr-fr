---
description: Close, méthode (ADO MD)
title: Close, méthode (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Close
- Cellset::Close
helpviewer_keywords:
- Close method [ADO MD]
ms.assetid: a3aa594d-f9d4-4654-8625-ec20153ff5d9
author: rothja
ms.author: jroth
ms.openlocfilehash: b90dab9bc42bb9aef0de5300c38f238b8ff67b34
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174336"
---
# <a name="close-method-ado-md"></a>Close, méthode (ADO MD)
Ferme un CellSet ouvert.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Cellset.Close  
```  
  
## <a name="remarks"></a>Notes  
 L’utilisation de la méthode **Close** pour fermer un objet [Cellset](./cellset-object-ado-md.md) permet de libérer les données associées, y compris les données dans les [cellules](./cell-object-ado-md.md), les [axes](./axis-object-ado-md.md), les [positions](./position-object-ado-md.md)ou les objets [membres](./member-object-ado-md.md) associés. La fermeture d’un **Cellset** ne le supprime pas de la mémoire ; vous pouvez modifier ses paramètres de propriété et l’ouvrir à nouveau ultérieurement. Pour éliminer complètement un objet de la mémoire, affectez à la variable objet la valeur **Nothing**.  
  
 Vous pouvez appeler ultérieurement la méthode [Open](./open-method-ado-md.md) pour rouvrir l' **Cellset** à l’aide de la même chaîne source ou d’une autre chaîne. Lorsque l’objet **Cellset** est fermé, la récupération de toutes les propriétés ou l’appel de toute méthode qui fait référence aux données ou métadonnées sous-jacentes génère une erreur.  
  
## <a name="applies-to"></a>S'applique à  
 [Cellset, objet (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [AXIS, objet (ADO MD)](./axis-object-ado-md.md)   
 [Cell, objet (ADO MD)](./cell-object-ado-md.md)   
 [Member, objet (ADO MD)](./member-object-ado-md.md)   
 [Méthode Open (ADO MD)](./open-method-ado-md.md)   
 [Position, objet (ADO MD)](./position-object-ado-md.md)   
 [State, propriété (ADO MD)](./state-property-ado-md.md)