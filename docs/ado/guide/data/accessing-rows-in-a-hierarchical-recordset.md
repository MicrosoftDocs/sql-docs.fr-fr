---
description: Accès aux lignes d’un jeu d’enregistrements hiérarchique (exemple)
title: Accès aux lignes d’un jeu d’enregistrements hiérarchique | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- hierarchical Recordsets [ADO]
- data shaping [ADO], hierarchical Recordsets
ms.assetid: 25f1d2a1-6d5e-4457-aa07-5db5c75dee18
author: rothja
ms.author: jroth
ms.openlocfilehash: 369d9ef5548a0ff30b08dc27c9df38b0f3c1fa1b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028634"
---
# <a name="accessing-rows-in-a-hierarchical-recordset-example"></a>Accès aux lignes d’un jeu d’enregistrements hiérarchique (exemple)
L’exemple suivant montre les étapes nécessaires pour accéder aux lignes d’un [jeu d’enregistrements](../../reference/ado-api/recordset-object-ado.md)hiérarchique :

1.  Les objets **Recordset** des tables **Authors** et **titleauthor** sont liés par l’ID de l’auteur.

2.  La boucle externe affiche le prénom et le nom, l’État et l’identification de chaque auteur.

3.  Le **Recordset** ajouté pour chaque ligne est extrait de la collection de [champs](../../reference/ado-api/fields-collection-ado.md) et affecté à *rstTitleAuthor*.

4.  La boucle interne affiche quatre champs de chaque ligne dans le **Recordset** ajouté.

 La propriété [StayInSync](../../reference/ado-api/stayinsync-property.md) a la valeur **false** à des fins d’illustration, afin que vous puissiez voir le chapitre changer explicitement dans chaque itération de la boucle externe. Pour améliorer l’efficacité de l’exemple de code, vous pouvez déplacer l’affectation à l’étape 3 avant la première ligne de l’étape 2, afin que l’assignation ne soit exécutée qu’une seule fois. Affectez ensuite la valeur **true** à la propriété [StayInSync](../../reference/ado-api/stayinsync-property.md) , afin que *rstTitleAuthor* passe implicitement et automatiquement au chapitre correspondant chaque fois que *RST* passe à une nouvelle ligne.

## <a name="example"></a>Exemple

```
Sub datashape()
   Dim cnn As New ADODB.Connection
   Dim rst As New ADODB.Recordset
   Dim rstTitleAuthor As New ADODB.Recordset

   cnn.Provider = "MSDataShape"
   cnn.Open    "Data Provider=MSDASQL;" & _
               "Data Source=SRV;Integrated Security=SSPI;Database=Pubs"
' STEP 1
   rst.StayInSync = FALSE
   rst.Open    "SHAPE  {select * from authors} "  & _
               "APPEND ({select * from titleauthor} " & _
               "RELATE au_id TO au_id) AS chapTitleAuthor", _
               cnn
' STEP 2
   While Not rst.EOF
      Debug.Print    rst("au_fname"), rst("au_lname"), _
                     rst("state"), rst("au_id")
' STEP 3
      Set rstTitleAuthor = rst("chapTitleAuthor").Value
' STEP 4
      While Not rstTitleAuthor.EOF
         Debug.Print rstTitleAuthor(0), rstTitleAuthor(1), _
                     rstTitleAuthor(2), rstTitleAuthor(3)
         rstTitleAuthor.MoveNext
      Wend
      rst.MoveNext
   Wend
End Sub
```

## <a name="see-also"></a>Voir aussi
 [Vue d’ensemble](./data-shaping-overview.md) de la mise en forme des données collection de champs d' [objets de champ](../../reference/ado-api/field-object.md) [(ADO)](../../reference/ado-api/fields-collection-ado.md) [grammaire de forme formelle](./formal-shape-grammar.md) [Microsoft Data Shaping Service pour OLE DB (ADO Service Provider)](../appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) [objet Recordset (ADO)](../../reference/ado-api/recordset-object-ado.md) [fournisseurs requis pour](./required-providers-for-data-shaping.md) la mise en forme des données commande de la forme [Ajouter](./shape-append-clause.md) une clause [dans](./shape-commands-in-general.md) la [clause COMPUTE Shape](./shape-compute-clause.md) [Visual Basic pour applications Functions](./visual-basic-for-applications-functions.md)