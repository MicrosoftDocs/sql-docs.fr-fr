---
description: AbsolutePosition, propriété (ADO)
title: AbsolutePosition, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Recordset15::AbsolutePosition
helpviewer_keywords:
- AbsolutePosition property [ADO]
ms.assetid: 79f8ee5e-fc70-46d8-8c29-ebf943c66592
author: rothja
ms.author: jroth
ms.openlocfilehash: e406012b917dd6e1694f5914088bac09201ff99e
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88451751"
---
# <a name="absoluteposition-property-ado"></a>AbsolutePosition, propriété (ADO)
Indique la position ordinale de l’enregistrement actif d’un objet [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Pour le code 32 bits, définit ou retourne une valeur de **type long** comprise entre 1 et le nombre d’enregistrements dans l’objet **Recordset** ([RecordCount](../../../ado/reference/ado-api/recordcount-property-ado.md)), ou retourne l’une des valeurs [PositionEnum](../../../ado/reference/ado-api/positionenum.md) .  
  
 Pour le code 64 bits, utilisez un type de données qui fournit le stockage d’une valeur 64 bits. Par exemple, vous pouvez utiliser long ou une autre valeur dont la longueur est égale à 64 bits, par exemple DBORDINAL. N’utilisez pas de valeurs **PositionEnum** , car elles sont limitées à une longueur de 32 bits.  
  
## <a name="remarks"></a>Notes  
 Pour définir la propriété **AbsolutePosition** , ADO exige que le fournisseur OLE DB que vous utilisez implémente l’interface [IRowsetLocate : IRowset](https://msdn.microsoft.com/library/windows/desktop/ms721190.aspx) .  
  
 L’accès à la propriété **AbsolutePosition** d’un **jeu d’enregistrements** ouvert avec un curseur avant uniquement ou dynamique génère l’erreur **adErrFeatureNotAvailable**. Avec les autres types de curseurs, la position correcte est retournée tant que le fournisseur OLE DB prend en charge l’interface **IRowsetScroll : IRowsetLocate** . Si le fournisseur ne prend pas en charge l’interface **IRowsetScroll** , la propriété est définie sur **adPosUnknown**. Consultez la documentation de votre fournisseur pour déterminer s’il prend en charge **IRowsetScroll**.  
  
 Utilisez la propriété **AbsolutePosition** pour vous déplacer vers un enregistrement en fonction de sa position ordinale dans l’objet **Recordset** ou pour déterminer la position ordinale de l’enregistrement actif. Le fournisseur doit prendre en charge les fonctionnalités appropriées pour que cette propriété soit disponible.  
  
 Comme la propriété [AbsolutePage](../../../ado/reference/ado-api/absolutepage-property-ado.md) , **AbsolutePosition** est de base 1 et est égal à 1 lorsque l’enregistrement actif est le premier enregistrement dans le **Recordset**. Vous pouvez obtenir le nombre total d’enregistrements dans l’objet **Recordset** à partir de la propriété [RecordCount](../../../ado/reference/ado-api/recordcount-property-ado.md) .  
  
 Lorsque vous définissez la propriété **AbsolutePosition** , même s’il s’agit d’un enregistrement dans le cache actuel, ADO recharge le cache avec un nouveau groupe d’enregistrements à partir de l’enregistrement que vous avez spécifié. La propriété [CacheSize](../../../ado/reference/ado-api/cachesize-property-ado.md) détermine la taille de ce groupe.  
  
> [!NOTE]
>  Vous ne devez pas utiliser la propriété **AbsolutePosition** comme numéro d’enregistrement de substitution. La position d’un enregistrement donné change lorsque vous supprimez un enregistrement précédent. En outre, il n’est pas garanti qu’un enregistrement donné aura la même **AbsolutePosition** si l’objet **Recordset** est à nouveau interrogé ou rouvert. Les signets sont toujours la méthode recommandée pour la conservation et le retour à une position donnée et constituent la seule façon de positionner tous les types d’objets **Recordset** .  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [AbsolutePosition et CursorLocation, exemple de propriétés (VB)](../../../ado/reference/ado-api/absoluteposition-and-cursorlocation-properties-example-vb.md)   
 [AbsolutePosition et CursorLocation, exemples de propriétés (VC + +)](../../../ado/reference/ado-api/absoluteposition-and-cursorlocation-properties-example-vc.md)   
 [AbsolutePage, propriété (ADO)](../../../ado/reference/ado-api/absolutepage-property-ado.md)   
 [RecordCount, propriété (ADO)](../../../ado/reference/ado-api/recordcount-property-ado.md)
