---
description: Utilisation de CacheSize
title: Utilisation de CacheSize | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- locks [ADO], CacheSize property
- CacheSize property [ADO]
ms.assetid: ca1c3422-b6a4-4ba6-af55-54f975b698b1
author: rothja
ms.author: jroth
ms.openlocfilehash: 31c9b1dd1ccc3ef61b120bf3d075b81fc34c0eb6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032281"
---
# <a name="using-cachesize"></a>Utilisation de CacheSize
Utilisez la propriété **CacheSize** pour contrôler le nombre d’enregistrements à récupérer à un moment donné dans la mémoire locale à partir du fournisseur. Par exemple, si la **CacheSize** est 10, après la première ouverture de l’objet **Recordset** , le fournisseur récupère les 10 premiers enregistrements dans la mémoire locale. Au fur et à mesure que vous parcourez l’objet **Recordset** , le fournisseur retourne les données à partir de la mémoire tampon locale. Dès que vous déplacez au-delà du dernier enregistrement du cache, le fournisseur récupère les 10 enregistrements suivants de la source de données dans le cache.  
  
> [!NOTE]
>  **CacheSize** est basé sur la propriété **nombre maximal de lignes ouvertes** spécifiques au fournisseur (dans la collection **Properties** de l’objet **Recordset** ). Vous ne pouvez pas définir **CacheSize** sur une valeur supérieure au **nombre maximal de lignes ouvertes.** Pour modifier le nombre de lignes qui peuvent être ouvertes par le fournisseur, définissez **nombre maximal de lignes ouvertes**.  
  
 La valeur de **CacheSize** peut être ajustée pendant la durée de vie de l’objet **Recordset** , mais la modification de cette valeur affecte uniquement le nombre d’enregistrements dans le cache après les récupérations ultérieures de la source de données. La modification de la valeur de propriété seule ne modifie pas le contenu actuel du cache.  
  
 S’il y a moins d’enregistrements à récupérer que le nombre de fois que **CacheSize** spécifie, le fournisseur retourne les enregistrements restants et aucune erreur ne se produit.  
  
 Un paramètre **CacheSize** de zéro n’est pas autorisé et retourne une erreur.  
  
 Les enregistrements récupérés à partir du cache ne reflètent pas les modifications simultanées apportées par d’autres utilisateurs aux données sources. Pour forcer une mise à jour de toutes les données mises en cache, utilisez la méthode [Resync](../../../ado/reference/ado-api/resync-method.md) .  
  
 Si **CacheSize** a une valeur supérieure à 1, les méthodes de navigation ([Move](../../../ado/reference/ado-api/move-method-ado.md), [MoveFirst, MoveLast, MoveNext et MovePrevious](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)) peuvent entraîner la navigation vers un enregistrement supprimé, si la suppression se produit après la récupération des enregistrements. Après l’extraction initiale, les suppressions suivantes ne sont pas reflétées dans votre cache de données tant que vous n’essayez pas d’accéder à une valeur de données à partir d’une ligne supprimée. Toutefois, la définition de **CacheSize** sur 1 élimine ce problème, car les lignes supprimées ne peuvent pas être récupérées.
