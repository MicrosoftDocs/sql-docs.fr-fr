---
description: MSSQLSERVER_
title: MSSQLSERVER_4214
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 4214 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 1ccce1ca406527cfca2485bec41f2fb9d287862c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348773"
---
# <a name="mssqlserver_4214"></a>MSSQLSERVER_4214
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|4214|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|DMPX_NODBBACKUP|
|Texte du message|BACKUP LOG ne peut pas être effectué, car aucune sauvegarde de base de données n’est en cours.|
||

## <a name="explanation"></a>Explication

Cette erreur se produit lorsque vous tentez une sauvegarde du journal des transactions avant d’effectuer une sauvegarde complète d’une base de données. Avant de sauvegarder le journal des transactions d’une base de données, vous devez effectuer une sauvegarde complète de celle-ci. En outre, vous verrez le message suivant dans le journal des erreurs :

> \<Datetime>Erreur de sauvegarde : 3041, Gravité : 16, état : 1.  
\<Datetime> Sauvegarde     BACKUP n’a pas réussi à terminer la commande BACKUP LOG \<db_name>. Consultez le journal de sauvegarde des applications pour des messages détaillés.

## <a name="user-action"></a>Action requise

Avant de sauvegarder le journal des transactions d’une base de données, vous devez effectuer une sauvegarde complète de celle-ci.

## <a name="more-information"></a>Informations complémentaires

Pour plus d’informations, consultez : [Sauvegarde et restauration des bases de données SQL Server](../backup-restore/back-up-and-restore-of-sql-server-databases.md).