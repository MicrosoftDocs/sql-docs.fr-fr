---
description: MSSQLSERVER_5515
title: MSSQLSERVER_5515 | Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5515 (Database Engine error)
ms.assetid: ccd793bc-ba5d-4782-8d72-731fd01fc177
author: MashaMSFT
ms.author: mathoma
robots: noindex,nofollow
ms.openlocfilehash: 59a09028b6710b84fd1b289ebb382461ac866fdf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198050"
---
# <a name="mssqlserver_5515"></a>MSSQLSERVER_5515
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|MSSQLSERVER|  
|ID de l’événement|5515|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|FS_OPEN_CONTAINER_FAILED|  
|Texte du message|Impossible d'ouvrir le répertoire conteneur « %.*ls » du fichier FILESTREAM. Le système d'exploitation a retourné le code d'état Windows 0x%x.|  
  
## <a name="explanation"></a>Explication  
Le répertoire conteneur spécifié pour le fichier FILESTREAM ne peut pas être ouvert.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Pour la cause de l'erreur, consultez le code d'état Windows spécifique.  
  
