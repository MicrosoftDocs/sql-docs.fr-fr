---
description: MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes (ADO)
title: Méthodes MoveFirst, MoveLast, MoveNext et MovePrevious (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::MoveLast
- Recordset15::MoveNext
- Recordset15::raw_MoveNext
- Recordset15::raw_MovePrevious
- Recordset15::MoveFirst
- Recordset15::raw_MoveLast
- Recordset15::MovePrevious
- Recordset15::raw_MoveFirst
helpviewer_keywords:
- MoveNext method [ADO]
- MoveLast method [ADO]
- MoveFirst method [ADO]
- MovePrevious method [ADO]
ms.assetid: a61a01a7-5b33-4150-9126-21dfa63654cb
author: rothja
ms.author: jroth
ms.openlocfilehash: a267298dd652a307d6e3365cb48e30f9fc667ed6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041729"
---
# <a name="movefirst-movelast-movenext-and-moveprevious-methods-ado"></a>MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes (ADO)
Passe au premier enregistrement, dernier, suivant ou précédent dans un objet [Recordset](./recordset-object-ado.md) spécifié et convertit cet enregistrement en enregistrement actif.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.{MoveFirst | MoveLast | MoveNext | MovePrevious}  
```  
  
## <a name="remarks"></a>Notes  
 Utilisez la méthode **MoveFirst** pour déplacer la position de l’enregistrement actif vers le premier enregistrement du **Recordset**.  
  
 Utilisez la méthode **MoveLast** pour déplacer la position de l’enregistrement actuel vers le dernier enregistrement du **Recordset**. L’objet **Recordset** doit prendre en charge les signets ou le déplacement du curseur vers l’arrière ; dans le cas contraire, l’appel de méthode génère une erreur.  
  
 Un appel à **MoveFirst** ou **MoveLast** lorsque le **Recordset** est vide (les paramètres **BOF** et **EOF** ont la valeur true) génère une erreur.  
  
 Utilisez la méthode **MoveNext** pour déplacer la position d’enregistrement actuelle d’un enregistrement vers l’avant (vers le bas de l’ensemble d' **enregistrements**). Si le dernier enregistrement est l’enregistrement en cours et que vous appelez la méthode **MoveNext** , ADO définit l’enregistrement actif à la position qui suit le dernier enregistrement dans le **jeu d’enregistrements** ([EOF](./bof-eof-properties-ado.md) a la **valeur true**). Une tentative de déplacement vers l’avant lorsque la propriété **EOF** a déjà la **valeur true** génère une erreur.  
  
 Dans ADO 2,5 et versions ultérieures, lorsque le **Recordset** a été filtré ou trié et que les données de l’enregistrement actuel sont modifiées, l’appel de la méthode **MoveNext** déplace le curseur de deux enregistrements de l’enregistrement actif. En effet, lorsque l’enregistrement actif est modifié, l’enregistrement suivant devient le nouvel enregistrement actif. L’appel de **MoveNext** après la modification déplace le curseur d’un enregistrement vers l’avant à partir du nouvel enregistrement actif. Cela diffère du comportement de ADO 2,1 et des versions antérieures. Dans ces versions antérieures, la modification des données d’un enregistrement actif dans le **jeu d’enregistrements** trié ou filtré ne change pas la position de l’enregistrement actuel, et **MoveNext** déplace le curseur vers l’enregistrement suivant immédiatement après l’enregistrement actif.  
  
 Utilisez la méthode **MovePrevious** pour déplacer la position d’enregistrement actuelle d’un enregistrement vers l’arrière (vers le haut de l’ensemble d' **enregistrements**). L’objet **Recordset** doit prendre en charge les signets ou le déplacement du curseur vers l’arrière ; dans le cas contraire, l’appel de méthode génère une erreur. Si le premier enregistrement est l’enregistrement en cours et que vous appelez la méthode **MovePrevious** , ADO définit l’enregistrement actif à la position précédant le premier enregistrement dans le **jeu d’enregistrements** ([BOF](./bof-eof-properties-ado.md) a la **valeur true**). Une tentative de déplacement vers l’arrière lorsque la propriété **BOF** a déjà la **valeur true** génère une erreur. Si l’objet **Recordset** ne prend pas en charge les signets ou le déplacement vers le haut du curseur, la méthode **MovePrevious** génère une erreur.  
  
 Si le **jeu d’enregistrements** est Forward uniquement et que vous souhaitez prendre en charge à la fois le défilement vers l’avant et vers l’arrière, vous pouvez utiliser la propriété [CacheSize](./cachesize-property-ado.md) pour créer un cache d’enregistrement qui prend en charge le déplacement du curseur vers l’arrière via la méthode [Move](./move-method-ado.md) . Étant donné que les enregistrements mis en cache sont chargés en mémoire, vous devez éviter de mettre en cache plus d’enregistrements que nécessaire. Vous pouvez appeler la méthode **MoveFirst** dans un objet **Recordset** avant uniquement ; Cela peut entraîner la réexécution de la commande qui a généré l’objet **Recordset** par le fournisseur.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [MoveFirst, MoveLast, MoveNext et MovePrevious, exemple de méthodes (VB)](./movefirst-movelast-movenext-and-moveprevious-methods-example-vb.md)   
 [MoveFirst, MoveLast, MoveNext et MovePrevious, exemple de méthode (VBScript)](./movefirst-movelast-movenext-and-moveprevious-methods-example-vbscript.md)   
 [MoveFirst, MoveLast, MoveNext et MovePrevious, exemple de méthodes (VC + +)](./movefirst-movelast-movenext-and-moveprevious-methods-example-vc.md)   
 [Move, méthode (ADO)](./move-method-ado.md)   
 [Méthodes MoveFirst, MoveLast, MoveNext et MovePrevious (RDS)](../rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)   
 [MoveRecord, méthode (ADO)](./moverecord-method-ado.md)