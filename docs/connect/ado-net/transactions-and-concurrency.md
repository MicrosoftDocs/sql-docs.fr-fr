---
title: Transactions et accès simultané
description: Explique comment utiliser le fournisseur de données Microsoft SqlClient pour SQL Server avec les transactions et l’accès concurrentiel.
ms.date: 11/24/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 2a00ef1ec1f2f5d8ee892289021f42cb139b5a12
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771316"
---
# <a name="transactions-and-concurrency"></a>Transactions et accès simultané

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Une transaction est composée d’une seule commande ou d’un groupe de commandes qui s’exécutent sous forme de package. Les transactions vous permettent de combiner plusieurs opérations en une seule unité de travail. Si une panne se produit pendant la transaction, toutes les mises à jour sont ramenées dans l’état qui était le leur avant le début de la transaction.

Une transaction doit être conforme aux propriétés ACID (atomicité, cohérence, isolation et durabilité) afin de garantir la cohérence des données. La plupart des systèmes de bases de données relationnelles, tels que Microsoft SQL Server, prennent en charge des transactions en offrant des fonctionnalités de verrouillage, de journalisation et de gestion des transactions lorsqu’une application cliente exécute une opération de mise à jour, d’insertion ou de suppression.

> [!NOTE]
> Les transactions impliquant plusieurs ressources peuvent réduire l'accès concurrentiel si des verrous sont conservés trop longtemps. Par conséquent, réduisez autant que possible les transactions.  

Si une transaction implique plusieurs tables dans la même base de données ou le même serveur, des transactions explicites dans des procédures stockées fonctionnent souvent mieux. Vous pouvez créer des transactions dans des procédures stockées SQL Server à l'aide des instructions Transact-SQL `BEGIN TRANSACTION`, `COMMIT TRANSACTION` et `ROLLBACK TRANSACTION`. Pour plus d'informations, consultez la documentation en ligne de SQL Server.

Les transactions impliquant différents gestionnaires de ressources, comme une transaction entre SQL Server et Oracle, nécessitent une transaction distribuée.

## <a name="in-this-section"></a>Dans cette section

[Transactions locales](local-transactions.md)  
Montre comment effectuer des transactions sur une base de données.  
  
[Transactions distribuées](distributed-transactions.md)  
Décrit comment effectuer des transactions distribuées dans ADO.NET.  
  
[Intégration de System.Transactions à SQL Server](system-transactions-integration-with-sql-server.md)  
Décrit l’intégration de <xref:System.Transactions> à SQL Server pour utiliser les transactions distribuées.  
  
[Accès concurrentiel optimiste](optimistic-concurrency.md) Explique l’accès concurrentiel optimiste et pessimiste et comment déceler les violations d’accès concurrentiel.  

## <a name="see-also"></a>Voir aussi

- [Notions de base des transactions](/dotnet/framework/data/transactions/transaction-fundamentals)
- [Connexion à une source de données](connecting-to-data-source.md)
- [Commandes et paramètres](commands-parameters.md)
- [DataAdapters et DataReaders](dataadapters-datareaders.md)
- [DbProviderFactories](dbproviderfactories.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
