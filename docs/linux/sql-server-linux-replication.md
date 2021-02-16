---
title: Réplication SQL Server sur Linux
description: Découvrez comment SQL Server 2017 (14.x) (CU18) et les versions ultérieures prennent en charge la réplication SQL Server pour les instances de SQL Server sur Linux.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 12/09/2019
ms.topic: article
ms.prod: sql
ms.prod_service: database-engine
ms.technology: linux
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: bb322f11b866a7593d22e0d722fad84765cb5139
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346449"
---
# <a name="sql-server-replication-on-linux"></a>Réplication SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[SQL Server 2017](../includes/sssql17-md.md)] ([CU18](https://support.microsoft.com/help/4527377)) et les versions ultérieures prennent en charge la réplication SQL Server pour les instances de SQL Server sur Linux.

Configurez la réplication sur Linux avec les [procédures stockées de réplication](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md) de SQL Server Management Studio (SSMS).

Une instance de SQL Server peut participer à n’importe quel rôle de réplication :

* Serveur de publication
* Serveur de distribution
* Abonné

Un schéma de réplication peut combiner et faire correspondre des plateformes de systèmes d’exploitation. Par exemple, un schéma de réplication peut inclure une instance de SQL Server sur Linux pour le serveur de publication et le serveur de distribution, et les abonnés incluent des instances de SQL Server sur Windows et Linux.

Les instances de SQL Server sur Linux peuvent participer à n’importe quel type de réplication.

* Transactionnelle
* Instantané

Pour plus d’informations sur la réplication, consultez la [documentation sur la réplication SQL Server](../relational-databases/replication/sql-server-replication.md).

## <a name="supported-features"></a>Fonctionnalités prises en charge

Les fonctionnalités de réplication suivantes sont prises en charge :

* Réplication d'instantané
* Réplication transactionnelle
* Réplication avec des ports autres que les ports par défaut <!--Add link to explanation-->
* Réplication avec authentification AD
* Configurations de réplication sur Windows et Linux
* Mises à jour immédiates pour la réplication transactionnelle

## <a name="limitations"></a>Limites

Les fonctionnalités suivantes ne sont pas prises en charge :

* Réplication de fusion
* Réplication d’égal à égal
* Publication Oracle

## <a name="next-steps"></a>Étapes suivantes

[Configurer la réplication SQL Server sur Linux](sql-server-linux-replication-tutorial-tsql.md)

[Exemple : Configurer la réplication SQL Server sur Linux](sql-server-linux-replication-configure.md)
