---
description: Fields, collection
title: La collection Fields | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Field object [ADO], fields collection
- Fields collection [ADO]
ms.assetid: 574cf36e-e5f5-403b-983c-749ef93c108f
author: rothja
ms.author: jroth
ms.openlocfilehash: 7dee12a0cdb274f58bb3dc79fddeb057e4913e81
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032411"
---
# <a name="the-fields-collection"></a>Fields, collection
La collection **Fields** est l’une des collections intrinsèques d’ADO. Une collection est un ensemble ordonné d’éléments qui peuvent être référencés sous la forme d’une unité. Pour plus d’informations sur les collections ADO, consultez [le modèle objet ADO](../../../ado/guide/data/ado-objects-and-collections.md).  
  
 La collection **Fields** contient un objet **Field** pour chaque champ (colonne) dans le **Recordset**. Comme toutes les collections ADO, elle comporte des propriétés **Count** et **Item** , ainsi que des méthodes d' **Ajout** et d' **actualisation** . Il comporte également des méthodes **CancelUpdate**, **Delete**, **Resync** et **Update** , qui ne sont pas disponibles pour d’autres collections ADO.  
  
## <a name="examining-the-fields-collection"></a>Examen de la collection Fields  
 Examinez la collection de **champs** de l’exemple **d’objet Recordset** présenté dans cette section. L’exemple **d’objet Recordset** a été dérivé de l’instruction SQL  
  
```  
SELECT ProductID, ProductName, UnitPrice FROM Products WHERE CategoryID = 7  
```  
  
 Par conséquent, vous devez trouver que la collection de **champs du Recordset** contient trois champs.  
  
```  
'BeginWalkFields  
    Dim objFields As ADODB.Fields  
    Dim intLoop As Integer  
  
    objRs.Open strSQL, strConnStr, adOpenForwardOnly, adLockReadOnly, adCmdText  
  
    Set objFields = objRs.Fields  
  
    For intLoop = 0 To (objFields.Count - 1)  
        Debug.Print objFields.Item(intLoop).Name  
    Next  
'EndWalkFields  
```  
  
 Ce code détermine simplement le nombre d' **objets Field** dans la collection **Fields** à l’aide de la propriété **Count** et effectue une boucle dans la collection, en retournant la valeur de la propriété **Name** de chaque objet **Field** . Vous pouvez utiliser beaucoup plus de propriétés de **champ** pour obtenir des informations sur un champ. Pour plus d’informations sur l’interrogation d’un **champ**, consultez [l’objet Field](../../../ado/guide/data/the-field-object.md).  
  
## <a name="counting-columns"></a>Compter les colonnes  
 Comme vous pouvez vous y attendre, la propriété **Count** retourne le nombre réel d’objets **Field** dans la collection **Fields** . Étant donné que la numérotation des membres d’une collection commence par zéro, vous devez toujours coder les boucles en commençant par le membre zéro et en terminant par la valeur de la propriété **Count** moins 1. Si vous utilisez Microsoft Visual Basic et souhaitez parcourir les membres d’une collection sans vérifier la propriété Count, utilisez l' **argument** **for each... Commande suivante** .  
  
 Si la propriété **Count** est égale à zéro, la collection ne contient aucun objet.  
  
## <a name="getting-to-the-field"></a>Accès au champ  
 Comme pour toute collection ADO, la propriété **Item** est la propriété par défaut de la collection. Elle retourne l’objet de **champ** individuel spécifié par le nom ou l’index qui lui est passé. Par conséquent, les instructions suivantes sont équivalentes à l’exemple de **jeu d’enregistrements**:  
  
