---
description: CancelBatch, méthode (ADO)
title: Méthode CancelBatch (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::raw_CancelBatch
- Recordset15::CancelBatch
helpviewer_keywords:
- CancelBatch method [ADO]
ms.assetid: dbdc2574-e44e-4d95-b03d-4a5d9e9adf3c
author: rothja
ms.author: jroth
ms.openlocfilehash: edbafa81c787bb4dafed6688f43907a87504c3c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164737"
---
# <a name="cancelbatch-method-ado"></a>CancelBatch, méthode (ADO)
Annule une mise à jour par lot en attente.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.CancelBatchAffectRecords  
```  
  
#### <a name="parameters"></a>Paramètres  
 *AffectRecords*  
 Facultatif. Valeur [AffectEnum](./affectenum.md) qui indique le nombre d’enregistrements affectés par la méthode **CancelBatch** .  
  
## <a name="remarks"></a>Notes  
 Utilisez la méthode **CancelBatch** pour annuler les mises à jour en attente dans un [Recordset](./recordset-object-ado.md) en mode de mise à jour par lot. Si le **jeu d’enregistrements** est en mode de mise à jour immédiate, l’appel de **CancelBatch** sans **adAffectCurrent** génère une erreur.  
  
 Si vous modifiez l’enregistrement en cours ou si vous ajoutez un nouvel enregistrement lorsque vous appelez **CancelBatch**, ADO appelle d’abord la méthode [CancelUpdate](./cancelupdate-method-ado.md) pour annuler les modifications mises en cache. Après cela, toutes les modifications en attente dans le **jeu d’enregistrements** sont annulées.  
  
 L’enregistrement en cours peut ne pas être déterminable après un appel de **CancelBatch** , en particulier si vous étiez en train d’ajouter un nouvel enregistrement. Pour cette raison, il est prudent de définir la position de l’enregistrement actuel sur un emplacement connu dans le **jeu d’enregistrements** après l’appel de la fonction **CancelBatch** . Par exemple, appelez la méthode [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) .  
  
 Si la tentative d’annulation des mises à jour en attente échoue en raison d’un conflit avec les données sous-jacentes (par exemple, si un enregistrement a été supprimé par un autre utilisateur), le fournisseur renvoie des avertissements à la collection d' [Erreurs](./errors-collection-ado.md) , mais n’interrompt pas l’exécution du programme. Une erreur d’exécution se produit uniquement en cas de conflit sur tous les enregistrements demandés. Utilisez la propriété [Filter](./filter-property.md) (**adFilterAffectedRecords**) et la propriété [Status](./status-property-ado-recordset.md) pour localiser les enregistrements avec conflits.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [UpdateBatch et CancelBatch, exemples de méthodes (VB)](./updatebatch-and-cancelbatch-methods-example-vb.md)   
 [UpdateBatch et CancelBatch, exemples de méthodes (VC + +)](./updatebatch-and-cancelbatch-methods-example-vc.md)   
 [Cancel, méthode (ADO)](./cancel-method-ado.md)   
 [Cancel, méthode (RDS)](../rds-api/cancel-method-rds.md)   
 [CancelUpdate, méthode (ADO)](./cancelupdate-method-ado.md)   
 [CancelUpdate, méthode (RDS)](../rds-api/cancelupdate-method-rds.md)   
 [Clear, méthode (ADO)](./clear-method-ado.md)   
 [LockType, propriété (ADO)](./locktype-property-ado.md)   
 [UpdateBatch, méthode](./updatebatch-method.md)