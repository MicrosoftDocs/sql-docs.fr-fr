---
title: Protocoles clients - Propriétés de Canaux nommés (onglet Protocole)
description: Découvrez comment afficher ou modifier la description du canal par défaut dans le Gestionnaire de configuration SQL Server. En savoir plus sur la connexion à un autre canal.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- pipes [SQL Server], connecting to
- Named Pipes [SQL Server], default pipe
- client protocols [SQL Server]
ms.assetid: 30fbae62-2f2e-4d36-9c6e-3444fff68781
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: afcc04a5b48014843c5aedcf2c3d62fc9993fd39
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349864"
---
# <a name="client-protocols---named-pipes-properties-protocol-tab"></a>Protocoles clients - Propriétés de Canaux nommés (onglet Protocole)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Dans le Gestionnaire de configuration [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , utilisez l'onglet **Protocole** de la boîte de dialogue **Propriétés de Canaux nommés** pour visualiser ou modifier la description du canal par défaut. Pour vous connecter à un autre canal, tapez son nom dans la zone **Canal par défaut** . Pour plus d'informations sur les chaînes de connexion, consultez [Creating a Valid Connection String Using Named Pipes](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130)).  
  
## <a name="options"></a>Options  
 **Canal par défaut**  
 Spécifie le canal par défaut que la Net-Library des canaux nommés essaie d'utiliser pour se connecter à l'instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est à l'écoute sur : `\\.\pipe\sql\query`  
  
 Pour vous connecter au canal par défaut, entrez `sql\query`  
  
 **Activé**  
 Les valeurs possibles sont **Yes** et **No**.  
  
## <a name="see-also"></a>Voir aussi  
 [Choix d’un protocole réseau](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))  
  
