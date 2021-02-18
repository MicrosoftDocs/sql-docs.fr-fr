---
title: Propriétés de Mémoire partagée
description: Découvrez comment activer ou désactiver le protocole de mémoire partagée, que les clients peuvent utiliser pour se connecter à une instance SQL s’exécutant sur le même ordinateur.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- shared memory [SQL Server]
ms.assetid: dc1704da-eacd-4d26-b529-c996f958ca4b
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 8f5d913a9517ef441b50ea4bc3e8cbac5791ce49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349640"
---
# <a name="shared-memory-properties"></a>Propriétés de Mémoire partagée
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  La page **Protocole** de la boîte de dialogue **Propriétés de Mémoire partagée** permet d’activer ou de désactiver le protocole de mémoire partagée. Il s'agit du protocole le plus simple à utiliser et pour lequel aucun paramètre ne doit être configuré. Étant donné que les clients qui utilisent le protocole de mémoire partagée peuvent se connecter uniquement à une instance [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécutée sur le même ordinateur, ce protocole n’est généralement pas utile pour les activités de base de données. Utilisez le protocole de mémoire partagée pour débloquer une situation dans laquelle vous suspectez que les autres protocoles ne sont pas configurés correctement.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être redémarré pour activer ou désactiver le protocole.  
  
## <a name="options"></a>Options  
 **Activé**  
 Les valeurs possibles sont **Yes** et **No**. Le protocole de mémoire partagée est activé par défaut.  
  
## <a name="see-also"></a>Voir aussi  
 [Choix d'un protocole réseau](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))   
 [Création d'une chaîne de connexion valide à l'aide du protocole de mémoire partagée](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)  
  
