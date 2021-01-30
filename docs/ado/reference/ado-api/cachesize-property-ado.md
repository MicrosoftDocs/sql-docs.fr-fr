---
description: CacheSize, propriété (ADO)
title: CacheSize, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::CacheSize
helpviewer_keywords:
- CacheSize property [ADO]
ms.assetid: 49dc9a49-af7b-433b-be36-7a14ca984fb7
author: rothja
ms.author: jroth
ms.openlocfilehash: 9aef38a384c352648d5487ee9dae9a79d605e8e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167815"
---
# <a name="cachesize-property-ado"></a>CacheSize, propriété (ADO)
Indique le nombre d’enregistrements d’un objet [Recordset](./recordset-object-ado.md) mis en cache localement en mémoire.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur de **type long** qui doit être supérieure à 0. 1 constitue la valeur par défaut.  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **CacheSize** pour contrôler le nombre d’enregistrements à récupérer à un moment donné dans la mémoire locale à partir du fournisseur. Par exemple, si la **CacheSize** est 10, après la première ouverture de l’objet **Recordset** , le fournisseur récupère les 10 premiers enregistrements dans la mémoire locale. Au fur et à mesure que vous parcourez l’objet **Recordset** , le fournisseur retourne les données à partir de la mémoire tampon locale. Dès que vous déplacez au-delà du dernier enregistrement du cache, le fournisseur récupère les 10 enregistrements suivants de la source de données dans le cache.  
  
> [!NOTE]
>  **CacheSize** est basé sur la propriété **nombre maximal de lignes ouvertes** spécifiques au fournisseur (dans la collection **Properties** de l’objet **Recordset** ). Vous ne pouvez pas définir **CacheSize** sur une valeur supérieure au **nombre maximal de lignes ouvertes**. Pour modifier le nombre de lignes qui peuvent être ouvertes par le fournisseur, définissez **nombre maximal de lignes ouvertes**.  
  
 La valeur de **CacheSize** peut être ajustée pendant la durée de vie de l’objet **Recordset** , mais la modification de cette valeur affecte uniquement le nombre d’enregistrements dans le cache après les récupérations ultérieures de la source de données. La modification de la valeur de propriété seule ne modifie pas le contenu actuel du cache.  
  
 S’il y a moins d’enregistrements à récupérer que le nombre de fois que **CacheSize** spécifie, le fournisseur retourne les enregistrements restants et aucune erreur ne se produit.  
  
 Un paramètre **CacheSize** de zéro n’est pas autorisé et retourne une erreur.  
  
 Les enregistrements récupérés à partir du cache ne reflètent pas les modifications simultanées apportées par d’autres utilisateurs aux données sources. Pour forcer une mise à jour de toutes les données mises en cache, utilisez la méthode [Resync](./resync-method.md) .  
  
 Si **CacheSize** est défini sur une valeur supérieure à un, les méthodes de navigation ([Move](./move-method-ado.md), [MoveFirst, MoveLast, MoveNext et MovePrevious](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md)) peuvent entraîner la navigation vers un enregistrement supprimé, si la suppression se produit après la récupération des enregistrements. Après l’extraction initiale, les suppressions suivantes ne sont pas reflétées dans votre cache de données tant que vous n’essayez pas d’accéder à une valeur de données à partir d’une ligne supprimée. Toutefois, la définition de **CacheSize** sur l’une élimine ce problème, car les lignes supprimées ne peuvent pas être extraites.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [CacheSize, exemple de propriété (VB)](./cachesize-property-example-vb.md)   
 [CacheSize, exemple de propriété (VC + +)](./cachesize-property-example-vc.md)   
 [CacheSize, exemple de propriété (JScript)](./cachesize-property-example-jscript.md)