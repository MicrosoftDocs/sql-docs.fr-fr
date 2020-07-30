---
title: MSSQL_ENG021331 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG021331 error
ms.assetid: 9acd75d9-fda1-44cd-ba17-20295ad53ea0
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 9e75adaa3ead3e081955d5797c4640a2a072d439
ms.sourcegitcommit: 99f61724de5edf6640efd99916d464172eb23f92
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87363211"
---
# <a name="mssql_eng021331"></a>MSSQL_ENG021331
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Détails du message  
  
|Attribut|Valeur|  
|-|-|  
|Nom du produit|SQL Server|  
|ID de l’événement|21331|  
|Source de l’événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|Impossible de copier le fichier de script utilisateur vers le distributeur.(%1!)|  
  
## <a name="explanation"></a>Explication  
 Cette erreur peut se produire lorsqu'un abonnement est initialisé manuellement et que les scripts générés par la réplication ou spécifiés dans une commande de réplication ne peuvent pas être enregistrés dans le répertoire spécifié. L'erreur peut être due à un problème d'autorisations : lorsqu'un abonnement est initialisé sans utiliser d'instantané, le compte sous lequel le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute sur le serveur de publication doit disposer d'autorisations en écriture sur le dossier d'instantané hébergé sur le serveur de distribution.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Vérifiez que le chemin d'accès correct a été spécifié pour le dossier d'instantané et que le compte sous lequel le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute sur le serveur de publication a des autorisations suffisantes.  
  
## <a name="see-also"></a>Voir aussi  
 [Spécifier les options des instantanés](../../relational-databases/replication/snapshot-options.md)   
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [Initialiser un abonnement transactionnel sans instantané](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)  
  
  
