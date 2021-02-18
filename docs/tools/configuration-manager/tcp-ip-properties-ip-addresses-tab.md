---
title: Propriétés TCP/IP (onglet Adresses IP)
description: Découvrez comment utiliser l’onglet Adresses IP de la boîte de dialogue Propriétés TCP/IP pour configurer les options de protocole TCP/IP pour une adresse IP spécifique dans SQL Server.
ms.custom: seo-lt-2019
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- ports [SQL Server], listening on
- listening [SQL Server], on ports
ms.assetid: 4c17ed45-9da7-4bec-bce6-970109fe7365
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 56055b7d3537413398980579342884ab6a58c142
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345083"
---
# <a name="tcpip-properties-ip-addresses-tab"></a>Propriétés TCP/IP (onglet Adresses IP)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  La boîte de dialogue **Propriétés TCP/IP (onglet Adresses IP)** permet de configurer les options du protocole TCP/IP pour une adresse IP spécifique. Seules les options **Ports TCP dynamiques** et **Port TCP** peuvent être configurées pour toutes les adresses en une seule fois en sélectionnant **IPAll**.  
  
 Les modifications prennent effet lors du redémarrage de SQL Server. Pour obtenir des informations sur le démarrage et l’arrêt du service SQL Server Browser, consultez [Démarrer et arrêter le service SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
## <a name="static-vs-dynamic-ports"></a>Ports statiques et Ports dynamiques  
 L’instance par défaut de SQL Server écoute les connexions entrantes sur le port 1433. Le port peut être modifié pour des raisons de sécurité ou parce qu'une application cliente le requiert. Par défaut, les instances nommées (y compris SQL Server Express) sont configurées pour être à l'écoute sur les ports dynamiques. Pour configurer un port statique, ne complétez pas la zone **Ports TCP dynamiques** et spécifiez un numéro de port disponible dans la zone **Port TCP** . Pour plus d'informations sur l'ouverture de ports dans le pare-feu, consultez Configuration du Pare-feu Windows pour autoriser l'accès à SQL Server dans la documentation en ligne.  
  
## <a name="dynamic-ports"></a>Ports dynamiques  
 Au démarrage, lorsqu’une instance de SQL Server est configurée pour écouter sur des ports dynamiques, elle vérifie s’il existe un port disponible dans le système d’exploitation et ouvre un point de terminaison pour ce port. Les connexions entrantes doivent spécifier ce numéro de port pour se connecter. Étant donné qu’il est possible de modifier le numéro de port à chaque démarrage de SQL Server, SQL Server fournit le service SQL Server Browser pour analyser les ports et diriger les connexions entrantes vers le port actif de cette instance. L’utilisation des ports dynamiques complique la connexion de SQL Server par le biais d’un pare-feu, car le numéro de port peut changer lors du redémarrage de SQL Server, ce qui nécessite la modification des paramètres du pare-feu. Pour éviter les problèmes de connexion liés à un pare-feu, configurez SQL Server de manière à utiliser un port statique.  
  
## <a name="options"></a>Options  
 **Actif**  
 Indique que l'adresse IP est active sur l'ordinateur. Option non disponible pour **IPAll**.  
  
 **Activé**  
 Si la propriété **Écouter tout** des **Propriétés TCP/IP (onglet Protocole)** a la valeur **Non**, elle indique si SQL Server est à l’écoute sur l’adresse IP. Si la propriété **Écouter tout** des **Propriétés TCP/IP (onglet Protocole)** a la valeur **Oui**, la propriété est ignorée. Option non disponible pour **IPAll**.  
  
 **Adresse IP**  
 Afficher ou modifier l'adresse IP utilisée par cette connexion. Indique l'adresse IP utilisée par l'ordinateur et l'adresse de boucle IP, 127.0.0.1. Option non disponible pour **IPAll**. L'adresse IP peut être au format IPv4 ou IPv6.  
  
 **Ports TCP dynamiques**  
 Aucune valeur si les ports ne sont pas activés. Pour utiliser les ports dynamiques, indiquez la valeur 0.  
  
 Pour **IPAll**, affiche le numéro du port dynamique utilisé.  
  
 **Port TCP**  
 Afficher ou modifier le port sur lequel écoute SQL Server. Par défaut, l’instance par défaut de SQL Server écoute sur le port 1433.  
  
 Le moteur de base de données peut être à l’écoute sur plusieurs ports sur la même adresse IP et répertorie les ports, séparés par des virgules, au format 1433,1500,1501. Ce champ est limité à 2 047 caractères.  
  
 Pour configurer une seule adresse IP pour l’écoute sur plusieurs ports, le paramètre **Écouter tout** doit également avoir la valeur **Non**, sous l’onglet **Protocoles** de la boîte de dialogue **Propriétés de TCP/IP** . Pour plus d’informations, consultez « Procédure : Configurer le moteur de base de données de manière à écouter sur plusieurs ports TCP » dans la documentation en ligne de SQL Server.  
  
## <a name="adding-or-removing-ip-addresses"></a>Ajout ou suppression d'adresses IP  
 Le Gestionnaire de configuration SQL Server affiche les adresses IP qui étaient disponibles au moment de l’installation de SQL Server. Les adresses IP disponibles peuvent changer lorsque : des cartes réseau sont ajoutées ou supprimées, une adresse IP affectée dynamiquement arrive à expiration, une structure de réseau est reconfigurée ou un ordinateur est déplacé physiquement, par exemple si un ordinateur portable est connecté au réseau d'un autre bâtiment. Pour changer une adresse IP, modifiez la zone **Adresse IP** , puis redémarrez SQL Server.  
  
## <a name="additional-topics-in-books-online"></a>Autres rubriques dans la documentation en ligne  
 Recherchez par exemple les rubriques **Configurer un serveur pour écouter un port TCP spécifique (Gestionnaire de configuration SQL Server)** et **Configurer le moteur de base de données de manière à écouter sur plusieurs ports TCP** dans MSDN.  
  
## <a name="see-also"></a>Voir aussi  
 [Choix d'un protocole réseau](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)   
 [Création d’une chaîne de connexion valide avec TCP/IP](creating-a-valid-connection-string-using-tcp-ip.md)   
 [Service SQL Server Browser](sql-server-browser-service.md)  
  
