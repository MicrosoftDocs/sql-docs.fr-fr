---
description: Déconnexion et reconnexion du recordset
title: Déconnexion et reconnexion du Recordset | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset object [ADO], disconnecting and reconnecting
ms.assetid: c5134af7-81d6-4de4-9fd1-cfe29973545e
author: rothja
ms.author: jroth
ms.openlocfilehash: dacaeae0406e42bb39c7fb7836aab20ff2bc1ad5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033239"
---
# <a name="disconnecting-and-reconnecting-the-recordset"></a>Déconnexion et reconnexion du recordset
L’une des fonctionnalités les plus puissantes de ADO est la possibilité d’ouvrir un jeu d’enregistrements côté client à partir d’une source de données, puis de déconnecter le Recordset de la source de données. Une fois le jeu d’enregistrements déconnecté, la connexion à la source de données peut être fermée, libérant ainsi les ressources sur le serveur utilisé pour la gérer. Vous pouvez continuer à afficher et modifier les données dans le jeu d’enregistrements pendant qu’il est déconnecté, puis vous reconnecter ultérieurement à la source de données et envoyer vos mises à jour en mode batch.  
  
 Pour déconnecter un jeu d’enregistrements, ouvrez-le avec un emplacement de curseur égal à adUseClient, puis affectez à la propriété ActiveConnection la valeur Nothing. (Les utilisateurs C++ doivent définir le ActiveConnection comme étant égal à NULL pour se déconnecter.)  
  
 Nous utiliserons un jeu d’enregistrements déconnecté plus loin dans cette section quand nous aborderons la persistance des jeux d’enregistrements pour traiter un scénario dans lequel les données d’un jeu d’enregistrements sont disponibles pour une application alors que l’ordinateur client n’est pas connecté à un réseau.  
  
## <a name="see-also"></a>Voir aussi  
 [Mode Batch](./batch-mode.md)