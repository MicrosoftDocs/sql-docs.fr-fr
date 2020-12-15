---
title: Exécution d’un DiffGram à l’aide d’ADO (SQLXML)
description: Découvrez comment exécuter un fichier DiffGram dans une application Microsoft Visual Basic à l’aide d’ADO (SQLXML 4,0) pour établir une connexion à une instance de Microsoft SQL Server.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- providers [SQLXML], SQLOLEDB Provider
- ADO [SQLXML]
- SQLXMLOLEDB Provider, DiffGrams
- data providers [SQLXML], SQLOLEDB Provider
- DiffGrams [SQLXML], ADO
ms.assetid: 741fce82-de83-4923-86eb-30acb5b9a5e6
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a28fe03f8e52d58f0e31c2e7d3cabebfd8a6113c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97431467"
---
# <a name="executing-a-diffgram-by-using-ado-sqlxml-40"></a>Exécution d'un DiffGram à l'aide d'ADO (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Cette application [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic utilise ADO pour établir une connexion à une instance de Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], puis exécute un DiffGram. Dans cette application, le DiffGram et le schéma XSD sont stockés dans un fichier. L'application charge le DiffGram à partir du fichier spécifié. Vous pouvez utiliser l’un des DiffGrams (et le schéma XSD associé) décrit dans [exemples DiffGram](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md).  
  
 Voici le processus de l'exemple d'application :  
  
-   Objet **conn** (**ADODB. Connexion**) établit une connexion à une instance en cours d’exécution de SQL Server sur un serveur spécifique.  
  
-   L’objet **cmd** (**ADODB. Command**) s’exécute sur la connexion établie.  
  
-   Le dialecte de la commande est défini sur DBGUID_MSSQLXML.  
  
-   Le DiffGram est copié dans le flux de commandes (**strmIn**) à partir d’un fichier.  
  
-   Le flux de sortie de la commande est défini sur l’objet **StrmOut** (**ADODB. Stream**) pour recevoir toutes les données retournées.  
  
-   Lorsque vous utilisez le fournisseur SQLOLEDB, par défaut, vous obtenez les fonctionnalités SQLXML de Microsoft fournies par Sqlxmlx.dll. Pour utiliser Sqlxml4.dll avec le fournisseur SQLOLEDB, la propriété de **version SQLXML** doit être définie sur **SQLXML. 4.0** sur l’objet de **connexion** du fournisseur SQLOLEDB.  
  
-   La commande (DiffGram) est exécutée.  
  
 Le code suivant est celui de l'exemple d'application.  
  
> [!NOTE]  
>  Dans le code, vous devez fournir le nom de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans la chaîne de connexion.  
  
```  
Private Sub Command1_Click()  
  Dim cmd As New ADODB.Command  
  Dim conn As New ADODB.Connection  
  Dim strmOut As New ADODB.Stream  
  Dim strmIn As New ADODB.Stream  
  
  'Open a connection to SQL Server.  
  conn.Provider = "SQLOLEDB"  
  conn.Open "server=SqlServerName; database=tempdb; Integrated Security=SSPI; "  
  conn.Properties("SQLXML Version") = "SQLXML.4.0"  
  Set cmd.ActiveConnection = conn  
  strmIn.Open  
  strmIn.Charset = "UTF-8"  
  strmIn.LoadFromFile "C:\SomeFilePath\SampleDiffGram.xml"  
  strmIn.Position = 0  
  Set cmd.CommandStream = strmIn  
  
  strmOut.Open  
  cmd.Properties("Output Stream").Value = strmOut  
  cmd.Properties("Output Encoding").Value = "UTF-8"  
  
  cmd.Dialect = "{5d531cb2-e6ed-11d2-b252-00c04f681b71}"  
  cmd.Properties("Mapping Schema") = "C:\SomeFilePath\SampleDiffGram.xml"  
  cmd.Execute , , adExecuteStream  
  strmOut.Position = 0  
  Set cmd = Nothing  
  strmOut.Charset = "UTF-8"  
  strmOut.SaveToFile "C:\DropIt.txt", adSaveCreateOverWrite  
  strmOut.Close  
  Set strmOut = Nothing  
  
End Sub  
```  
  
### <a name="to-test-the-diffgram"></a>Pour tester le DiffGram  
  
1.  Dans un dossier sur votre ordinateur, copiez l’un des DiffGrams et le schéma XSD correspondant dans l’un des exemples des [exemples DiffGram](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md).  
  
2.  Ouvrez Visual Basic et créez un projet EXE standard.  
  
3.  Ajoutez ces références au projet :  
  
    ```  
    Microsoft ActiveX Data Objects 2.8 Library  
    ```  
  
4.  Dans la boîte à outils, cliquez sur **CommandButton**, puis dessinez un bouton sur le formulaire.  
  
5.  Double-cliquez sur le bouton pour modifier le code et ajoutez le code d'application fourni dans la rubrique.  
  
6.  Modifiez le code pour spécifier le DiffGram et les noms de fichier XSD. Modifiez aussi la chaîne de connexion si nécessaire.  
  
7.  Exécutez l'application. Le résultat de l'exécution dépend du DiffGram que vous exécutez.  

