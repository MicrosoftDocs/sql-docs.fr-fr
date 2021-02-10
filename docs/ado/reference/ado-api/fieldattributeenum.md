---
description: FieldAttributeEnum
title: FieldAttributeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FieldAttributeEnum
helpviewer_keywords:
- FieldAttributeEnum enumeration [ADO]
ms.assetid: 6e34d886-005a-40dc-bd5c-6adcbf81e5cd
author: rothja
ms.author: jroth
ms.openlocfilehash: aa880e4a9a39e50cef558a77ba6a72fedfbbd8b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034039"
---
# <a name="fieldattributeenum"></a>FieldAttributeEnum
Spécifie un ou plusieurs attributs d’un objet de [champ](../../../ado/reference/ado-api/field-object.md) .  
  
|Constante|Valeur|Description|  
|--------------|-----------|-----------------|  
|**adFldCacheDeferred**|0x1000|Indique que le fournisseur met en cache les valeurs de champ et que les lectures suivantes sont effectuées à partir du cache.|  
|**adFldFixed**|0x10|Indique que le champ contient des données de longueur fixe.|  
|**adFldIsChapter**|0x2000|Indique que le champ contient une valeur de chapitre, qui spécifie un jeu d’enregistrements enfant spécifique associé à ce champ parent. En général, les champs de chapitre sont utilisés avec la mise en forme des données ou des filtres.|  
|**adFldIsCollection**|0x40000|Indique que le champ spécifie que la ressource représentée par l’enregistrement est une collection d’autres ressources, par exemple un dossier, plutôt qu’une ressource simple, telle qu’un fichier texte.|  
|**adFldKeyColumn**|0x8000|Indique que le champ spécifie la totalité ou une partie de la clé primaire de la colonne.|  
|**adFldIsDefaultStream**|0x20000|Indique que le champ contient le flux par défaut pour la ressource représentée par l’enregistrement. Par exemple, le flux par défaut peut être le contenu HTML d’un dossier racine sur un site Web, qui est automatiquement servi lorsque l’URL racine est spécifiée.|  
|**adFldIsNullable**|0x20|Indique que le champ accepte des valeurs NULL.|  
|**adFldIsRowURL**|0x10000|Indique que le champ contient l’URL qui nomme la ressource du magasin de données représenté par l’enregistrement.|  
|**adFldLong**|0x80|Indique que le champ est un champ binaire long. Indique également que vous pouvez utiliser les méthodes [AppendChunk](../../../ado/reference/ado-api/appendchunk-method-ado.md) et [GetChunk](../../../ado/reference/ado-api/getchunk-method-ado.md) .|  
|**adFldMayBeNull**|0x40|Indique que vous pouvez lire les valeurs NULL à partir du champ.|  
|**adFldMayDefer**|0x2|Indique que le champ est différé, c’est-à-dire que les valeurs de champ ne sont pas récupérées de la source de données avec l’enregistrement entier, mais uniquement lorsque vous y accédez explicitement.|  
|**adFldNegativeScale**|0x4000|Indique que le champ représente une valeur numérique d’une colonne qui prend en charge les valeurs d’échelle négatives. L’échelle est spécifiée par la propriété [NumericScale](../../../ado/reference/ado-api/numericscale-property-ado.md) .|  
|**adFldRowID**|0x100|Indique que le champ contient un identificateur de ligne persistant qui ne peut pas être écrit et n’a aucune valeur significative, sauf pour identifier la ligne (par exemple, un numéro d’enregistrement, un identificateur unique, etc.).|  
|**adFldRowVersion**|0x200|Indique que le champ contient un type d’heure ou un horodatage utilisé pour suivre les mises à jour.|  
|**adFldUnknownUpdatable**|0x8|Indique que le fournisseur ne peut pas déterminer si vous pouvez écrire dans le champ.|  
|**adFldUnspecified**|-1 0xFFFFFFFF|Indique que le fournisseur ne spécifie pas les attributs du champ.|  
|**adFldUpdatable**|0x4|Indique que vous pouvez écrire dans le champ.|  
  
## <a name="adowfc-equivalent"></a>Équivalent ADO/WFC  
 Package : **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums.FieldAttribute.CACHEDEFERRED|  
|AdoEnums.FieldAttribute.FIXED|  
|AdoEnums.FieldAttribute.ISNULLABLE|  
|AdoEnums. FieldAttribute. LONG|  
|AdoEnums. FieldAttribute. MAYBENULL|  
|AdoEnums.FieldAttribute.MAYDEFER|  
|AdoEnums. FieldAttribute. NEGATIVESCALE|  
|AdoEnums.FieldAttribute.ROWID|  
|AdoEnums.FieldAttribute.ROWVERSION|  
|AdoEnums. FieldAttribute. UNKNOWNUPDATABLE|  
|AdoEnums.FieldAttribute.UNSPECIFIED|  
|AdoEnums.FieldAttribute.UPDATABLE|  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Append, méthode (ADO)](../../../ado/reference/ado-api/append-method-ado.md)  
    :::column-end:::
    :::column:::
        [Attributes, propriété (ADO)](../../../ado/reference/ado-api/attributes-property-ado.md)  
    :::column-end:::
:::row-end:::
