---
description: MSSQLSERVER_1205
title: MSSQLSERVER_1205 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1205 (Database Engine error)
ms.assetid: 9fe3f67c-df3c-4642-a3a4-ccc0e138b632
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8ce9d2d99d3db0fc18586b9292876d2e4e6c985b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197170"
---
# <a name="mssqlserver_1205"></a>MSSQLSERVER_1205
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|1205|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|LK_VICTIM|  
|Texte du message|La transaction (ID de processus %d) a été bloquée sur les ressources %.*ls par un autre processus et a été choisie comme victime. Relancez la transaction.|  
  
## <a name="explanation"></a>Explication

Des ressources font l’objet d’accès dans un ordre conflictuel sur des transactions distinctes, ce qui provoque un [interblocage](../sql-server-transaction-locking-and-row-versioning-guide.md?#deadlocks). Par exemple :  
  
- Transaction1 met à jour **Table1.Ligne1**, tandis que Transaction2 met à jour **Table2.Ligne2**.
- Transaction1 essaie de mettre à jour **Table2.Ligne2** mais elle est bloquée, car Transaction2 n’a pas encore été validée.  
- Transaction2 essaie alors de mettre à jour **Table1.Ligne1**, mais elle est bloquée, car Transaction1 n’a pas encore été validée.
- Un blocage survient car Transaction1 attend que Transaction2 se termine, alors que Transaction2 attend que Transaction1 se termine.  
  
Le système va détecter cet interblocage et choisir une des transactions impliquées comme « victime ». Il va ensuite émettre ce message d’erreur, en annulant la transaction de la victime.  Pour plus d’informations, consultez [Interblocages](../sql-server-transaction-locking-and-row-versioning-guide.md?#deadlocks).

## <a name="user-action"></a>Action de l'utilisateur  

Exécutez de nouveau la transaction. Vous pouvez également réviser l'application pour éviter les blocages. La transaction qui a été choisie comme victime peut être retentée et réussira probablement, en fonction des opérations qui sont exécutées simultanément.  
  
Pour empêcher ou éviter l’apparition de blocages, faites en sorte que toutes les transactions accèdent aux lignes dans le même ordre (**Table1**, puis **Table2**). De cette façon, bien qu’un blocage puisse survenir, un interblocage sera évité.  
  
Pour plus d’informations, consultez [Gestion des interblocages](../sql-server-transaction-locking-and-row-versioning-guide.md?#handling-deadlocks) et [Réduction des interblocages](../sql-server-transaction-locking-and-row-versioning-guide.md#deadlock_minimizing).
