---
description: Fields, collection (ADO)
title: Fields, collection (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Fields
- Recordset15::Fields
- _Record::Fields
helpviewer_keywords:
- Fields collection [ADO]
ms.assetid: 7c371474-b88f-4730-afa5-44163a0488d5
author: rothja
ms.author: jroth
ms.openlocfilehash: 7466a56d3b9ae5e9a35968f34bb13f9e748c934d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034049"
---
# <a name="fields-collection-ado"></a>Fields, collection (ADO)
Contient tous les objets [Field](./field-object.md) d’un objet [Recordset](./recordset-object-ado.md) ou [Record](./record-object-ado.md) .  
  
## <a name="remarks"></a>Notes  
 Un objet **Recordset** possède une collection **Fields** composée d’objets **Field** . Chaque objet **Field** correspond à une colonne dans le **Recordset**. Vous pouvez remplir la collection de **champs** avant d’ouvrir le **jeu d’enregistrements** en appelant la méthode [Refresh](./refresh-method-ado.md) sur la collection.  
  
> [!NOTE]
>  Pour obtenir une explication plus détaillée de l’utilisation des objets de **champ** , consultez la rubrique relative aux objets de **champ** .  
  
 La collection **Fields** possède une méthode [Append](./append-method-ado.md) , qui crée et ajoute un objet **Field** à la collection, et une méthode **Update** qui finalise les ajouts ou les suppressions.  
  
 Un objet **Record** a deux champs spéciaux qui peuvent être indexés avec des constantes [FieldEnum](./fieldenum.md) . Une constante accède à un champ contenant le flux par défaut de l' **enregistrement**, tandis que l’autre accède à un champ contenant la chaîne d’URL absolue de l' **enregistrement**.  
  
 Certains fournisseurs (par exemple, le [fournisseur Microsoft OLE DB pour la publication Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)) peuvent remplir la collection **Fields** avec un sous-ensemble de champs disponibles pour l' **enregistrement** ou le **Recordset**. Les autres champs ne sont pas ajoutés à la collection tant qu’ils ne sont pas référencés pour la première fois par un nom ou par votre code.  
  
 Si vous tentez de référencer un champ inexistant par son nom, un nouvel objet **Field** est ajouté à la collection **Fields** avec l' [État](./status-property-ado-field.md) **adFieldPendingInsert**. Quand vous appelez [Update](./update-method.md), ADO crée un nouveau champ dans la source de données si votre fournisseur l’autorise.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de la collection Fields](./fields-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Objet Field](./field-object.md)