---
description: Présentation de Microsoft OLE DB simple Provider
title: Fournisseur Microsoft OLE DB simple | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- simple provider [ADO]
- providers [ADO], OLE DB simple provider
- OLE DB simple provider [ADO]
ms.assetid: 1e7dc6f0-482c-4103-8187-f890865e40fc
author: rothja
ms.author: jroth
ms.openlocfilehash: a07224d352f059b8aad3709c6d4f9605c16696b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029233"
---
# <a name="microsoft-ole-db-simple-provider-overview"></a>Présentation de Microsoft OLE DB simple Provider
Le fournisseur Microsoft OLE DB simple (OSP) permet à ADO d’accéder à toutes les données pour lesquelles un fournisseur a été écrit à l’aide de [OLE DB boîte à outils OSP (simple Provider)](/previous-versions/windows/desktop/ms715822(v=vs.85)). Les fournisseurs simples sont destinés à accéder aux sources de données qui nécessitent uniquement une prise en charge fondamentale de la OLE DB, telles que les tableaux en mémoire ou les documents XML.

## <a name="connection-string-parameters"></a>Paramètres de chaîne de connexion
 Pour vous connecter au OLE DB DLL de fournisseur simple, affectez à l’argument *Provider* la valeur de la propriété [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) :

```vb
MSDAOSP
```

 Cette valeur peut également être définie ou lue à l’aide de la propriété [Provider](../../reference/ado-api/provider-property-ado.md) .

 Vous pouvez vous connecter à des fournisseurs simples qui ont été enregistrés en tant que fournisseurs OLE DB complets en utilisant le nom du fournisseur inscrit, déterminé par le writer du fournisseur.

## <a name="typical-connection-string"></a>Chaîne de connexion classique
 Une chaîne de connexion classique pour ce fournisseur est la suivante :

```vb
"Provider=MSDAOSP;Data Source=serverName"
```

 La chaîne se compose des mots clés suivants :

|Mot clé|Description|
|-------------|-----------------|
|**Fournisseur**|Spécifie le fournisseur de OLE DB pour SQL Server.|
|**Source de données**|Spécifie le nom d'un serveur.|

## <a name="xml-document-example"></a>Exemple de document XML
 Le OLE DB fournisseur simple (OSP) dans MDAC 2,7 ou version ultérieure et Windows DAC (Windows Data Access Components) a été amélioré pour prendre en charge l’ouverture de **jeux d’enregistrements** ADO hiérarchiques sur des fichiers XML arbitraires. Ces fichiers XML peuvent contenir le schéma de persistance ADO XML, mais ce n’est pas obligatoire. Cela a été implémenté en connectant l’OSP au **MSXML2.DLL**; par conséquent, **MSXML2.DLL** ou version ultérieure est requis.

 Le fichier **portfolio.xml** utilisé dans l’exemple suivant contient l’arborescence suivante :

```console
Portfolio
   Stock
      Shares
      Symbol
      Price
      Info
         Company Name
         WebSite
```

 Le DSO XML utilise des heuristiques intégrées pour convertir les nœuds d’une arborescence XML en chapitres dans un **jeu d’enregistrements** hiérarchique.

 À l’aide de ces heuristiques intégrées, l’arborescence XML est convertie en un **jeu d’enregistrements** hiérarchique à deux niveaux sous la forme suivante :

```console
Parent Recordset
Shares, Symbol, Price, $Text
   Child Recordset
      Company Name, WebSite, $Text
```

 Notez que les balises de portefeuille et d’informations ne sont pas représentées dans le **jeu d’enregistrements** hiérarchique. Pour obtenir une explication de la façon dont l’objet DSO XML convertit les arborescences XML en **jeux d’enregistrements** hiérarchiques, consultez les règles suivantes. La colonne $Text est présentée dans la section suivante.

## <a name="rules-for-assigning-xml-elements-and-attributes-to-columns-and-rows"></a>Règles d’affectation d’éléments et d’attributs XML à des colonnes et des lignes
 Le DSO XML suit une procédure pour assigner des éléments et des attributs à des colonnes et des lignes dans des applications liées aux données. XML est modélisé sous la forme d’une arborescence avec une balise qui contient l’ensemble de la hiérarchie. Par exemple, une description XML d’un livre peut contenir des balises de chapitre, des balises de figure et des balises de section. Au niveau le plus élevé, la balise Book contient les sous-éléments Chapter, figures et section. Lorsque le DSO XML mappe des éléments XML à des lignes et des colonnes, les sous-éléments, et non l’élément de niveau supérieur, sont convertis.

 Le DSO XML utilise cette procédure pour convertir les sous-éléments :

-   Chaque sous-élément et attribut correspond à une colonne dans un **jeu d’enregistrements** de la hiérarchie.

-   Le nom de la colonne est le même que le nom du sous-élément ou de l’attribut, sauf si l’élément parent a un attribut et un sous-élément portant le même nom, auquel cas un «  ! » est ajouté au nom de colonne du sous-élément.

-   Chaque colonne est soit une colonne *simple* qui contient des valeurs scalaires (généralement des chaînes), soit une colonne du **jeu d’enregistrements** qui contient des **recordsets** enfants.

-   Les colonnes correspondant aux attributs sont toujours simples.

-   Les colonnes correspondant à des sous-éléments sont des colonnes de **jeu d’enregistrements** si le sous-élément a ses propres sous-éléments ou attributs (ou les deux), ou si le parent du sous-élément a plusieurs instances du sous-élément en tant qu’enfant. Dans le cas contraire, la colonne est simple.

