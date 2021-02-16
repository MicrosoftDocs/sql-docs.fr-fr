---
title: " Option de configuration Délai d’attente de nouvelle tentative de nettoyage d’ADR (min) | Microsoft Docs"
description: Décrit le paramètre de configuration de l’instance SQL Server pour le délai d’attente de nouvelle tentative de nettoyage d’ADR.
ms.custom: ''
ms.date: 06/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- ADR cleaner retry timeout (min)
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 05502bbf760b0f8aa18d81739ce89e19e92397cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347472"
---
# <a name="adr-cleaner-retry-timeout-min-configuration-option"></a>Option de configuration Délai d’attente de nouvelle tentative de nettoyage d’ADR (min)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Introduite dans SQL Server 2019.

Ce paramètre de configuration est requis pour la [récupération de base de données accélérée](../../relational-databases/accelerated-database-recovery-concepts.md). Le nettoyeur est le processus asynchrone qui sort de veille régulièrement et nettoie les versions des pages qui ne sont pas nécessaires.

Parfois, le nettoyeur rencontre des problèmes lors de l’acquisition de verrous au niveau de l’objet en raison de conflits avec la charge de travail de l’utilisateur pendant son balayage. Il effectue le suivi de ces pages dans une liste distincte. Le délai d’attente de nouvelle tentative de nettoyage d’ADR (valeur par défaut de 15) contrôle la durée pendant laquelle le nettoyeur retente en exclusivité l’acquisition du verrou de l’objet et le nettoyage de la page avant d’abandonner le balayage. L’achèvement d’un balayage avec une réussite de 100 % est essentiel pour conserver l’augmentation des transactions abandonnées dans la carte des transactions abandonnées. Si la liste distincte ne peut pas être nettoyée dans le délai imparti, le balayage actuel est abandonné et le balayage suivant démarre.

## <a name="remarks"></a>Notes  

Le nettoyeur est un thread unique dans SQL Server 2019 et par conséquent, une instance SQL Server peut fonctionner sur une base de données à la fois. Si l’instance a plusieurs bases de données utilisateur avec la règle ADR activée, n’augmentez pas le délai d’attente à une valeur élevée car cela pourrait retarder le nettoyage sur une base de données pendant la nouvelle tentative sur une autre base de données.

## <a name="examples"></a>Exemples

Les exemples suivants définissent le délai de nouvelle tentative de nettoyage.

```tsql
sp_configure 'show advanced options', 1;  
RECONFIGURE;
GO 
sp_configure 'ADR cleaner retry timeout', 15;  
RECONFIGURE;  
GO  
```  

## <a name="see-also"></a>Voir aussi  

- [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
- [Récupération de base de données accélérée](../../relational-databases/accelerated-database-recovery-concepts.md)
- [Gérer la récupération de base de données accélérée](../../relational-databases/accelerated-database-recovery-management.md)