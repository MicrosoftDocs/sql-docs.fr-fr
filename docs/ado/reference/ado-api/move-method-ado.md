---
description: Move, méthode (ADO)
title: Move, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Move
- Recordset15::raw_Move
helpviewer_keywords:
- Move method [ADO]
ms.assetid: 13fe9381-d00b-4f4a-9162-83c3f21b3837
author: rothja
ms.author: jroth
ms.openlocfilehash: f870db346e384f4c131ec24bf6c15a5d15a009c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167115"
---
# <a name="move-method-ado"></a>Move, méthode (ADO)
Déplace la position de l’enregistrement actuel dans un objet [Recordset](./recordset-object-ado.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.Move NumRecords, Start  
```  
  
#### <a name="parameters"></a>Paramètres  
 *NumRecords*  
 Expression **longue** signée qui spécifie le nombre d’enregistrements déplacés par la position actuelle de l’enregistrement.  
  
 *Start*  
 Facultatif. Valeur de **chaîne** ou **Variant** qui prend la valeur d’un signet. Vous pouvez également utiliser une valeur [BookmarkEnum](./bookmarkenum.md) .  
  
## <a name="remarks"></a>Notes  
 La méthode **Move** est prise en charge sur tous les objets **Recordset** .  
  
 Si l’argument *numRecords* est supérieur à zéro, la position actuelle de l’enregistrement se déplace vers l’avant (vers la fin de l’ensemble d' **enregistrements**). Si *numRecords* est inférieur à zéro, la position actuelle de l’enregistrement est déplacée vers l’arrière (vers le début de l’ensemble d' **enregistrements**).  
  
 Si l’appel de **déplacement** déplace la position d’enregistrement active vers un point avant le premier enregistrement, ADO définit l’enregistrement actif à la position précédant le premier enregistrement dans le jeu d’enregistrements ([BOF](./bof-eof-properties-ado.md) a la **valeur true**). Une tentative de déplacement vers l’arrière lorsque la propriété **BOF** a déjà la **valeur true** génère une erreur.  
  
 Si l’appel de **déplacement** déplace la position d’enregistrement active vers un point après le dernier enregistrement, ADO définit l’enregistrement actif à la position après le dernier enregistrement dans le jeu d’enregistrements ([EOF](./bof-eof-properties-ado.md) a la **valeur true**). Une tentative de déplacement vers l’avant lorsque la propriété **EOF** a déjà la **valeur true** génère une erreur.  
  
 L’appel de la méthode **Move** à partir d’un objet **Recordset** vide génère une erreur.  
  
 Si vous passez l’argument *Start* , le déplacement est relatif à l’enregistrement avec ce signet, en supposant que l’objet **Recordset** prend en charge les signets. S’il n’est pas spécifié, le déplacement est relatif à l’enregistrement actif.  
  
 Si vous utilisez la propriété [CacheSize](./cachesize-property-ado.md) pour mettre en cache localement des enregistrements à partir du fournisseur, le passage d’un argument *numRecords* qui déplace la position de l’enregistrement actuel en dehors du groupe actuel d’enregistrements mis en cache force ADO à récupérer un nouveau groupe d’enregistrements, en commençant par l’enregistrement de destination. La propriété **CacheSize** détermine la taille du groupe nouvellement récupéré, et l’enregistrement de destination est le premier enregistrement récupéré.  
  
 Si l’objet **Recordset** est Forward uniquement, un utilisateur peut toujours passer un argument *numRecords* inférieur à zéro, à condition que la destination se trouve dans l’ensemble actuel d’enregistrements mis en cache. Si l’appel de **déplacement** déplace la position d’enregistrement en cours vers un enregistrement avant le premier enregistrement mis en cache, une erreur se produit. Par conséquent, vous pouvez utiliser un cache d’enregistrements qui prend en charge le défilement complet sur un fournisseur qui prend uniquement en charge le défilement vers l’avant. Étant donné que les enregistrements mis en cache sont chargés en mémoire, vous devez éviter de mettre en cache plus d’enregistrements que nécessaire. Même si un objet **Recordset** avant uniquement prend en charge les déplacements vers l’arrière de cette manière, l’appel de la méthode [MovePrevious](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) sur un objet **Recordset** avant uniquement génère toujours une erreur.  
  
> [!NOTE]
>  La prise en charge du déplacement vers l’arrière dans un **jeu d’enregistrements** avant uniquement n’est pas prévisible, en fonction de votre fournisseur. Si l’enregistrement actif a été placé après le dernier enregistrement dans le **jeu d’enregistrements**, le **déplacement** vers l’arrière peut ne pas aboutir à la position actuelle correcte.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Move, exemple de méthode (VB)](./move-method-example-vb.md)   
 [Move, exemple de méthode (VBScript)](./move-method-example-vbscript.md)   
 [Move, exemple de méthode (VC + +)](./move-method-example-vc.md)   
 [MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes (ADO)](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md)   
 [Méthodes MoveFirst, MoveLast, MoveNext et MovePrevious (RDS)](../rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)   
 [MoveRecord, méthode (ADO)](./moverecord-method-ado.md)