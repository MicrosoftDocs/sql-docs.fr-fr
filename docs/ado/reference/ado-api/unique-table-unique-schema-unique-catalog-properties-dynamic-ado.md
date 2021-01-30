---
description: Table unique, schéma unique, Properties-Dynamic de catalogue unique (ADO)
title: Contrôle des modifications apportées à la table de base du recordset (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Unique Table property [ADO]
- Unique Schema property [ADO]
- Unique Catalog property [ADO]
ms.assetid: d0e775d8-e353-46a1-ad10-ed4cc240dfaa
author: rothja
ms.author: jroth
ms.openlocfilehash: 485fe00e6f55e6916341fd4420f0aaaa395fe54b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170044"
---
# <a name="unique-table-unique-schema-unique-catalog-properties-dynamic-ado"></a>Table unique, schéma unique, Properties-Dynamic de catalogue unique (ADO)
Permet de contrôler étroitement les modifications apportées à une table de base particulière dans un [jeu d’enregistrements](./recordset-object-ado.md) formé par une opération de jointure sur plusieurs tables de base.  
  
-   **Table unique** spécifie le nom de la table de base sur laquelle les mises à jour, les insertions et les suppressions sont autorisées.  
  
-   Le **schéma unique** spécifie le *schéma*, ou le nom du propriétaire de la table.  
  
-   **Unique Catalog** spécifie le *catalogue* ou le nom de la base de données contenant la table.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une valeur de **chaîne** qui est le nom d’une table, d’un schéma ou d’un catalogue.  
  
## <a name="remarks"></a>Notes  
 La table de base souhaitée est identifiée de manière unique par son nom de catalogue, de schéma et de table. Lorsque la propriété de **table unique** est définie, les valeurs des propriétés de **schéma unique** ou **unique de catalogue** sont utilisées pour rechercher la table de base. Il est prévu, mais pas obligatoire, que les propriétés de **schéma unique** et de **catalogue unique** soient définies avant que la propriété de **table unique** soit définie.  
  
 La clé primaire de la **table unique** est traitée comme la clé primaire de l’ensemble du **Recordset**. Il s’agit de la clé utilisée pour toute méthode nécessitant une clé primaire.  
  
 Alors que la **table unique** est définie, la méthode [Delete](./delete-method-ado-recordset.md) affecte uniquement la table nommée. Les méthodes [AddNew](./addnew-method-ado.md), [Resync](./resync-method.md), [Update](./update-method.md)et [UpdateBatch](./updatebatch-method.md) affectent les tables de base sous-jacentes appropriées du **Recordset**.  
  
 Une **table unique** doit être spécifiée avant d’exécuter des resynchronisations personnalisées. Si la **table unique** n’a pas été spécifiée, la propriété de [commande Resync](./resync-command-property-dynamic-ado.md) n’aura aucun effet.  
  
 Une erreur d’exécution se produit si une table de base unique est introuvable.  
  
 Ces propriétés dynamiques sont toutes ajoutées à la collection de [Propriétés](./properties-collection-ado.md) de l’objet **Recordset** lorsque la propriété [CursorLocation](./cursorlocation-property-ado.md) a la valeur **adUseClient**.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Recordset, objet (ADO)](./recordset-object-ado.md)