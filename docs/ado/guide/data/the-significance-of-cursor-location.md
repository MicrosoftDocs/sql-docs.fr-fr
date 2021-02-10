---
description: Signification de l’emplacement du curseur
title: L’importance de l’emplacement du curseur | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- server-side cursors [ADO]
- cursors [ADO], client-side
- client-side cursors [ADO]
- cursors [ADO], server-side
ms.assetid: 70ef5b1c-0459-41a1-b796-031f61a29a8a
author: rothja
ms.author: jroth
ms.openlocfilehash: a3caa56a5c3560367cc2f816c6aa2b1c12cbba2b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036929"
---
# <a name="the-significance-of-cursor-location"></a>Signification de l’emplacement du curseur
Chaque curseur utilise des ressources temporaires pour stocker ses données. Ces ressources peuvent être de la mémoire, un fichier d’échange de disque, des fichiers de disque temporaire ou même un stockage temporaire dans la base de données. Le curseur est appelé curseur *côté client* lorsque ces ressources se trouvent sur l’ordinateur client. Le curseur est appelé curseur *côté serveur* lorsque ces ressources se trouvent sur le serveur.  
  
## <a name="client-side-cursors"></a>Curseurs de Client-Side  
 Dans ADO, appelez pour un curseur côté client à l’aide de l' **CursorLocationEnum adUseClient.** Avec un curseur côté client non keyset, le serveur envoie l’intégralité du jeu de résultats sur le réseau à l’ordinateur client. L’ordinateur client fournit et gère les ressources temporaires requises par le curseur et le jeu de résultats. L’application côté client peut parcourir l’ensemble du jeu de résultats afin de déterminer les lignes dont elle a besoin.  
  
 Les curseurs côté client statiques et pilotés par keyset peuvent placer une charge importante sur votre station de travail s’ils incluent trop de lignes. Alors que toutes les bibliothèques de curseurs sont en charge de la création de curseurs avec des milliers de lignes, les applications conçues pour extraire ces ensembles de lignes de grande taille peuvent s’exécuter de manière médiocre. Bien entendu, il existe des exceptions. Pour certaines applications, un grand curseur côté client peut être parfaitement approprié et les performances peuvent ne pas être un problème.  
  
 L’un des avantages évidents du curseur côté client est la réponse rapide. Une fois que le jeu de résultats a été téléchargé sur l’ordinateur client, l’exploration des lignes est très rapide. Votre application est généralement plus évolutive avec les curseurs côté client car les besoins en ressources du curseur sont placés sur chaque client distinct et non sur le serveur.  
  
## <a name="server-side-cursors"></a>Curseurs de Server-Side  
 Dans ADO, appelez pour un curseur côté serveur à l’aide de la **CursorLocationEnum adUseServer.** Avec un curseur côté serveur, le serveur gère le jeu de résultats à l’aide des ressources fournies par l’ordinateur serveur. Le curseur côté serveur retourne uniquement les données demandées sur le réseau. Ce type de curseur peut parfois fournir de meilleures performances que le curseur côté client, en particulier dans les situations où un trafic réseau excessif est un problème.  
  
 Toutefois, il est important de souligner qu’un curseur côté serveur est, au moins, à consommer temporairement des ressources serveur précieuses pour chaque client actif. Vous devez planifier en conséquence pour vous assurer que le matériel de votre serveur est capable de gérer tous les curseurs côté serveur demandés par les clients actifs. En outre, un curseur côté serveur peut être lent, car il ne fournit qu’un seul accès à une seule ligne. aucun curseur batch n’est disponible.  
  
 Les curseurs côté serveur sont utiles lors de l’insertion, de la mise à jour ou de la suppression d’enregistrements. Avec les curseurs côté serveur, vous pouvez avoir plusieurs instructions actives sur la même connexion.
