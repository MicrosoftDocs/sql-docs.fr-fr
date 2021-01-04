---
title: Transactions distribuées
description: Explique comment effectuer des transactions distribuées dans ADO.NET
ms.date: 11/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 77ed8486f8b059f12fe7178826d81a743a97bbc4
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559231"
---
# <a name="distributed-transactions"></a>Transactions distribuées

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Une transaction est un ensemble de tâches liées entre elles qui échouent (abort) ou réussissent (commit) globalement, entre autres choses. Une *transaction distribuée* est une transaction qui concerne plusieurs ressources. Pour qu’une transaction distribuée soit validée, l’ensemble des participants doivent garantir que tout changement apporté aux données sera permanent. Ces modifications doivent persister même en cas de panne du système ou de tout autre événement imprévu. Il suffit qu’un seul des participants n’apporte pas cette garantie pour que l’ensemble de la transaction échoue et toutes les modifications apportées aux données dans le cadre de la transaction sont annulées. 

> [!NOTE]
> Une exception est levée lorsque vous tentez de valider ou d’annuler une transaction si `DataReader` démarre pendant que la transaction est active.

## <a name="working-with-systemtransactions"></a>Utilisation de System.Transactions

Dans .NET, les transactions distribuées sont managées via l’API de l’espace de noms <xref:System.Transactions>. L’API <xref:System.Transactions> délègue la gestion de transaction distribuée à un moniteur de transaction tel que le Microsoft Distributed Transaction Coordinator (MS DTC) lorsque plusieurs gestionnaires de ressources persistants sont impliqués. Pour plus d’informations, consultez [Notions de base des transactions](/dotnet/framework/data/transactions/transaction-fundamentals).

ADO.NET 2.0 a introduit la prise en charge de l’inscription dans une transaction distribuée à l’aide de la méthode `EnlistTransaction`, qui inscrit une connexion dans une instance <xref:System.Transactions.Transaction>. Dans les versions précédentes d’ADO.NET, l’inscription explicite dans des transactions distribuées était effectuée à l’aide de la méthode `EnlistDistributedTransaction` d’une connexion afin d’inscrire une connexion dans une instance de l’objet <xref:System.EnterpriseServices.ITransaction>, qui est prise en charge à des fins de compatibilité ascendante. Pour plus d’informations sur les transactions Enterprise Services, consultez [Interopérabilité avec Enterprise Services et les transactions COM+](/dotnet/framework/data/transactions/interoperability-with-enterprise-services-and-com-transactions).

Quand une transaction <xref:System.Transactions> est utilisée avec le fournisseur de données Microsoft SqlClient pour SQL Server sur une base de données SQL Server, un <xref:System.Transactions.Transaction> léger est automatiquement utilisé. La transaction peut être promue en transaction totalement distribuée en fonction des besoins. Pour plus d’informations, consultez [Intégration de System.Transactions à SQL Server](system-transactions-integration-with-sql-server.md).

## <a name="automatically-enlisting-in-a-distributed-transaction"></a>Inscription automatique dans une transaction distribuée

L'inscription automatique est la méthode par défaut (et conseillée) pour intégrer des connexions ADO.NET avec `System.Transactions`. Un objet Connection est automatiquement inscrit dans une transaction distribuée existante s’il détermine qu’une transaction est active, ce qui, pour `System.Transaction`, signifie que `Transaction.Current` n’a pas la valeur null. Une inscription de transaction automatique se produit lors de l’ouverture de la connexion. Elle ne se produit pas ensuite, même si une commande est exécutée dans le cadre d’une transaction. Vous pouvez désactiver l’inscription automatique dans des transactions existantes en spécifiant `Enlist=false` comme paramètre de chaîne de connexion pour un <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType>.

## <a name="manually-enlisting-in-a-distributed-transaction"></a>Inscription manuelle dans une transaction distribuée

Si l’inscription automatique est désactivée ou si vous devez inscrire une transaction qui a été lancée après l’ouverture de la connexion, vous pouvez inscrire dans une transaction distribuée existante à l’aide de la méthode `EnlistTransaction` de l’objet <xref:Microsoft.Data.SqlClient.SqlConnection> pour le fournisseur de données Microsoft SqlClient pour SQL Server. L'inscription dans une transaction distribuée existante garantit que si la transaction vient à être annulée ou validée, les modifications apportées par le code dans la source de données seront elles aussi validées ou annulées.

L’inscription dans des transactions distribuées s’applique en particulier lors du regroupement d’objets métier. Si un objet métier est regroupé avec une connexion ouverte, l'inscription automatique dans la transaction se produit uniquement lorsque cette connexion est ouverte. Si plusieurs transactions sont effectuées à l’aide de l’objet métier regroupé, la connexion ouverte pour cet objet n’est pas automatiquement inscrite dans les nouvelles transactions lancées. Dans ce cas, vous pouvez désactiver l'inscription de transaction automatique pour la connexion et inscrire la connexion dans des transactions à l'aide de `EnlistTransaction`.

`EnlistTransaction` accepte un argument unique de type <xref:System.Transactions.Transaction> qui est une référence à la transaction existante. Après avoir appelé la méthode `EnlistTransaction` de la connexion, toutes les modifications apportées à la source de données à l’aide de la connexion sont incluses dans la transaction. Le fait de passer une valeur null désinscrit la connexion de l’inscription de transaction distribuée actuelle. Notez que la connexion doit être ouverte avant l'appel de `EnlistTransaction`.

> [!NOTE]
> Une fois qu’une connexion est explicitement inscrite sur une transaction, il n’est plus possible de la désinscrire ou de l’inscrire dans une autre transaction tant que la première transaction n’est pas terminée.

> [!CAUTION]
> `EnlistTransaction` lève une exception si la connexion a déjà entamé une transaction à l'aide de la méthode <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> de la connexion. Toutefois, si la transaction est une transaction locale démarrée dans la source de données (par exemple, l'exécution explicite de l'instruction BEGIN TRANSACTION à l'aide d'un <xref:Microsoft.Data.SqlClient.SqlCommand>), `EnlistTransaction` annule la transaction locale et s'inscrit dans la transaction distribuée existante comme requis. Vous ne recevrez pas de notification indiquant que la transaction locale a été annulée et que vous devez gérer les transactions locales non lancées à l’aide de <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A>. Si vous utilisez le fournisseur de données Microsoft SqlClient pour SQL Server avec SQL Server, toute tentative d’inscription lève une exception. Les autres cas ne sont pas détectés.  

## <a name="promotable-transactions-in-sql-server"></a>Transactions pouvant être promues dans SQL Server

SQL Server prend en charge les transactions pouvant être promues dans lesquelles une transaction légère locale peut être automatiquement promue en transaction distribuée uniquement si c’est requis. Une transaction pouvant être promue n'invoque pas la charge supplémentaire d'une transaction distribuée à moins qu'elle ne soit requise. Pour obtenir des informations complémentaires et un exemple de code, consultez [Intégration de System.Transactions à SQL Server](system-transactions-integration-with-sql-server.md).

## <a name="configuring-distributed-transactions"></a>Configuration de transactions distribuées

 Vous devrez peut-être activer MS DTC sur le réseau afin d'utiliser des transactions distribuées. Si le Pare-feu Windows est activé, vous devez autoriser le service MS DTC à utiliser le réseau ou à ouvrir le port MS DTC.  
  
## <a name="see-also"></a>Voir aussi

- [Transactions et accès simultané](transactions-and-concurrency.md)
- [Intégration de System.Transactions à SQL Server](system-transactions-integration-with-sql-server.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
