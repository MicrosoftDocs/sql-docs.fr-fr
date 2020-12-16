---
description: Démarrer le Moniteur de réplication
title: Démarrer le Moniteur de réplication | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Replication Monitor, starting
ms.assetid: e037bd27-cc87-4ee9-9e5f-83f6d717cfa4
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: d88c974bbf6d8f18271f4ea6f68b95e5c0506203
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97432547"
---
# <a name="start-the-replication-monitor"></a>Démarrer le Moniteur de réplication
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Vous pouvez démarrer le moniteur de réplication depuis [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] dans n'importe quelle instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]ou à partir de l'invite de commandes. Après avoir démarré le Moniteur de réplication, ajoutez un ou plusieurs serveurs de publication à surveiller. Pour plus d’informations, consultez [Ajouter et supprimer des serveurs de publication à partir du moniteur de réplication](../../../relational-databases/replication/monitor/add-and-remove-publishers-from-replication-monitor.md).  
  
### <a name="to-start-replication-monitor-from-sql-server-management-studio"></a>Pour démarrer le moniteur de réplication à partir de SQL Server Management Studio  
  
1.  Connectez-vous à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2.  Cliquez avec le bouton droit sur le dossier **Réplication** ou sur l'un de ses sous-dossiers, puis cliquez sur **Lancer le Moniteur de réplication**.  

### <a name="to-start-replication-monitor-from-the-command-prompt"></a>Pour démarrer le moniteur de réplication à partir de l'invite de commandes  
  
1.  À partir de l'invite de commandes, accédez au répertoire d'installation des outils. Le chemin d'accès par défaut est [!INCLUDE[ssInstallPath](../../../includes/ssinstallpath-md.md)]Tools\Binn\.  
  
2.  Exécutez sqlmonitor.exe.  
  
## <a name="see-also"></a>Voir aussi  
 [Surveillance de la réplication](../../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
