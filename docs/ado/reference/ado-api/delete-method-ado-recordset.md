---
description: Delete, méthode (objet Recordset ADO)
title: Delete, méthode (objet Recordset ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::raw_Delete
- Recordset15::Delete
helpviewer_keywords:
- Delete method [ADO]
ms.assetid: 1eb9209c-602c-4507-b0c2-6527a599b67d
author: rothja
ms.author: jroth
ms.openlocfilehash: 81caf2da7f342cc7c31d1e4cd2bbad5b83852e9a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034399"
---
# <a name="delete-method-ado-recordset"></a>Delete, méthode (objet Recordset ADO)
Supprime l’enregistrement en cours ou un groupe d’enregistrements.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.Delete AffectRecords  
```  
  
#### <a name="parameters"></a>Paramètres  
 *AffectRecords*  
 Valeur [AffectEnum](../../../ado/reference/ado-api/affectenum.md) qui détermine le nombre d’enregistrements affectés par la méthode **Delete** . La valeur par défaut est **adAffectCurrent**.  
  
> [!NOTE]
>  **adAffectAll** et **adAffectAllChapters** ne sont pas des arguments valides à **supprimer**.  
  
## <a name="remarks"></a>Notes  
 L’utilisation de la méthode **Delete** marque l’enregistrement en cours ou un groupe d’enregistrements dans un objet [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) pour suppression. Si l’objet **Recordset** n’autorise pas la suppression d’enregistrements, une erreur se produit. Si vous êtes en mode de mise à jour immédiate, les suppressions se produisent immédiatement dans la base de données. Si un enregistrement ne peut pas être correctement supprimé (en raison de violations d’intégrité de la base de données, par exemple), l’enregistrement reste en mode édition après l’appel à [Update](../../../ado/reference/ado-api/update-method.md). Cela signifie que vous devez annuler la mise à jour avec [CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md) avant de quitter l’enregistrement actif (par exemple, avec [Close](../../../ado/reference/ado-api/close-method-ado.md), [Move](../../../ado/reference/ado-api/move-method-ado.md)ou [NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md)).  
  
 Si vous êtes en mode de mise à jour par lot, les enregistrements sont marqués pour suppression dans le cache et la suppression réelle se produit lorsque vous appelez la méthode [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) . Utilisez la propriété [Filter](../../../ado/reference/ado-api/filter-property.md) pour afficher les enregistrements supprimés.  
  
 La récupération des valeurs de champ de l’enregistrement supprimé génère une erreur. Après la suppression de l’enregistrement actif, l’enregistrement supprimé reste actif jusqu’à ce que vous passiez à un autre enregistrement. Une fois que vous quittez l’enregistrement supprimé, il n’est plus accessible.  
  
 Si vous imbriquez des suppressions dans une transaction, vous pouvez récupérer les enregistrements supprimés avec la méthode [RollbackTrans](../../../ado/reference/ado-api/begintrans-committrans-and-rollbacktrans-methods-ado.md) . Si vous êtes en mode de mise à jour par lot, vous pouvez annuler une suppression en attente ou un groupe de suppressions en attente à l’aide de la méthode [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) .  
  
 Si la tentative de suppression d’enregistrements échoue en raison d’un conflit avec les données sous-jacentes (par exemple, un enregistrement a déjà été supprimé par un autre utilisateur), le fournisseur renvoie des avertissements à la collection d' [Erreurs](../../../ado/reference/ado-api/errors-collection-ado.md) , mais n’interrompt pas l’exécution du programme. Une erreur d’exécution se produit uniquement en cas de conflit sur tous les enregistrements demandés.  
  
 Si la propriété dynamique de la [table unique](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) est définie et que le **jeu d’enregistrements** est le résultat de l’exécution d’une opération de jointure sur plusieurs tables, la méthode **Delete** supprime uniquement les lignes de la table nommée dans la propriété [table unique](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) .  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Delete, exemple de méthode (VB)](../../../ado/reference/ado-api/delete-method-example-vb.md)   
 [Delete, exemple de méthode (VBScript)](../../../ado/reference/ado-api/delete-method-example-vbscript.md)   
 [Delete, exemple de méthode (VC + +)](../../../ado/reference/ado-api/delete-method-example-vc.md)   
 [Delete, méthode (collection Fields ADO)](../../../ado/reference/ado-api/delete-method-ado-fields-collection.md)   
 [Delete, méthode (collection Parameters ADO)](../../../ado/reference/ado-api/delete-method-ado-parameters-collection.md)   
 [DeleteRecord, méthode (ADO)](../../../ado/reference/ado-api/deleterecord-method-ado.md)
