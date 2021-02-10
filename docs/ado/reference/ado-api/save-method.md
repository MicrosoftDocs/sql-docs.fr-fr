---
description: Save, méthode
title: Save, méthode | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Recordset::Save
- _Recordset::raw_Save
helpviewer_keywords:
- Save method [ADO]
ms.assetid: ed3d9678-5c28-4e61-8bb3-7dfb66d99cf5
author: rothja
ms.author: jroth
ms.openlocfilehash: a15cb79b3dc6a00e8096cec5289a5c8ce5304938
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051511"
---
# <a name="save-method"></a>Save, méthode
Enregistre le [Recordset](./recordset-object-ado.md) dans un objet de fichier ou de [flux](./stream-object-ado.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.Save Destination, PersistFormat  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Destination*  
 facultatif. **Variant** qui représente le nom de chemin d’accès complet du fichier dans lequel le **Recordset** doit être enregistré, ou une référence à un objet de **flux** .  
  
 *PersistFormat*  
 facultatif. Valeur [PersistFormatEnum](./persistformatenum.md) qui spécifie le format dans lequel l' **objet Recordset** doit être enregistré (XML ou ADTG). La valeur par défaut est **adPersistADTG**.  
  
## <a name="remarks"></a>Notes  
 La méthode **Save Method** ne peut être appelée que sur un **Recordset** ouvert. Utilisez la méthode [Open (ADO Recordset)](./open-method-ado-recordset.md) pour restaurer ultérieurement le **Recordset** de la *destination*.  
  
 Si la propriété Filter de la [propriété](./filter-property.md) est appliquée à l' **objet Recordset**, seules les lignes accessibles sous le filtre sont enregistrées. Si le **jeu d’enregistrements** est hiérarchique, le **jeu d’enregistrements** enfant actuel et ses enfants sont enregistrés, y compris le **Recordset** parent. Si la méthode Save d’un **jeu d’enregistrements** enfant est appelée, l’enfant et tous ses enfants sont enregistrés, mais le parent n’est pas.  
  
 La première fois que vous enregistrez le **Recordset**, il est facultatif de spécifier la *destination*. Si vous omettez la *destination*, un nouveau fichier est créé avec un nom défini sur la valeur de la propriété source du **Recordset**.  
  
 Omettez la *destination* lorsque vous appelez ensuite **Save** après le premier enregistrement, ou une erreur d’exécution se produit. Si, par la suite, vous appelez **Save** avec une nouvelle *destination*, le **jeu d’enregistrements** est enregistré dans la nouvelle destination. Toutefois, la nouvelle destination et la destination d’origine sont toutes deux ouvertes.  
  
 L' **enregistrement** ne ferme pas le **jeu d’enregistrements** ou la *destination*. vous pouvez donc continuer à utiliser le **Recordset** et enregistrer les modifications les plus récentes. La *destination* reste ouverte jusqu’à la fermeture du **Recordset** .  
  
 Pour des raisons de sécurité, la méthode **Save** autorise uniquement l’utilisation de paramètres de sécurité bas et personnalisés à partir d’un script exécuté par Microsoft Internet Explorer.  
  
 Si la méthode **Save** est appelée alors qu’une opération d’extraction, d’exécution ou de mise à jour d’un **jeu d’enregistrements** asynchrone est en cours, **Save** attend jusqu’à ce que l’opération asynchrone soit terminée.  
  
 Les enregistrements sont enregistrés à partir de la première ligne de l’ensemble d' **enregistrements**. Lorsque la méthode **Save** est terminée, la position de ligne actuelle est déplacée vers la première ligne du **Recordset**.  
  
 Pour de meilleurs résultats, affectez à la propriété [CursorLocation (ADO)](./cursorlocation-property-ado.md) la valeur **adUseClient** avec **Save**. Si votre fournisseur ne prend pas en charge toutes les fonctionnalités nécessaires pour enregistrer des objets **Recordset** , le service de curseur fournit cette fonctionnalité.  
  
 Lorsqu’un **jeu d’enregistrements** est conservé avec la propriété **CursorLocation** définie sur **adUseServer**, la fonctionnalité de mise à jour du **Recordset** est limitée. En règle générale, seules les mises à jour, les insertions et les suppressions sur une seule table sont autorisées (dépend de la fonctionnalité du fournisseur). La méthode [Resync](./resync-method.md) n’est pas non plus disponible dans cette configuration.  
  
> [!NOTE]
>  L’enregistrement d’un **jeu d’enregistrements** avec des **champs** de type **adVariant**, **adIDispatch** ou **adIUnknown** n’est pas pris en charge par ADO et peut entraîner des résultats imprévisibles.  
  
 Seuls les filtres sous la forme de chaînes de critères (par exemple, OrderDate > ' 12/31/1999 ') affectent le contenu d’un **jeu d’enregistrements** persistant. Les filtres créés à l’aide d’un tableau de **signets** ou de l’utilisation d’une valeur de [FilterGroupEnum](./filtergroupenum.md) n’affectent pas le contenu de l' **objet Recordset** persistant. Ces règles s’appliquent aux **jeux d’enregistrements** créés à l’aide de curseurs côté client ou côté serveur.  
  
 Étant donné que le paramètre de *destination* peut accepter tout objet qui prend en charge l’interface OLE DB IStream, vous pouvez enregistrer un **jeu d’enregistrements** directement dans l’objet de réponse ASP. Pour plus d’informations, consultez le **scénario de persistance d’un jeu d’enregistrements XML**.  
  
 Vous pouvez également enregistrer un **jeu d’enregistrements** au format XML dans une instance d’un objet DOM MSXML, comme indiqué dans le code Visual Basic suivant :  
  
```  
Dim xDOM As New MSXML.DOMDocument  
Dim rsXML As New ADODB.Recordset  
Dim sSQL As String, sConn As String  
  
sSQL = "SELECT customerid, companyname, contactname FROM customers"  
sConn="Provider=Microsoft.Jet.OLEDB.4.0;Data Source=Northwind.mdb"  
rsXML.Open sSQL, sConn  
rsXML.Save xDOM, adPersistXML   'Save Recordset directly into a DOM tree.  
...  
```  
  
> [!NOTE]
>  Deux limitations s’appliquent lors de l’enregistrement de recordsets hiérarchiques (formes de données) au format XML. Vous ne pouvez pas enregistrer dans XML si le **jeu d’enregistrements** hiérarchique contient des mises à jour en attente et que vous ne pouvez pas enregistrer un **jeu d’enregistrements** hiérarchique paramétré.  
  
 Un **jeu d’enregistrements** enregistré au format XML est enregistré à l’aide du format UTF-8. Lorsqu’un fichier de ce type est chargé dans un flux ADO, l’objet Stream ne tente pas d’ouvrir un **Recordset** à partir du flux, sauf si la propriété Charset du flux est définie sur la valeur appropriée pour le format UTF-8.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Recordset, objet (ADO)](./recordset-object-ado.md)  
    :::column-end:::
    :::column:::
        [Stream, objet (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [Save et Open, exemples de méthodes (VB)](./save-and-open-methods-example-vb.md)   
 [Save et Open, exemples de méthodes (VC + +)](./save-and-open-methods-example-vc.md)   
 [Open, méthode (objet Recordset ADO)](./open-method-ado-recordset.md)   
 [Open, méthode (objet Stream ADO)](./open-method-ado-stream.md)   
 [SaveToFile, méthode](./savetofile-method.md)