```  
objField = objRecordset.Fields.Item("ProductID")  
objField = objRecordset.Fields("ProductID")  
objField = objRecordset.Fields.Item(0)  
objField = objRecordset.Fields(0)  
```  
  
 Si ces méthodes sont équivalentes, qui est la meilleure ? Cela dépend. L’utilisation d’un index pour récupérer un **champ** de la collection est plus rapide, car elle accède directement au **champ** sans avoir à effectuer une recherche de chaîne. D’un autre côté, l’ordre des **champs** dans la collection doit être connu, et si l’ordre change, la référence à l’index **du champ** devra être changée où qu’il se produise. Bien qu’un peu plus lentement, l’utilisation du nom du **champ** est plus flexible, car il ne dépend pas de l’ordre des **champs** de la collection.  
  
## <a name="using-the-refresh-method"></a>Utilisation de la méthode Refresh  
 Contrairement à d’autres collections ADO, l’utilisation de la méthode **Refresh** sur la collection **Fields** n’a aucun effet visible. Pour récupérer les modifications de la structure de base de données sous-jacente, vous devez utiliser la méthode **Requery** ou, si l’objet **Recordset** ne prend pas en charge les signets, la méthode **MoveFirst** , qui provoquera l’exécution de la commande sur le fournisseur.  
  
## <a name="adding-fields-to-a-recordset"></a>Ajout de champs à un Recordset  
 La méthode **Append** est utilisée pour ajouter des champs à un **Recordset**.  
  
 Vous pouvez utiliser la méthode **Append** pour fabriquer un **jeu d’enregistrements** par programmation sans ouvrir de connexion à une source de données. Une erreur d’exécution se produit si la méthode **Append** est appelée sur la collection **Fields** d’un **Recordset** ouvert ou sur un **Recordset** dans lequel la propriété **ActiveConnection** a été définie. Vous pouvez ajouter des champs uniquement à un **jeu d’enregistrements** qui n’est pas ouvert et qui n’a pas encore été connecté à une source de données. Toutefois, pour spécifier des valeurs pour les **champs** nouvellement ajoutés, vous devez d’abord ouvrir le **jeu d’enregistrements** .  
  
 Les développeurs ont souvent besoin d’un emplacement pour stocker temporairement des données ou souhaitent que certaines données agissent comme si elles provenaient d’un serveur afin de pouvoir participer à la liaison de données dans une interface utilisateur. ADO (conjointement avec le [service de curseur Microsoft pour OLE DB](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md)) permet au développeur de créer un objet **Recordset** vide en spécifiant les informations de colonne et en appelant **Open**. Dans l’exemple suivant, trois nouveaux champs sont ajoutés à un nouvel objet **Recordset** . Le **jeu d'** enregistrements est ensuite ouvert, deux nouveaux enregistrements sont ajoutés et le **Recordset** est conservé dans un fichier. (Pour plus d’informations sur la persistance des **jeux d’enregistrements** , consultez [mise à jour et persistance des données](../../../ado/guide/data/updating-and-persisting-data.md).)  
  
```  
'BeginFabricate  
    Dim objRs As ADODB.Recordset  
    Set objRs = New ADODB.Recordset  
  
    With objRs.Fields  
        .Append "StudentID", adChar, 11, adFldUpdatable  
        .Append "FullName", adVarChar, 50, adFldUpdatable  
        .Append "PhoneNmbr", adVarChar, 20, adFldUpdatable  
    End With  
  
    With objRs  
        .Open  
  
        .AddNew  
        .Fields(0) = "123-45-6789"  
        .Fields(1) = "John Doe"  
        .Fields(2) = "(425) 555-5555"  
        .Update  
  
        .AddNew  
        .Fields(0) = "123-45-6780"  
        .Fields(1) = "Jane Doe"  
        .Fields(2) = "(615) 555-1212"  
        .Update  
    End With  
  
    objRs.Save App.Path & "FabriTest.adtg", adPersistADTG  
  
    objRs.Close  
'EndFabricate  
```  
  
 L’utilisation de la méthode **Append des champs** diffère entre l’objet **Recordset** et l’objet **Record** . Pour plus d’informations sur l’objet **Record** , consultez [enregistrements et flux](../../../ado/guide/data/records-and-streams.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Fabrication de recordsets hiérarchiques](../../../ado/guide/data/fabricating-hierarchical-recordsets.md)
