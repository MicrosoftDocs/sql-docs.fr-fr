---
description: MSSQLSERVER_5180
title: MSSQLSERVER_5180
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5180 (Database Engine error)
ms.assetid: ''
author: rgward
ms.author: ramakoni
ms.openlocfilehash: f80f4dafad6b0570837728e15a79eebdd7d6f41c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185529"
---
# <a name="mssqlserver_5180"></a>MSSQLSERVER_5180
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|5180|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|DSK_BAD_FCB_FILEID|
|Texte du message|Impossible d'ouvrir FCB (File Control Bank) pour l'ID de fichier non valide %d dans la base de données '%.*ls'. Vérifiez l'emplacement du fichier. Exécutez DBCC CHECKDB.|
||

## <a name="explanation"></a>Explication

Une requête ou une opération peut échouer avec une erreur 5180 lorsqu’un ID de fichier non valide est référencé par le [!INCLUDE[ssDENoVersion](../../includes/ssdenoversion_md.md)]. Voici un exemple :

> Message 5180, niveau 22, état 1, ligne 1  
Impossible d'ouvrir FCB (File Control Bank) pour l'ID de fichier non valide %d dans la base de données '%.*ls'. Vérifiez l'emplacement du fichier. Exécutez DBCC CHECKDB.

Étant donné que l’erreur est générée avec le niveau de gravité 22, la session de l’utilisateur est déconnectée. Ce message d’erreur est écrit dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et dans le journal des événements des applications Windows avec EventID = 5180.

## <a name="possible-causes"></a>Causes possibles

Le [!INCLUDE[ssDENoVersion](../../includes/ssdenoversion-md.md)] fait référence à un ID de fichier dans de nombreuses situations, principalement lors du référencement d’un ID de page (car l’ID de fichier est la première partie de l’ID de page). Si, pour une raison quelconque, l’ID de fichier référencé est < 0 ou n’est pas un ID de fichier valide dans une base de données (selon les ID de fichier valides figurant dans les vues du catalogue système comme sys.database_files), une erreur 5180 peut se produire.

L’une des causes possibles est qu’un ID de fichier stocké ne soit pas valide. Étant donné que l’enregistrement transmis dans une ligne fait référence à une autre page, une erreur 5180 peut se produire lors de l’accès à cette page si l’ID de fichier n’est pas valide. Cette condition constitue une erreur d’endommagement de base de données dans la page avec l’enregistrement transmis. (Dans cet exemple, `DBCC CHECKDB` signale le message 8993).

Un autre exemple est un ID de fichier non valide en tant que partie d’un ID de page, tel qu’il est stocké dans l’en-tête de page pour un ID de page suivante ou précédente. Ce champ est utilisé pour lier une série de pages, par exemple dans un index cluster. Si l’ID de fichier n’est pas valide pour la page précédente ou suivante, lorsque le moteur doit y faire référence pour accéder à la page suivante ou précédente, une erreur 5180 peut se produire. (Dans cet exemple, `DBCC CHECKDB` signale le message 8981).

Si le problème n’est pas un ID de fichier non valide en raison d’un ID de page stocké endommagé, le problème peut provenir du [!INCLUDE[ssDENoVersion](../../includes/ssdenoversion-md.md)].

## <a name="user-action"></a>Action requise

Si vous rencontrez cette erreur, vous devez exécuter `DBCC CHECKDB` sur la base de données, comme indiqué dans le message d’erreur. Si vous trouvez des erreurs, vous devez procéder à une restauration à partir d’une sauvegarde reconnue fiable. Si vous ne pouvez pas effectuer une restauration à partir d’une sauvegarde, votre autre option consiste à utiliser l’option de réparation de `DBCC CHECKDB` comme recommandé par sa sortie. Dans la plupart des cas, une réparation de ce type d’erreur entraîne une perte de données. Pour plus d’informations sur l’utilisation de CHECKDB et les causes des problèmes d’endommagement de base de données, consultez l’article : [Guide pratique pour résoudre les erreurs de cohérence de base de données signalées par DBCC CHECKDB](https://support.microsoft.com/kb/2015748).

Si `DBCC CHECKDB` ne signale aucune erreur et que le problème persiste, vous devez contacter le support technique Microsoft pour obtenir de l’aide. Préparez-vous à fournir la requête qui rencontre l’erreur 5180. Consultez la section [Informations complémentaires](#more-information) sur la façon d’identifier la requête qui a rencontré l’erreur.

## <a name="more-information"></a>Informations complémentaires

Le bloc de contrôle de fichier (FCB) est une structure de mémoire interne qui effectue le suivi des informations importantes sur le fichier associé à la base de données. Un ID de fichier est utilisé pour identifier de manière unique un bloc FCB pour une base de données. Si le moteur du serveur tente de faire référence à un ID de fichier non valide, la structure FCB ne peut pas être localisée, ce qui déclenche la condition d’erreur 5180.

Pour savoir quelle requête a rencontré cette erreur, vous pouvez utiliser les techniques suivantes :

- Pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008 et versions ultérieures, regardez si la session system_health contient un enregistrement de l’erreur, qui doit inclure le texte de la requête. Pour plus d’informations sur la session system_health, consultez la ressource : [Prise en charge de SQL Server 2008 : session system_health](https://techcommunity.microsoft.com/t5/sql-server-support/supporting-sql-server-2008-the-system-health-session/ba-p/315509).
- Utilisez le Générateur de profils [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et capturez les événements `SQL:BatchStarting`, `RPC:Starting` et d’exception. Recherchez la requête qui précède l’événement d’exception pour 5180 de la session associée à l’événement d’exception.
