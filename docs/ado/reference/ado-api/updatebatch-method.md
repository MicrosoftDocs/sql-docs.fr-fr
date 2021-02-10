---
description: UpdateBatch, méthode
title: UpdateBatch, méthode | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::UpdateBatch
- Recordset15::raw_UpdateBatch
helpviewer_keywords:
- UpdateBatch method [ADO]
ms.assetid: 23f9314c-b027-4a51-aeae-50caa2977740
author: rothja
ms.author: jroth
ms.openlocfilehash: 4e754d71d3b616ebd8b44af399c6386e940c8ada
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056224"
---
# <a name="updatebatch-method"></a>UpdateBatch, méthode
Écrit toutes les mises à jour par lots en attente sur le disque.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.UpdateBatch AffectRecords, PreserveStatus  
```  
  
#### <a name="parameters"></a>Paramètres  
 *AffectRecords*  
 facultatif. Valeur [AffectEnum](./affectenum.md) qui indique le nombre d’enregistrements affectés par la méthode **UpdateBatch** .  
  
 *PreserveStatus*  
 facultatif. Valeur **booléenne** qui spécifie si les modifications locales, comme indiqué par la propriété [Status](./status-property-ado-recordset.md) , doivent être validées. Si cette valeur est définie sur **true**, la propriété **Status** de chaque enregistrement reste inchangée une fois la mise à jour terminée.  
  
## <a name="remarks"></a>Notes  
 Utilisez la méthode **UpdateBatch** lors de la modification d’un objet **Recordset** en mode de mise à jour par lot pour transmettre toutes les modifications apportées à un objet **Recordset** à la base de données sous-jacente.  
  
 Si l’objet **Recordset** prend en charge la mise à jour par lot, vous pouvez mettre en cache plusieurs modifications dans un ou plusieurs enregistrements localement jusqu’à ce que vous appeliez la méthode **UpdateBatch** . Si vous modifiez l’enregistrement en cours ou si vous ajoutez un nouvel enregistrement lorsque vous appelez la méthode **UpdateBatch** , ADO appelle automatiquement la méthode [Update](./update-method.md) pour enregistrer toutes les modifications en attente dans l’enregistrement en cours avant de transmettre les modifications par lot au fournisseur. Vous devez utiliser la mise à jour par lot avec un curseur de jeu de clés ou statique uniquement.  
  
> [!NOTE]
>  Si vous spécifiez **adAffectGroup** comme valeur pour ce paramètre, une erreur se produit lorsqu’il n’y a aucun enregistrement visible dans le **jeu d'** enregistrements actuel (par exemple, un filtre pour lequel aucun enregistrement ne correspond).  
  
 Si la tentative de transmission des modifications échoue pour un ou tous les enregistrements en raison d’un conflit avec les données sous-jacentes (par exemple, si un enregistrement a déjà été supprimé par un autre utilisateur), le fournisseur renvoie des avertissements à la collection d' [Erreurs](./errors-collection-ado.md) et une erreur d’exécution se produit. Utilisez la propriété [Filter](./filter-property.md) (**adFilterAffectedRecords**) et la propriété [Status](./status-property-ado-recordset.md) pour localiser les enregistrements avec conflits.  
  
 Pour annuler toutes les mises à jour par lots en attente, utilisez la méthode [CancelBatch](./cancelbatch-method-ado.md) .  
  
 Si les propriétés dynamiques [table unique](./unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) et [Update Resync](./update-resync-property-dynamic-ado.md) sont définies et que le **jeu d’enregistrements** est le résultat de l’exécution d’une opération de jointure sur plusieurs tables, l’exécution de la méthode **UpdateBatch** est implicitement suivie par la méthode [Resync](./resync-method.md) , selon les paramètres de la propriété [Update Resync](./update-resync-property-dynamic-ado.md) .  
  
 L’ordre dans lequel les mises à jour individuelles d’un lot sont effectuées sur la source de données n’est pas nécessairement le même que l’ordre dans lequel elles ont été exécutées sur le **jeu d’enregistrements** local. L’ordre des mises à jour dépend du fournisseur. Prenez cela en compte lors du codage des mises à jour liées les unes aux autres, telles que les contraintes de clé étrangère sur une insertion ou une mise à jour.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [UpdateBatch et CancelBatch, exemples de méthodes (VB)](./updatebatch-and-cancelbatch-methods-example-vb.md)   
 [UpdateBatch et CancelBatch, exemples de méthodes (VC + +)](./updatebatch-and-cancelbatch-methods-example-vc.md)   
 [Méthode CancelBatch (ADO)](./cancelbatch-method-ado.md)   
 [Clear, méthode (ADO)](./clear-method-ado.md)   
 [LockType, propriété (ADO)](./locktype-property-ado.md)   
 [Update, méthode](./update-method.md)