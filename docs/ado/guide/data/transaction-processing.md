---
description: Traitement transactionnel
title: Traitement des transactions | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- transactions [ADO]
- data updates [ADO], transaction processing
- updating data [ADO], transaction processing
- nested transactions [ADO]
ms.assetid: 74ab6706-e2dc-42cb-af77-dbc58a9cf4ce
author: rothja
ms.author: jroth
ms.openlocfilehash: f534d38ea52e942a00e2e95f837b096ed6fef0f1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032381"
---
# <a name="transaction-processing"></a>Traitement des transactions
Une *transaction* délimite le début et la fin d’une série d’opérations d’accès aux données exécutées sur une connexion. Selon les fonctionnalités transactionnelles de votre source de données, l’objet de **connexion** vous permet également de créer et de gérer des transactions. Par exemple, en utilisant le fournisseur Microsoft OLE DB pour SQL Server pour accéder à une base de données sur Microsoft SQL Server, vous pouvez créer plusieurs transactions imbriquées pour les commandes que vous exécutez.  
  
 ADO garantit que les modifications apportées à une source de données résultant d’opérations dans une transaction se produisent correctement ensemble ou pas du tout.  
  
 Si vous annulez la transaction, ou si l’une de ses opérations échoue, le résultat sera comme si aucune des opérations de la transaction ne se produisait. La source de données restera telle qu’elle était avant le début de la transaction.  
  
 ADO fournit les méthodes suivantes pour contrôler les transactions : **BeginTrans**, **CommitTrans** et **RollbackTrans**. Utilisez ces méthodes avec un objet de **connexion** lorsque vous souhaitez enregistrer ou annuler une série de modifications apportées aux données sources en tant qu’unité unique. Par exemple, pour transférer de l’argent entre les comptes, vous devez soustraire un montant d’un et ajouter le même montant à l’autre. Si l’une des mises à jour échoue, les comptes ne sont plus équilibrés. L’apport de ces modifications dans une transaction ouverte garantit que toutes les modifications ou aucune d’entre elles sont effectuées.  
  
> [!NOTE]
>  Tous les fournisseurs ne prennent pas en charge les transactions. Vérifiez que la propriété «**transaction DDL**» définie par le fournisseur apparaît dans la collection de [Propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) de l’objet de **connexion** , ce qui indique que le fournisseur prend en charge les transactions. Si le fournisseur ne prend pas en charge les transactions, l’appel de l’une de ces méthodes renverra une erreur.  
  
 Une fois que vous avez appelé la méthode **BeginTrans** , le fournisseur ne valide plus instantanément les modifications apportées jusqu’à ce que vous appeliez **CommitTrans** ou **RollbackTrans** pour terminer la transaction.  
  
 L’appel de la méthode **CommitTrans** enregistre les modifications apportées dans une transaction ouverte sur la connexion et met fin à la transaction. L’appel de la méthode **RollbackTrans** inverse toutes les modifications apportées dans une transaction ouverte et met fin à la transaction. L’appel de l’une ou l’autre méthode quand aucune transaction n’est ouverte génère une erreur.  
  
 En fonction de la propriété [attributes](../../../ado/reference/ado-api/attributes-property-ado.md) de l’objet de **connexion** , l’appel de la méthode **CommitTrans** ou **RollbackTrans** peut démarrer automatiquement une nouvelle transaction. Si la propriété **attributes** a la valeur **adXactCommitRetaining**, le fournisseur démarre automatiquement une nouvelle transaction après un appel **CommitTrans** . Si la propriété **attributes** a la valeur **adXactAbortRetaining**, le fournisseur démarre automatiquement une nouvelle transaction après un appel **RollbackTrans** .  
  
## <a name="transaction-isolation-level"></a>Niveau d’isolation de la transaction  
 Utilisez la propriété **IsolationLevel** pour définir le niveau d’isolation d’une transaction sur un objet de **connexion** . Le paramètre ne prend pas effet avant la prochaine fois que vous appelez la méthode [BeginTrans](../../../ado/reference/ado-api/begintrans-committrans-and-rollbacktrans-methods-ado.md) . Si le niveau d’isolation demandé n’est pas disponible, le fournisseur peut retourner le niveau d’isolation supérieur suivant. Pour plus d’informations sur les valeurs valides, reportez-vous à la propriété **IsolationLevel** dans le Guide de référence du programmeur ADO.  
  
## <a name="nested-transactions"></a>Transactions imbriquées  
 Pour les fournisseurs qui prennent en charge les transactions imbriquées, l’appel de la méthode **BeginTrans** dans une transaction ouverte démarre une nouvelle transaction imbriquée. La valeur de retour indique le niveau d’imbrication : une valeur de retour de « 1 » indique que vous avez ouvert une transaction de niveau supérieur (c’est-à-dire que la transaction n’est pas imbriquée dans une autre transaction), « 2 » indique que vous avez ouvert une transaction de second niveau (une transaction imbriquée dans une transaction de niveau supérieur), et ainsi de suite. L’appel de **CommitTrans** ou **RollbackTrans** affecte uniquement la dernière transaction ouverte ; vous devez fermer ou restaurer la transaction en cours avant de pouvoir résoudre les transactions de niveau supérieur.
