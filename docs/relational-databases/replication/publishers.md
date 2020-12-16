---
description: Serveurs de publication
title: Serveurs de publication | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.configuredistributionwizard.enablepublishers.f1
ms.assetid: 116cd6a5-32ac-4273-81a2-d184408e0f07
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 6aa802cc14b494a3892fea71d2ddbc99f42c8749
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479830"
---
# <a name="publishers"></a>Serveurs de publication
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Vous pouvez autoriser les autres serveurs de publication à utiliser ce serveur de distribution. Notez que l'activation d'un serveur de publication pour utiliser ce serveur en tant que serveur de distribution distant ne fait pas de ce serveur un serveur de publication. Vous devez vous connecter au serveur de publication, le configurer pour la publication, et choisir ce serveur comme distributeur. Vous pouvez configurer le serveur de publication et choisir un serveur de distribution à l'aide de l'Assistant Nouvelle publication.  
  
 Les serveurs, que vous sélectionnez en tant que serveurs de publication, utilisent la base de données de distribution spécifiée dans la page **Base de données de distribution** de cet Assistant. Si vous souhaitez utiliser une autre base de données de distribution, n'activez pas le serveur de publication. Utilisez plutôt la boîte de dialogue **Propriétés du serveur de distribution** pour ajouter des serveurs de publication après avoir terminé l'exécution de l'Assistant Configuration de la distribution.  
  
## <a name="options"></a>Options  
 **Serveurs de publication**  
 Sélectionnez les serveurs autorisés à utiliser ce serveur de distribution. Cliquez sur le bouton des propriétés ( **...** ) en regard d'un serveur de publication pour afficher et définir des propriétés supplémentaires.  
  
 **Ajouter**  
 Si le serveur souhaité ne figure pas dans la liste, cliquez sur **Ajouter** afin d'ajouter un serveur de publication [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou un serveur de publication Oracle à la liste des serveurs de publication disponibles.  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer la distribution](../../relational-databases/replication/configure-distribution.md)   
 [Configurer la publication et la distribution](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [Afficher et modifier les propriétés d’un serveur de distribution et d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [Créer une publication](../../relational-databases/replication/publish/create-a-publication.md)  
  
  
