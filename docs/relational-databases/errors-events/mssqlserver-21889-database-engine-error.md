---
description: MSSQLSERVER_21889
title: MSSQLSERVER_21889 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 21889 (Database Engine error)
ms.assetid: ae199540-7986-4cc2-b782-cd22793236d3
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1e22046bafea480d385d2aac456ee8d748c61a53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196375"
---
# <a name="mssqlserver_21889"></a>MSSQLSERVER_21889
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|21889|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SQLErrorNum21889|  
|Texte du message|L’instance SQL Server '%s' n’est pas un serveur de publication de réplication. Exécutez **sp_adddistributor** sur l’instance SQL Server « %s » avec le serveur de distribution '%s' pour permettre à l’instance d’héberger la base de données de publication '%s'. Veillez à spécifier la même connexion et mot de passe que ceux utilisés pour le serveur de publication d'origine.|  
  
## <a name="explanation"></a>Explication  
Pour pouvoir héberger la base de données du serveur de publication, l’instance SQL Server doit être un serveur de publication de réplication. **sp_validate_redirected_publisher** appelle **sp_helpdistributor** sur le serveur distant pour déterminer si le serveur est un serveur de publication de réplication. Cette erreur indique que l’instance SQL Server cible n’est pas un serveur de publication de réplication.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Exécutez **sp_adddistributor** sur l’instance SQL Server qui héberge la base de données du serveur de publication. Lors de l’exécution de **sp_adddistributor**, spécifiez le serveur de distribution approprié. Utilisez la même valeur pour le paramètre *\@mot de passe* que celle utilisée quand **sp_adddistributor** a été initialement exécuté sur la base de données du serveur de distribution.  
  
