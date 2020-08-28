---
description: Open, méthode (ADO MD)
title: Open, méthode (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Open
- Cellset::Open
helpviewer_keywords:
- Open method [ADO MD]
ms.assetid: a87d8080-a238-45e5-bc80-9a8625b3810f
author: rothja
ms.author: jroth
ms.openlocfilehash: 5c7b403ed89ae9933b4169af4e53921205318ccd
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88986230"
---
# <a name="open-method-ado-md"></a>Open, méthode (ADO MD)
Récupère les résultats d’une requête multidimensionnelle et retourne les résultats à un [Cellset](./cellset-object-ado-md.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Cellset.Open Source, ActiveConnection  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Source*  
 facultatif. **Variante** qui prend la valeur d’une requête multidimensionnelle valide, telle qu’une requête MDX (Multidimensional Expression). L’argument *source* correspond à la propriété [source](./source-property-ado-md.md) . Pour plus d’informations sur MDX, consultez la OLE DB pour la documentation sur le [traitement analytique en ligne (OLAP)](/previous-versions/windows/desktop/ms717005(v=vs.85)) dans le kit de développement logiciel (SDK) Microsoft Data Access Components.  
  
 *ActiveConnection*  
 facultatif. **Variant** qui prend la valeur d’une chaîne spécifiant soit un nom de variable objet de [connexion](../ado-api/connection-object-ado.md) ADO valide, soit une définition pour une connexion. L’argument *ActiveConnection* spécifie la connexion dans laquelle ouvrir l’objet [Cellset](./cellset-object-ado-md.md) . Si vous transmettez une définition de connexion pour cet argument, ADO ouvre une nouvelle connexion à l’aide des paramètres spécifiés. L’argument *ActiveConnection* correspond à la propriété [ActiveConnection](./activeconnection-property-ado-md.md) .  
  
## <a name="remarks"></a>Notes  
 La méthode **Open** génère une erreur si l’un de ses paramètres est omis et que sa valeur de propriété correspondante n’a pas été définie avant la tentative d’ouverture de l’ensemble de **cellules**.  
  
## <a name="applies-to"></a>S'applique à  
 [Cellset, objet (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [CellSet, exemple (VB)](./cellset-example-vb.md)   
 [ActiveConnection, propriété (ADO MD)](./activeconnection-property-ado-md.md)   
 [Close, méthode (ADO MD)](./close-method-ado-md.md)   
 [Connection, objet (ADO)](../ado-api/connection-object-ado.md)   
 [Source, propriété (ADO MD)](./source-property-ado-md.md)   
 [State, propriété (ADO MD)](./state-property-ado-md.md)