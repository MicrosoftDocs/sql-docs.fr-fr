---
description: MSSQLSERVER_26014
title: MSSQLSERVER_26014 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 26014 (Database Engine error)
ms.assetid: e2b0dfc7-0681-4e5d-8875-1d5f63534086
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f179b8ecef4e2c80a045b858ca60539374260cbd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198205"
---
# <a name="mssqlserver_26014"></a>MSSQLSERVER_26014
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|26014|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SNI_SSL_USER_CERT_FAILURE|  
|Texte du message|Impossible de charger un certificat spécifié par l'utilisateur [Cert Hash(sha1) "%hs"]. Le serveur n'acceptera pas de connexion. Vérifiez que ce certificat est installé correctement. Consultez "Configuration du certificat en vue de son utilisation par SSL" dans la documentation en ligne.|  
  
## <a name="explanation"></a>Explication  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a essayé de charger le certificat nommé dans le message, mais l’opération a échoué. Ce problème doit être résolu pour que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puisse utiliser ce certificat.  
  
Cette erreur peut avoir plusieurs causes :  
  
-   Le certificat peut avoir été déplacé ou supprimé.  
  
-   Si la connexion utilisée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a changé, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut ne pas avoir l'autorisation d'accéder au certificat.  
  
-   Le certificat a peut-être expiré.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Assurez-vous que le certificat nommé dans le message existe sur le système, qu'il est accessible et qu'il est toujours valide.  
  
