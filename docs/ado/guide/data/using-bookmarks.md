---
description: Utilisation de signets
title: Utilisation des signets | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- bookmarks [ADO]
- Recordset object [ADO]
ms.assetid: cca244e6-84f8-4394-bca9-f7a819b8f4df
author: rothja
ms.author: jroth
ms.openlocfilehash: f07ad3e4f5dc69da31a74af96842b1c8aab97eda
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036769"
---
# <a name="using-bookmarks"></a>Utilisation de signets
Il est souvent utile de revenir directement à un enregistrement spécifique après l’avoir déplacé dans le **Recordset** sans avoir à faire défiler chaque enregistrement et à comparer des valeurs. Par exemple, si vous tentez de rechercher un enregistrement à l’aide de la méthode **Find** alors que la recherche ne retourne aucun enregistrement, vous êtes automatiquement placé à chaque extrémité du **Recordset**. Si votre fournisseur les prend en charge, vous pouvez utiliser des signets pour marquer votre lieu avant d’utiliser la méthode **Find** afin de pouvoir revenir à votre emplacement. Un signet est une valeur de type **Variant** qui identifie de façon unique un enregistrement dans un objet **Recordset** .  
  
 Vous pouvez également utiliser un tableau de signets de type Variant avec la méthode de **filtre Recordset** pour filtrer sur un jeu d’enregistrements sélectionné. Pour plus d’informations sur cette technique, consultez Filtrage des résultats dans la rubrique, [utilisation des recordsets](../../../ado/guide/data/working-with-recordsets.md), plus loin dans cette section.  
  
 Vous pouvez utiliser la propriété **Bookmark** pour obtenir un signet pour un enregistrement ou définir l’enregistrement en cours dans un objet **Recordset** sur l’enregistrement identifié par un signet valide. Le code suivant utilise la propriété **Bookmark** pour définir un signet, puis revenir à l’enregistrement marqué d’un signet après un déplacement vers d’autres enregistrements. Pour déterminer si votre **Recordset** prend en charge les signets, utilisez la méthode **supports** .  
  
```  
'BeginBookmarkEg  
Dim varBookmark As Variant  
Dim blnCanBkmrk As Boolean  
  
objRs.Open strSQL, strConnStr, adOpenStatic, adLockOptimistic, adCmdText  
  
If objRs.RecordCount > 4 Then  
    objRs.Move 4                       ' move to the fifth record  
    blnCanBkmrk = objRs.Supports(adBookmark)  
    If blnCanBkmrk = True Then  
        varBookmark = objRs.Bookmark   ' record the bookmark  
        objRs.MoveLast                 ' move to a different record  
        objRs.Bookmark = varBookmark   ' return to the bookmarked (sixth) record  
    End If  
End If  
'EndBookmarkEg  
```  
  
 La méthode [supports](../../../ado/reference/ado-api/supports-method.md) est traitée plus en détail ultérieurement.  
  
 Sauf dans le cas des **recordsets** clonés, les signets sont uniques dans le **jeu d’enregistrements** dans lequel ils ont été créés, même si la même commande est utilisée. Cela signifie que vous ne pouvez pas utiliser un **signet** obtenu à partir d’un **jeu d’enregistrements** pour vous déplacer vers le même enregistrement dans un deuxième **Recordset** ouvert avec la même commande.
