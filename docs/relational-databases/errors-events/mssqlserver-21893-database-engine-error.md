---
description: MSSQLSERVER_21893
title: MSSQLSERVER_21893 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 21893 (Database Engine error)
ms.assetid: 1ab1195a-fe2a-4e06-b871-b177b6bea1fe
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d6c77bc40541245f5a713a4548464c377e6d1884
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196355"
---
# <a name="mssqlserver_21893"></a>MSSQLSERVER_21893
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|21893|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SQLErrorNum21893|  
|Texte du message|Les abonnés (%s) du serveur de publication d'origine '%s' n'apparaissent pas en tant que serveurs distants sur le serveur de publication redirigé '%s'. Exécutez **sp_addlinkedserver** sur le serveur de publication redirigé pour ajouter ces abonnés comme serveurs distants.|  
  
## <a name="explanation"></a>Explication  
**sp_validate_redirected_publisher** utilise les tables de métadonnées d’abonnement de la base de données du serveur de publication sur le serveur distant pour identifier ses abonnés associés et vérifie qu’il existe des entrées associées dans master.dbo.sysservers pour les abonnés. Cette erreur est retournée si des abonnés identifiés ne sont pas présents.  
  
Elle n'est pas considérée comme une erreur fatale. Les agents qui reçoivent cette erreur vont la consigner dans le journal à titre d'information mais ne vont pas s'arrêter, car l'absence d'entrées d'abonnés appropriées sur le nouveau serveur de publication a un impact limité sur la réplication. Sans une entrée appropriée pour un abonné dans sysservers, certaines activités de gestion d’abonnement peuvent échouer quand elles sont exécutées par [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Toutefois, ces mêmes opérations peuvent être effectuées avec succès en exécutant les procédures stockées de gestion explicitement.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Exécutez **sp_addlinkedserver** sur le serveur de publication redirigé pour chacun des abonnés identifiés afin de les ajouter comme serveurs distants. Exécutez ensuite **sp_serveroption** pour définir le bit d’abonné pour le serveur.  
  
