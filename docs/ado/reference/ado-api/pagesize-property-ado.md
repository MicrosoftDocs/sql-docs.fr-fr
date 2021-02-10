---
description: PageSize, propriété (ADO)
title: PageSize, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::PageSize
helpviewer_keywords:
- PageSize property [ADO]
ms.assetid: e57930a6-46c4-4a17-a3b6-f79e94d5c9c7
author: rothja
ms.author: jroth
ms.openlocfilehash: 8d37ac91b1899c0eb515d8443cfc5c3e2f9463cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041209"
---
# <a name="pagesize-property-ado"></a>PageSize, propriété (ADO)
Indique le nombre d’enregistrements constituant une page dans le [jeu d’enregistrements](./recordset-object-ado.md).  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur de **type long** qui indique le nombre d’enregistrements sur une page. La valeur par défaut est **10**.  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **pageSize** pour déterminer le nombre d’enregistrements constituant une page logique de données. L’établissement d’une taille de page vous permet d’utiliser la propriété [AbsolutePage](./absolutepage-property-ado.md) pour accéder au premier enregistrement d’une page particulière. Cela est utile dans les scénarios de serveur Web lorsque vous souhaitez autoriser l’utilisateur à parcourir les données, en affichant un certain nombre d’enregistrements à la fois.  
  
 Cette propriété peut être définie à tout moment et sa valeur sera utilisée pour calculer l’emplacement du premier enregistrement d’une page particulière.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [AbsolutePage, PageCount et PageSize, propriétés, exemple (VB)](./absolutepage-pagecount-and-pagesize-properties-example-vb.md)   
 [AbsolutePage, PageCount et PageSize, propriétés, exemple (VC + +)](./absolutepage-pagecount-and-pagesize-properties-example-vc.md)   
 [AbsolutePage, propriété (ADO)](./absolutepage-property-ado.md)   
 [PageCount, propriété (ADO)](./pagecount-property-ado.md)