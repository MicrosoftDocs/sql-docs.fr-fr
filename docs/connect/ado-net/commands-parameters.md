---
title: Commandes et paramètres
description: Découvrez comment utiliser des objets Commande pour le Fournisseur de données Microsoft SqlClient pour SQL Server afin d’exécuter des commandes et de retourner des résultats à partir d’une source de données.
ms.date: 11/25/2020
ms.assetid: b623f810-d871-49a5-b0f5-078cc3c34db6
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 07f23aa6545636fd8cbbf1717f6c8ee81ebe9294
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771471"
---
# <a name="commands-and-parameters"></a>Commandes et paramètres

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Après avoir établi une connexion à une source de données, vous pouvez exécuter des commandes et retourner les résultats de la source de données à l'aide d'un objet <xref:System.Data.Common.DbCommand>. Vous pouvez créer une commande à l’aide de l’un des constructeurs de commande pour le Fournisseur de données Microsoft SqlClient pour SQL Server. Les constructeurs acceptent des arguments optionnels, comme une instruction SQL à exécuter au niveau de la source de données, un objet <xref:System.Data.Common.DbConnection> ou un objet <xref:System.Data.Common.DbTransaction>.

Vous pouvez également configurer ces objets en tant que propriétés de la commande. Vous pouvez aussi créer une commande pour une connexion particulière à l'aide de la méthode <xref:System.Data.Common.DbConnection.CreateCommand%2A> d'un objet `DbConnection`. L'instruction SQL en cours d'exécution par la commande peut être configurée à l'aide de la propriété <xref:System.Data.Common.DbCommand.CommandText%2A>. Le Fournisseur de données Microsoft SqlClient pour SQL Server a le projet <xref:Microsoft.Data.SqlClient.SqlCommand>.

## <a name="in-this-section"></a>Dans cette section

[Exécution d’une commande](execute-command.md)  
Décrit l'objet `Command` et son utilisation pour exécuter des requêtes et des commandes sur une source de données.

[Configuration des paramètres](configure-parameters.md)  
Décrit l'utilisation des paramètres `Command`, y compris la direction, les types de données et la syntaxe des paramètres.

[Génération de commandes avec CommandBuilders](generate-commands-with-commandbuilders.md)  
Décrit l'utilisation de générateurs de commande pour générer automatiquement les commandes INSERT, UPDATE et DELETE pour un `DataAdapter` ayant une commande SELECT d'analyse unique.

[Obtention d’une valeur unique dans une base de données](obtain-single-value-from-database.md)  
Décrit comment utiliser la méthode `ExecuteScalar` d'un objet `Command` pour retourner une valeur unique à partir d'une requête de base de données.

[Utilisation des commandes pour modifier les données](use-commands-to-modify-data.md)  
Décrit comment utiliser le Fournisseur de données Microsoft SqlClient pour SQL Server pour exécuter des procédures stockées ou des instructions du langage de définition de données (DDL).

## <a name="see-also"></a>Voir aussi

- [DataAdapters et DataReaders](dataadapters-datareaders.md)
- [Connexion à une source de données](connecting-to-data-source.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
