---
description: BeginTrans, CommitTrans et RollbackTrans, méthodes (ADO)
title: BeginTrans, CommitTrans et RollbackTrans, méthodes (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::raw_RollbackTrans
- Connection15::CommitTrans
- Connection15::raw_CommitTrans
- Connection15::raw_BeginTrans
- Connection15::BeginTrans
- Connection15::RollbackTrans
helpviewer_keywords:
- BeginTrans method [ADO]
- CommitTrans method [ADO]
- RollbackTrans method [ADO]
ms.assetid: d4683472-4120-4236-8640-fa9ae289e23e
author: rothja
ms.author: jroth
ms.openlocfilehash: 7b8eca71f9b9a17b026a084a51c0f02b63e028a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035579"
---
# <a name="begintrans-committrans-and-rollbacktrans-methods-ado"></a>BeginTrans, CommitTrans et RollbackTrans, méthodes (ADO)
Ces méthodes de transaction gèrent le traitement des transactions dans un objet de [connexion](./connection-object-ado.md) comme suit :  
  
-   **BeginTrans** Commence une nouvelle transaction.  
  
-   **CommitTrans** Enregistre les modifications et met fin à la transaction en cours. Elle peut également démarrer une nouvelle transaction.  
  
-   **RollbackTrans** Annule toutes les modifications apportées pendant la transaction en cours et met fin à la transaction. Elle peut également démarrer une nouvelle transaction.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
level = object.BeginTrans()  
object.BeginTrans  
object.CommitTrans  
object.RollbackTrans  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **BeginTrans** peut être appelée comme une fonction qui retourne une variable de **type long** indiquant le niveau d’imbrication de la transaction.  
  
#### <a name="parameters"></a>Paramètres  
 *object*  
 Objet de **connexion** .  
  
## <a name="connection"></a>Connexion  
 Utilisez ces méthodes avec un objet de **connexion** lorsque vous souhaitez enregistrer ou annuler une série de modifications apportées aux données sources en tant qu’unité unique. Par exemple, pour transférer de l’argent entre les comptes, vous devez soustraire un montant d’un et ajouter le même montant à l’autre. Si l’une des mises à jour échoue, les comptes ne sont plus équilibrés. L’apport de ces modifications dans une transaction ouverte garantit que toutes les modifications ou aucune d’entre elles sont effectuées.  
  
> [!NOTE]
>  Tous les fournisseurs ne prennent pas en charge les transactions. Vérifiez que la propriété «**transaction DDL**» définie par le fournisseur apparaît dans la collection de [Propriétés](./properties-collection-ado.md) de l’objet de **connexion** , ce qui indique que le fournisseur prend en charge les transactions. Si le fournisseur ne prend pas en charge les transactions, l’appel de l’une de ces méthodes renverra une erreur.  
  
 Une fois que vous avez appelé la méthode **BeginTrans** , le fournisseur ne valide plus instantanément les modifications apportées jusqu’à ce que vous appeliez **CommitTrans** ou **RollbackTrans** pour terminer la transaction.  
  
 Pour les fournisseurs qui prennent en charge les transactions imbriquées, l’appel de la méthode **BeginTrans** dans une transaction ouverte démarre une nouvelle transaction imbriquée. La valeur de retour indique le niveau d’imbrication : une valeur de retour de « 1 » indique que vous avez ouvert une transaction de niveau supérieur (c’est-à-dire que la transaction n’est pas imbriquée dans une autre transaction), « 2 » indique que vous avez ouvert une transaction de second niveau (une transaction imbriquée dans une transaction de niveau supérieur), et ainsi de suite. L’appel de **CommitTrans** ou **RollbackTrans** affecte uniquement la dernière transaction ouverte ; vous devez fermer ou restaurer la transaction en cours avant de pouvoir résoudre les transactions de niveau supérieur.  
  
 L’appel de la méthode **CommitTrans** enregistre les modifications apportées dans une transaction ouverte sur la connexion et met fin à la transaction. L’appel de la méthode **RollbackTrans** inverse toutes les modifications apportées dans une transaction ouverte et met fin à la transaction. L’appel de l’une ou l’autre méthode quand aucune transaction n’est ouverte génère une erreur.  
  
 En fonction de la propriété [attributes](./attributes-property-ado.md) de l’objet de **connexion** , l’appel des méthodes **CommitTrans** ou **RollbackTrans** peut démarrer automatiquement une nouvelle transaction. Si la propriété **attributes** a la valeur **adXactCommitRetaining**, le fournisseur démarre automatiquement une nouvelle transaction après un appel **CommitTrans** . Si la propriété **attributes** a la valeur **adXactAbortRetaining**, le fournisseur démarre automatiquement une nouvelle transaction après un appel **RollbackTrans** .  
  
## <a name="remote-data-service"></a>Service de données à distance  
 Les méthodes **BeginTrans**, **CommitTrans** et **RollbackTrans** ne sont pas disponibles sur un objet de **connexion** côté client.  
  
## <a name="applies-to"></a>S'applique à  
 [Connection, objet (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [BeginTrans, CommitTrans et RollbackTrans, exemples de méthodes (VB)](./begintrans-committrans-and-rollbacktrans-methods-example-vb.md)   
 [BeginTrans, CommitTrans et RollbackTrans, exemples de méthodes (VC + +)](./begintrans-committrans-and-rollbacktrans-methods-example-vc.md)   
 [Attributes, propriété (ADO)](./attributes-property-ado.md)