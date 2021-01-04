---
title: Intégration de System.Transactions à SQL Server
description: Décrit l’intégration de System.Transactions à SQL Server pour utiliser des transactions distribuées.
ms.date: 11/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: af0ba2865719a5388314a4ca695e09191cb56173
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051315"
---
# <a name="systemtransactions-integration-with-sql-server"></a>Intégration de System.Transactions à SQL Server

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

.NET comprend une infrastructure de transactions à laquelle il est possible d’accéder via l’espace de noms <xref:System.Transactions>. Cette infrastructure expose les transactions de façon totalement intégrée à .NET, dont ADO.NET.  
  
En plus des améliorations de la programmabilité, <xref:System.Transactions> et ADO.NET peuvent fonctionner ensemble pour coordonner les optimisations quand des transactions sont utilisées. Une transaction susceptible d'être promue est une transaction légère (locale) qui peut être promue automatiquement en une transaction entièrement distribuée en fonction des besoins.

Le fournisseur de données Microsoft SqlClient pour SQL Server prend en charge les transactions pouvant être promues quand SQL Server est utilisé. Une transaction pouvant être promue n'invoque pas la charge supplémentaire d'une transaction distribuée à moins qu'elle ne soit requise. Les transactions pouvant être promues sont automatiques et ne nécessitent aucune intervention de la part du développeur.

## <a name="creating-promotable-transactions"></a>Création de transactions pouvant être promues

Le fournisseur de données Microsoft SqlClient pour SQL Server assure la prise en charge des transactions pouvant être promues, qui sont gérées au moyen de classes dans l’espace de noms <xref:System.Transactions>. Les transactions pouvant être promues optimisent les transactions distribuées en différant la création d'une transaction distribuée jusqu'à ce qu'elle soit nécessaire. Si un seul gestionnaire de ressources est requis, aucune transaction distribuée n'a lieu.

> [!NOTE]
> Dans un scénario de niveau de confiance partiel, l'objet <xref:System.Transactions.DistributedTransactionPermission> est requis lorsqu'une transaction est promue en transaction distribuée.

## <a name="promotable-transaction-scenarios"></a>Scénarios de transactions pouvant être promues

Les transactions distribuées consomment généralement une partie importante des ressources système, car elles sont gérées par Microsoft Distributed Transaction Coordinator (MS DTC), qui intègre tous les gestionnaires de ressources auxquels la transaction accède. Une transaction pouvant être promue est une forme spéciale de transaction <xref:System.Transactions> qui délègue efficacement le travail à une simple transaction SQL Server. <xref:System.Transactions>, <xref:Microsoft.Data.SqlClient> et SQL Server coordonnent le travail impliqué dans la gestion de la transaction en la promouvant en transaction distribuée complète, si nécessaire.

L'avantage de l'utilisation de transactions pouvant être promues réside dans le fait que, quand une connexion est ouverte à l'aide d'une transaction <xref:System.Transactions.TransactionScope> active alors qu'aucune autre connexion n'est ouverte, la transaction est validée comme transaction légère, au lieu de générer la charge supplémentaire d'une transaction entièrement distribuée.

### <a name="connection-string-keywords"></a>Mots clés de chaîne de connexion

La propriété <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> prend en charge un mot clé, `Enlist`, qui indique si <xref:Microsoft.Data.SqlClient> détecte des contextes transactionnels et inscrit automatiquement la connexion dans une transaction distribuée. Si `Enlist=true`, la connexion est automatiquement inscrite dans le contexte de transaction actuel du thread qui s'ouvre. Si `Enlist=false`, la connexion `SqlClient` n'interagit pas avec une transaction distribuée. La valeur par défaut de `Enlist` est true. Si `Enlist` n'est pas spécifié dans la chaîne de connexion, la connexion est automatiquement inscrite dans une transaction distribuée détectée lors de l'ouverture de la connexion.

Les mots clés `Transaction Binding` d'une chaîne de connexion <xref:Microsoft.Data.SqlClient.SqlConnection> contrôlent l'association de la connexion à une transaction `System.Transactions` inscrite. Il est également possible d'utiliser la propriété <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.TransactionBinding%2A> d'un objet <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>.

Le tableau suivant décrit les valeurs possibles.
  
|Mot clé|Description|  
|-------------|-----------------|  
|Implicit Unbind|Valeur par défaut. La connexion se détache de la transaction une fois cette dernière terminée et revient au mode de validation automatique.|
|Explicit Unbind|La connexion reste attachée à la transaction jusqu'à la fermeture de cette dernière. La connexion échoue si la transaction associée n'est pas active ou ne correspond pas à la propriété <xref:System.Transactions.Transaction.Current%2A>.|

