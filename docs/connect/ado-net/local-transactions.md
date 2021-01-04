---
title: Transactions locales
description: Montre comment effectuer des transactions sur une base de données avec le fournisseur de données Microsoft SqlClient pour SQL Server.
ms.date: 11/24/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: a310ab409c2300eb31d1e3c0e58b7ebe32096bfd
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051270"
---
# <a name="local-transactions"></a>Transactions locales

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Les transactions dans ADO.NET servent à lier plusieurs tâches ensemble de sorte qu’elles s’exécutent comme une seule unité de travail. Par exemple, imaginez qu'une application effectue deux tâches. Premièrement, elle met à jour une table avec des informations de commande. Deuxièmement, elle met à jour une table qui contient des informations de stock, en débitant les articles commandés. Si l’une ou l’autre des tâches échoue, les deux mises à jour sont annulées.  

## <a name="determining-the-transaction-type"></a>Détermination du type de transaction

Une transaction est considérée comme locale quand il s’agit d’une transaction monophase et qu’elle est gérée directement par la base de données. Une transaction est considérée comme étant distribuée quand elle est coordonnée par un moniteur de transaction et qu’elle utilise des mécanismes de prévention de défaillance (tels qu’une validation en deux phases) pour sa résolution.

Le fournisseur de données Microsoft SqlClient pour SQL Server dispose de son propre objet <xref:Microsoft.Data.SqlClient.SqlTransaction> pour effectuer des transactions locales dans des bases de données SQL Server. D’autres fournisseurs de données .NET fournissent également leurs propres objets `Transaction`. Par ailleurs, il existe une classe <xref:System.Data.Common.DbTransaction> qui permet d’écrire du code indépendant du fournisseur qui exige des transactions.

> [!NOTE]
> Les transactions ont une efficacité maximale quand elles sont exécutées sur le serveur. Si vous utilisez une base de données SQL Server qui utilise beaucoup des transactions explicites, envisagez de les écrire sous la forme de procédures stockées à l’aide de l’instruction Transact-SQL BEGIN TRANSACTION.

## <a name="performing-a-transaction-using-a-single-connection"></a>Exécution d’une transaction avec une seule connexion 

Dans ADO.NET, vous contrôlez les transactions avec l’objet `Connection`. Vous pouvez initier une transaction locale avec la méthode `BeginTransaction`. Après avoir commencé une transaction, vous pouvez inscrire une commande dans cette transaction avec la propriété `Transaction` d’un objet `Command`. Vous pouvez ensuite valider ou annuler les modifications apportées à la source de données en fonction de la réussite ou de l’échec des composants de la transaction.

> [!NOTE]
> La méthode `EnlistDistributedTransaction` ne doit pas être utilisée pour une transaction locale.

La portée de la transaction est limitée à la connexion. L’exemple suivant exécute une transaction explicite consistant en deux commandes distinctes dans le bloc `try`. Les commandes exécutent des instructions INSERT sur la table `Production.ScrapReason` dans l’exemple de base de données SQL Server AdventureWorks, qui sont validées si aucune exception n’est levée. Le code dans le bloc `catch` annule la transaction en cas d'exception. En cas d'annulation de la transaction ou de fermeture de la connexion avant que la transaction ne soit terminée, celle-ci est automatiquement annulée.

## <a name="example"></a>Exemple  

 Procédez comme suit pour effectuer une transaction.

1. Appelez la méthode <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> de l’objet <xref:Microsoft.Data.SqlClient.SqlConnection> pour marquer le début de la transaction. La méthode <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> retourne une référence à la transaction. Cette référence est affectée aux objets <xref:Microsoft.Data.SqlClient.SqlCommand> inscrits dans la transaction.

2. Assignez l'objet `Transaction` à la propriété <xref:Microsoft.Data.SqlClient.SqlCommand.Transaction%2A> de l'objet <xref:Microsoft.Data.SqlClient.SqlCommand> à exécuter. Si une commande est exécutée sur une connexion sur laquelle une transaction est active et si l’objet `Transaction` n’a pas été affecté à la propriété `Transaction` de l’objet `Command`, une exception est levée.

3. Exécutez les commandes requises.

4. Appelez la méthode <xref:Microsoft.Data.SqlClient.SqlTransaction.Commit%2A> de l'objet <xref:Microsoft.Data.SqlClient.SqlTransaction> pour effectuer la transaction ou la méthode <xref:Microsoft.Data.SqlClient.SqlTransaction.Rollback%2A> pour y mettre fin. Si la connexion est fermée ou libérée avant que l’une des méthodes <xref:Microsoft.Data.SqlClient.SqlTransaction.Commit%2A> ou <xref:Microsoft.Data.SqlClient.SqlTransaction.Rollback%2A> ait été exécutée, la transaction est annulée.

L’exemple de code suivant illustre la logique transactionnelle en utilisant le fournisseur de données Microsoft SqlClient pour SQL Server.  

[!code-csharp[SqlTransactionLocal#1](~/../sqlclient/doc/samples/SqlTransactionLocal.cs#1)]

## <a name="see-also"></a>Voir aussi

- [Transactions et accès simultané](transactions-and-concurrency.md)
- [Transactions distribuées](distributed-transactions.md)
- [Intégration de System.Transactions à SQL Server](system-transactions-integration-with-sql-server.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
