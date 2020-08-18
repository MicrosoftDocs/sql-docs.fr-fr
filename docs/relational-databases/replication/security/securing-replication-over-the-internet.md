---
description: Sécurité de la réplication sur Internet
title: Sécurité de la réplication sur Internet | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- security [SQL Server replication], Internet
- Internet [SQL Server replication], security
ms.assetid: 25b7af05-2721-4b24-9083-fb671e8bf4e0
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 5242c8e1d42c19f45eda2f1cc03e501e7aa32b95
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88326585"
---
# <a name="securing-replication-over-the-internet"></a>Sécurité de la réplication sur Internet
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  La réplication sur Internet est une solution souple, notamment pour les Abonnés itinérants, à condition qu'elle soit configurée correctement afin d'assurer une sécurité adaptée. Pour partager des informations sur Internet en toute sécurité,[!INCLUDE[msCoName](../../../includes/msconame-md.md)] recommande l'utilisation de l'une des deux techniques suivantes :  
  
-   le réseau privé virtuel (VPN, Virtual Private Network) ;  
  
-   l'option de synchronisation Web pour la réplication de fusion.  
  
## <a name="virtual-private-network"></a>Réseau privé virtuel  
 Les réseaux privés virtuels fournissent une méthode en couche simple et sécurisée pour répliquer des données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur Internet. La connexion au réseau privé virtuel via Internet fonctionne, sur le plan logique, comme une liaison de réseau étendu (WAN, Wide Area Network) entre les sites.  
  
 Ce processus n'est possible qu'en établissant une connexion tunnel pour l'utilisateur via Internet ou tout autre réseau public à l'aide d'un protocole tel que le protocole [!INCLUDE[msCoName](../../../includes/msconame-md.md)] PPTP (Point-to-Point Tunneling Protocol), disponible dans le système d'exploitation [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows 2000, ou le protocole L2TP (Layer Two Tunneling Protocol), disponible dans le système d'exploitation Windows 2000. Ce processus offre la sécurité et des fonctionnalités similaires à celles qui sont disponibles dans un réseau privé.  
  
 Pour plus d'informations sur la configuration d'un réseau privé virtuel (VPN), consultez la documentation [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows.  
  
## <a name="web-synchronization-through-iis"></a>Synchronisation Web par le biais d'IIS  
 L'option de synchronisation Web pour la réplication de fusion permet de répliquer des données via le protocole HTTPS, ce qui peut être une méthode de réplication des données pratique via un pare-feu. Pour plus d’informations sur la sécurité de la synchronisation web, consultez [Configurer la synchronisation web](../../../relational-databases/replication/configure-web-synchronization.md) et [Architecture de la sécurité pour la synchronisation web](../../../relational-databases/replication/security/security-architecture-for-web-synchronization.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Afficher et modifier les paramètres de sécurité de la réplication](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)  
  
  
