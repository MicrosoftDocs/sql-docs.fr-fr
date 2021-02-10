---
description: RecordCount, propriété (ADO)
title: RecordCount, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::RecordCount
- Recordset15::GetRecordCount
- Recordset15::get_RecordCount
helpviewer_keywords:
- RecordCount property [ADO]
ms.assetid: 834f0121-394a-44d4-ad7d-999b43a6fe63
author: rothja
ms.author: jroth
ms.openlocfilehash: a4fb478b2320ac2f216ae0d0f1a175a94d346813
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051860"
---
# <a name="recordcount-property-ado"></a>RecordCount, propriété (ADO)

Indique le nombre d’enregistrements dans un objet [Recordset](./recordset-object-ado.md) .
  
## <a name="return-value"></a>Valeur de retour

Retourne une valeur de **type long** qui indique le nombre d’enregistrements dans le **Recordset**.
  
## <a name="remarks"></a>Notes

Utilisez la propriété **RecordCount** pour déterminer le nombre d’enregistrements dans un objet **Recordset** . La propriété retourne-1 lorsque ADO ne peut pas déterminer le nombre d’enregistrements ou si le type de fournisseur ou de curseur ne prend pas en charge **RecordCount**. La lecture de la propriété **RecordCount** sur un **jeu d’enregistrements** fermé génère une erreur.

#### <a name="bookmarks-or-approximate-positioning"></a>Signets ou positionnement approximatif

Si *l’objet Recordset prend en* charge les signets ou le positionnement approximatif, cette propriété retourne le nombre exact d’enregistrements dans le jeu d’enregistrements. Cette propriété retourne le nombre exact, que le jeu d’enregistrements ait été entièrement rempli ou non.

En revanche, si l’objet Recordset ne prend *pas* en charge les signets ou le positionnement approximatif, l’accès à cette propriété peut être un drainage significatif des ressources. La vidange se produit parce que tous les enregistrements doivent être récupérés et comptés pour retourner une valeur RecordCount exacte.

- **adBookmark** lié aux signets.
- **adApproxPosition** se réfère au positionnement approximatif.

> [!NOTE]
> Dans les versions 2,8 et antérieures d’ADO, le fournisseur SQLOLEDB extrait tous les enregistrements lorsqu’un curseur côté serveur est utilisé, malgré le fait qu’il retourne la **valeur true** pour les deux **prises en charge (AdApproxPosition)** et **prend en charge (adBookmark)**.
  
Le type de curseur de l’objet **Recordset** détermine si le nombre d’enregistrements peut être déterminé. La propriété **RecordCount** retourne-1 pour un curseur avant uniquement ; nombre réel pour un curseur statique ou de jeu de clés ; et soit-1, soit le nombre réel pour un curseur dynamique, en fonction de la source de données.
  
## <a name="applies-to"></a>S'applique à

[Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi

[Filter et RecordCount, exemple de propriétés (VB)](./filter-and-recordcount-properties-example-vb.md)   
[Filter et RecordCount, exemples de propriétés (VC + +)](./filter-and-recordcount-properties-example-vc.md)   
[AbsolutePosition, propriété (ADO)](./absoluteposition-property-ado.md)   
[PageCount, propriété (ADO)](./pagecount-property-ado.md)