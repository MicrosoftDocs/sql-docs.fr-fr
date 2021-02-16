---
title: Option de configuration Facteur de préallocation de l’ADR | Microsoft Docs
description: Décrit le paramètre de configuration d’une instance SQL Server pour le facteur de préallocation de l’ADR.
ms.custom: ''
ms.date: 06/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- ADR Preallocation Factor
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 9469b5de278c01b7b256ee348380ebd1ae19cde4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347465"
---
# <a name="adr-preallocation-factor-configuration-option"></a>Option de configuration Facteur de préallocation de l’ADR

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Introduite dans SQL Server 2019.

Ce paramètre de configuration est requis pour la [récupération de base de données accélérée](../../relational-databases/accelerated-database-recovery-concepts.md).

La récupération de base de données accélérée (ADR, Accelerated Database Recovery) gère les versions de données à des fins de récupération. Ces versions sont générées dans le cadre de diverses opérations DML (Data Manipulation Language). Les versions sont stockées dans une table interne appelée banque des versions persistante (PVS, Persistent Version Store). 

## <a name="remarks"></a>Notes  

Les performances peuvent se dégrader si les pages sont allouées pour la table PVS dans le cadre des opérations DML d’utilisateur de premier plan. Pour résoudre ce problème, un thread d’arrière-plan préalloue les pages et les maintient à disposition immédiate pour les transactions DML. Les performances sont optimales lorsque le thread d’arrière-plan préalloue suffisamment de pages et que le pourcentage d’allocations de la PVS de premier plan est proche de 0. Le journal des erreurs contient des entiers associés à la balise `PreallocatePVS` si le pourcentage devient élevé et affecte les performances.

Le nombre de pages que le thread d’arrière-plan préalloue est basé sur différentes heuristiques de charge de travail. Celui-ci alloue le plus souvent les pages par segment de 512 pages. Le facteur de préallocation de l’ADR est un multiple du segment. Par défaut, il est égal à 4. Cela signifie qu’il préalloue 2 048 pages à la fois au besoin. 

Si le thread d’arrière-plan tient compte des modèles de charge de travail, ce facteur peut être augmenté si nécessaire pour améliorer les performances.

> [!CAUTION]
> Si la préallocation de la PVS est trop élevée, elle se retrouvera en compétition avec d’autres allocations dans le système et pourrait en réalité réduire les performances globales.
>
> Avant de modifier ce paramètre, testez les performances globales du système.

## <a name="examples"></a>Exemples  

L’exemple suivant définit le facteur de préallocation sur 4.

```tsql
sp_configure 'show advanced options', 1;
RECONFIGURE;
GO 
sp_configure 'ADR Preallocation Factor', 4;
RECONFIGURE;
GO
```

## <a name="see-also"></a>Voir aussi  

- [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
- [Récupération de base de données accélérée](../../relational-databases/accelerated-database-recovery-concepts.md)
- [Gérer la récupération de base de données accélérée](../../relational-databases/accelerated-database-recovery-management.md)