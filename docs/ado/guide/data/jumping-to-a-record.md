---
description: Accès à un enregistrement
title: Saut à un enregistrement | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- record jumping [ADO]
- jumping to record [ADO]
ms.assetid: 6caf6299-2eea-4d34-9b0e-b75aab07b740
author: rothja
ms.author: jroth
ms.openlocfilehash: 1a8a34ac8cafffe9f22595a76c5389369290a2b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032721"
---
# <a name="jumping-to-a-record"></a>Accès à un enregistrement
La méthode [Move](../../reference/ado-api/move-method-ado.md) vous permet de vous déplacer vers l’avant ou vers l’arrière dans le **jeu** d’enregistrements à l’aide de la syntaxe suivante :  
  
```  
oRs.Move NumRecords, Start  
```  
  
## <a name="remarks"></a>Notes  
 La méthode **Move** est prise en charge sur tous les objets **Recordset** .  
  
 Si l’argument *numRecords* est supérieur à zéro, la position actuelle de l’enregistrement se déplace vers l’avant (vers la fin de l’ensemble d' **enregistrements**). Si *numRecords* est inférieur à zéro, la position actuelle de l’enregistrement est déplacée vers l’arrière (vers le début de l’ensemble d' **enregistrements**).  
  
 Si l’appel de **déplacement** déplace la position d’enregistrement active vers un point avant le premier enregistrement, ADO définit l’enregistrement actif à la position précédant le premier enregistrement dans le **jeu d’enregistrements** (**BOF** a la **valeur true**). Une tentative de déplacement vers l’arrière lorsque la propriété **BOF** a déjà la **valeur true** génère une erreur.  
  
 Si l’appel de **déplacement** déplace la position d’enregistrement active vers un point après le dernier enregistrement, ADO définit l’enregistrement actif à la position après le dernier enregistrement dans le **jeu d’enregistrements** (**EOF** a la **valeur true**). Une tentative de déplacement vers l’avant lorsque la propriété **EOF** a déjà la **valeur true** génère une erreur.  
  
 L’appel de la méthode **Move** à partir d’un objet **Recordset** vide génère une erreur.  
  
 Si vous passez un signet dans l’argument *Start* , le déplacement est relatif à l’enregistrement avec ce signet, en supposant que l’objet **Recordset** prend en charge les signets. Un signet est obtenu à l’aide de la propriété [Bookmark](../../reference/ado-api/bookmark-property-ado.md) . S’il n’est pas spécifié, le déplacement est relatif à l’enregistrement actif.  
  
 Si vous utilisez la propriété **CacheSize** pour mettre en cache localement des enregistrements à partir du fournisseur, le passage d’un argument *numRecords* qui déplace la position de l’enregistrement actuel en dehors du groupe actuel d’enregistrements mis en cache force ADO à récupérer un nouveau groupe d’enregistrements, en commençant par l’enregistrement de destination. La propriété **CacheSize** détermine la taille du groupe nouvellement récupéré, et l’enregistrement de destination est le premier enregistrement récupéré.