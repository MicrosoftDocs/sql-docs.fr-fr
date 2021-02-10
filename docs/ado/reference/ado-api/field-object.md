---
description: Objet Field
title: Field, objet | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field
helpviewer_keywords:
- Field object [ADO]
ms.assetid: b10a72fc-3c4b-4186-a70b-993dc9f7a092
author: rothja
ms.author: jroth
ms.openlocfilehash: 04e7faf5525c676ecc9c20bb312414742e17fc4e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034089"
---
# <a name="field-object"></a>Objet Field
Représente une colonne de données avec un type de données commun.  
  
## <a name="remarks"></a>Notes  
 Chaque objet **Field** correspond à une colonne dans le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md). Vous utilisez la propriété [value](../../../ado/reference/ado-api/value-property-ado.md) des objets **Field** pour définir ou renvoyer des données pour l’enregistrement actif. Selon la fonctionnalité que le fournisseur expose, certaines collections, méthodes ou propriétés d’un objet de **champ** peuvent ne pas être disponibles.  
  
 Avec les collections, les méthodes et les propriétés d’un objet **Field** , vous pouvez effectuer les opérations suivantes :  
  
-   Retourne le nom d’un champ avec la propriété [Name](../../../ado/reference/ado-api/name-property-ado.md) .  
  
-   Affichez ou modifiez les données du champ avec la propriété **valeur** . La **valeur** est la propriété par défaut de l’objet de **champ** .  
  
-   Retourne les caractéristiques de base d’un champ avec les propriétés [type](../../../ado/reference/ado-api/type-property-ado.md), [PRECISION](../../../ado/reference/ado-api/precision-property-ado.md)et [NumericScale](../../../ado/reference/ado-api/numericscale-property-ado.md) .  
  
-   Retourne la taille déclarée d’un champ avec la propriété [DefinedSize](../../../ado/reference/ado-api/definedsize-property.md) .  
  
-   Retourne la taille réelle des données dans un champ donné avec la propriété [ActualSize](../../../ado/reference/ado-api/actualsize-property-ado.md) .  
  
-   Déterminez les types de fonctionnalités pris en charge pour un champ donné avec la propriété [attributs](../../../ado/reference/ado-api/attributes-property-ado.md) et la collection [Properties](../../../ado/reference/ado-api/properties-collection-ado.md) .  
  
-   Manipuler les valeurs des champs contenant des données de type long Binary ou long Character avec les méthodes [AppendChunk](../../../ado/reference/ado-api/appendchunk-method-ado.md) et [GetChunk](../../../ado/reference/ado-api/getchunk-method-ado.md) .  
  
-   Si le fournisseur prend en charge les mises à jour par lot, résolvez les incohérences dans les valeurs de champ lors de la mise à jour par lot avec les propriétés [OriginalValue](../../../ado/reference/ado-api/originalvalue-property-ado.md) et [UnderlyingValue](../../../ado/reference/ado-api/underlyingvalue-property.md) .  
  
 Toutes les propriétés de métadonnées (**Name**, **type**, **DefinedSize**, **PRECISION** et **NumericScale**) sont disponibles avant l’ouverture du **Recordset** de l’objet **Field** . Leur définition à ce moment est utile pour la construction dynamique de formulaires.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de l’objet Field](../../../ado/reference/ado-api/field-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Fields, collection (ADO)](../../../ado/reference/ado-api/fields-collection-ado.md)   
 [Properties, collection (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)   
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)
