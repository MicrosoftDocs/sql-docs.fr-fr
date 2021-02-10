---
description: Provider, propriété (ADO)
title: Provider, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::get_Provider
- Connection15::PutProvider
- Connection15::put_Provider
- Connection15::GetProvider
- Connection15::Provider
helpviewer_keywords:
- Provider property [ADO]
ms.assetid: 0ff70e72-0061-4ffc-90fb-e3ea23129bb2
author: rothja
ms.author: jroth
ms.openlocfilehash: 3e022db374dc818529ed241f9d2ed5b525c8f374
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040829"
---
# <a name="provider-property-ado"></a>Provider, propriété (ADO)
Indique le nom du fournisseur pour un objet de [connexion](./connection-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur de **chaîne** qui indique le nom du fournisseur.  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **Provider** pour définir ou retourner le nom du fournisseur pour une connexion. Cette propriété peut également être définie par le contenu de la propriété [ConnectionString](./connectionstring-property-ado.md) ou de l’argument *ConnectionString* de la méthode [Open](./open-method-ado-connection.md) . Toutefois, la spécification d’un fournisseur dans plusieurs emplacements pendant l’appel de la méthode **Open** peut avoir des résultats imprévisibles. Si aucun fournisseur n’est spécifié, la valeur par défaut de la propriété est MSDASQL ([fournisseur Microsoft OLE DB pour ODBC](../../guide/appendixes/microsoft-ole-db-provider-for-odbc.md)).  
  
 La propriété du **fournisseur** est en lecture/écriture lorsque la connexion est fermée et en lecture seule lorsqu’elle est ouverte. Le paramètre ne prend pas effet tant que vous n’ouvrez pas l’objet de **connexion** ou que vous n’accédez pas à la collection de [Propriétés](./properties-collection-ado.md) de l’objet de **connexion** . Si le paramètre n’est pas valide, une erreur se produit.  
  
## <a name="applies-to"></a>S'applique à  
 [Connection, objet (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Provider et DefaultDatabase, exemple de propriétés (VB)](./provider-and-defaultdatabase-properties-example-vb.md)   
 [Provider et DefaultDatabase, exemple de propriétés (VB)](./provider-and-defaultdatabase-properties-example-vb.md)   
 [Fournisseur Microsoft OLE DB pour ODBC](../../guide/appendixes/microsoft-ole-db-provider-for-odbc.md)   
 [Annexe A : Fournisseurs](../../guide/appendixes/appendix-a-providers.md)