---
description: Présentation du fournisseur Microsoft OLE DB pour la publication Internet
title: Fournisseur Microsoft OLE DB pour la publication Internet | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB provider for Internet publishing [ADO]
- providers [ADO], OLE DB provider for Internet publishing
- Internet Publishing provider [ADO]
ms.assetid: 66a208d9-b580-4655-a41e-1d36e5b5bfca
author: rothja
ms.author: jroth
ms.openlocfilehash: 051185b9b40b1f7d4472e957f3a09a8f6416c8b6
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88454091"
---
# <a name="microsoft-ole-db-provider-for-internet-publishing-overview"></a>Présentation du fournisseur Microsoft OLE DB pour la publication Internet
Le fournisseur Microsoft OLE DB pour la publication Internet permet à ADO d’accéder aux ressources prises en charge par Microsoft FrontPage ou Microsoft Internet Information Server. Les ressources incluent des fichiers source Web tels que des fichiers HTML ou des dossiers Web Windows 2000.

## <a name="connection-string-parameters"></a>Paramètres de chaîne de connexion
 Pour vous connecter à ce fournisseur, définissez l’argument *Provider* de la propriété [ConnectionString](../../../ado/reference/ado-api/connectionstring-property-ado.md) sur :

```vb
MSDAIPP.DSO
```

 Cette valeur peut également être définie ou lue à l’aide de la propriété [Provider](../../../ado/reference/ado-api/provider-property-ado.md) .

## <a name="typical-connection-string"></a>Chaîne de connexion classique
 Une chaîne de connexion classique pour ce fournisseur est la suivante :

```vb
"Provider=MSDAIPP.DSO;Data Source=ResourceURL;User ID=MyUserID;Password=MyPassword;"
```

 - ou -

```vb
"URL=ResourceURL;User ID=MyUserID;Password=MyPassword;"
```

 La chaîne se compose des mots clés suivants :

|Mot clé|Description|
|-------------|-----------------|
|**Fournisseur**|Spécifie le fournisseur de OLE DB pour la publication Internet.|
|**Source de données** -ou- **URL**|Spécifie l’URL d’un fichier ou d’un répertoire publié dans un dossier Web.|
|**ID d'utilisateur**|Spécifie le nom d'utilisateur.|
|**Mot de passe**|Spécifie le mot de passe de l’utilisateur.|

> [!NOTE]
>  Si vous vous connectez à un fournisseur de sources de données qui prend en charge l’authentification Windows, vous devez spécifier **Trusted_Connection = Yes** ou **Integrated Security = SSPI** à la place des informations d’ID d’utilisateur et de mot de passe dans la chaîne de connexion.

 Si vous définissez la valeur *resourceurl n'* de « URL = » dans la chaîne de connexion sur une valeur non valide, par défaut, le fournisseur de publication Internet génère une boîte de dialogue qui vous invite à entrer une valeur valide. Il s’agit d’un comportement indésirable pour un composant de la couche intermédiaire d’une application, car il interrompt l’exécution du programme jusqu’à ce que la boîte de dialogue soit désactivée et le client semble se figer, car il n’a pas reçu de réponse du composant.

> [!NOTE]
>  Si MSDAIPP. DSO est explicitement spécifié comme valeur du fournisseur, soit avec le mot clé de chaîne de connexion du *fournisseur* , soit avec la propriété **Provider** , vous ne pouvez pas utiliser "URL =" dans la chaîne de connexion. Si c’est le cas, une erreur se produit. Au lieu de cela, il vous suffit de spécifier l’URL comme indiqué dans la rubrique [utilisation d’ADO avec le fournisseur de OLE DB pour la publication Internet](../../../ado/guide/data/the-ole-db-provider-for-internet-publishing.md).

## <a name="see-also"></a>Voir aussi
 [Scénario de publication Internet](../../../ado/guide/data/internet-publishing-scenario.md) [fournisseur de OLE DB pour la publication Internet](../../../ado/guide/data/the-ole-db-provider-for-internet-publishing.md)
