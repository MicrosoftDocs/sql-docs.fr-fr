---
description: Persistance des données
title: Persistance des données | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- persisting data [ADO]
- data updates [ADO], persisting data
- data persistence [ADO]
- updating data [ADO], persisting data
ms.assetid: 21c162ca-2845-4dd8-a49d-e715aba8c461
author: rothja
ms.author: jroth
ms.openlocfilehash: f9a2ad809efbc847a2dfaf2292a15c1c7376e9b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037209"
---
# <a name="persisting-data"></a>Persistance des données
L’informatique portable (par exemple, à l’aide d’ordinateurs portables) a généré le besoin d’applications qui peuvent s’exécuter dans un état connecté et déconnecté. ADO a ajouté la prise en charge pour cela en donnant au développeur la possibilité d’enregistrer un **jeu d’enregistrements** de curseur client sur le disque et de le recharger ultérieurement.  
  
 Il existe plusieurs scénarios dans lesquels vous pouvez utiliser ce type de fonctionnalité, y compris les éléments suivants :  
  
-   En **déplacement :** Lorsque vous prenez l’application en déplacement, il est essentiel de fournir la possibilité d’apporter des modifications et d’ajouter de nouveaux enregistrements qui peuvent ensuite être reconnectés à la base de données ultérieurement et validés.  
  
-   **Recherches rarement mises à jour :** Souvent, dans une application, les tables sont utilisées comme recherches, par exemple les tables de taxes d’État. Ils sont rarement mis à jour et sont en lecture seule. Au lieu de relire ces données à partir du serveur chaque fois que l’application est démarrée, l’application peut simplement charger les données à partir d’un **Recordset** conservé localement.  
  
 Dans ADO, pour enregistrer et charger des **jeux d’enregistrements**, utilisez les méthodes **Recordset. Save** et **recordset. Open (,,,, adCmdFile)** sur l’objet **Recordset** ADO.  
  
 Vous pouvez utiliser la méthode **Save Recordset** pour conserver votre **jeu d’enregistrements** ADO dans un fichier sur un disque. (Vous pouvez également enregistrer un **Recordset** dans un objet de **flux** ADO. Les objets de **flux** sont présentés plus loin dans ce guide.) Plus tard, vous pouvez utiliser la méthode **Open** pour rouvrir le **Recordset** lorsque vous êtes prêt à l’utiliser. Par défaut, ADO enregistre l' **objet Recordset** au format Microsoft Advanced Data TABLEGRAM (ADTG) propriétaire. Ce format binaire est spécifié à l’aide de la valeur **AdPersistADTG PersistFormatEnum** . Vous pouvez également choisir d’enregistrer votre **jeu d’enregistrements** au format XML à la place à l’aide de **adPersistXML**. Pour plus d’informations sur l’enregistrement des jeux d’enregistrements au format XML, consultez [persistance des enregistrements au format XML](../../../ado/guide/data/persisting-records-in-xml-format.md).  
  
 La syntaxe de la méthode **Save** est la suivante :  
  
