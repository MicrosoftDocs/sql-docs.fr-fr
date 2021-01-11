---
title: Programmation asynchrone
description: En savoir plus sur la programmation asynchrone dans le fournisseur de données Microsoft SqlClient pour SQL Server.
ms.date: 12/04/2020
ms.assetid: 85da7447-7125-426e-aa5f-438a290d1f77
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 6eec681974499219a1ca649f9a47449a27ff4002
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804302"
---
# <a name="asynchronous-programming"></a>Programmation asynchrone

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Cette rubrique décrit la prise en charge de la programmation asynchrone dans le fournisseur de données Microsoft SqlClient pour SQL Server (SqlClient).

## <a name="legacy-asynchronous-programming"></a>Programmation asynchrone héritée

Le fournisseur de données Microsoft SqlClient pour SQL Server comprend des méthodes de **System.Data.SqlClient** afin de garantir la compatibilité descendante pour les applications qui migrent vers <xref:Microsoft.Data.SqlClient>. Il n’est pas recommandé d’utiliser les méthodes de programmation asynchrones héritées suivantes pour les nouveaux développements :

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteNonQuery%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteReader%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteXmlReader%2A?displayProperty=nameWithType>

> [!TIP]
> Dans le Fournisseur de données Microsoft SqlClient pour SQL Server, ces méthodes héritées n’ont plus besoin de `Asynchronous Processing=true` dans la chaîne de connexion.

## <a name="asynchronous-programming-features"></a>Fonctionnalités de programmation asynchrone

Ces fonctionnalités de programmation asynchrones fournissent une technique simple pour rendre le code asynchrone.

Pour plus d’informations sur la programmation asynchrone en .NET, consultez :

