---
description: MSSQLSERVER_854
title: MSSQLSERVER_854
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 854 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 9c3f2071748702b7537d1bc05450f09bb437ab0b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194083"
---
# <a name="mssqlserver_854"></a>MSSQLSERVER_854
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|854|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|HARDWARE_MEMORY_SCRUBBER|
|Texte du message|L’ordinateur prend en charge la récupération des erreurs de mémoire. La protection de la mémoire SQL est activée pour la récupération suite à une altération de la mémoire|
||

## <a name="explanation"></a>Explication

Ce message indique que le matériel dans le système d’exploitation prend en charge la capacité à récupérer suite à des erreurs de mémoire. Sur les ordinateurs équipés de matériel plus récent et exécutant Windows Server 2012 ou ultérieur, le matériel peut informer le système d’exploitation et les applications que les pages mémoire (pages du système d’exploitation) sont marquées comme étant incorrectes ou endommagées. Les applications telles que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent inscrire ces notifications de page mémoire erronée à l’aide du jeu d’API suivant :

- `GetMemoryErrorHandlingCapabilities`
- `RegisterBadMemoryNotification`
- `BadMemoryCallbackRoutine`

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ajoute la prise en charge de ces notifications dans Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012 et versions ultérieures. Lors du démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vérifie si le matériel prend en charge cette nouvelle fonctionnalité. Vous verrez également le message suivant dans le journal des erreurs :

> \<Datetime> Server L’ordinateur prend en charge la récupération des erreurs de mémoire. La protection de la mémoire SQL est activée pour la récupération suite à une altération de la mémoire.

## <a name="user-action"></a>Action requise

Vérifiez si vous rencontrez d’autres erreurs, telles que 855 et 856, et prenez les mesures correctives appropriées.

## <a name="more-information"></a>Informations complémentaires

Vous pouvez utiliser l’indicateur de trace [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 849 afin d’empêcher [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de s’inscrire auprès du système d’exploitation pour recevoir les notifications d’erreur de mémoire. Toutefois, sachez que l’activation de l’indicateur de trace 849 empêchera [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de recevoir des notifications de mémoire incorrecte du système d’exploitation. Par conséquent, nous vous déconseillons d’utiliser cet indicateur de trace dans des circonstances classiques.
