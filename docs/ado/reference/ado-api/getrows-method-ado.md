---
description: GetRows, méthode (ADO)
title: GetRows, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::GetRows
- Recordset15::raw_GetRows
helpviewer_keywords:
- Getrows method [ADO]
ms.assetid: 14b92860-4171-47d9-a413-dd60dd6a8880
author: rothja
ms.author: jroth
ms.openlocfilehash: df59c3d464d1c19a3b98e8611e2db2983db2a833
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171000"
---
# <a name="getrows-method-ado"></a>GetRows, méthode (ADO)
Récupère plusieurs enregistrements d’un objet [Recordset](./recordset-object-ado.md) dans un tableau.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
array = recordset.GetRows(Rows, Start, Fields )  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une valeur **Variant** dont la valeur est un tableau à deux dimensions.  
  
#### <a name="parameters"></a>Paramètres  
 *Lignes*  
 Facultatif. Valeur [GetRowsOptionEnum](./getrowsoptionenum.md) qui indique le nombre d’enregistrements à récupérer. La valeur par défaut est **adGetRowsRest**.  
  
 *Start*  
 Facultatif. Valeur de **chaîne** ou **Variant** qui prend la valeur du signet de l’enregistrement à partir duquel l’opération **GetRows** doit commencer. Vous pouvez également utiliser une valeur [BookmarkEnum](./bookmarkenum.md) .  
  
 *Fields*  
 Facultatif. **Variante** qui représente un nom de champ unique ou une position ordinale, ou un tableau de noms de champs ou de numéros de position ordinale. ADO retourne uniquement les données de ces champs.  
  
## <a name="remarks"></a>Notes  
 Utilisez la méthode **GetRows** pour copier les enregistrements d’un **jeu d’enregistrements** dans un tableau à deux dimensions. Le premier indice identifie le champ et le second identifie le numéro d’enregistrement. La variable de *tableau* est automatiquement dimensionnée à la taille correcte lorsque la méthode **GetRows** retourne les données.  
  
 Si vous ne spécifiez pas de valeur pour l’argument *Rows* , la méthode **GetRows** récupère automatiquement tous les enregistrements dans l’objet **Recordset** . Si vous demandez plus d’enregistrements que ce qui est disponible, **GetRows** retourne uniquement le nombre d’enregistrements disponibles.  
  
 Si l’objet **Recordset** prend en charge les signets, vous pouvez spécifier à quel enregistrement la méthode **GetRows** doit commencer à récupérer les données en passant la valeur de la propriété [Bookmark](./bookmark-property-ado.md) de cet enregistrement dans l’argument *Start* .  
  
 Si vous souhaitez limiter les champs que l’appel **GetRows** retourne, vous pouvez transmettre un nom de champ unique ou un tableau de noms de champs/nombres dans l’argument *Fields* .  
  
 Une fois que vous avez appelé **GetRows**, le prochain enregistrement non lu devient l’enregistrement actif, ou la propriété [EOF](./bof-eof-properties-ado.md) a la valeur **true** s’il n’y a plus d’enregistrements.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [GetRows, exemple de méthode (VB)](./getrows-method-example-vb.md)   
 [GetRows, exemple de méthode (VC++)](./getrows-method-example-vc.md)