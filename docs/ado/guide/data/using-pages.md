---
description: Utilisation de pages
title: Utilisation des pages | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- PageSize property [ADO]
- pages [ADO]
- Recordset object [ADO]
- AbsolutePage property [ADO]
- PageCount property [ADO]
ms.assetid: 442b08c5-ccc7-4192-a1cc-22f250867782
author: rothja
ms.author: jroth
ms.openlocfilehash: aae6cbd7d843771fbf88d6d257706ce66e424745
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036759"
---
# <a name="using-pages"></a>Utilisation de pages
Utilisez la propriété **PageCount** pour déterminer le nombre de pages de données qui se trouvent dans l’objet **Recordset** . Les *pages* sont des groupes d’enregistrements dont la taille est égale au paramètre de la propriété **pageSize** . Même si la dernière page est incomplète parce qu’il y a moins d’enregistrements que la valeur **pageSize** , elle est comptabilisée comme une page supplémentaire dans la valeur **PageCount** . Si l’objet **Recordset** ne prend pas en charge cette propriété, **PageCount** aura la valeur-1 pour indiquer que l’objet **PageCount** est non déterminable.  
  
 Utilisez la propriété **pageSize** pour déterminer le nombre d’enregistrements constituant une page logique de données. L’établissement d’une taille de page vous permet d’utiliser la propriété **AbsolutePage** pour accéder au premier enregistrement d’une page particulière. Cela est utile dans les scénarios de serveur Web lorsque vous souhaitez autoriser l’utilisateur à parcourir les données, en affichant un certain nombre d’enregistrements à la fois.  
  
 Cette propriété peut être définie à tout moment et sa valeur sera utilisée pour calculer l’emplacement du premier enregistrement d’une page particulière.  
  
 Utilisez la propriété **AbsolutePage** pour identifier le numéro de la page sur laquelle se trouve l’enregistrement en cours. Là encore, le fournisseur doit prendre en charge les fonctionnalités appropriées pour que cette propriété soit disponible.  
  
 **AbsolutePage** est de base 1 et est égal à 1 lorsque l’enregistrement actif est le premier enregistrement dans le **Recordset**. Définissez cette propriété pour passer au premier enregistrement d’une page particulière. Obtient le nombre total de pages de la propriété **PageCount** .
