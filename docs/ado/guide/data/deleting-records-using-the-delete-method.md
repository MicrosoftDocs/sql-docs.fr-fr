---
description: Suppression d’enregistrements avec la méthode Delete
title: Suppression d’enregistrements à l’aide de la méthode Delete | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, deleting records
- deleting records [ADO]
- editing data [ADO], Delete method
- Delete method [ADO]
ms.assetid: bfed5cfa-7f57-463b-9da2-0c612a079d30
author: rothja
ms.author: jroth
ms.openlocfilehash: 43695feb469483bfe1b3a7d9e629adb7d2f7d4e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033259"
---
# <a name="deleting-records-using-the-delete-method"></a>Suppression d’enregistrements avec la méthode Delete
L’utilisation de la méthode **Delete** marque l’enregistrement en cours ou un groupe d’enregistrements dans un objet **Recordset** pour suppression. Si l’objet **Recordset** n’autorise pas la suppression d’enregistrements, une erreur se produit. Si vous êtes en mode de mise à jour immédiate, les suppressions se produisent immédiatement dans la base de données. Si un enregistrement ne peut pas être correctement supprimé (en raison de violations d’intégrité de la base de données, par exemple), l’enregistrement reste en mode édition après l’appel à **Update.** Cela signifie que vous devez annuler la mise à jour à l’aide de [CancelUpdate](../../reference/ado-api/cancelupdate-method-ado.md) avant de quitter l’enregistrement actif (par exemple, à l’aide de la méthode [Close](../../reference/ado-api/close-method-ado.md), [Move](../../reference/ado-api/move-method-ado.md)ou [NextRecordset](../../reference/ado-api/nextrecordset-method-ado.md)).  
  
 Si vous êtes en mode de mise à jour par lot, les enregistrements sont marqués pour suppression dans le cache et la suppression réelle se produit lorsque vous appelez la méthode **UpdateBatch** . (Pour afficher les enregistrements supprimés, affectez à la propriété **Filter** la valeur **adFilterAffectedRecords** après l’appel de la **méthode Delete** .)  
  
 Toute tentative d’extraction des valeurs de champ de l’enregistrement supprimé génère une erreur. Après la suppression de l’enregistrement actif, l’enregistrement supprimé reste actif jusqu’à ce que vous passiez à un autre enregistrement. Une fois que vous quittez l’enregistrement supprimé, il n’est plus accessible.  
  
 Si vous imbriquez des suppressions dans une transaction, vous pouvez récupérer les enregistrements supprimés à l’aide de la méthode **RollbackTrans** . Si vous êtes en mode de mise à jour par lot, vous pouvez annuler une suppression en attente ou un groupe de suppressions en attente à l’aide de la méthode **CancelBatch** .  
  
 Si la tentative de suppression d’enregistrements échoue en raison d’un conflit avec les données sous-jacentes (par exemple, un enregistrement a déjà été supprimé par un autre utilisateur), le fournisseur renvoie des avertissements à la collection d' **Erreurs** , mais n’interrompt pas l’exécution du programme. Une erreur d’exécution se produit uniquement en cas de conflit sur tous les enregistrements demandés.  
  
 Si la propriété dynamique de la **table unique** est définie et que le **jeu d’enregistrements** est le résultat de l’exécution d’une opération de jointure sur plusieurs tables, la méthode **Delete** supprime uniquement les lignes de la table nommée dans la propriété **table unique** .  
  
 La méthode **Delete** accepte un argument facultatif qui vous permet de spécifier les enregistrements affectés par l’opération de **suppression** . Les seules valeurs valides pour cet argument sont l’une des constantes énumérées de l’objet ADO **AffectEnum** suivantes :  
  
-   **adAffectCurrent** Affecte uniquement l’enregistrement actif.  
  
-   **adAffectGroup** Affecte uniquement les enregistrements qui répondent au paramètre de propriété de **filtre** actuel. La propriété **Filter** doit être définie sur une valeur **FilterGroupEnum** ou un tableau de **signets** pour utiliser cette option.  
  
 Le code suivant illustre un exemple de spécification de **adAffectGroup** lors de l’appel de la méthode **Delete** . Cet exemple ajoute des enregistrements à l’exemple **d’objet Recordset** et met à jour la base de données. Ensuite, il filtre le **Recordset** à l’aide de la constante énumérée de filtre **adFilterAffectedRecords** , qui laisse uniquement les enregistrements récemment ajoutés dans le **jeu d’enregistrements.** Enfin, il appelle la méthode **Delete** et spécifie que tous les enregistrements qui répondent au paramètre de propriété de **filtre** actuel (les nouveaux enregistrements) doivent être supprimés.  
  
```  
'BeginDeleteGroup  
    'add some bogus records  
    With objRs  
        For i = 0 To 8  
            .AddNew  
            .Fields("CompanyName") = "Shipper Number " & i + 1  
            .Fields("Phone") = "(425) 555-000" & (i + 1)  
            .Update  
        Next i  
  
        ' update  
        .UpdateBatch  
  
        'filter on newly added records  
        .Filter = adFilterAffectedRecords  
        Debug.Print "Deleting the " & .RecordCount & _  
                    " records you just added."  
  
        'delete the newly added bogus records  
        .Delete adAffectGroup  
        .Filter = adFilterNone  
        Debug.Print .RecordCount & " records remain."  
    End With  
'EndDeleteGroup  
```