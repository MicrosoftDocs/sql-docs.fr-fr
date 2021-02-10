---
description: Seek, méthode
title: Seek, méthode | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset21::Seek
- Recordset21::raw_Seek
helpviewer_keywords:
- Seek method [ADO]
ms.assetid: 129293d2-19d3-4940-bf64-483ee72fb4a1
author: rothja
ms.author: jroth
ms.openlocfilehash: 9819ce55d8898fb55e0cc98fb9dab10f339cfb17
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041699"
---
# <a name="seek-method"></a>Seek, méthode
Recherche l’index d’un [jeu d’enregistrements](./recordset-object-ado.md) pour localiser rapidement la ligne qui correspond aux valeurs spécifiées et modifie la position de ligne actuelle en cette ligne.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.Seek KeyValues, SeekOption  
```  
  
#### <a name="parameters"></a>Paramètres  
 *KeyValues*  
 Tableau de valeurs de **type Variant** . Un index se compose d’une ou plusieurs colonnes et le tableau contient une valeur à comparer à chaque colonne correspondante.  
  
 *SeekOption*  
 Valeur de [SeekEnum](./seekenum.md) qui spécifie le type de comparaison à effectuer entre les colonnes de l’index et les *valeurs de KeyValues* correspondantes.  
  
## <a name="remarks"></a>Notes  
 Utilisez la méthode **Seek** conjointement à la propriété [index](./index-property.md) si le fournisseur sous-jacent prend en charge les index sur l’objet **Recordset** . Utilisez la méthode [supports](./supports-method.md)**(adSeek)** pour déterminer si le fournisseur sous-jacent prend en charge la **recherche**, et la méthode **prend en charge (adIndex)** pour déterminer si le fournisseur prend en charge les index. (Par exemple, le [fournisseur de OLE DB pour Microsoft Jet](../../guide/appendixes/microsoft-ole-db-provider-for-microsoft-jet.md) prend en charge la **recherche** et l' **indexation**.)  
  
 Si la **recherche** ne trouve pas la ligne souhaitée, aucune erreur ne se produit et la ligne est positionnée à la fin de l’ensemble d' **enregistrements**. Définissez la propriété **index** sur l’index souhaité avant d’exécuter cette méthode.  
  
 Cette méthode est prise en charge uniquement avec les curseurs côté serveur. Seek n’est pas pris en charge lorsque la valeur de la propriété [CursorLocation](./cursorlocation-property-ado.md) de l’objet **Recordset** est **adUseClient**.  
  
 Cette méthode peut uniquement être utilisée lorsque l’objet **Recordset** a été ouvert avec une valeur [CommandTypeEnum](./commandtypeenum.md) de **adCmdTableDirect**.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode Seek et index, exemple de propriété (VB)](./seek-method-and-index-property-example-vb.md)   
 [Méthode Seek et index, exemple de propriété (VC + +)](./seek-method-and-index-property-example-vc.md)   
 [Find, méthode (ADO)](./find-method-ado.md)   
 [Index, propriété](./index-property.md)