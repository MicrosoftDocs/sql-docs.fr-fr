---
title: Protocoles réseau et bibliothèques réseau | Microsoft Docs
description: Un serveur peut être configuré pour surveiller plusieurs protocoles réseau. Vous pouvez modifier la configuration à l’aide du Gestionnaire de configuration SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- protocols [SQL Server]
- configuration options [SQL Server], protocols
- network libraries [SQL Server]
- protocols [SQL Server], about network protocols
- pipes [SQL Server]
- network protocols [SQL Server]
- default SQL Server configurations
- library [SQL Server]
- network protocols [SQL Server], about network protocols
- configuration options [SQL Server], libraries
ms.assetid: 8cd437f6-9af1-44ce-9cb0-4d10c83da9ce
author: cawrites
ms.author: chadam
ms.openlocfilehash: 81bf039f0a63aaad7a2214c48bb5948618e7ce7e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336155"
---
# <a name="network-protocols-and-network-libraries"></a>Protocoles réseau et bibliothèques réseau
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Un serveur peut être à l'écoute ou contrôler plusieurs protocoles réseau simultanément. Cependant, chaque protocole doit être configuré. Lorsqu'un protocole spécifique n'est pas configuré, le serveur ne peut pas se placer à l'écoute de ce protocole. Après l'installation, vous pouvez modifier ces configurations de protocole avec le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="default-sql-server-network-configuration"></a>Configuration réseau par défaut de SQL Server  
 Une instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est configurée pour le port TCP/IP 1433 et le canal nommé \\\\.\pipe\sql\query. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont configurées pour des ports dynamiques TCP, avec un numéro de port attribué par le système d'exploitation.  
  
 Si vous ne pouvez pas utiliser les adresses de port dynamiques (par exemple, lorsque des connexions d' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent traverser un serveur pare-feu configuré pour passer des adresses de port spécifiques), sélectionnez un numéro de port non assigné. Les affectations de numéro de port sont gérées par l’IANA (Internet Assigned Numbers Authority) et sont listées dans [https://www.iana.org](https://go.microsoft.com/fwlink/?LinkId=48844).  
  
 Pour renforcer la sécurité, la connectivité réseau n'est pas entièrement activée lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé. Pour activer, désactiver et configurer les protocoles réseau une fois l'installation terminée, utilisez la zone Configuration du réseau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="server-message-block-protocol"></a>Protocole SMB  
 Les serveurs déployés dans le réseau de périmètre doivent avoir tous les protocoles inutilisés désactivés, y compris le protocole SMB (Server Message Block). Les serveurs Web et les serveurs DNS (Domain Name System) ne nécessitent pas SMB. Ce protocole doit être désactivé pour limiter le risque lié à l'énumération des utilisateurs.  
  
> [!WARNING]
>  La désactivation du protocole SMB empêchera au service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Windows Cluster d'accéder au partage de fichiers distant. Ne désactivez pas SMB si vous effectuez l'une des opérations suivantes ou si vous planifiez de le faire :  
> 
>  -   Utiliser le nœud de cluster Windows et le mode de quorum majoritaire du partage de fichiers.  
> -   Spécifier un partage de fichiers SMB comme répertoire de données pendant l'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
> -   Créer un fichier de base de données sur un partage de fichiers SMB.  
  
#### <a name="to-disable-smb"></a>Pour désactiver le protocole SMB  
  
1.  Dans le menu **Démarrer** , pointez sur **Paramètres**, puis cliquez sur **Connexions réseau et accès à distance**.  
  
     Cliquez avec le bouton droit sur la connexion Internet, puis cliquez sur **Propriétés**.  
  
2.  Activez la case à cocher **Client pour les réseaux Microsoft** , puis cliquez sur **Désinstaller**.  
  
3.  Suivez les étapes de désinstallation.  
  
4.  Sélectionnez **Partage de fichiers et d'imprimantes pour les réseaux Microsoft**, puis cliquez sur **Désinstaller**.  
  
5.  Suivez les étapes de désinstallation.  
  
#### <a name="to-disable-smb-on-servers-accessible-from-the-internet"></a>Pour désactiver SMB sur les serveurs accessibles à partir d'Internet  
  
-   Dans les propriétés de Connexion au réseau local, utilisez la boîte de dialogue **Propriétés de Protocole Internet TCP/IP** pour supprimer **Partage de fichiers et d’imprimantes pour les réseaux Microsoft** et **Client pour les réseaux Microsoft**.  
  
## <a name="endpoints"></a>Points de terminaison  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] introduit un nouveau concept pour les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; la connexion est représentée à l’extrémité du serveur sous la forme d’un [!INCLUDE[tsql](../../includes/tsql-md.md)]*point de terminaison*. Des autorisations peuvent être accordées, retirées et refusées pour les points de terminaison [!INCLUDE[tsql](../../includes/tsql-md.md)] . Par défaut, tous les utilisateurs ont l'autorisation d'accéder à un point de terminaison sauf si cette autorisation est refusée ou retirée par un membre du groupe sysadmin ou par le propriétaire du point de terminaison. La syntaxe GRANT, REVOKE et DENY ENDPOINT utilise un ID de point de terminaison que l'administrateur doit prendre dans l'affichage catalogue du point de terminaison.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée des points de terminaison [!INCLUDE[tsql](../../includes/tsql-md.md)] pour tous les protocoles réseau pris en charge, ainsi que pour la connexion administrateur dédiée.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] créés par le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se présentent comme suit :  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] ordinateur local  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] canaux nommés  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] par défaut  
  
 Pour plus d’informations sur les points de terminaison, consultez [Configurer le moteur de base de données de manière à écouter sur plusieurs ports TCP](../../database-engine/configure-windows/configure-the-database-engine-to-listen-on-multiple-tcp-ports.md) et [Affichages catalogue de points de terminaison &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md).  
  
 Pour plus d’informations sur les configurations réseau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez les articles suivants dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   [Configuration réseau du serveur](../../database-engine/configure-windows/server-network-configuration.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Configuration de la surface d'exposition](../../relational-databases/security/surface-area-configuration.md)   
 [Considérations sur la sécurité pour une installation SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)   
 [Planification d'une installation SQL Server](../../sql-server/install/planning-a-sql-server-installation.md)  
  
  
