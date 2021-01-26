---
title: Se connecter à SQL Server avec un serveur proxy (Gestionnaire de configuration SQL Server) | Microsoft Docs
description: Découvrez comment utiliser le Gestionnaire de configuration SQL Server pour se connecter à SQL Server par le biais d’un serveur proxy. Découvrez comment utiliser Remote WinSock (RWS) pour écouter à distance.
ms.custom: ''
ms.date: 12/15/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- Remote WinSock
- RWS
- LATs
- proxy servers [SQL Server]
- connections [SQL Server], proxy server
- Microsoft Proxy Server [SQL Server]
- local address tables [SQL Server]
ms.assetid: 39714de0-2a1f-4179-9091-5c3fa4612545
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 325bdae16fdc8309d0f40134ba31041e5b47b547
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783259"
---
# <a name="connect-to-sql-server-through-a-proxy-server-sql-server-configuration-manager"></a>Se connecter à SQL Server par le biais d'un serveur proxy (Gestionnaire de configuration SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment se connecter à SQL Server via un serveur proxy dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide du Gestionnaire de configuration SQL Server. Pour pouvoir écouter à distance au moyen de RWS (Remote WinSock), définissez la table d'adresses locales (LAT, Local Address Table) du serveur proxy pour que l'adresse du nœud à l'écoute se situe en dehors de la plage des entrées LAT.  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Utilisation du Gestionnaire de configuration SQL Server  
  
#### <a name="to-enable-connections-to-sql-server-through-proxy-server"></a>Pour autoriser les connexions à SQL Server via un serveur proxy  
  
1.  Suivez les étapes décrites dans [Configurer un serveur pour écouter un port TCP spécifique &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md) pour déterminer quels ports TCP/IP sont utilisés par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ou pour configurer le [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour qu’il utilise le port de votre choix.  
  
2.  Dans votre serveur proxy, définissez la table d'adresses locales de ce serveur pour que l'adresse du nœud à l'écoute se situe en dehors de la plage des entrées de la table. Pour plus d'informations, consultez la documentation de votre serveur proxy.  
  
> [!NOTE]
>  Cette rubrique s’applique à [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]en local. En cas de problèmes de connexion associés à [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], consultez [Résoudre des problèmes de connexion à la base de données SQL Azure](/azure/sql-database/sql-database-troubleshoot-common-connection-issues).
