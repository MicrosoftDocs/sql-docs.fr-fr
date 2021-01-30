---
description: Collections (syntaxe ADO pour Visual C++)
title: Collections (ADO pour la syntaxe Visual C++) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
dev_langs:
- C++
helpviewer_keywords:
- ADO for Visual C++ syntax [ADO]
- syntax indexes [ADO], ADO for Visual C++ syntax
- collections [ADO], ADO for Visual C++ syntax
ms.assetid: 6a0109a0-f2d9-4f7c-8e1e-42763f9acaea
author: rothja
ms.author: jroth
ms.openlocfilehash: 9cda61fc0d1d206a75059263c5450c813a9e5ddb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164683"
---
# <a name="collections-ado-for-visual-c-syntax"></a>Collections (syntaxe ADO pour Visual C++)
## <a name="parameters"></a>Paramètres  
  
### <a name="methods"></a>Méthodes  
  
```  
Append(IDispatch *Object);  
Delete(VARIANT Index);  
Refresh(void);  
```  
  
 Pour plus d'informations, consultez la rubrique  
  
-   [Append, méthode (ADO)](./append-method-ado.md)  
  
-   [Delete, méthode (collection Parameters ADO)](./delete-method-ado-parameters-collection.md)  
  
-   [Refresh, méthode (ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>Propriétés  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, _ADOParameter **ppvObject);  
```  
  
 Pour plus d'informations, consultez la rubrique  
  
-   [Count, propriété (ADO)](./count-property-ado.md)  
  
-   [Item, propriété (ADO)](./item-property-ado.md)  
  
## <a name="fields"></a>Champs  
  
### <a name="methods"></a>Méthodes  
  
```  
Append(BSTR bstrName, DataTypeEnum Type, long DefinedSize, FieldAttributeEnum Attrib);  
Delete(VARIANT Index);  
Refresh(void);  
```  
  
 Pour plus d'informations, consultez la rubrique  
  
-   [Append, méthode (ADO)](./append-method-ado.md)  
  
-   [Delete, méthode (collection Parameters ADO)](./delete-method-ado-parameters-collection.md)  
  
-   [Refresh, méthode (ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>Propriétés  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, ADOField **ppvObject);  
```  
  
 Pour plus d'informations, consultez la rubrique  
  
-   [Count, propriété (ADO)](./count-property-ado.md)  
  
-   [Item, propriété (ADO)](./item-property-ado.md)  
  
## <a name="errors"></a>Erreurs  
  
### <a name="methods"></a>Méthodes  
  
```  
Clear(void);  
Refresh(void);  
```  
  
 Pour plus d'informations, consultez la rubrique  
  
-   [Clear, méthode (ADO)](./clear-method-ado.md)  
  
-   [Refresh, méthode (ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>Propriétés  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, ADOError **ppvObject);  
```  
  
 Pour plus d'informations, consultez la rubrique  
  
-   [Count, propriété (ADO)](./count-property-ado.md)  
  
-   [Item, propriété (ADO)](./item-property-ado.md)  
  
## <a name="properties"></a>Propriétés  
  
### <a name="methods"></a>Méthodes  
  
```  
Refresh(void);  
```  
  
 Pour plus d'informations, consultez la rubrique  
  
-   [Refresh, méthode (ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>Propriétés  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, ADOProperty **ppvObject);  
```  
  
 Pour plus d'informations, consultez la rubrique  
  
-   [Count, propriété (ADO)](./count-property-ado.md)  
  
-   [Item, propriété (ADO)](./item-property-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Errors, collection (ADO)](./errors-collection-ado.md)   
 [Fields, collection (ADO)](./fields-collection-ado.md)   
 [Parameters, collection (ADO)](./parameters-collection-ado.md)   
 [Properties, collection (ADO)](./properties-collection-ado.md)