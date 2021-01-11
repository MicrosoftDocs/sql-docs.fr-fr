---
description: MSSQLSERVER_3988
title: MSSQLSERVER_3988
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 3988 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 46070023b397f949971cf35a7c6ced936fc91e6d
ms.sourcegitcommit: d819173fb91af6f20ca6ee59686c35c71b060fbc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797785"
---
# <a name="mssqlserver_3988"></a>MSSQLSERVER_3988
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|3988|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|XACT_UNSUPPORT_PARALLEL_TRAN2|
|Texte du message|Une nouvelle transaction n'est pas autorisée parce que d'autres threads sont en cours d'exécution dans la session.|
||

## <a name="explanation"></a>Explication

Cette erreur se produit lorsque vous exécutez une requête distribuée qui joint plusieurs tables hébergées par des instances distantes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], alors que le paramètre de session `XACT_ABORT` est **activé**. Un message d’erreur semblable au suivant est signalé à l’utilisateur :

> Message 3988, niveau 16, état 1, ligne #  
Une nouvelle transaction n'est pas autorisée parce que d'autres threads sont en cours d'exécution dans la session.

## <a name="cause"></a>Cause

Il existe certaines limitations de conception dans la façon dont [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gère les requêtes distribuées (DQ) lorsque les conditions suivantes sont remplies :

- SQL Server joint plusieurs tables d’une source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] distante.
- La session qui émet la requête n’est pas inscrite dans une transaction distribuée.

Dans ce cas, une tentative d’exécution de la requête peut déclencher l’une ou l’autre des deux erreurs mentionnées dans la section **Explication**.

## <a name="user-action"></a>Action requise

Pour contourner ce problème, mettez la requête distribuée dans une instruction « begin distributed transaction » :

```sql
BEGIN DISTRIBUTED TRANSACTION <Distributed Query> COMMIT TRANSACTION
```
