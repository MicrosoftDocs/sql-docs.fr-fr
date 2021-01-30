---
description: Record, objet (ADO)
title: Record, objet (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Record
helpviewer_keywords:
- Record object [ADO]
ms.assetid: db83ed2c-a8e3-460c-8682-64667e4d5d01
author: rothja
ms.author: jroth
ms.openlocfilehash: b6064030bd26ab6abb0bb003b2fcf26e72c4f24d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166749"
---
# <a name="record-object-ado"></a>Record, objet (ADO)
Représente une ligne d’un [jeu d’enregistrements](./recordset-object-ado.md) ou du fournisseur de données, ou un objet retourné par un fournisseur de données semi-structuré, tel qu’un fichier ou un répertoire.  
  
## <a name="remarks"></a>Notes  
 Un objet **Record** représente une ligne de données et présente des similarités conceptuelles avec un **Recordset** d’une seule ligne. Selon les capacités de votre fournisseur, les objets d' **enregistrement** peuvent être retournés directement à partir de votre fournisseur au lieu d’un **Recordset** d’une seule ligne, par exemple lorsqu’une requête SQL qui sélectionne une seule ligne est exécutée. Ou un objet **enregistrement** peut être obtenu directement à partir d’un objet **Recordset** . Ou un **enregistrement** peut être retourné directement d’un fournisseur à des données semi-structurées, telles que le fournisseur de OLE DB Microsoft Exchange.  
  
 Vous pouvez afficher les champs associés à l’objet **Record** par le biais de la collection [Fields](./fields-collection-ado.md) sur l’objet **Record** . ADO autorise les colonnes à valeurs objet, y compris les valeurs **Recordset**, **SAFEARRAY** et scalaires dans la collection **Fields** des objets **Record** .  
  
 Si l’objet **Record** représente une ligne dans un **Recordset**, il est possible de retourner à ce **Recordset** d’origine avec la propriété [source](./source-property-ado-record.md) .  
  
 L’objet **Record** peut également être utilisé par des fournisseurs de données semi-structurés, tels que le [fournisseur Microsoft OLE DB pour la publication Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md), pour modéliser des espaces de noms structurés en arborescence. Chaque nœud de l’arborescence est un objet **Record** avec des colonnes associées. Les colonnes peuvent représenter les attributs de ce nœud et d’autres informations pertinentes. L’objet **Record** peut représenter un nœud terminal et un nœud non-terminal dans l’arborescence. Les nœuds non terminaux ont d’autres nœuds comme contenu, mais les nœuds terminaux n’ont pas ce contenu. Les nœuds terminaux contiennent généralement des flux binaires de données et des nœuds non terminaux peuvent également être associés à un flux binaire par défaut. Les propriétés de l’objet **Record** identifient le type de nœud.  
  
 L’objet **Record** représente également une autre façon de parcourir les données organisées hiérarchiquement. Un objet **Record** peut être créé pour représenter la racine d’une sous-arborescence spécifique dans une arborescence de grande taille et de nouveaux objets **Record** peuvent être ouverts pour représenter des nœuds enfants.  
  
 Une ressource (par exemple, un fichier ou un répertoire) peut être identifiée de manière unique par une URL absolue. Un objet [Connection](./connection-object-ado.md) est implicitement créé et défini sur l’objet **Record** lorsque l' **enregistrement** est ouvert à l’aide d’une URL absolue. Un objet de **connexion** peut être défini explicitement sur l’objet **Record** via la propriété [ActiveConnection](./activeconnection-property-ado.md) . Les fichiers et répertoires accessibles à l’aide de l’objet de **connexion** définissent le *contexte* dans lequel les opérations d' **enregistrement** peuvent se produire.  
  
 Les méthodes de modification et de navigation des données sur l’objet **Record** acceptent également une URL relative, qui localise une ressource à l’aide d’une URL absolue ou du contexte de l’objet de **connexion** comme point de départ.  
  
> [!NOTE]
>  Les URL utilisant le schéma http appellera automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../guide/data/absolute-and-relative-urls.md).  
  
 Un objet de **connexion** est associé à chaque objet **enregistrement** . Par conséquent, les opérations d’objet **Record** peuvent faire partie d’une transaction en appelant les méthodes de transaction des objets de **connexion** .  
  
 L’objet **Record** ne prend pas en charge les événements ADO et, par conséquent, ne répond pas aux notifications.  
  
 Avec les méthodes et propriétés d’un objet **Record** , vous pouvez effectuer les opérations suivantes :  
  
-   Définit ou retourne l’objet de **connexion** associé à l’aide de la propriété [ActiveConnection](./activeconnection-property-ado.md) .  
  
-   Indiquez les autorisations d’accès avec la propriété [mode](./mode-property-ado.md) .  
  
-   Retourne l’URL du répertoire, le cas échéant, qui contient la ressource représentée par l' **enregistrement** avec la propriété [ParentURL](./parenturl-property-ado.md) .  
  
-   Indiquez l’URL absolue, l’URL relative ou l’ensemble d' **enregistrements** à partir duquel l' **enregistrement** est dérivé de la propriété [source](./source-property-ado-record.md) .  
  
-   Indique l’état actuel de l' **enregistrement** avec la propriété [State](./state-property-ado.md) .  
  
-   Indiquez le type d' **enregistrement**  -  *simple*, *collection* ou *document structuré* -avec la propriété [RecordType](./recordtype-property-ado.md).  
  
-   Arrête l’exécution d’une opération asynchrone avec la méthode [Cancel](./cancel-method-ado.md) .  
  
-   Dissociez l' **enregistrement** d’une source de données avec la méthode [Close](./close-method-ado.md) .  
  
-   Copiez le fichier ou le répertoire représenté par un **enregistrement** à un autre emplacement à l’aide de la méthode [CopyRecord](./copyrecord-method-ado.md) .  
  
-   Supprimez le fichier, ou le répertoire et les sous-répertoires, représentés par un **enregistrement** avec la méthode [DeleteRecord](./deleterecord-method-ado.md) .  
  
-   Ouvrez un **jeu d’enregistrements** qui contient des lignes qui représentent les sous-répertoires et les fichiers de l’entité représentée par l' **enregistrement** avec la méthode [GetChildren](./getchildren-method-ado.md) .  
  
-   Déplacez (renommez) le fichier, ou le répertoire et les sous-répertoires, représentés par un **enregistrement** à un autre emplacement avec la méthode [MoveRecord](./moverecord-method-ado.md) .  
  
-   Associez l' **enregistrement** à une source de données existante ou créez un nouveau fichier ou répertoire avec la méthode [Open](./open-method-ado-record.md) .  
  
 L’objet **Record** est sécurisé pour l’écriture de scripts.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de l’objet Record](./record-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Fields, collection (ADO)](./fields-collection-ado.md)   
 [Properties, collection (ADO)](./properties-collection-ado.md)   
 [Enregistrements et flux](../../guide/data/records-and-streams.md)   
 [Recordset, objet (ADO)](./recordset-object-ado.md)