---
description: Bookmark, propriété (ADO)
title: Bookmark, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Bookmark
helpviewer_keywords:
- Bookmark property [ADO]
ms.assetid: 481dcc93-487b-490e-ac58-a1e9b2ebfd43
author: rothja
ms.author: jroth
ms.openlocfilehash: dd484230ba2801abcb553fa8f9013dce53799b09
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171559"
---
# <a name="bookmark-property-ado"></a>Bookmark, propriété (ADO)
Indique un signet qui identifie de façon unique l’enregistrement en cours dans un objet [Recordset](./recordset-object-ado.md) ou définit l’enregistrement en cours dans un objet **Recordset** sur l’enregistrement identifié par un signet valide.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une expression **Variant** qui prend la valeur d’un signet valide.  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **Bookmark** pour enregistrer la position de l’enregistrement en cours et revenir à cet enregistrement à tout moment. Les signets sont uniquement disponibles dans les objets **Recordset** qui prennent en charge la fonctionnalité de signet.  
  
 Lorsque vous ouvrez un objet **Recordset** , chacun de ses enregistrements a un signet unique. Pour enregistrer le signet de l’enregistrement actif, assignez la valeur de la propriété **Bookmark** à une variable. Pour revenir rapidement à cet enregistrement à tout moment après l’avoir déplacé vers un autre enregistrement, affectez à la propriété **Bookmark** de l’objet **Recordset** la valeur de cette variable.  
  
 Il se peut que l’utilisateur ne soit pas en mesure d’afficher la valeur du signet. En outre, les utilisateurs ne doivent pas s’attendre à ce que les signets soient directement comparables, car deux signets qui font référence au même enregistrement peuvent avoir des valeurs différentes.  
  
 Si vous utilisez la méthode [clone](./clone-method-ado.md) pour créer une copie d’un objet **Recordset** , les paramètres de propriété **Bookmark** pour les objets **Recordset** d’origine et dupliqués sont identiques et vous pouvez les utiliser de manière interchangeable. Toutefois, vous ne pouvez pas utiliser les signets de différents objets **Recordset** de manière interchangeable, même s’ils ont été créés à partir de la même source ou commande.  
  
> [!NOTE]
>  **Utilisation des services de données distants** Lorsqu’elle est utilisée sur un objet **Recordset** côté client, la propriété **Bookmark** est toujours disponible.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [BOF, EOF et Bookmark, exemple de propriétés (VB)](./bof-eof-and-bookmark-properties-example-vb.md)   
 [BOF, EOF et Bookmark, exemple de propriétés (VC + +)](./bof-eof-and-bookmark-properties-example-vc.md)   
 [Supports, méthode](./supports-method.md)