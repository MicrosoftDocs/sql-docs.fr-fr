---
description: MSSQLSERVER_18482
title: MSSQLSERVER_18482
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18482 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 07da09291b3bb3bf75593ed74ded516ed2638b46
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196499"
---
# <a name="mssqlserver_18482"></a>MSSQLSERVER_18482
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|18482|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|REMLOGIN_INVALID_SITE|
|Texte du message|Impossible de se connecter au serveur '%.ls', car '%. ls' n'est pas défini en tant que serveur distant. Vérifiez que vous avez spécifié le bon nom de serveur. %.*ls|
||

## <a name="explanation"></a>Explication

Cette erreur se produit lorsque vous tentez d’exécuter un appel de procédure distante (RPC) d’un serveur à un autre (par exemple, en exécutant une procédure stockée sur un ordinateur distant avec une instruction telle que `EXEC SERV_REMOTE.pubs..byroyalty`). Un message d’erreur semblable au suivant est signalé à l’utilisateur

> Erreur 18482 : Impossible de se connecter au serveur \<SERV_REMOTE>, car \<SERV_REMOTE> n’est pas défini en tant que serveur distant. Vérifiez que vous avez spécifié le bon nom de serveur.

## <a name="cause"></a>Cause

Cette erreur se produit quand SQL Server ne peut pas exécuter un appel de procédure distante. Cela peut être dû à un serveur local configuré de façon incorrecte. Pour effectuer un appel de procédure distante, SQL Server détermine d’abord qui est le serveur local en recherchant le nom du serveur avec srvid = 0 dans sysservers. Si une entrée avec srvid = 0 est introuvable dans sysservers ou si le nom du serveur avec srvid = 0 appartient à un nom de serveur différent du nom de l’ordinateur Windows local, vous recevez cette erreur.

## <a name="user-action"></a>Action requise

Pour déterminer si le serveur local est configuré correctement, examinez la colonne `srvstatus` dans master..sysservers. Cette valeur doit être égale à 0 pour le serveur local.

Par exemple, supposons que votre serveur local était nommé SERV_LOCAL, que le serveur distant était nommé *SERV_REMOTE* et que sysservers contenait les informations suivantes :

|srvid|srvstatus|srvname|srvname|
|---|---|---|---|
|1|2|SERV_LOCAL|SERV_LOCAL|
|2|1|SERV_REMOTE|SERV_REMOTE|
||||

Dans la sortie précédente, SERV_LOCAL est le serveur local, mais il a un srvid de 1 ; il doit être égal à 0. Pour corriger cela, procédez comme suit :

1. Exécutez `sp_dropserver` nom_serveur_local, droplogins (dans cet exemple, vous devez exécuter `sp_dropserver SERV_LOCAL, droplogins`).
1. Exécutez `sp_addserver` nom_serveur_local, LOCAL (dans cet exemple, vous devez exécuter `sp_addserver SERV_LOCAL, LOCAL`).
1. Arrêtez et redémarrez SQL Server.

Après avoir exécuté ces étapes, la table sysservers doit ressembler à ce qui suit :

|srvid|srvstatus|srvname|srvname|
|---|---|---|---|
|0|0|SERV_LOCAL|SERV_LOCAL|
|2|1|SERV_REMOTE|SERV_REMOTE|
||||

> [!NOTE]
> L’ID de serveur (srvid) doit être 0 pour le serveur local.

Dans certains cas, les entrées de la table sysservers sont correctes, mais lorsque vous exécutez `select @@servername`, la valeur NULL est retournée. Dans ces scénarios, vous devez toujours exécuter les étapes 1 à 3 indiquées ci-dessus pour résoudre le problème.

## <a name="more-information"></a>Informations complémentaires

Vous pouvez recevoir ce message d’erreur lors de l’installation de la réplication, car le processus d’installation effectue des appels de procédure distante entre les serveurs impliqués dans la réplication.
