---
description: MSSQLSERVER_21892
title: MSSQLSERVER_21892 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 21892 (Database Engine error)
ms.assetid: 199818e8-28f4-440e-ad85-7bea88f51153
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 56ba58d0166913aab8c789884eb98f1d6ce7fbb6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196368"
---
# <a name="mssqlserver_21892"></a>MSSQLSERVER_21892
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|21892|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SQLErrorNum21892|  
|Texte du message|Impossible d’interroger sys.availability_replicas sur le groupe de disponibilité principal associé au nom de réseau virtuel '%s' pour les noms de serveur des réplicas membres : erreur = %d, message d’erreur = %s.'|  
  
## <a name="explanation"></a>Explication  
**sp_validate_replica_hosts_as_publishers** interroge le principal actuel du groupe de disponibilité associé au serveur de publication redirigé pour déterminer les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui hébergent les réplicas membres.  Lorsque cette requête échoue, l'erreur 21892 est retournée.  
  
**sp_validate_replica_hosts_as_publishers** est généralement l’une des premières utilisations faite du serveur lié temporaire. De ce fait, s’il existe des problèmes de connectivité, ils sont susceptibles d’apparaître d’abord avec **sp_validate_replica_hosts_as_publishers**. Contrairement à **sp_validate_redirected_publisher**, le serveur lié utilisé par **sp_validate_replica_hosts_as_publishers** utilise toujours les informations d’identification de l’appelant lors de la connexion à n’importe lequel des hôtes de réplica de groupe de disponibilité.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Lorsque vous exécutez cette procédure stockée, assurez-vous que vous utilisez une connexion valide sur tous les réplicas. La connexion nécessite des autorisations suffisantes pour interroger les tables de métadonnées du groupe de disponibilité ainsi que les tables de métadonnées d'abonnement dans le réplica de base de données du serveur de publication.  
  
Examinez l'erreur référencée d'origine pour déterminer la cause de l'échec et l'action corrective appropriée.  
  
