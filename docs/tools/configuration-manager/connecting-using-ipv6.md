---
title: Connexion avec IPv6
description: Découvrez le support d’IPv4 et IPv6 dans SQL Server et SQL Server Native Client et comment configurer le Moteur de base de données pour l’adresse que vous souhaitez utiliser.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- Internet Protocol
- IPv4
- IPv6
ms.assetid: 2669098c-f5f1-43da-aec6-e91003ac89f6
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: bd244c33134c3864a85bebb97439fdd44f1df7d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349842"
---
# <a name="connecting-using-ipv6"></a>Connexion avec IPv6
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client prennent intégralement en charge IPv4 (Internet Protocol version 4) et IPv6 (Internet Protocol version 6). Quand Windows est configuré avec IPv6 les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]détectent automatiquement la présence d'IPv6. Aucune configuration particulière de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est nécessaire.  
  
 La prise en charge inclut, entre autres, les éléments suivants :  
  
-   Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et les autres composants serveur peuvent écouter simultanément sur les deux adresses IPv4 et IPv6. Quand IPv4 et IPv6 sont tous deux présents, vous pouvez utiliser le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Gestionnaire de configuration pour configurer le [!INCLUDE[ssDE](../../includes/ssde-md.md)] de façon à n'écouter que les adresses IPv4 ou IPv6.  
  
-   Quand le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser en cours d'exécution sur un ordinateur qui prend en charge à la fois IPv4 et IPv6 est interrogé sur une adresse IPv4, il répond avec une adresse IPv4 et le premier port TCP IPv4 de la liste. En cas d'interrogation sur une adresse IPv6, il répond avec une adresse IPv6 et le premier port TCP IPv6 de la liste. Pour éviter toute incohérence, nous recommandons de configurer les écouteurs IPv4 et IPv6 de façon à ce qu'ils écoutent sur le même port.  
  
-   Des outils comme [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] acceptent les deux formats IPv4 et IPv6 pour les adresses IP. Dans la plupart des cas, la chaîne de connexion n’a pas besoin d’être modifiée si \<*computer_name*>\\<*instance_name*> est spécifié avec le nom d’hôte du serveur ou le nom de domaine complet. Si l'ordinateur serveur possède à la fois IPv4 et IPv6, son nom d'hôte ou son nom de domaine complet est résolu en plusieurs adresses IP, incluant au moins une adresse IPv4 et plusieurs adresses IPv6. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client tente d’établir des connexions à l’aide de ces adresses IP en respectant l’ordre reçu à partir de TCP/IP et utilise la première connexion qui aboutit. Du fait que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client est dans l'impossibilité de prévoir l'ordre, celui-ci doit être considéré comme aléatoire. Les adresses IPv4 sont tentées en premier si les adresses IPv4 et IPv6 sont toutes deux présentes. Pour les utilisateurs d'ODBC, OLE DB ou ADO.NET, la logique est totalement transparente.  
  
    > [!NOTE]  
    >  Si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] n’écoute pas sur IPv4, la tentative de connexion IPv4 doit attendre la période d’expiration avant de tenter l’adresse IPv6. Pour pallier ce désagrément, connectez-vous directement à l'adresse IPv6 ou configurez un alias sur le client avec l'adresse IPv6.  
  
## <a name="see-also"></a>Voir aussi  
 [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md)  
  
  
