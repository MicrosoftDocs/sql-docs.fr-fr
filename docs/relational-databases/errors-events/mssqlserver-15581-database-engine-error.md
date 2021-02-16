---
description: MSSQLSERVER_15581
title: MSSQLSERVER_15581
ms.custom: ''
ms.date: 09/03/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha, VenCher
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 15581 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: fd227045a859f81fc6f96ba99cc402032ee00e99
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343154"
---
# <a name="mssqlserver_15581"></a>MSSQLSERVER_15581
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|15581|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|SEC_NODBMASTERKEYERR|
|Texte du message|Créez une clé principale dans la base de données ou ouvrez la clé principale dans la session avant d'effectuer cette opération.|
||

## <a name="explanation"></a>Explication

L’erreur 15581 est générée quand [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’est pas en mesure de récupérer une base de données qui est activée pour le chiffrement transparent des données (TDE). Un message d’erreur semblable au suivant s’est journalisé dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

> 2020-01-14 22:16:26.47 spid20s Erreur : 15581, Gravité : 16, état : 3.  
2020-01-14 22:16:26.47 spid20s Créez une clé principale dans la base de données ou ouvrez la clé principale dans la session avant d’effectuer cette opération.

## <a name="possible-cause"></a>Cause possible

Ce problème se produit quand la clé principale du service pour la clé principale de la base de données Master est supprimée lors de l’exécution de la commande suivante :

```sql
Use master
go
alter master key drop encryption by service master key
```

La clé principale du service est utilisée pour chiffrer le certificat utilisé par la clé principale de base de données. Toute tentative d’utilisation de la base de données pour laquelle le chiffrement transparent des données (TDE) est activé nécessite l’accès à la clé principale de base de données dans la base de données Master. Une clé principale qui n’est pas chiffrée par la clé principale du service doit être ouverte à l’aide de l’instruction [OPEN MASTER KEY (Transact-SQL)](../../t-sql/statements/open-master-key-transact-sql.md) avec un mot de passe pour chaque session nécessitant l’accès à la clé principale. Comme cette commande ne peut pas être exécutée sur des sessions système, la récupération ne peut pas être effectuée sur des bases de données sur lesquelles TDE est activé.

## <a name="user-action"></a>Action requise

Pour résoudre le problème, activez le déchiffrement automatique de la clé principale. Pour cela, exécutez les commandes suivantes :

```sql
Use master
go
open master key DECRYPTION BY PASSWORD = 'password'
alter master key add encryption by service master key
```

Utilisez la requête suivante pour déterminer si le déchiffrement automatique de la clé principale par la clé principale du service a été désactivé pour la base de données Master :

```sql
select is_master_key_encrypted_by_server from sys.databases where name = 'master'
```

Si cette requête retourne la valeur 0, le déchiffrement automatique de la clé principale par la clé principale du service a été désactivé.

## <a name="more-information"></a>Informations complémentaires

Dans certains cas, l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut sembler ne pas répondre. Si vous interrogez la vue de gestion dynamique `sys.dm_exec_requests`, vous remarquez que le thread `LogWriter` et les autres threads qui effectuent des opérations DML attendent indéfiniment avec le type wait_type WRITELOG. D’autres sessions peuvent également être en attente pendant qu’elles essaient d’obtenir des verrous.