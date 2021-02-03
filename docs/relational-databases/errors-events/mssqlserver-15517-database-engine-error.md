---
description: MSSQLSERVER_15517
title: MSSQLSERVER_15517 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 15517 (Database Engine error)
ms.assetid: f94287f5-129f-4c52-9d34-62b996088001
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 592439f0e90f1b8ba607f518ad5125510b8e8a3e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196898"
---
# <a name="mssqlserver_15517"></a>MSSQLSERVER_15517
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|ID de l’événement|15517|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SEC_CANNOTEXECUTEASUSER|  
|Texte du message|Exécution impossible en tant que principal de la base de données car le principal «*principal*» n’existe pas, ce type de principal n’autorise pas l’emprunt d’identité ou vous n’avez pas l’autorisation requise.|  
  
## <a name="user-action"></a>Action de l'utilisateur  
Utilisez le nom d'un principal existant ou obtenez l'autorisation IMPERSONATE sur ce principal.  
  
15517 peut également se produire après un attachement et une restauration de base de données par quelqu'un d'autre que le propriétaire d'origine de la base de données. Pour résoudre cette erreur, remplacez le db_owner par une connexion sur votre serveur, en exécutant la commande suivante :  
  
```  
ALTER AUTHORIZATION ON DATABASE:: DBName TO [NewLogin]  
```  
  
## <a name="see-also"></a>Voir aussi  
[OLTP en mémoire &#40;Optimisation en mémoire&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
