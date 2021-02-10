---
description: Qu’est qu’un verrou ?
title: Qu’est qu’un verrou ? | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], locking
- locks [ADO], about locking
ms.assetid: f8989555-28c6-4c17-9bf8-7f44a8a5c407
author: rothja
ms.author: jroth
ms.openlocfilehash: 245a0cd91b9d6c7675df146d9d5808b42bb8c618
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036729"
---
# <a name="what-is-a-lock"></a>Qu’est qu’un verrou ?
Le verrouillage est le processus par lequel un SGBD restreint l’accès à une ligne dans un environnement multi-utilisateur. Lorsqu’une ligne ou une colonne est verrouillée de manière exclusive, les autres utilisateurs ne sont pas autorisés à accéder aux données verrouillées tant que le verrou n’est pas libéré. Cela permet de s’assurer que deux utilisateurs ne peuvent pas simultanément mettre à jour la même colonne dans une ligne.  
  
 Les verrous peuvent être très coûteux du point de vue des ressources et doivent être utilisés uniquement lorsque cela est nécessaire pour préserver l’intégrité des données. Dans une base de données où des centaines ou des milliers d’utilisateurs peuvent tenter d’accéder à un enregistrement chaque seconde, par exemple une base de données connectée à Internet, un verrouillage inutile peut rapidement entraîner une baisse des performances dans votre application.  
  
 Vous pouvez contrôler la façon dont la source de données et la bibliothèque de curseurs ADO gèrent la concurrence en choisissant l’option de verrouillage appropriée.  
  
 Définissez la propriété **LockType** avant d’ouvrir un **Recordset** pour spécifier le type de verrouillage que le fournisseur doit utiliser lors de son ouverture. Lisez la propriété pour retourner le type de verrouillage en cours d’utilisation sur un objet **Recordset** ouvert.  
  
 Les fournisseurs ne prennent peut-être pas en charge tous les types de verrous. Si un fournisseur ne prend pas en charge le paramètre **LockType** demandé, il remplace un autre type de verrouillage. Pour déterminer les fonctionnalités de verrouillage réelles disponibles dans un objet **Recordset** , utilisez la méthode [supports](../../../ado/reference/ado-api/supports-method.md) avec **adUpdate** et **adUpdateBatch**.  
  
 Le paramètre **adLockPessimistic** n’est pas pris en charge si la propriété [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) a la valeur **adUseClient.** Si une valeur non prise en charge est définie, aucune erreur ne se produit ; le **LockType** le plus proche pris en charge sera utilisé à la place.  
  
 La propriété **LockType** est en lecture/écriture lorsque le **Recordset** est fermé et en lecture seule lorsqu’elle est ouverte.  
  
 Cette section contient les rubriques suivantes :  
  
-   [Types de verrous](../../../ado/guide/data/types-of-locks.md)