## <a name="using-transactionscope"></a>Utilisation de TransactionScope

La classe <xref:System.Transactions.TransactionScope> crée un bloc de code transactionnel en inscrivant implicitement les connexions dans une transaction distribuée. Vous devez appeler la méthode <xref:System.Transactions.TransactionScope.Complete%2A> à la fin du bloc <xref:System.Transactions.TransactionScope> avant de le quitter. Le fait de quitter le bloc invoque la méthode <xref:System.Transactions.TransactionScope.Dispose%2A> . Si une exception a été levée qui a pour effet que le code sorte de la portée, la transaction est considérée comme abandonnée.

Il est recommandé d'utiliser un bloc `using` pour s'assurer que la méthode <xref:System.Transactions.TransactionScope.Dispose%2A> est appelée sur l'objet <xref:System.Transactions.TransactionScope> en cas de sortie du bloc using. La non-validation ou la non-restauration des transactions en attente peut considérablement nuire aux performances, le délai d'attente par défaut de l'objet <xref:System.Transactions.TransactionScope> étant d'une minute. Si vous n'utilisez pas une instruction `using`, vous devez effectuer tout le travail dans un bloc `Try` et appeler explicitement la méthode <xref:System.Transactions.TransactionScope.Dispose%2A> dans le bloc `Finally`.

Si une exception est levée dans l'objet <xref:System.Transactions.TransactionScope>, la transaction est marquée comme incohérente et est abandonnée. Elle sera annulée lors de la suppression du <xref:System.Transactions.TransactionScope> . Si aucune exception ne se produit, les transactions sont validées.

> [!NOTE]
> La classe `TransactionScope` crée par défaut une transaction avec un <xref:System.Transactions.Transaction.IsolationLevel%2A> ayant la valeur `Serializable`. Selon votre application, vous pouvez envisager de baisser le niveau d'isolation pour éviter une contention élevée au sein de votre application.

> [!NOTE]
> Il est recommandé de n'effectuer des mises à jour, des insertions et des suppressions que dans des transactions distribuées car ces opérations consomment des ressources de base de données importantes. Les instructions select risquent de verrouiller inutilement les ressources de base de données ; dans certains cas, vous pouvez être amené à utiliser des transactions pour effectuer des sélections. Tout travail autre qu'un travail de base de données doit être réalisé en dehors de la transaction, à moins qu'il n'implique d'autres gestionnaires de ressources.
Bien qu'une exception dans la transaction empêche la validation de celle-ci, la classe <xref:System.Transactions.TransactionScope> n'a aucune disposition pour annuler les modifications que votre code a apportées en dehors de la transaction proprement dite. Pour intervenir lors de l'annulation de la transaction, vous devez écrire votre propre implémentation de l'interface <xref:System.Transactions.IEnlistmentNotification> et vous inscrire explicitement dans la transaction.

## <a name="example"></a>Exemple

L'utilisation de l'espace de noms <xref:System.Transactions> exige que vous disposiez d'une référence à System.Transactions.dll.

La fonction suivante montre comment créer une transaction pouvant être promue en relation avec deux instances différentes de SQL Server, représentées par deux objets <xref:Microsoft.Data.SqlClient.SqlConnection> différents, enveloppés dans un bloc <xref:System.Transactions.TransactionScope> .

Le code ci-dessous crée le bloc <xref:System.Transactions.TransactionScope> avec une instruction `using` et ouvre la première connexion, qui l’inscrit automatiquement dans <xref:System.Transactions.TransactionScope>.

La transaction est inscrite initialement comme transaction légère, et non comme transaction distribuée complète. La seconde connexion est inscrite dans l'objet <xref:System.Transactions.TransactionScope> uniquement si la commande dans la première connexion ne lève pas une exception. Une fois la seconde connexion ouverte, la transaction est automatiquement promue en transaction entièrement distribuée.

Par la suite, la méthode <xref:System.Transactions.TransactionScope.Complete%2A> est appelée, qui ne valide la transaction que si aucune exception n’a été levée. Si une exception a été levée dans le bloc <xref:System.Transactions.TransactionScope> , la méthode `Complete` n'est pas appelée et la transaction distribuée est annulée lors de la suppression de l'objet <xref:System.Transactions.TransactionScope> à la fin de son bloc `using` .

[!code-csharp[SqlTransactionScope#1](~/../sqlclient/doc/samples/SqlTransactionScope.cs#1)]

## <a name="see-also"></a>Voir aussi

- [Transactions et accès simultané](transactions-and-concurrency.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
