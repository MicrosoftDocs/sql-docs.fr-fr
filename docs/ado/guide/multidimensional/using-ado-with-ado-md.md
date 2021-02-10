---
description: Utilisation d’ADO avec ADO MD
title: Utilisation d’ADO avec ADO MD | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, using with ADO MD
ms.assetid: cfae435e-2ac3-4312-8c1e-9ca4a74cd875
author: rothja
ms.author: jroth
ms.openlocfilehash: d6844590043e0f806381f35c3f74123c24a6dc30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032161"
---
# <a name="using-ado-with-ado-md"></a>Utilisation d’ADO avec ADO MD
ADO et ADO MD sont associés à des modèles objet distincts. ADO fournit des objets pour se connecter à des sources de données, exécuter des commandes, récupérer des données tabulaires et des métadonnées de schéma dans un format tabulaire, et afficher des informations sur les erreurs du fournisseur. ADO MD fournit des objets pour la récupération de données multidimensionnelles et l’affichage des métadonnées de schéma multidimensionnel.  
  
 Quand vous utilisez un MDP, vous pouvez choisir d’utiliser ADO, ADO MD ou les deux avec votre application. En référençant les deux bibliothèques dans votre projet, vous aurez un accès complet à la fonctionnalité fournie par votre MDP.  
  
 Il est souvent utile pour les consommateurs d’obtenir une vue tabulaire aplatie d’un jeu de données multidimensionnel. Pour ce faire, vous pouvez utiliser l’objet [Recordset](../../reference/ado-api/recordset-object-ado.md) ADO. Spécifiez la source de votre ensemble de [cellules](../../reference/ado-md-api/cellset-object-ado-md.md) en tant que paramètre ***source** _ pour la méthode [Open](../../reference/ado-api/open-method-ado-recordset.md) d’un _ * Recordset * *, plutôt que comme la source d’un ensemble de **cellules** ADO MD.  
  
 Il peut également être utile d’afficher les métadonnées de schéma dans une vue tabulaire plutôt que sous la forme d’une hiérarchie d’objets. La méthode [OPENSCHEMA](../../reference/ado-api/openschema-method.md) ADO sur l’objet de [connexion](../../reference/ado-api/connection-object-ado.md) permet à l’utilisateur d’ouvrir un **jeu d’enregistrements** qui contient des informations de schéma. Le **paramètre _QueryType_*_ de la méthode _* OpenSchema** a plusieurs valeurs [SchemaEnum](../../reference/ado-api/schemaenum.md) qui se rapportent spécifiquement à MDPS. Ces valeurs sont les suivantes :  
  
-   **adSchemaCubes**  
  
-   **adSchemaDimensions**  
  
-   **adSchemaHierarchies**  
  
-   **adSchemaLevels**  
  
-   **adSchemaMeasures**  
  
-   **adSchemaMembers**  
  
 Pour utiliser des valeurs d’énumération ADO avec des propriétés ou des méthodes ADO MD, votre projet doit faire référence aux bibliothèques ADO et ADO MD. Par exemple, vous pouvez utiliser les valeurs d’énumération ADO **adState** avec la propriété [État](../../reference/ado-md-api/state-property-ado-md.md) de ADO MD. Pour plus d’informations sur la façon d’établir des références aux bibliothèques, consultez la documentation de votre outil de développement.  
  
 Pour plus d’informations sur les objets et les méthodes ADO, consultez Référence de l' [API ADO](../../reference/ado-api/ado-api-reference.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Modèle objet ADO MD](../../reference/ado-md-api/ado-md-object-model.md)   
 [ADO (multidimensionnel) (ADO MD)](./ado-multidimensional-ado-md.md)   
 [Vue d’ensemble des schémas et des données multidimensionnels](./overview-of-multidimensional-schemas-and-data.md)   
 [Programmation avec ADO MD](./programming-with-ado-md.md)   
 [Utilisation de données multidimensionnelles](./working-with-multidimensional-data.md)