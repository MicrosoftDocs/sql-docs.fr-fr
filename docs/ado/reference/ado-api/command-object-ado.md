---
description: Command, objet (ADO)
title: Command, objet (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command
helpviewer_keywords:
- Command object [ADO]
ms.assetid: a02c22fb-542d-465e-a629-30fd59dcbebf
author: rothja
ms.author: jroth
ms.openlocfilehash: c8cb910eef7ef810b444813c1e6678b745c524e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100026955"
---
# <a name="command-object-ado"></a>Command, objet (ADO)
Définit une commande spécifique que vous avez l’intention d’exécuter sur une source de données.  
  
## <a name="remarks"></a>Notes  
 Utilisez un objet de **commande** pour interroger une base de données et retourner des enregistrements dans un objet [Recordset](./recordset-object-ado.md) , pour exécuter une opération en bloc ou pour manipuler la structure d’une base de données. Selon la fonctionnalité du fournisseur, certaines collections de **commandes** , méthodes ou propriétés peuvent générer une erreur lorsqu’elles sont référencées.  
  
 Avec les collections, les méthodes et les propriétés d’un objet **Command** , vous pouvez effectuer les opérations suivantes :  
  
-   Définissez le texte exécutable de la commande (par exemple, une instruction SQL) avec la propriété [CommandText](./commandtext-property-ado.md) . Pour les structures de commande ou de requête autres que les chaînes simples (par exemple, les requêtes de modèle XML), vous pouvez également définir la commande avec la propriété [CommandStream](./commandstream-property-ado.md) .  
  
-   Éventuellement, indiquez le dialecte de commande utilisé dans **CommandText** ou **CommandStream** avec la propriété [Dialect](./dialect-property.md) .  
  
-   Définir des requêtes paramétrables ou des arguments de procédure stockée avec des objets de [paramètre](./parameter-object.md) et la collection de [paramètres](./parameters-collection-ado.md) .  
  
-   Indiquez si les noms de paramètres doivent être passés au fournisseur avec la propriété [NamedParameters](./namedparameters-property-ado.md) .  
  
-   Exécutez une commande et renvoyez un objet **Recordset** , le cas échéant, avec la méthode [Execute](./execute-method-ado-command.md) .  
  
-   Spécifiez le type de commande avec la propriété [CommandType](./commandtype-property-ado.md) avant l’exécution pour optimiser les performances.  
  
-   Contrôler si le fournisseur enregistre une version préparée (ou compilée) de la commande avant l’exécution avec la propriété [Prepared](./prepared-property-ado.md) .  
  
-   Définit le nombre de secondes pendant lesquelles un fournisseur attendra qu’une commande s’exécute avec la propriété [CommandTimeout](./commandtimeout-property-ado.md) .  
  
-   Associez une connexion ouverte à un objet de **commande** en définissant sa propriété [ActiveConnection](./activeconnection-property-ado.md) .  
  
-   Définissez la propriété [Name](./name-property-ado.md) pour identifier l’objet **Command** comme une méthode sur l’objet [Connection](./connection-object-ado.md) associé.  
  
-   Transmettez un objet **Command** à la propriété [source](./source-property-ado-recordset.md) d’un **Recordset** pour obtenir des données.  
  
-   Accédez aux attributs spécifiques au fournisseur avec la collection [Properties](./properties-collection-ado.md) .  
  
> [!NOTE]
>  Pour exécuter une requête sans utiliser d’objet **Command** , transmettez une chaîne de requête à la méthode [Execute](./execute-method-ado-connection.md) d’un objet **Connection** ou à la méthode [Open](./open-method-ado-recordset.md) d’un objet **Recordset** . Toutefois, un objet **Command** est requis lorsque vous souhaitez conserver le texte de la commande et le réexécuter, ou utiliser des paramètres de requête.  
  
 Pour créer un objet **Command** indépendamment d’un objet **Connection** défini précédemment, définissez sa propriété **ActiveConnection** sur une chaîne de connexion valide. ADO crée toujours un objet de **connexion** , mais il n’affecte pas cet objet à une variable objet. Toutefois, si vous associez plusieurs objets de **commande** à la même connexion, vous devez créer et ouvrir explicitement un objet de **connexion** ; Cela affecte l’objet de **connexion** à une variable objet. Assurez-vous que l’objet de **connexion** a été ouvert correctement avant de l’affecter à la propriété **ActiveConnection** de l’objet **Command** , car l’attribution d’un objet **Connection** fermé provoque une erreur. Si vous ne définissez pas la propriété **ActiveConnection** de l’objet **Command** sur cette variable objet, ADO crée un objet **Connection** pour chaque objet **Command** , même si vous utilisez la même chaîne de connexion.  
  
 Pour exécuter une **commande**, appelez-la par sa propriété [Name](./name-property-ado.md) sur l’objet **Connection** associé. La valeur de la propriété **ActiveConnection** de la **commande** doit être définie sur l’objet de **connexion** . Si la **commande** a des paramètres, transmettez leurs valeurs comme arguments à la méthode.  
  
 Si deux objets de **commande** ou plus sont exécutés sur la même connexion et que l’un des objets de **commande** est une procédure stockée avec des paramètres de sortie, une erreur se produit. Pour exécuter chaque objet de **commande** , utilisez des connexions distinctes ou déconnectez tous les autres objets de **commande** de la connexion.  
  
 La collection **Parameters** est le membre par défaut de l’objet **Command** . Par conséquent, les deux instructions de code suivantes sont équivalentes.  
  
```  
objCmd.Parameters.Item(0)  
objCmd(0)  
```  
  
-   L’objet **Command** n’est pas sûr pour l’écriture de scripts.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements de l’objet Command](./command-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Connection, objet (ADO)](./connection-object-ado.md)   
 [Parameters, collection (ADO)](./parameters-collection-ado.md)   
 [Properties, collection (ADO)](./properties-collection-ado.md)   
 [Annexe A : Fournisseurs](../../guide/appendixes/appendix-a-providers.md)