-   Lorsqu’il existe plusieurs instances d’un sous-élément (sous différents parents), sa colonne est une colonne du **jeu d’enregistrements** si l' *une* des instances implique une colonne du **jeu d’enregistrements** ; sa colonne n’est simple que si *toutes les* instances impliquent une colonne simple.

-   Tous les **jeux d’enregistrements** ont une colonne supplémentaire nommée $Text.

 Le code nécessaire pour construire un **jeu d’enregistrements** est le suivant :

```vb
Dim adoConn as ADODB.Connection
Dim adoRS as ADODB.Recordset

Set adoRS = New ADODB.Connection
Set adoRS = New ADODB.Recordset

adoConn.Open "Provider=MSDAOSP; Data Source=MSXML2.DSOControl.2.6;"
adoRS.Open "https://WebServer/VRoot/portfolio.xml, adoConn
```

> [!NOTE]
>  Le chemin d’accès du fichier de données peut être spécifié à l’aide de quatre conventions d’attribution de noms différentes.

```vb
'HTTP://
adoRS.Open "https://WebServer/VRoot/portfolio.xml", adoConn
'FILE://
adoRS.Open "file:/// C:\\Directory\\portfolio.xml", adoConn
'UNC Path
adoRS.Open "\\ComputerName\ShareName\portfolio.xml", adoConn
'Full DOS Path
adoRS.Open "C:\Directory\portfolio.xml", adoConn
```

 Dès que l’ensemble **d’enregistrements** a été ouvert, les commandes de navigation ADO **Recordset** habituelles peuvent être utilisées.

 Les **jeux d’enregistrements** générés par l’OSP présentent quelques limitations :

-   Les curseurs clients (**adUseClient**) ne sont pas pris en charge.

-   Les **jeux d’enregistrements** hiérarchiques créés sur du code XML arbitraire ne peuvent pas être rendus persistants à l’aide de **Recordset. Save**.

-   Les **jeux d’enregistrements** créés avec OSP sont en lecture seule.

-   XMLDSO ajoute une colonne de données supplémentaire ($Text) à chaque **jeu d’enregistrements** dans la hiérarchie.

 Pour plus d’informations sur le OLE DB fournisseur simple, consultez [création d’un fournisseur simple](/previous-versions/windows/desktop/ms721067(v=vs.85)).

## <a name="code-example"></a>Exemple de code
 Le code Visual Basic suivant illustre l’ouverture d’un fichier XML arbitraire, la construction d’un **jeu d’enregistrements** hiérarchique et l’écriture récursive de chaque enregistrement de chaque **Recordset** dans la fenêtre de débogage.

 Voici un simple fichier XML qui contient les cotations boursières. Le code suivant utilise ce fichier pour construire un **jeu d’enregistrements** hiérarchique à deux niveaux.

```xml
<portfolio>
   <stock>
      <shares>100</shares>
      <symbol>MSFT</symbol>
      <price>$70.00</price>
      <info>
         <companyname>Microsoft Corporation</companyname>
         <website>https://www.microsoft.com</website>
      </info>
   </stock>
   <stock>
      <shares>100</shares>
      <symbol>AAPL</symbol>
      <price>$107.00</price>
      <info>
         <companyname>Apple Computer, Inc.</companyname>
         <website>https://www.apple.com</website>
      </info>
   </stock>
   <stock>
      <shares>100</shares>
      <symbol>DELL</symbol>
      <price>$50.00</price>
      <info>
         <companyname>Dell Corporation</companyname>
         <website>https://www.dell.com</website>
      </info>
    </stock>
    <stock>
       <shares>100</shares>
       <symbol>INTC</symbol>
       <price>$115.00</price>
       <info>
          <companyname>Intel Corporation</companyname>
          <website>https://www.intel.com</website>
       </info>
   </stock>
</portfolio>
```

 Vous trouverez ci-dessous deux Visual Basic procédures Sub. La première crée le **Recordset** et le passe à la procédure Sub *WalkHier* , qui parcourt de manière récursive la hiérarchie, en écrivant chaque **champ** dans chaque enregistrement de chaque **Recordset** dans la fenêtre de débogage.

```vb
Private Sub BrowseHierRecordset()
' Add ADO 2.7 or later to Project/References
' No need to add MSXML2, ADO just passes the ProgID through to the OSP.

    Dim adoConn As ADODB.Connection
    Dim adoRS As ADODB.Recordset
    Dim adoChildRS As ADODB.Recordset

    Set adoConn = New ADODB.Connection
    Set adoRS = New ADODB.Recordset
    Set adoChildRS = ADODB.Recordset

    adoConn.Open "Provider=MSDAOSP; Data Source=MSXML2.DSOControl.2.6;"
    adoRS.Open "https://bwillett3/Kowalski/portfolio.xml", adoConn

    Dim iLevel As Integer
    iLevel = 0
    WalkHier iLevel, adoRS

End Sub

Sub WalkHier(ByVal iLevel As Integer, ByVal adoRS As ADODB.Recordset)
    iLevel = iLevel + 1
    PriorLevel = iLevel
    While Not adoRS.EOF
        For ndx = 0 To adoRS.Fields.Count - 1
            If adoRS.Fields(ndx).Name <> "$Text" Then
                If adoRS.Fields(ndx).Type = adChapter Then
                    Set adoChildRS = adoRS.Fields(ndx).Value
                    WalkHier iLevel, adoChildRS
                Else
                    Debug.Print iLevel & ": adoRS.Fields(" & ndx & _
                       ") = " & adoRS.Fields(ndx).Name & " = " & _
                       adoRS.Fields(ndx).Value
                End If
            End If
        Next ndx
        adoRS.MoveNext
    Wend
    iLevel = PriorLevel
End Sub
```