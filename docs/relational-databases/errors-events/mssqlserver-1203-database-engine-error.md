---
description: MSSQLSERVER_1203
title: MSSQLSERVER_1203 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 1203 (Database Engine error)
ms.assetid: 33a35f00-98c8-46c6-b432-544b326b6117
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d430fa9aad8314e134a5b8de37d09a121b499dbe
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88336335"
---
# <a name="mssqlserver_1203"></a>MSSQLSERVER_1203
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|1203|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|LK_NOT|  
|Texte du message|L'ID de processus %d a essayé de déverrouiller une ressource qu'il ne possède pas :%.*ls. Réessayez la transaction, car cette erreur est peut-être provoquée par une condition basée sur le temps. Si le problème persiste, contactez l'administrateur de base de données.|  
  
## <a name="explanation"></a>Explication  
Cette erreur se produit lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est engagé dans une activité autre que le nettoyage de post-traitement standard et que la page qu'il est en train d'essayer de déverrouiller est déjà déverrouillée.  
  
### <a name="possible-causes"></a>Causes possibles  
La cause sous-jacente de cette erreur peut être liée à des problèmes structurels au sein de la base de données concernée. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gère l'acquisition et la libération de pages pour maintenir le contrôle de simultanéité dans l'environnement multi-utilisateur. Ce mécanisme est géré à l'aide de diverses structures de verrous internes qui identifient la page et le type de verrou présent. Les verrous sont acquis pour le traitement des pages concernées, puis libérés une fois le traitement terminé.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Exécutez DBCC CHECKDB sur la base de données à laquelle appartient l'objet. Si DBCC CHECKDB n'indique aucune erreur, essayez de rétablir la connexion et d'exécuter la commande.  
  
> [!IMPORTANT]  
> Si l'exécution de DBCC CHECKDB avec l'une des clauses REPAIR ne résout pas le problème d'index ou si vous ne savez pas quelles conséquences l'exécution de DBCC CHECKDB avec une clause REPAIR peut avoir sur vos données, contactez votre fournisseur d'assistance principal.  
  
