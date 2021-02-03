---
title: MSSQLSERVER_233 | Microsoft Docs
description: Le client SQL Server ne peut pas se connecter au serveur. Consultez l’explication de l’erreur 233 et les résolutions possibles.
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
f1_keywords:
- "233"
helpviewer_keywords:
- 233 (Database Engine error)
ms.assetid: 201665dc-7ac8-4c19-90d3-33354c5caa72
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7f029adf0a822d7ebfd8ce22708ecb83ab7f6248
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208484"
---
# <a name="mssqlserver_233"></a>MSSQLSERVER_233
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|233|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique||  
|Texte du message|Une connexion a été établie avec le serveur, mais une erreur s’est ensuite produite pendant le processus de connexion. (fournisseur : Fournisseur de mémoire partagée, erreur : 0 - Il n’y a pas de processus à l’autre extrémité du canal.) (Microsoft SQL Server, erreur : 233)|  
  
## <a name="explanation"></a>Explication  
Le client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas se connecter au serveur. Cette erreur s'est peut-être produite parce que le serveur n'est pas configuré pour accepter des connexions distantes.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Utilisez l'outil Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour permettre à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'accepter des connexions distantes.  
  
## <a name="see-also"></a>Voir aussi  
[Protocoles réseau et bibliothèques réseau](~/sql-server/install/network-protocols-and-network-libraries.md)  
[Configuration du réseau client](~/database-engine/configure-windows/client-network-configuration.md)  
[Configurer des protocoles clients](~/database-engine/configure-windows/configure-client-protocols.md)  
[Activer ou désactiver un protocole réseau de serveur](~/database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)  
  
