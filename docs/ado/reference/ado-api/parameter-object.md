---
description: Objet Parameter
title: Parameter, objet | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Parameter
helpviewer_keywords:
- Parameter object [ADO]
ms.assetid: e010e794-7f0f-4026-8b5b-37328e437d63
author: rothja
ms.author: jroth
ms.openlocfilehash: 6e9d4f0770cdc15ac44882fa808f60f1a85f071c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041149"
---
# <a name="parameter-object"></a>Objet Parameter
Représente un paramètre ou un argument associé à un objet de [commande](./command-object-ado.md) basé sur une requête paramétrable ou une procédure stockée.  
  
## <a name="remarks"></a>Notes  
 De nombreux fournisseurs prennent en charge les commandes paramétrables. Il s’agit de commandes dans lesquelles l’action souhaitée est définie une seule fois, mais les variables (ou paramètres) sont utilisées pour modifier certains détails de la commande. Par exemple, une instruction SQL SELECT peut utiliser un paramètre pour définir les critères de correspondance d’une clause WHERE, et une autre pour définir le nom de colonne d’une clause SORT BY.  
  
 Les objets de **paramètres** représentent des paramètres associés à des requêtes paramétrables, ou les arguments in/out et les valeurs de retour des procédures stockées. Selon la fonctionnalité du fournisseur, certaines collections, méthodes ou propriétés d’un objet de **paramètre** peuvent ne pas être disponibles.  
  
 Avec les collections, les méthodes et les propriétés d’un objet **Parameter** , vous pouvez effectuer les opérations suivantes :  
  
-   Définit ou retourne le nom d’un paramètre avec la propriété [Name](./name-property-ado.md) .  
  
-   Définit ou retourne la valeur d’un paramètre à l’aide de la propriété [value](./value-property-ado.md) . La **valeur** est la propriété par défaut de l’objet de **paramètre** .  
  
-   Définissez ou retournez les caractéristiques des paramètres avec les propriétés [attributes](./attributes-property-ado.md), [direction](./direction-property.md), [précision](./precision-property-ado.md), [NumericScale](./numericscale-property-ado.md), [taille](./size-property-ado-parameter.md)et [type](./type-property-ado.md) .  
  
-   Transmettre des données binaires ou caractères longues à un paramètre avec la méthode [AppendChunk](./appendchunk-method-ado.md) .  
  
-   Accédez aux attributs spécifiques au fournisseur à l’aide de la collection [Properties](./properties-collection-ado.md) .  
  
 Si vous connaissez les noms et les propriétés des paramètres associés à la procédure stockée ou à la requête paramétrable que vous souhaitez appeler, vous pouvez utiliser la méthode [CreateParameter](./createparameter-method-ado.md) pour créer des objets **Parameter** avec les paramètres de propriété appropriés et utiliser la méthode [Append](./append-method-ado.md) pour les ajouter à la collection [Parameters](./parameters-collection-ado.md) . Cela vous permet de définir et de retourner des valeurs de paramètres sans avoir à appeler la méthode [Refresh](./refresh-method-ado.md) sur la collection **Parameters** pour récupérer les informations sur les paramètres auprès du fournisseur, une opération potentiellement gourmande en ressources.  
  
 L’objet de **paramètre** n’est pas sûr pour l’écriture de scripts.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de l’objet Parameter](./parameter-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Command, objet (ADO)](./command-object-ado.md)   
 [CreateParameter, méthode (ADO)](./createparameter-method-ado.md)   
 [Parameters, collection (ADO)](./parameters-collection-ado.md)   
 [Properties, collection (ADO)](./properties-collection-ado.md)