---
description: MSSQL_ENG018456
title: MSSQL_ENG018456 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG018456 error
ms.assetid: 3daa8144-d81f-445a-b6c3-4bb3e9fd1526
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 0afdbb3340cb0162da95c654d3d72026256c3a60
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205623"
---
# <a name="mssql_eng018456"></a>MSSQL_ENG018456
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Détails du message  
  
|Attribut|Valeur|  
|-|-|  
|Nom du produit|SQL Server|  
|ID de l’événement|18456|  
|Source de l’événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|Échec de la connexion pour l’utilisateur '%.*ls'.%.\*ls|  
  
## <a name="explanation"></a>Explication  
 L'erreur MSSQL_ENG018456 se produit chaque fois qu'une tentative de connexion échoue. Si le message d'erreur inclut le compte **distributor_admin** (Échec de la connexion pour l'utilisateur 'distributor_admin'.), le problème vient d'un compte utilisé par la réplication. La réplication crée un serveur distant, **repl_distributor**, qui permet la communication entre le serveur de distribution et le serveur de publication. Le nom de connexion **distributor_admin** est associé à ce serveur distant et doit avoir un mot de passe valide.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Assurez-vous que vous avez spécifié un mot de passe pour ce compte. Pour plus d’informations, consultez [Protéger le serveur de distribution](../../relational-databases/replication/security/secure-the-distributor.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
