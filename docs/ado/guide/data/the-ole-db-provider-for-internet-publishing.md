---
description: Fournisseur OLE DB pour la publication Internet
title: Fournisseur OLE DB pour la publication Internet | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB provider for Internet publishing [ADO]
- ADO, Internet publishing
- publishing to Internet [ADO]
- Internet publishing [ADO]
- providers [ADO], OLE DB provider for Internet publishing
ms.assetid: 4869aafa-7401-4ce1-93ce-45406a60274f
author: rothja
ms.author: jroth
ms.openlocfilehash: 9282bdc81d71ecd938f3d115678769915f8e3367
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032401"
---
# <a name="the-ole-db-provider-for-internet-publishing"></a>Fournisseur OLE DB pour la publication Internet
Les objets d' [enregistrement](../../../ado/reference/ado-api/record-object-ado.md) et de [flux](../../../ado/reference/ado-api/stream-object-ado.md) ADO peuvent être utilisés avec le fournisseur Microsoft OLE DB pour la publication Internet (fournisseur de publication Internet) afin d’accéder à des ressources et de les manipuler, telles que des dossiers Web ou des fichiers pris en charge par Microsoft FrontPage. Avec ADO, vous pouvez spécifier la source d’un **enregistrement**, d’un **flux** ou d’un [jeu d’enregistrements](../../../ado/reference/ado-api/recordset-object-ado.md) comme URL. Vous pouvez ensuite charger, télécharger, déplacer, copier et supprimer des ressources, ou manipuler directement les propriétés des ressources.  
  
 Pour obtenir un exemple de code qui utilise des **enregistrements** et des **flux** avec le fournisseur de publication Internet, consultez [scénario de publication Internet](../../../ado/guide/data/internet-publishing-scenario.md).  
  
 Le fournisseur de publication Internet est installé avec Microsoft Windows 2000. Les versions antérieures du fournisseur de publication Internet sont également disponibles avec Microsoft Office 2000 et Microsoft Internet Explorer 5,0.  
  
 Il existe trois façons de connecter ADO au fournisseur de publication Internet :  
  
-   Spécifiez « URL = » dans la chaîne de connexion. Par exemple :  
  
    ```  
    objConn.Open "URL=https://servername"  
    ```  
  
-   Spécifiez Msdaipp. DSO pour le mot clé *Provider* de la chaîne de connexion. Par exemple :  
  
    ```  
    objConn.Open "provider=MSDAIPP.DSO;data source=https://servername"  
    ```  
  
-   Spécifiez Msdaipp. DSO pour la propriété [Provider](../../../ado/reference/ado-api/provider-property-ado.md) de l’objet [Connection](../../../ado/reference/ado-api/connection-object-ado.md) . Par exemple :  
  
    ```  
    objConn.Provider = "MSDAIPP.DSO"  
    objConn.Open "https://servername"  
    ```  
  
> [!NOTE]
>  Si msdaipp. DSO est spécifié explicitement comme valeur du fournisseur, soit avec le mot clé de chaîne de connexion du *fournisseur* , soit avec la propriété **Provider** , vous ne pouvez pas utiliser "URL =" dans la chaîne de connexion. Si c’est le cas, une erreur se produit. Au lieu de cela, spécifiez simplement l’URL comme indiqué plus haut.  
  
 Pour obtenir des informations plus spécifiques sur le fournisseur de publication Internet, consultez [fournisseur Microsoft OLE DB pour la publication Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)ou la documentation du fournisseur fournie avec l’application source avec laquelle le fournisseur de OLE DB pour la publication Internet a été installé : Windows 2000, Office 2000 ou Internet Explorer 5,0.