```  
  
recordset.  
Save  
Destination, PersistFormat  
  
```  
  
 La première fois que vous enregistrez le **Recordset**, il est facultatif de spécifier la *destination*. Si vous omettez la *destination*, un nouveau fichier est créé avec un nom défini sur la valeur de la propriété [source](../../../ado/reference/ado-api/source-property-ado-recordset.md) du **Recordset**.  
  
 Omettez la *destination* lorsque vous appelez ensuite **Save** après le premier enregistrement ou une erreur d’exécution se produit. Si, par la suite, vous appelez **Save** avec une nouvelle *destination*, le **jeu d’enregistrements** est enregistré dans la nouvelle destination. Toutefois, la nouvelle destination et la destination d’origine sont toutes deux ouvertes.  
  
 L' **enregistrement** ne ferme pas le **jeu d’enregistrements** ou la *destination*. vous pouvez donc continuer à utiliser le **Recordset** et enregistrer les modifications les plus récentes. La *destination* reste ouverte jusqu’à la fermeture de l’ensemble **d’enregistrements** , pendant laquelle les autres applications peuvent lire, mais pas écrire dans la *destination*.  
  
 Pour des raisons de sécurité, la méthode **Save** autorise uniquement l’utilisation de paramètres de sécurité bas et personnalisés à partir d’un script exécuté par Microsoft Internet Explorer.  
  
 Si la méthode **Save** est appelée alors qu’une opération d’extraction, d’exécution ou de mise à jour d’un **jeu d’enregistrements** asynchrone est en cours, **Save** attend jusqu’à ce que l’opération asynchrone soit terminée.  
  
 Les enregistrements sont enregistrés à partir de la première ligne de l’ensemble d' **enregistrements**. Lorsque la méthode **Save** est terminée, la position de ligne actuelle est déplacée vers la première ligne du **Recordset**.  
  
 Pour de meilleurs résultats, affectez à la propriété [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) la valeur **adUseClient** avec **Save**. Si votre fournisseur ne prend pas en charge toutes les fonctionnalités nécessaires pour enregistrer des objets **Recordset** , le service de curseur fournit cette fonctionnalité.  
  
 Lorsqu’un **jeu d’enregistrements** est conservé avec la propriété **CursorLocation** définie sur **adUseServer**, la fonctionnalité de mise à jour du **Recordset** est limitée. En règle générale, seules les mises à jour, les insertions et les suppressions dans une seule table sont autorisées (selon les fonctionnalités du fournisseur). La méthode [Resync](../../../ado/reference/ado-api/resync-method.md) n’est pas non plus disponible dans cette configuration.  
  
 Étant donné que le paramètre de *destination* peut accepter tout objet qui prend en charge l’interface OLE DB **IStream** , vous pouvez enregistrer un **jeu d’enregistrements** directement dans l’objet de **réponse** ASP.  
  
 Dans l’exemple suivant, les méthodes **Save** et **Open** sont utilisées pour conserver un **Recordset** et le rouvrir ultérieurement :  
  
```  
'BeginPersist  
   conn.ConnectionString = _  
   "Provider=SQLOLEDB;Data Source=MySQLServer;" _  
      & "Integrated Security=SSPI;Initial Catalog=pubs"  
   conn.Open  
  
   conn.Execute "create table testtable (dbkey int " & _  
      "primary key, field1 char(10))"  
   conn.Execute "insert into testtable values (1, 'string1')"  
  
   Set rst.ActiveConnection = conn  
   rst.CursorLocation = adUseClient  
  
   rst.Open "select * from testtable", conn, adOpenStatic, _  
      adLockBatchOptimistic  
  
   'Change the row on the client  
   rst!field1 = "NewValue"  
  
   'Save to a file--the .dat extension is an example; choose  
   'your own extension. The changes will be saved in the file  
   'as well as the original data.  
   MyFile = Dir("c:\temp\temptbl.dat")  
   If MyFile <> "" Then  
       Kill "c:\temp\temptbl.dat"  
   End If  
  
   rst.Save "c:\temp\temptbl.dat", adPersistADTG  
   Set rst = Nothing  
  
   'Now reload the data from the file  
   Set rst = New ADODB.Recordset  
   rst.Open "c:\temp\temptbl.dat", , adOpenStatic, _  
      adLockBatchOptimistic, adCmdFile  
  
   Debug.Print "After Loading the file from disk"  
   Debug.Print "   Current Edited Value: " & rst!field1.Value  
   Debug.Print "   Value Before Editing: " & rst!field1.OriginalValue  
  
   'Note that you can reconnect to a connection and  
   'submit the changes to the data source  
   Set rst.ActiveConnection = conn  
   rst.UpdateBatch  
'EndPersist  
```  
  
## <a name="remarks"></a>Notes  
 Cette section contient les rubriques suivantes :  
  
-   [En savoir plus sur la persistance des recordsets](../../../ado/guide/data/more-about-recordset-persistence.md)  
  
-   [Persistance des recordsets filtrés et hiérarchiques](../../../ado/guide/data/persisting-filtered-and-hierarchical-recordsets.md)  
  
-   [Persistance des enregistrements au format XML](../../../ado/guide/data/persisting-records-in-xml-format.md)
