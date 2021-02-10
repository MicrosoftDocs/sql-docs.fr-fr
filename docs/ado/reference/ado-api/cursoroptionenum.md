---
description: CursorOptionEnum
title: CursorOptionEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CursorOptionEnum
helpviewer_keywords:
- CursorOptionEnum enumeration [ADO]
ms.assetid: 4e10cda7-ce81-4466-94c2-844d38191cf1
author: rothja
ms.author: jroth
ms.openlocfilehash: 53916dc6392bc2889b694233c80dae809e254240
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025973"
---
# <a name="cursoroptionenum"></a>CursorOptionEnum
Spécifie les fonctionnalités que la méthode de [prise en charge](./supports-method.md) doit tester.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adAddNew**|0x1000400|Prend en charge la méthode [AddNew](./addnew-method-ado.md) pour ajouter de nouveaux enregistrements.|  
|**adApproxPosition**|0x4000|Prend en charge les propriétés [AbsolutePosition](./absoluteposition-property-ado.md) et [AbsolutePage](./absolutepage-property-ado.md) .|  
|**adBookmark**|0x2000|Prend en charge la propriété [Bookmark](./bookmark-property-ado.md) pour accéder à des enregistrements spécifiques.|  
|**adDelete**|0x1000800|Prend en charge la méthode [Delete](./delete-method-ado-recordset.md) pour supprimer des enregistrements.|  
|**adFind**|0x80000|Prend en charge la méthode [Find](./find-method-ado.md) pour localiser une ligne dans un [Recordset](./recordset-object-ado.md).|  
|**adHoldRecords**|0x100|Récupère plus d’enregistrements ou modifie la position suivante sans valider toutes les modifications en attente.|  
|**adIndex**|0x100000|Prend en charge la propriété [index](./index-property.md) pour nommer un index.|  
|**adMovePrevious**|0x200|Prend en charge les méthodes [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) et [MovePrevious](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) , ainsi que les méthodes [Move](./move-method-ado.md) ou [GetRows](./getrows-method-ado.md) pour déplacer la position d’enregistrement actuelle vers l’arrière sans avoir besoin de signets.|  
|**adNotify**|0x40000|Indique que le fournisseur de données sous-jacent prend en charge les notifications (qui détermine si les événements **Recordset** sont pris en charge).|  
|**adResync**|0x20000|Prend en charge la méthode [Resync](./resync-method.md) pour mettre à jour le curseur avec les données qui sont visibles dans la base de données sous-jacente.|  
|**adSeek**|0x200000|Prend en charge la méthode [Seek](./seek-method.md) pour localiser une ligne dans un **Recordset**.|  
|**adUpdate**|0x1008000|Prend en charge la méthode [Update](./update-method.md) pour modifier des données existantes.|  
|**adUpdateBatch**|0x10000|Prend en charge la mise à jour par lots (méthodes[UpdateBatch](./updatebatch-method.md) et [CancelBatch](./cancelbatch-method-ado.md) ) pour transmettre des groupes de modifications au fournisseur.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums.CursorOption.ADDNEW|  
|AdoEnums.CursorOption.APPROXPOSITION|  
|AdoEnums.CursorOption.BOOKMARK|  
|AdoEnums.CursorOption.DELETE|  
|AdoEnums.CursorOption.FIND|  
|AdoEnums.CursorOption.HOLDRECORDS|  
|AdoEnums.CursorOption.INDEX|  
|AdoEnums. CursorOption. MOVEPREVIOUS|  
|AdoEnums.CursorOption.NOTIFY|  
|AdoEnums. CursorOption. Resync|  
|AdoEnums.CursorOption.SEEK|  
|AdoEnums. CursorOption. UPDATE|  
|AdoEnums. CursorOption. UPDATEBATCH|  
  
## <a name="applies-to"></a>S'applique à  
 [Supports, méthode](./supports-method.md)