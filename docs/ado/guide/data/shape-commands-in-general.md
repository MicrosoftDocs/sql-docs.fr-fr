---
description: Généralités sur les commandes SHAPE
title: Mettre en forme des commandes en général | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO]
- data shaping [ADO], shape commands
ms.assetid: 1fac7831-a187-4b15-9b43-aad380c5556c
author: rothja
ms.author: jroth
ms.openlocfilehash: 021d47eb011a4c0628b6f356772e82c591747103
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032461"
---
# <a name="shape-commands-in-general"></a>Généralités sur les commandes SHAPE
La mise en forme des données définit les colonnes d’un **jeu d’enregistrements** mis en forme, les relations entre les entités représentées par les colonnes et la manière dont le **Recordset** est rempli avec les données.  
  
 Un **Recordset** mis en forme peut être constitué des types de colonnes suivants.  
  
|Type de colonne|Description|  
|-----------------|-----------------|  
|data|Les champs d’un **jeu d’enregistrements** retournés par une commande de requête à un fournisseur de données, à une table ou à un **jeu d’enregistrements** précédemment mis en forme.|  
|chapitre|Référence à un autre **Recordset**, appelée *chapitre*. Les colonnes de chapitre permettent de définir une relation *parent-enfant* où le *parent* est le **Recordset** qui contient la colonne de chapitre et l' *enfant* est le **jeu d’enregistrements** représenté par le chapitre.|  
|aggregate|La valeur de la colonne est dérivée de l’exécution d’une *fonction d’agrégation* sur toutes les lignes ou une colonne de toutes les lignes d’un **jeu d’enregistrements** enfant. (Pour plus d’informations, consultez fonctions d’agrégation dans la rubrique suivante, [fonctions d’agrégation, fonction Calc et mot clé New](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md).)|  
|expression calculée|La valeur de la colonne est dérivée du calcul d’une expression Visual Basic pour Applications sur les colonnes de la même ligne du **Recordset**. L’expression est l’argument de la fonction CALC. (Voir l’expression calculée dans la rubrique suivante, [fonctions d’agrégation, fonction Calc et le mot clé New](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md) et dans les [fonctions Visual Basic pour applications](../../../ado/guide/data/visual-basic-for-applications-functions.md).)|  
|new|Champs vides, fabriqués, qui peuvent être remplis avec des données ultérieurement. La colonne est définie avec le mot clé NEW. (Consultez nouveau mot clé dans la rubrique suivante, [fonctions d’agrégation, fonction Calc et mot clé New](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md).)|  
  
 Une commande SHAPE peut contenir une clause qui spécifie une commande de requête à un fournisseur de données sous-jacent qui retournera un objet **Recordset** . La syntaxe de la requête dépend des spécifications du fournisseur de données sous-jacent. Il s’agit généralement de SQL, bien qu’ADO ne nécessite pas l’utilisation d’un langage de requête particulier.  
  
 Les commandes Shape peuvent être émises par des objets **Recordset** ou en définissant la propriété [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) de l’objet [Command](../../../ado/reference/ado-api/command-object-ado.md) , puis en appelant la méthode [Execute](../../../ado/reference/ado-api/execute-method-ado-command.md) .  
  
 Vous pouvez utiliser une clause JOIN SQL pour lier deux tables ; Toutefois, un **jeu d’enregistrements** hiérarchique peut représenter les informations plus efficacement. Chaque ligne d’un **jeu d’enregistrements** créé par une jointure répète des informations de façon redondante à partir de l’une des tables. Un **jeu d’enregistrements** hiérarchique n’a qu’un seul **jeu d’enregistrements** parent pour chacun des objets **Recordset** enfants multiples.  
  
 Les commandes Shape peuvent être imbriquées. Autrement dit, la commande *parente* ou la *commande enfant* peut elle-même être une autre commande Shape.  
  
 Le fournisseur Shape retourne toujours un curseur client, même lorsque l’utilisateur spécifie un emplacement de curseur **adUseServer**.  
  
 Vous pouvez accéder aux composants du **Recordset** de l' **objet Recordset** mis en forme par programmation ou par le biais d’un contrôle visuel approprié.  
  
 Microsoft fournit un outil visuel qui génère des commandes de forme (voir le [Concepteur d’environnement de données](/previous-versions/visualstudio/aa445793(v=vs.60)) dans la documentation Visual Basic 6) et un autre qui affiche des curseurs hiérarchiques (consultez « Utilisation du contrôle Microsoft Hierarchical FlexGrid » dans la documentation de Visual Basic 6).  
  
 Pour plus d’informations sur la navigation dans un **jeu d’enregistrements** hiérarchique, consultez [accès aux lignes d’un jeu d’enregistrements hiérarchique](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md).  
  
 Pour obtenir des informations précises sur les commandes de forme syntaxiquement correctes, consultez [grammaire de forme formelle](../../../ado/guide/data/formal-shape-grammar.md).  
  
 Cette section contient les rubriques suivantes :  
  
-   [Fonctions d’agrégation, fonction CALC et mot clé NEW](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md)  
  
-   [Émission de commandes vers le fournisseur de données sous-jacent](../../../ado/guide/data/issuing-commands-to-the-underlying-data-provider.md)