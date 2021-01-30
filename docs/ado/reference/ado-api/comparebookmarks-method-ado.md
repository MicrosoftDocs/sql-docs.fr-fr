---
description: CompareBookmarks, méthode (ADO)
title: CompareBookmarks, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CompareBookmarks
- Recordset20::CompareBookmarks
- Recordset20::raw_CompareBookmarks
helpviewer_keywords:
- CompareBookmarks method [ADO]
ms.assetid: d0b64286-2cc4-4a22-8f1d-9aefeebbcbc6
author: rothja
ms.author: jroth
ms.openlocfilehash: 63fc23d638a3ffecf29d6f3d3ad56e803d914072
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99155687"
---
# <a name="comparebookmarks-method-ado"></a>CompareBookmarks, méthode (ADO)
Compare deux signets et retourne une indication de leurs valeurs relatives.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
result = recordset.CompareBookmarks(Bookmark1, Bookmark2)  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une valeur [CompareEnum](./compareenum.md) qui indique la position de ligne relative de deux enregistrements représentés par leurs signets.  
  
#### <a name="parameters"></a>Paramètres  
 *Signet1*  
 Signet de la première ligne.  
  
 *Signet2*  
 Signet de la deuxième ligne.  
  
## <a name="remarks"></a>Notes  
 Les signets doivent s’appliquer au même objet [Recordset](./recordset-object-ado.md) , ou à un objet **Recordset** et à son [clone](./clone-method-ado.md). Vous ne pouvez pas comparer de manière fiable des signets de différents objets **Recordset** , même s’ils ont été créés à partir de la même source ou commande. Vous ne pouvez pas non plus comparer les signets d’un objet **Recordset** dont le fournisseur sous-jacent ne prend pas en charge les comparaisons.  
  
 Un signet identifie de façon unique une ligne dans un objet **Recordset** . Utilisez la propriété [Bookmark](./bookmark-property-ado.md) de la ligne actuelle pour obtenir son signet.  
  
 Étant donné que le type de données d’un signet est spécifique à chaque fournisseur, ADO l’expose en tant que **variante**. Par exemple, SQL Server signets sont de type DBTYPE_R8 (**double**). ADO expose ce type en tant que **Variant** avec un sous-type de **double**.  
  
 Lors de la comparaison de signets, ADO ne tente aucun type de contrainte. Les valeurs sont simplement transmises au fournisseur où la comparaison est effectuée. Si les signets passés à la méthode **CompareBookmarks** sont stockés dans des variables de types différents, il peut générer l’erreur d’incompatibilité de type suivante : « les arguments sont d’un type incorrect, sont en dehors de la plage acceptable ou sont en conflit les uns avec les autres. »  
  
 Un signet qui n’est pas valide ou de forme incorrecte génère une erreur.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [CompareBookmarks, exemple de méthode (VB)](./comparebookmarks-method-example-vb.md)   
 [CompareBookmarks, exemple de méthode (VC + +)](./comparebookmarks-method-example-vc.md)   
 [Bookmark, propriété (ADO)](./bookmark-property-ado.md)