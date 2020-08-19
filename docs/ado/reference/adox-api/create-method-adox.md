---
description: Create, méthode (ADOX)
title: Create, méthode (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- _Catalog::raw_Create
- _Catalog::Create
helpviewer_keywords:
- Create method [ADOX]
ms.assetid: 64f5c21c-b581-42d8-bdc7-c4f1bebaf105
author: rothja
ms.author: jroth
ms.openlocfilehash: b291651caa93e93999d87a926c9abe391e71d21e
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88440211"
---
# <a name="create-method-adox"></a>Create, méthode (ADOX)
Crée un nouveau catalogue.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Catalog.Create ConnectString  
```  
  
#### <a name="parameters"></a>Paramètres  
 *ConnectString*  
 Valeur de **chaîne** utilisée pour se connecter à la source de données.  
  
## <a name="remarks"></a>Notes  
 La méthode **Create** crée et ouvre une nouvelle [connexion](../../../ado/reference/ado-api/connection-object-ado.md) ADO à la source de données spécifiée dans *connectString*. En cas de réussite, le nouvel objet de **connexion** est affecté à la propriété [ActiveConnection](../../../ado/reference/adox-api/activeconnection-property-adox.md) .  
  
 Une erreur se produit si le fournisseur ne prend pas en charge la création de nouveaux catalogues.  
  
## <a name="applies-to"></a>S'applique à  
 [Catalog, objet (ADOX)](../../../ado/reference/adox-api/catalog-object-adox.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Create, exemple de méthode (VB)](../../../ado/reference/adox-api/create-method-example-vb.md)   
 [ActiveConnection, propriété (ADOX)](../../../ado/reference/adox-api/activeconnection-property-adox.md)
