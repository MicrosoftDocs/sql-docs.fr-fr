---
description: MSSQLSERVER_855
title: MSSQLSERVER_855
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 855 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: f4679f3018ef6821ba9b4894c5ef1a7787ca6d81
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348664"
---
# <a name="mssqlserver_855"></a>MSSQLSERVER_855
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|855|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|BAD_MEMORY_OUTSIDE_BPOOL|
|Texte du message|Altération de la mémoire matérielle non corrigeable détectée. Votre système peut devenir instable. Pour plus d’informations, consultez le journal des événements Windows|
||

## <a name="explanation"></a>Explication

Ce message indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a détecté une page mémoire erronée dans un objet mis en cache en dehors du pool de mémoires tampons. Ce message est généré sur les systèmes qui prennent en charge la capacité à récupérer suite à des erreurs de mémoire. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas effectuer de récupération à partir de ces scénarios et consigne ce message.

## <a name="user-action"></a>Action requise

Vous devez exécuter des vérifications matérielles ou système pour déterminer s’il existe des problèmes de mémoire ou de processeur. Vérifiez que tous les pilotes système, les mises à jour du système d’exploitation et les mises à jour matérielles ont été appliqués à votre système. Envisagez d’exécuter des diagnostics de fabrication de matériel, y compris des tests liés à la mémoire. Chaque fois que vous voyez cette erreur, envisagez d’exécuter `DBCC CHECKDB` sur toutes les bases de données de cette instance.

## <a name="more-information"></a>Informations complémentaires

Sur les ordinateurs équipés de matériel plus récent et exécutant Windows Server 2012 ou ultérieur, le matériel peut informer le système d’exploitation et les applications que les pages mémoire (pages du système d’exploitation) sont marquées comme étant incorrectes ou endommagées. Les applications telles que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent inscrire ces notifications de page mémoire erronée à l’aide du jeu d’API suivant :

- `GetMemoryErrorHandlingCapabilities`
- `RegisterBadMemoryNotification`
- `BadMemoryCallbackRoutine`

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ajoute la prise en charge de ces notifications dans Microsoft [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures. Lors du démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vérifie si le matériel prend en charge cette nouvelle fonctionnalité. Vous verrez également le message suivant dans le journal des erreurs :

> \<Datetime> Server L’ordinateur prend en charge la récupération des erreurs de mémoire. La protection de la mémoire SQL est activée pour la récupération suite à une altération de la mémoire.

Actuellement, seul le pool de mémoires tampons entreprend une action quand [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reçoit ces notifications. Lorsqu’il reçoit une notification, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit parcourir l’ensemble du pool de mémoires tampons et découvrir l’adresse de chaque mémoire tampon allouée. Ensuite, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise l’API `QueryWorkingSetEX` pour vérifier si l’une des pages mémoire associées à la page de données est marquée comme étant incorrecte. La structure de sortie `PSAPI_WORKING_SET_EX_BLOCK` qui correspond à cette page mémoire verra son membre incorrect avec la valeur 1 si un dommage quelconque est signalé.

Si ce pool de mémoires tampons ou cette page de données n’est pas modifié ni en train de traiter des E/S, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut ignorer la page de données et en annuler la validation. Ensuite, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consigne le message suivant :

> SQL Server a détecté une altération de la mémoire matérielle dans la base de données '%ls', ID de fichier : %u, ID de page : %u, adresse mémoire : 0x%I64X et a correctement récupéré la page.

Lorsque les requêtes nécessitent une nouvelle fois cette page de données, le pool de mémoires tampons peut lire la page de données à partir du disque et remettre le contenu dans le pool de mémoires tampons. Il est également possible que la version sur disque de la page se trouve dans un état endommagé. Dans ce cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut consigner des erreurs supplémentaires telles que l’erreur 824.

Si la page erronée est utilisée non pas par le pool de mémoires tampons mais par une autre structure ou un autre objet mis en cache, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consigne le message suivant :

> Altération de la mémoire matérielle non corrigeable détectée. Votre système peut devenir instable. Pour plus d’informations, consultez le journal des événements Windows.

Si le serveur signale des erreurs de mémoire, contactez le fournisseur du matériel informatique et effectuez les actions appropriées, telles que l’exécution de diagnostics de la mémoire, la mise à jour du BIOS et du microprogramme ainsi que le remplacement des modules de mémoire défectueux.

Vous pouvez utiliser l’indicateur de trace [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 849 afin d’empêcher [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de s’inscrire auprès du système d’exploitation pour recevoir les notifications d’erreur de mémoire. Toutefois, sachez que l’activation de l’indicateur de trace 849 empêchera [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de recevoir des notifications de mémoire incorrecte du système d’exploitation. Par conséquent, nous vous déconseillons d’utiliser cet indicateur de trace dans des circonstances classiques.

Sachez également que, par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recevra ces notifications sur le matériel pris en charge.

Vous devez également garder à l’esprit que, quand [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’inscrit pour recevoir ces notifications d’erreur de mémoire, le processus système d’écriture différée n’effectue pas de vérifications de pages constantes. Pour plus d’informations sur les vérifications de pages constantes, consultez [Guide pratique pour résoudre les problèmes liés au message 832 (changement d’une page constante) dans SQL Server](https://support.microsoft.com/help/2015759).
