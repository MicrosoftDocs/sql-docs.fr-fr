---
title: Protocoles clients – Propriétés TCP/IP (onglet Protocole)
description: Découvrez comment spécifier des options TCP/IP dans le Gestionnaire de configuration SQL Server, telles que le paramètre Keep Alive et le numéro de port par défaut.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- TCP/IP [SQL Server], client protocols
- client protocols [SQL Server]
ms.assetid: d04f1bce-069c-4a02-b561-c87c3282be36
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: b173a2ac4e735f5a3924e957baab784ecc828efd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349831"
---
# <a name="client-protocols---tcp-ip-properties-protocol-tab"></a>Protocoles clients – Propriétés TCP/IP (onglet Protocole)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Dans le Gestionnaire de connexions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , utilisez l’onglet **Protocole** de la boîte de dialogue **Propriétés TCP/IP** pour afficher ou spécifier les options suivantes. Pour vous connecter à un autre port, tapez le numéro du port dans la zone **Port par défaut** . Pour plus d’informations sur les chaînes de connexion, consultez [Création d’une chaîne de connexion valide à l’aide du protocole TCP/IP](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md).  
  
## <a name="options"></a>Options  
 **Port par défaut**  
 Spécifie le port par défaut que la Net-Library TCP/IP essaie d'utiliser pour se connecter à l'instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le port par défaut est 1433.  
  
 Lorsqu'un client se connecte à une instance par défaut du [!INCLUDE[ssDE](../../includes/ssde-md.md)], il utilise cette valeur. Si une instance par défaut a été configurée pour écouter sur un port différent, remplacez cette valeur par ce numéro de port.  
  
 Lorsqu'un client se connecte à une instance nommée de [!INCLUDE[ssDE](../../includes/ssde-md.md)], il essaie d'obtenir le numéro de port auprès du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser exécuté sur l'ordinateur serveur. Si le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser n'est pas en cours d'exécution, le numéro de port doit être spécifié par le biais de ce paramètre ou dans la chaîne de connexion.  
  
 **Activé**  
 Les valeurs possibles sont **Yes** et **No**.  
  
 **Keep Alive**  
 Ce paramètre (en millisecondes) contrôle la fréquence à laquelle TCP tente de vérifier qu’une connexion inactive est toujours intacte en envoyant un paquet **KEEPALIVE** . La valeur par défaut est 30  000 millisecondes.  
  
 **Intervalle Keep Alive**  
 Ce paramètre (en millisecondes), détermine l’intervalle qui sépare les retransmissions **KEEPALIVE** jusqu’à ce qu’une réponse soit reçue. La valeur par défaut est 1 000 millisecondes.  
  
## <a name="see-also"></a>Voir aussi  
 [Choix d'un protocole réseau](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))   
 [Nouvel alias &#40;onglet Alias&#41;](../../tools/configuration-manager/new-alias-alias-tab.md)   
 [Propriétés d’&#60;alias&#62; &#40;onglet Alias&#41;](../../tools/configuration-manager/alias-properties-alias-tab.md)  
  
