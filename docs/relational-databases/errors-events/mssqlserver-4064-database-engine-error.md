---
description: MSSQLSERVER_4064
title: MSSQLSERVER_4064 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 4064 (Database Engine error)
ms.assetid: 32112b90-0a2f-4834-a027-756811732be7
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: cdf7443042df535359320ccee9631a7612ecbdf1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185147"
---
# <a name="mssqlserver_4064"></a>MSSQLSERVER_4064
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|4064|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DB_UFAIL_FATAL|  
|Texte du message|La base de données utilisateur par défaut ne peut pas être ouverte. Échec de la connexion.|  
  
## <a name="explanation"></a>Explication  
La connexion SQL Server n'a pas pu se connecter en raison d'un problème avec sa base de données par défaut. Soit la base de données n'est pas valide, soit la connexion ne dispose pas de l'autorisation CONNECT sur la base de données.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Utilisez ALTER LOGIN pour modifier la base de données par défaut de la connexion. Accordez l'autorisation CONNECT à la connexion.  
  
