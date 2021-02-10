---
description: Commandes paramétrées avec des commandes COMPUTE intermédiaires
title: Commandes paramétrables avec des commandes Compute intermédiaires | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], parameterized commands
- parameterized commands [ADO]
- APPEND clause [ADO]
- COMPUTE command [ADO]
ms.assetid: 732f624f-8900-4608-9815-194302d22e8b
author: rothja
ms.author: jroth
ms.openlocfilehash: ce78d5edf2ded2744c3633368d571d72912c1322
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037219"
---
# <a name="parameterized-commands-with-intervening-compute-commands"></a>Commandes paramétrées avec des commandes COMPUTE intermédiaires
Une commande d’ajout de forme paramétrable classique a une clause qui crée un **jeu d’enregistrements** parent avec une commande de requête et une autre clause qui crée un **jeu d’enregistrements** enfant avec une commande de requête paramétrable, c’est-à-dire une commande contenant un espace réservé de paramètre (un point d’interrogation, «  ? »). Le **Recordset** mis en forme obtenu a deux niveaux, dans lesquels le parent occupe le niveau supérieur et l’enfant occupe le niveau inférieur.  
  
 La clause qui crée le **Recordset** enfant peut désormais être un nombre arbitraire de commandes de calcul de forme imbriquées, où la commande la plus profondément imbriquée contient la requête paramétrable. Le **Recordset** mis en forme qui en résulte a plusieurs niveaux, dans lesquels le parent occupe le niveau supérieur, l’enfant occupe le niveau lowermost, et un nombre arbitraire de **jeux d’enregistrements** générés par les commandes Compute de Shape occupent les niveaux intermédiaires.  
  
 L’utilisation courante de cette fonctionnalité consiste à appeler la fonction d’agrégation et les capacités de regroupement des commandes shapeCOMPUTE pour créer des objets **Recordset** intermédiaires avec des informations analytiques sur le **Recordset** enfant. En outre, étant donné qu’il s’agit d’une commande de forme paramétrable, chaque fois qu’une colonne de chapitre du parent est accessible, un nouvel ensemble **d’enregistrements** enfant peut être récupéré. Étant donné que les niveaux intermédiaires sont dérivés de l’enfant, ils sont également recalculés.  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de mise en forme des données](../../../ado/guide/data/data-shaping-example.md)
