---
description: SET BLOCKSIZE, commande
title: SET BlockSize, commande | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- set blocksize command [ODBC]
ms.assetid: 0c11580f-37f5-4a8e-99be-9fb9c44bb433
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6677a397542f4bcd7f27b11cd032092b7087a9fd
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88466391"
---
# <a name="set-blocksize-command"></a>SET BLOCKSIZE, commande
Spécifie la manière dont l’espace disque est alloué pour le stockage des champs Mémo.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET BLOCKSIZE TO nBytes  
```  
  
## <a name="arguments"></a>Arguments  
 *nBytes*  
 Spécifie la taille de bloc dans laquelle l’espace disque pour les champs mémo est alloué. Si *nBytes* est égal à 0, l’espace disque est alloué en octets uniques (blocs de 1 octet). Si *nBytes* est un entier compris entre 1 et 32, l’espace disque est alloué dans des blocs d’octets *nBytes* multipliés par 512. Si *nBytes* est supérieur à 32, l’espace disque est alloué en blocs de *nBytes* octets. Si vous spécifiez une valeur de taille de bloc supérieure à 32, vous pouvez économiser un espace disque important.  
  
## <a name="remarks"></a>Notes  
 La valeur par défaut de l’option SET BlockSize est 64. Pour rétablir la taille du bloc à une valeur différente après la création du fichier, affectez-lui une nouvelle valeur, puis utilisez l’opération copier pour créer une nouvelle table. La nouvelle table a la taille de bloc spécifiée.
