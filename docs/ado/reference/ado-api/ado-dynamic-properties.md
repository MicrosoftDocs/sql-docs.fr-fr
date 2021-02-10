---
description: Propriétés dynamiques ADO
title: Propriétés dynamiques ADO | Microsoft Docs
ms.prod: sql
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- dynamic properties [ADO]
- properties [ADO], dynamic
ms.assetid: d7b06d72-f792-4328-93a2-5006b9e2c581
author: rothja
ms.author: jroth
ms.openlocfilehash: 438b14f2a3fc0ffde38983610a10b1a9f5c46a1b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031301"
---
# <a name="ado-dynamic-properties"></a>Propriétés dynamiques ADO
Les propriétés dynamiques peuvent être ajoutées aux collections [Properties](./properties-collection-ado.md) des objets [Connection](./connection-object-ado.md), [Command](./command-object-ado.md)ou [Recordset](./recordset-object-ado.md) . La source de ces propriétés est un fournisseur de données, tel que le [fournisseur de OLE DB pour SQL Server](../../guide/appendixes/microsoft-ole-db-provider-for-sql-server.md)ou un fournisseur de services, tel que le [service de curseur Microsoft pour OLE DB](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md). Pour plus d’informations sur une propriété dynamique spécifique, reportez-vous à la documentation appropriée du fournisseur de données ou du fournisseur de services.  
  
 L' [index de propriété dynamique ADO](./ado-dynamic-property-index.md) fournit une référence croisée entre les noms ado et OLE DB pour chaque propriété dynamique du fournisseur OLE DB standard.  
  
 Les propriétés dynamiques suivantes sont particulièrement intéressantes et sont également documentées dans les sources mentionnées précédemment. Les fonctionnalités spéciales avec ADO sont documentées dans les rubriques d’aide ADO de la liste suivante.  
  
|Propriété dynamique|Description|  
|-|-|  
|[Optimize](./optimize-property-dynamic-ado.md)|Spécifie si un index doit être créé sur ce champ.|  
|[Demander](./prompt-property-dynamic-ado.md)|Spécifie si le fournisseur de OLE DB doit inviter l’utilisateur à fournir des informations d’initialisation.|  
|[Reformer le nom](./reshape-name-property-dynamic-ado.md)|Spécifie un nom pour l’objet **Recordset** .|  
|[Commande Resync](./resync-command-property-dynamic-ado.md)|Spécifie une chaîne de commande fournie par l’utilisateur que la méthode de **resynchronisation** émet pour actualiser les données de la table nommée dans la propriété dynamique de la **table unique** .|  
|[Table unique, schéma unique, catalogue unique](./unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|**Table unique** Spécifie le nom de la table de base sur laquelle les mises à jour, les insertions et les suppressions sont autorisées.<br /><br /> **Schéma unique** Spécifie le schéma ou le nom du propriétaire de la table.<br /><br /> **Catalogue unique** Spécifie le catalogue ou le nom de la base de données qui contient la table.|  
|[Mettre à jour la resynchronisation](./update-resync-property-dynamic-ado.md)|Spécifie si la méthode **UpdateBatch** est suivie d’une opération de méthode de **resynchronisation** implicite et, le cas échéant, de l’étendue de cette opération.|
| &nbsp; | &nbsp; |

## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ADO](./ado-api-reference.md)   
 [Collections ADO](./ado-collections.md)   
 [Constantes énumérées ADO](./ado-enumerated-constants.md)   
 [Annexe B : erreurs ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Événements ADO](./ado-events.md)   
 [Méthodes ADO](./ado-methods.md)   
 [Modèle objet ADO](./ado-object-model.md)   
 [Objets et interfaces ADO](./ado-objects-and-interfaces.md)   
 [Propriétés ADO](./ado-properties.md)