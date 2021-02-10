---
description: Considérations relatives à la sécurité de XML
title: Considérations relatives à la sécurité XML | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- XML security in ADO
ms.assetid: fadbd38e-6e7b-4b81-96ea-85169c664374
author: rothja
ms.author: jroth
ms.openlocfilehash: 82557d241ff804ec2da6f97a1dc96073da2a2b7d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036709"
---
# <a name="xml-security-considerations"></a>Considérations relatives à la sécurité de XML
Les méthodes ADO Save et Open sur l’objet Recordset ne sont pas considérées comme des opérations sécurisées pour s’exécuter dans Internet Explorer. Ainsi, si ces méthodes sont utilisées dans un code de script qui s’exécute dans une application ou un contrôle hébergé dans un navigateur, la configuration de la sécurité du navigateur aura un effet sur son comportement.  
  
 Internet Explorer 5 fournit des restrictions de sécurité pour ces opérations par défaut dans les zones Internet. Dans cette configuration, le jeu d’enregistrements ne peut pas accéder au système de fichiers local sur le client ou accéder à des sources de données situées en dehors du domaine du serveur à partir duquel la page a été téléchargée. Plus précisément, lors de l’exécution au sein de l’hôte du navigateur, un jeu d’enregistrements peut être enregistré dans un fichier uniquement s’il se trouve sur le même serveur que celui à partir duquel la page a été téléchargée. De même, vous pouvez ouvrir un Recordset en le chargeant à partir d’un fichier uniquement si ce fichier se trouve sur le même serveur que celui à partir duquel la page a été téléchargée.  
  
## <a name="see-also"></a>Voir aussi  
 [Persistance des enregistrements au format XML](./persisting-records-in-xml-format.md)