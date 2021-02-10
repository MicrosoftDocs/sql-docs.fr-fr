---
description: ExecuteOptionEnum
title: ExecuteOptionEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ExecuteOptionEnum
helpviewer_keywords:
- ExecuteOptionEnum enumeration [ADO]
ms.assetid: 68bfa83a-5df4-4bef-8736-0f88ae8c29ea
author: rothja
ms.author: jroth
ms.openlocfilehash: b5d363e1255a1f34554dc6aceec212f5a79a7df8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034079"
---
# <a name="executeoptionenum"></a>ExecuteOptionEnum
Spécifie comment un fournisseur doit exécuter une commande.  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adAsyncExecute**|0x10|Indique que la commande doit s’exécuter de façon asynchrone.<br /><br /> Cette valeur ne peut pas être combinée avec la valeur [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md) **adCmdTableDirect**.|  
|**adAsyncFetch**|0x20|Indique que les lignes restantes après la quantité initiale spécifiée dans la propriété [CacheSize](../../../ado/reference/ado-api/cachesize-property-ado.md) doivent être récupérées de manière asynchrone.|  
|**adAsyncFetchNonBlocking**|0x40|Indique que le thread principal n’est jamais bloqué pendant la récupération. Si la ligne demandée n’a pas été récupérée, la ligne actuelle se déplace automatiquement à la fin du fichier.<br /><br /> Si vous ouvrez un [jeu d’enregistrements](../../../ado/reference/ado-api/recordset-object-ado.md) à partir d’un [flux](../../../ado/reference/ado-api/stream-object-ado.md) contenant un **Recordset** stocké de manière permanente, **adAsyncFetchNonBlocking** n’a aucun effet. l’opération sera synchrone et se bloquera.<br /><br /> **adAsynchFetchNonBlocking** n’a aucun effet lorsque l’option [adCmdTableDirect](../../../ado/reference/ado-api/commandtypeenum.md) est utilisée pour ouvrir le **jeu d’enregistrements**.|  
|**adExecuteNoRecords**|0x80|Indique que le texte de la commande est une commande ou une procédure stockée qui ne retourne pas de lignes (par exemple, une commande qui insère uniquement des données). Si des lignes sont récupérées, elles sont ignorées et ne sont pas retournées.<br /><br /> **adExecuteNoRecords** ne peut être passé comme paramètre facultatif que pour la méthode d' **exécution** de la **commande** ou de la connexion.|  
|**adExecuteStream**|0x400|Indique que les résultats d’une exécution de commande doivent être retournés sous forme de flux.<br /><br /> **adExecuteStream** peut uniquement être passé comme paramètre facultatif à la méthode **Execute** de la commande.|  
|**adExecuteRecord**||Indique que **CommandText** est une commande ou une procédure stockée qui retourne une seule ligne qui doit être retournée en tant qu’objet **enregistrement** .|  
|**adOptionUnspecified**|-1|Indique que la commande n’est pas spécifiée.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums.ExecuteOption. ASYNCEXECUTE|  
|AdoEnums.ExecuteOption.ASYNCFETCH|  
|AdoEnums.ExecuteOption. ASYNCFETCHNONBLOCKING|  
|AdoEnums.ExecuteOption.NORECORDS|  
|AdoEnums.ExecuteOption.UNSPECIFIED|  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Execute, méthode (commande ADO)](../../../ado/reference/ado-api/execute-method-ado-command.md)  
        [Execute, méthode (objet Connection ADO)](../../../ado/reference/ado-api/execute-method-ado-connection.md)  
    :::column-end:::
    :::column:::
        [Open, méthode (objet Recordset ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)  
        [Requery, méthode](../../../ado/reference/ado-api/requery-method.md)  
    :::column-end:::
:::row-end:::
