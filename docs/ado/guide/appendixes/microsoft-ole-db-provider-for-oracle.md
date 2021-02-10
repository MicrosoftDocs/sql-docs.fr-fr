---
description: Présentation de Fournisseur Microsoft OLE DB pour Oracle
title: Fournisseur Microsoft OLE DB pour Oracle | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], OLE DB provider for Oracle
- OLE DB provider for Oracle [ADO]
- Oracle provider [ADO]
ms.assetid: 44fae9dd-5585-4cd6-8bbd-3248a78931b4
author: rothja
ms.author: jroth
ms.openlocfilehash: 04865a03bdb352d36e1ac3b445c7c0b4eb7c3da2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029293"
---
# <a name="microsoft-ole-db-provider-for-oracle-overview"></a>Présentation de Fournisseur Microsoft OLE DB pour Oracle
> [!IMPORTANT]
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Au lieu de cela, utilisez le fournisseur OLE DB d’Oracle.

 Le Fournisseur Microsoft OLE DB pour Oracle permet à ADO d’accéder aux bases de données Oracle.

## <a name="connection-string-parameters"></a>Paramètres de chaîne de connexion
 Pour vous connecter à ce fournisseur, définissez l’argument *Provider* de la propriété [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) sur :

```vb
MSDAORA
```

 La lecture de la propriété [Provider](../../reference/ado-api/provider-property-ado.md) retourne également cette chaîne.

 Si une requête de jointure avec un jeu de clés ou un curseur dynamique est exécutée dans une base de données Oracle, une erreur se produit. Oracle ne prend en charge qu’un curseur statique en lecture seule.

## <a name="typical-connection-string"></a>Chaîne de connexion classique
 Une chaîne de connexion classique pour ce fournisseur est la suivante :

```vb
"Provider=MSDAORA;Data Source=serverName;User ID=MyUserID; Password=MyPassword;"
```

 La chaîne se compose des mots clés suivants :

|Mot clé|Description|
|-------------|-----------------|
|**Fournisseur**|Spécifie le fournisseur de OLE DB pour Oracle.|
|**Source de données**|Spécifie le nom d'un serveur.|
|**ID d'utilisateur**|Spécifie le nom d'utilisateur.|
|**Mot de passe**|Spécifie le mot de passe de l’utilisateur.|

> [!NOTE]
>  Si vous vous connectez à un fournisseur de sources de données qui prend en charge l’authentification Windows, vous devez spécifier **Trusted_Connection = Yes** ou **Integrated Security = SSPI** à la place des informations d’ID d’utilisateur et de mot de passe dans la chaîne de connexion.

## <a name="provider-specific-connection-parameters"></a>Provider-Specific les paramètres de connexion
 Le fournisseur prend en charge plusieurs paramètres de connexion spécifiques au fournisseur, en plus de ceux définis par ADO. Comme avec les propriétés de connexion ADO, ces propriétés spécifiques au fournisseur peuvent être définies via la collection [Properties](../../reference/ado-api/properties-collection-ado.md) d’une [connexion](../../reference/ado-api/connection-object-ado.md) ou dans le cadre de **ConnectionString**.

 Ces paramètres sont décrits en détail dans le [Guide de référence du programmeur OLE DB](/previous-versions/windows/desktop/ms713643(v=vs.85)). L' [index de propriété dynamique ADO](../../reference/ado-api/ado-dynamic-property-index.md) fournit une référence croisée entre ces noms de paramètres et les propriétés de OLE DB correspondantes.

|Paramètre|Description|
|---------------|-----------------|
|**Handle de la fenêtre**|Indique le handle de fenêtre à utiliser pour demander des informations supplémentaires.|
|**Identificateur de paramètres régionaux**|Indique un nombre 32 bits unique (par exemple, 1033) qui spécifie les préférences relatives à la langue de l’utilisateur. Ces préférences indiquent comment les dates et les heures sont mises en forme, les éléments sont triés par ordre alphabétique, les chaînes sont comparées, et ainsi de suite.|
|**Services OLE DB**|Indique un masque de masque qui spécifie OLE DB services à activer ou à désactiver.|
|**Demander**|Indique s’il faut inviter l’utilisateur lors de l’établissement d’une connexion.|
|**Propriétés étendues**|Chaîne contenant des informations de connexion étendues spécifiques au fournisseur. Utilisez cette propriété uniquement pour les informations de connexion spécifiques au fournisseur qui ne peuvent pas être décrites par le biais du mécanisme de propriété.|

## <a name="see-also"></a>Voir aussi
 [ConnectionString, propriété (ADO)](../../reference/ado-api/connectionstring-property-ado.md) [Provider, propriété](../../reference/ado-api/provider-property-ado.md) (ADO), [objet Recordset (](../../reference/ado-api/recordset-object-ado.md) ADO)