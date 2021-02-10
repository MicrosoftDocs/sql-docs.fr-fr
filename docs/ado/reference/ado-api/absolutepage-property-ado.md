---
description: AbsolutePage, propriété (ADO)
title: AbsolutePage, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::AbsolutePage
helpviewer_keywords:
- AbsolutePage property [ADO]
ms.assetid: ddb58a35-ec3a-423c-a504-3c65e62c23d4
author: rothja
ms.author: jroth
ms.openlocfilehash: 5f5b8019bedb2273c15ee3362f7fd3cb9688308e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035973"
---
# <a name="absolutepage-property-ado"></a>AbsolutePage, propriété (ADO)
Indique sur quelle page se trouve l’enregistrement en cours.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Pour le code 32 bits, définit ou retourne une valeur de **type long** comprise entre 1 et le nombre de pages de l’objet [Recordset](./recordset-object-ado.md) ([PageCount](./pagecount-property-ado.md)), ou retourne l’une des valeurs [PositionEnum](./positionenum.md) .  
  
 Pour le code 64 bits, utilisez un type de données qui fournit le stockage d’une valeur 64 bits. Par exemple, vous pouvez utiliser **long** ou une autre valeur qui peut être une longueur de 64 bits telle que DBORDINAL. N’utilisez pas de valeurs **PositionEnum** , car elles sont limitées à une longueur de 32 bits.  
  
## <a name="remarks"></a>Notes  
 Cette propriété peut être utilisée pour identifier le numéro de la page sur laquelle se trouve l’enregistrement en cours. Elle utilise la propriété [pageSize](./pagesize-property-ado.md) pour diviser logiquement le nombre total d’ensembles de lignes de l’objet **Recordset** en une série de pages, dont chacune a le nombre d’enregistrements égal à **pageSize** (à l’exception de la dernière page, qui peut avoir moins d’enregistrements). Le fournisseur doit prendre en charge les fonctionnalités appropriées pour que cette propriété soit disponible.  
  
-   Lors de l’obtention ou de la définition de la propriété **AbsolutePage** , ADO utilise la propriété [AbsolutePosition](./absoluteposition-property-ado.md) et la propriété [pageSize](./pagesize-property-ado.md) de la même manière :  
  
-   Pour obtenir la valeur **AbsolutePage**, ADO commence par récupérer **AbsolutePosition**, puis le divise par le **pageSize**.  
  
-   Pour définir **AbsolutePage**, ADO déplace le **AbsolutePosition** comme suit : il multiplie **pageSize** par la nouvelle valeur **AbsolutePage** , puis ajoute 1 à la valeur. Par conséquent, la position actuelle dans le **jeu d’enregistrements** après avoir correctement défini **AbsolutePage** est le premier enregistrement dans cette page.  
  
 Comme la propriété **AbsolutePosition** , **AbsolutePage** est de base 1 et égale 1 lorsque l’enregistrement actif est le premier enregistrement dans le **Recordset**. Définissez cette propriété pour passer au premier enregistrement d’une page particulière. Obtient le nombre total de pages de la propriété **PageCount** .  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [AbsolutePage, PageCount et PageSize, propriétés, exemple (VB)](./absolutepage-pagecount-and-pagesize-properties-example-vb.md)   
 [AbsolutePage, PageCount et PageSize, propriétés, exemple (VC + +)](./absolutepage-pagecount-and-pagesize-properties-example-vc.md)   
 [AbsolutePosition, propriété (ADO)](./absoluteposition-property-ado.md)   
 [PageCount, propriété (ADO)](./pagecount-property-ado.md)   
 [PageSize, propriété (ADO)](./pagesize-property-ado.md)