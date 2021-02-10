---
description: Détermination de ce qui est pris en charge
title: Détermination des éléments pris en charge | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- editing data [ADO], Supports method
- Supports method [ADO]
ms.assetid: 65090cba-6d46-4775-8d61-f6838e7752a6
author: rothja
ms.author: jroth
ms.openlocfilehash: 6a20514318de8222d016f2d345bd6758a075ab5e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037529"
---
# <a name="determining-what-is-supported"></a>Détermination de ce qui est pris en charge
La méthode **prend en charge** l’utilisation de la méthode pour déterminer si un objet **Recordset** spécifié prend en charge un type particulier de fonctionnalité. Sa syntaxe est la suivante :  
  
```  
  
boolean = recordset.Supports(CursorOptions )  
```  
  
## <a name="remarks"></a>Notes  
 La méthode **supports** retourne une valeur booléenne qui indique si le fournisseur prend en charge toutes les fonctionnalités identifiées par l’argument CursorOptions. Vous pouvez utiliser la méthode **supports** pour déterminer les types de fonctionnalités qu’un objet **Recordset** prend en charge. Si l’objet **Recordset** prend en charge les fonctionnalités dont les constantes correspondantes se trouvent dans *CursorOptions*, la méthode **supports** retourne la **valeur true**. Sinon, elle retourne **false**.  
  
 À l’aide de la méthode **supports** , vous pouvez vérifier la capacité de l’objet **Recordset** à ajouter de nouveaux enregistrements, utiliser des signets, utiliser la méthode **Find** , utiliser le défilement, utiliser la propriété **index** et effectuer des mises à jour par lots. Pour obtenir la liste complète des constantes et leurs significations, consultez [CursorOptionEnum](../../reference/ado-api/cursoroptionenum.md).  
  
 Bien que la méthode **supports** puisse retourner la **valeur true** pour une fonctionnalité donnée, elle ne garantit pas que le fournisseur peut rendre la fonctionnalité disponible dans toutes les circonstances. La méthode **supports** retourne simplement une valeur indiquant si le fournisseur peut prendre en charge les fonctionnalités spécifiées, en supposant que certaines conditions sont remplies. Par exemple, la méthode **supports** peut indiquer qu’un objet **Recordset** prend en charge les mises à jour, même si le curseur est basé sur une jointure de plusieurs tables, dont certaines colonnes ne peuvent pas être mises à jour.