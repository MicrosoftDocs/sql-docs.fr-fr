---
description: Clause APPEND de la commande SHAPE
title: Clause APPEND de la forme | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO]
- data shaping [ADO], APPEND clause
- append clause [ADO]
ms.assetid: f90fcf55-6b24-401d-94e1-d65bd24bd342
author: rothja
ms.author: jroth
ms.openlocfilehash: 7c6d62d410e6709c93ad6e3d04db627f14f091a1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036989"
---
# <a name="shape-append-clause"></a>Clause APPEND de la commande SHAPE
La clause APPEND de la commande SHAPE ajoute une ou plusieurs colonnes à un **Recordset**. Souvent, ces colonnes sont des colonnes de chapitres, qui font référence à un **jeu d’enregistrements** enfant.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
SHAPE [parent-command [[AS] parent-alias]] APPEND column-list  
```  
  
## <a name="description"></a>Description  
 Les parties de cette clause sont les suivantes :  
  
 *commande parente*  
 Zéro ou l’un des éléments suivants (vous pouvez omettre complètement la *commande parente* ) :  
  
-   Commande de fournisseur placée entre accolades (" {} ") qui retourne un objet **Recordset** . La commande est émise pour le fournisseur de données sous-jacent, et sa syntaxe dépend des spécifications de ce fournisseur. Il s’agit généralement du langage SQL, bien qu’ADO ne nécessite pas de langage de requête particulier.  
  
-   Autre commande de forme incorporée entre parenthèses.  
  
-   Le mot clé TABLE, suivi du nom d’une table dans le fournisseur de données.  
  
 *parent-alias*  
 Alias facultatif qui fait référence au **jeu d’enregistrements** parent.  
  
 *colonne-liste*  
 Un ou plusieurs des éléments suivants :  
  
-   Colonne d’agrégation.  
  
-   Colonne calculée.  
  
-   Nouvelle colonne créée à l’aide de la clause NEW.  
  
-   Colonne de chapitre. Une définition de colonne de chapitre est placée entre parenthèses (« () »). Consultez la syntaxe suivante.  
  
```  
SHAPE [parent-command [[AS] parent-alias]]  
   APPEND (child-recordset [ [[AS] child-alias]   
      RELATE parent-column TO child-column | PARAMETER param-number, ... ])  
   [[AS] chapter-alias]   
   [, ... ]  
```  
  
## <a name="remarks"></a>Notes  
 *child-recordset*  
 -   Commande de fournisseur placée entre accolades (" {} ") qui retourne un objet **Recordset** . La commande est émise pour le fournisseur de données sous-jacent, et sa syntaxe dépend des spécifications de ce fournisseur. Il s’agit généralement du langage SQL, bien qu’ADO ne nécessite pas de langage de requête particulier.  
  
-   Autre commande de forme incorporée entre parenthèses.  
  
-   Nom d’un **jeu d’enregistrements** mis en forme existant.  
  
-   Le mot clé TABLE, suivi du nom d’une table dans le fournisseur de données.  
  
 *alias-enfant*  
 Alias qui fait référence au **Recordset** enfant.  
  
 *parent-column*  
 Colonne du **Recordset** retournée par la *commande parent-.*  
  
 *child-column*  
 Colonne du **Recordset** retournée par la *commande enfant*.  
  
 *Param-nombre*  
 Consultez [fonctionnement des commandes paramétrées](../../../ado/guide/data/operation-of-parameterized-commands.md).  
  
 *alias-Chapter*  
 Alias qui fait référence à la colonne de chapitre ajoutée au parent.  
  
> [!NOTE]
>  La clause *« parent-colonne* à *enfant-colonne »* est en fait une liste, où chaque relation définie est séparée par une virgule.  
  
> [!NOTE]
>  La clause après le mot clé APPEND est en fait une liste, où chaque clause est séparée par une virgule et définit une autre colonne à ajouter au parent.  
  
Lorsque vous construisez des commandes de fournisseur à partir de l’entrée utilisateur dans le cadre d’une commande de forme, SHAPE traite la commande de fournisseur fournie par l’utilisateur comme une chaîne opaque et la transmet fidèlement au fournisseur. Par exemple, dans la commande SHAPE suivante,  
  
```  
SHAPE {select * from t1} APPEND ({select * from t2} RELATE k1 TO k2)  
```  
  
 SHAPE exécutera deux commandes : `select * from t1` et ( `select * from t2 RELATE k1 TO k2)` . Si l’utilisateur fournit une commande composée composée de plusieurs commandes fournisseur séparées par des points-virgules, la forme n’est pas en mesure de distinguer la différence. Par conséquent, dans la commande SHAPE suivante,  
  
```  
SHAPE {select * from t1; drop table t1} APPEND ({select * from t2} RELATE k1 TO k2)  
```  
  
 SHAPE s’exécute `select * from t1; drop table t1` et ( `select * from t2 RELATE k1 TO k2),` sans réaliser qu’il `drop table t1` s’agit d’un distinct et dans ce cas, une commande de fournisseur dangereuse. Les applications doivent toujours valider l’entrée utilisateur pour empêcher l’apparition de telles attaques potentielles de pirates.  
  
 Cette section contient les rubriques suivantes :  
  
-   [Fonctionnement des commandes non paramétrées](../../../ado/guide/data/operation-of-non-parameterized-commands.md)  
  
-   [Fonctionnement des commandes paramétrées](../../../ado/guide/data/operation-of-parameterized-commands.md)  
  
-   [Commandes hybrides](../../../ado/guide/data/hybrid-commands.md)  
  
-   [Clauses COMPUTE intermédiaires de la commande SHAPE](../../../ado/guide/data/intervening-shape-compute-clauses.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de mise en forme des données](../../../ado/guide/data/data-shaping-example.md)   
 [Grammaire de forme formelle](../../../ado/guide/data/formal-shape-grammar.md)   
 [Généralités sur les commandes SHAPE](../../../ado/guide/data/shape-commands-in-general.md)