- [Programmation asynchrone en C#](/dotnet/csharp/async)

- [Programmation asynchrone avec Async et Await (Visual Basic)](/dotnet/visual-basic/programming-guide/concepts/async/index)

Lorsque votre interface utilisateur ne répond pas ou votre serveur ne monte pas en charge, il est probable que vous votre code doit être plus asynchrone. Écrire du code asynchrone implique en général l'installation d'un rappel (également appelé suite) pour exprimer la logique qui se produit après que l'opération asynchrone s'est terminée. Cela complique la structure du code asynchrone par rapport au code synchrone.

Vous pouvez appeler des méthodes asynchrones sans utiliser de rappels et sans fractionner votre code entre plusieurs méthodes ou expressions lambda.

Le modificateur `async` spécifie qu'une méthode est asynchrone. Lors de l’appel d’une méthode `async`, une tâche est retournée. Lorsque l’opérateur `await` est appliqué à une tâche, la méthode actuelle se termine immédiatement. Lorsque la tâche se termine, l’exécution reprend dans la même méthode.

> [!TIP]
> Dans le fournisseur de données Microsoft SqlClient pour SQL Server, les appels asynchrones ne sont pas requis pour définir le mot clé de chaîne de connexion `Context Connection`.

L'appel d'une méthode `async` n'alloue aucun thread supplémentaire. Elle peut utiliser le thread de terminaison d'E/S existant brièvement à la fin.

Les méthodes suivantes de du fournisseur de données Microsoft SqlClient pour SQL Server prennent en charge la programmation asynchrone :

- <xref:System.Data.Common.DbConnection.OpenAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteDbDataReaderAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteNonQueryAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteReaderAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteScalarAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbDataReader.GetFieldValueAsync%2A>

- <xref:System.Data.Common.DbDataReader.IsDBNullAsync%2A>

- <xref:System.Data.Common.DbDataReader.NextResultAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbDataReader.ReadAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlConnection.OpenAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQueryAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReaderAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteScalarAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteXmlReaderAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.NextResultAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.ReadAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlBulkCopy.WriteToServerAsync%2A?displayProperty=nameWithType>

D’autres membres asynchrones prennent en charge [la diffusion en continu de SqlClient](sqlclient-streaming-support.md).

> [!TIP]
> Les méthodes asynchrones n’ont pas besoin de `Asynchronous Processing=true` dans la chaîne de connexion. Et cette propriété est obsolète dans le fournisseur de données Microsoft SqlClient pour SQL Server.

### <a name="synchronous-to-asynchronous-connection-open"></a>Ouverture de connexion synchrone à asynchrone

Vous pouvez mettre à niveau une application existante pour utiliser la fonctionnalité asynchrone. Par exemple, supposons qu'une application possède un algorithme de connexion synchrone et bloque le thread d'interface utilisateur chaque fois qu'elle se connecte à la base de données et, une fois connectée, l'application appelle une procédure stockée qui signale d'autres utilisateurs que celui qui vient de se connecter.

[!code-csharp[SqlCommand_ExecuteNonQuery#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery.cs#1)]

En cas de conversion pour utiliser les fonctionnalités asynchrones, le programme ressemble à ce qui suit :

[!code-csharp[SqlCommand_ExecuteNonQueryAsync#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQueryAsync.cs#1)]

### <a name="add-the-asynchronous-feature-in-an-existing-application-mixing-old-and-new-patterns"></a>Ajouter la fonctionnalité asynchrone dans une application existante (en combinant les modèles anciens et nouveaux)

Il est également possible d’ajouter une fonctionnalité asynchrone (SqlConnection::OpenAsync) sans modifier la logique asynchrone existante. Par exemple, si d'une application utilise actuellement :

[!code-csharp[SqlConnection_OpenAsync_ContinueWith#1](~/../sqlclient/doc/samples/SqlConnection_OpenAsync_ContinueWith.cs#1)]

Vous pouvez commencer à utiliser le modèle asynchrone sans modifier considérablement l’algorithme existant.

[!code-csharp[SqlConnection_OpenAsync_ContinueWith#2](~/../sqlclient/doc/samples/SqlConnection_OpenAsync_ContinueWith.cs#2)]

### <a name="use-the-base-provider-model-and-the-asynchronous-feature"></a>Utiliser le modèle de fournisseur de base et la fonctionnalité asynchrone

Vous devrez peut-être créer un outil qui peut se connecter à différentes bases de données et exécuter des requêtes. Vous pouvez utiliser le modèle de fournisseur de base et la fonctionnalité asynchrone.

Microsoft Distributed Transaction Controller (MSDTC) doit être activé sur le serveur pour utiliser des transactions distribuées. Pour plus d’informations sur l’activation de MSDTC, consultez [Procédure : activer MSDTC sur un serveur web](/previous-versions/commerce-server/dd327979(v=cs.90)).

[!code-csharp[SqlClient_AsyncProgramming_ProviderModel#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_ProviderModel.cs#1)]

### <a name="use-sql-transactions-and-the-new-asynchronous-feature"></a>Utiliser des transactions SQL et la nouvelle fonctionnalité asynchrone

[!code-csharp[SqlClient_AsyncProgramming_SqlTransaction#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_SqlTransaction.cs#1)]

### <a name="use-distributed-transactions-and-the-new-asynchronous-feature"></a>Utiliser des transactions distribuées et la nouvelle fonctionnalité asynchrone

Dans une application d’entreprise, vous devrez peut-être ajouter des **transactions distribuées** dans certains scénarios, pour permettre les transactions entre plusieurs serveurs de base de données. Vous pouvez utiliser l’espace de noms System.Transactions et enregistrer une transaction distribuée, comme suit :

[!code-csharp[SqlClient_AsyncProgramming_DistributedTransaction#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_DistributedTransaction.cs#1)]

### <a name="cancel-an-asynchronous-operation"></a>Annuler une opération asynchrone

Vous pouvez annuler une requête asynchrone à l'aide de <xref:System.Threading.CancellationToken>.

[!code-csharp[SqlClient_AsyncProgramming_Cancellation#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_Cancellation.cs#1)]

### <a name="asynchronous-operations-with-sqlbulkcopy"></a>Opérations asynchrones avec SqlBulkCopy

Les fonctionnalités asynchrones sont également dans <xref:Microsoft.Data.SqlClient.SqlBulkCopy?displayProperty=nameWithType> avec <xref:Microsoft.Data.SqlClient.SqlBulkCopy.WriteToServerAsync%2A?displayProperty=nameWithType>.

[!code-csharp[SqlClient_AsyncProgramming_SqlBulkCopy#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_SqlBulkCopy.cs#1)]

## <a name="asynchronously-use-multiple-commands-with-mars"></a>Utiliser de manière asynchrone plusieurs commandes avec MARS

L’exemple ouvre une connexion unique à la base de données **AdventureWorks**. À l’aide d’un objet <xref:Microsoft.Data.SqlClient.SqlCommand>, un <xref:Microsoft.Data.SqlClient.SqlDataReader> est créé. À mesure que le lecteur est utilisé, un deuxième <xref:Microsoft.Data.SqlClient.SqlDataReader> est ouvert, en utilisant les données du premier <xref:Microsoft.Data.SqlClient.SqlDataReader> comme entrée de la clause WHERE pour le deuxième lecteur.

> [!NOTE]
> L’exemple suivant utilise l’exemple de [base de données **AdventureWorks**](../../samples/adventureworks-install-configure.md). La chaîne de connexion fournie dans l’exemple de code suppose que la base de données est installée et disponible sur l’ordinateur local. Modifiez la chaîne de connexion en fonction des besoins de votre environnement.

[!code-csharp[SqlClient_AsyncProgramming_MultipleCommands#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_MultipleCommands.cs#1)]

## <a name="asynchronously-read-and-update-data-with-mars"></a>Lire et mettre à jour des données de façon asynchrone avec MARS

MARS permet l’utilisation d’une connexion pour les opérations de lecture et les opérations DML (Data Manipulation Language) avec plusieurs opérations en attente. Cette fonctionnalité évite à une application de traiter les erreurs de connexion occupée. En outre, MARS peut remplacer l’utilisateur des curseurs côté serveur, qui consomme généralement davantage de ressources. Pour finir, comme plusieurs opérations peuvent opérer sur une seule connexion, elles peuvent partager le même contexte de transaction, en éliminant la nécessité d’utiliser les procédures stockées **système sp_getbindtoken** et **sp_bindsession**.

L’application Console suivante montre comment utiliser deux objets <xref:Microsoft.Data.SqlClient.SqlDataReader> avec trois objets <xref:Microsoft.Data.SqlClient.SqlCommand> et un objet <xref:Microsoft.Data.SqlClient.SqlConnection> unique avec MARS activé. Le premier objet de commande récupère une liste de fournisseurs dont le niveau de solvabilité est 5. Le deuxième objet de commande utilise l’ID de fournisseur fournie par un <xref:Microsoft.Data.SqlClient.SqlDataReader> pour charger le deuxième <xref:Microsoft.Data.SqlClient.SqlDataReader> avec tous les produits du fournisseur en question. Chaque enregistrement de produit est visité par le deuxième <xref:Microsoft.Data.SqlClient.SqlDataReader>. Un calcul est effectué afin de déterminer ce que doit être le nouveau **OnOrderQty**. Le troisième objet de commande est ensuite utilisé pour mettre à jour la table **ProductVendor** avec la nouvelle valeur. Tout ce processus entier se déroule au sein d’une seule transaction, qui est restaurée à la fin.

> [!NOTE]
> L’exemple suivant utilise l’exemple de [base de données **AdventureWorks**](../../samples/adventureworks-install-configure.md). La chaîne de connexion fournie dans l’exemple de code suppose que la base de données est installée et disponible sur l’ordinateur local. Modifiez la chaîne de connexion en fonction des besoins de votre environnement.

[!code-csharp[SqlClient_AsyncProgramming_MultipleCommandsEx#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_MultipleCommandsEx.cs#1)]

## <a name="see-also"></a>Voir aussi

- [Récupération et modification de données dans ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
