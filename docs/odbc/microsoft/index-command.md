---
description: INDEX, commande
title: INDEX, commande | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- index command [ODBC]
ms.assetid: 694e8cf5-2f69-4001-9c1e-b735a4da3aff
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5ac225132e2d89f1edd4baab012b2383cd508588
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183393"
---
# <a name="index-command"></a>INDEX, commande
Crée un fichier d’index pour afficher les enregistrements de table et y accéder dans un ordre logique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
INDEX ON eExpression TO IDXFileName | TAG TagName [OF CDXFileName]  
   [FOR lExpression]  
   [COMPACT]  
   [ASCENDING | DESCENDING]  
   [UNIQUE | CANDIDATE]  
   [ADDITIVE]  
```  
  
## <a name="arguments"></a>Arguments  
 *eExpression*  
 Spécifie une expression d’index qui peut inclure le nom d’un champ ou des champs de la table actuelle. Une clé d’index basée sur l’expression d’index est créée dans le fichier d’index pour chaque enregistrement de la table. Visual FoxPro utilise ces clés pour afficher les enregistrements et y accéder dans la table.  
  
> [!NOTE]  
>  Bien que cela ne soit pas recommandé, *eExpression* peut également être une variable de mémoire, un élément de tableau ou une expression de champ ou de champ d’une table dans une autre zone de travail. Les champs Mémo ne peuvent pas être utilisés seuls dans les expressions de fichier d’index ; elles doivent être combinées avec d’autres expressions de caractères. Si vous accédez à un index qui contient une variable ou un champ qui n’existe plus ou qui est introuvable, Visual FoxPro génère un message d’erreur.  
  
 Si vous tentez de créer un index avec une clé qui varie en fonction de la longueur, la clé est complétée par des espaces. Les clés d’index de longueur variable ne sont pas prises en charge dans Visual FoxPro.  
  
 Il est possible de créer une clé d’index avec une longueur nulle. Par exemple, une clé d’index de longueur égale à zéro est créée lorsque l’expression d’index est une sous-chaîne d’un champ Mémo vide. Une clé d’index de longueur égale à zéro génère un message d’erreur. Lorsque Visual FoxPro crée un index, il évalue les champs du premier enregistrement de la table. Si un champ est vide, il peut être nécessaire d’entrer des données temporaires dans le champ du premier enregistrement pour empêcher une clé d’index de longueur égale à 0.  
  
 À *IDXFileName*  
 Crée un fichier d’index. idx. L’extension par défaut est affectée au fichier d’index. idx.  
  
 TAG *tagname*[of *CDXFileName*]  
 Crée un fichier d’index composé. Un fichier d’index composé est un fichier d’index unique qui se compose d’un nombre quelconque de balises distinctes (entrées d’index). Chaque balise est identifiée par son nom de balise unique. Les noms de balises doivent commencer par une lettre ou un trait de soulignement et peuvent se composer d’une combinaison quelconque de 10 lettres, chiffres ou traits de soulignement. Le nombre de balises dans un fichier d’index composé est limité uniquement par la mémoire disponible et l’espace disque.  
  
 Les fichiers d’index composés à plusieurs entrées sont toujours compacts. Il n’est pas nécessaire d’inclure COMPACT lors de la création d’un fichier d’index composé. Les noms des fichiers d’index composés reçoivent une extension. CDX.  
  
 Deux types de fichiers d’index composés peuvent être créés : structurel et non structurel.  
  
 **Fichiers d’index composés structurels** Vous pouvez créer un fichier d’index composé structurel avec TAG *tagname* en excluant la clause facultative de *CDXFileName* . Un fichier d’index composé structurel a toujours le même nom de base que la table et s’ouvre automatiquement lorsque la table est ouverte.  
  
 **Fichiers d’index composés non structurels** Vous pouvez créer un fichier d’index composé non structurel en incluant *CDXFileName* après tag *tagname*. Contrairement à un fichier d’index composé structurel, un fichier d’index composé non structurel doit être ouvert explicitement avec la clause INDEX en cours d’utilisation.  
  
 Si un fichier d’index composé a déjà été créé et ouvert, l’émission de l’INDEX avec la BALIse *tagname* ajoute une balise au fichier d’index composé.  
  
 POUR *lExpression*  
 Spécifie une condition dans laquelle seuls les enregistrements qui satisfont à l’expression de filtre *lExpression* sont disponibles pour l’affichage et l’accès ; les clés d’index sont créées dans le fichier d’index uniquement pour les enregistrements correspondant à l’expression de filtre.  
  
 La technologie Rushmore Visual FoxPro optimise un INDEX... POUR la commande *lExpression* si *lExpression* est une expression optimisable. Pour de meilleures performances, utilisez une expression optimisable dans la clause FOR.  
  
 ROM  
 Crée un fichier compact. idx.  
  
 CROISSANT  
 Spécifie un ordre croissant pour le fichier. CDX. Par défaut, les balises. CDX sont créées dans l’ordre croissant. (Vous pouvez inclure l’ordre croissant comme rappel de l’ordre du fichier d’index.) Une table peut être indexée dans l’ordre inverse en incluant décroissant.  
  
 DÉCROISSANT  
 Spécifie un ordre décroissant pour le fichier. CDX. Vous ne pouvez pas inclure l’ordre décroissant lors de la création de fichiers d’index. idx.  
  
 UNIQUE  
 Spécifie que seul le premier enregistrement rencontré avec une valeur de clé d’index particulière est inclus dans un fichier. idx ou une balise. CDX. UNIQUE peut être utilisé pour empêcher l’affichage ou l’accès aux enregistrements dupliqués. Tous les enregistrements ajoutés avec des clés d’index dupliqués sont exclus du fichier d’index. L’utilisation de l’option UNIQUE de l’INDEX est identique à l’exécution de SET UNIQUE ON avant l’émission de l’INDEX ou de la réindexation.  
  
 Lorsqu’un index UNIQUE ou une balise d’index est actif et qu’un enregistrement dupliqué est modifié de manière à modifier sa clé d’index, l’index ou la balise d’index est mis à jour. Toutefois, il est impossible d’accéder à l’enregistrement dupliqué suivant avec la clé d’index d’origine ou de l’afficher tant que vous n’avez pas réindexé le fichier à l’aide de la réindexation.  
  
 FINALES  
 Crée une balise d’index structurel de candidat. Le mot clé candidat ne peut être inclus que lors de la création d’une balise d’index structurel. dans le cas contraire, Visual FoxPro génère un message d’erreur.  
  
 Une balise d’index candidate empêche les valeurs dupliquées dans le champ ou la combinaison de champs spécifiés dans l’expression d’index *eExpression*. Le terme *candidat* fait référence au type d’index ; étant donné que les index candidats empêchent les valeurs dupliquées, ils peuvent être considérés comme un « candidat » comme index primaire.  
  
 Visual FoxPro génère une erreur si vous créez une balise d’index candidat pour un champ ou une combinaison de champs qui contient déjà des valeurs dupliquées.  
  
 ADDITIF  
 Maintient l’ouverture des fichiers d’index précédemment ouverts. Si vous omettez la clause ADDITIVE lorsque vous créez un fichier d’index ou des fichiers pour une table avec un INDEX, tous les fichiers d’index précédemment ouverts (à l’exception de l’index composé structurel) sont fermés.  
  
## <a name="remarks"></a>Notes  
 Les enregistrements d’une table qui contient un fichier d’index sont affichés et accessibles dans l’ordre spécifié par l’expression d’index. L’ordre physique des enregistrements de la table n’est pas modifié par un fichier d’index.  
  
## <a name="index-types"></a>Types d’index  
 Visual FoxPro vous permet de créer deux types de fichiers d’index :  
  
-   Fichiers d’index composés. CDX contenant plusieurs entrées d’index appelées balises  
  
-   fichiers d’index. idx contenant une entrée d’index  
  
 Vous pouvez également créer un fichier d’index composé structurel, qui est automatiquement ouvert avec la table.  
  
> [!NOTE]  
>  Étant donné que les fichiers d’index composés structurels s’ouvrent automatiquement lorsque la table est ouverte, il s’agit du type d’index par défaut.  
  
 Incluez COMPACT pour créer des fichiers d’index compact. idx. Les fichiers d’index composés sont toujours compacts.  
  
## <a name="index-order-and-updating"></a>Ordre d’index et mise à jour  
 Un seul fichier d’index (le fichier d’index principal) ou une balise (la balise principale) contrôle l’ordre dans lequel la table est affichée ou accessible. Certaines commandes (par exemple, SEEK) utilisent le fichier d’index maître ou la balise pour rechercher des enregistrements. Toutefois, tous les fichiers d’index. idx et. CDX ouverts sont mis à jour à mesure que des modifications sont apportées à la table.  
  
## <a name="user-defined-functions"></a>Fonctions définies par l'utilisateur  
 Bien qu’une expression d’index puisse contenir une fonction définie par l’utilisateur, vous ne devez pas utiliser de fonctions définies par l’utilisateur dans une expression d’index. Les fonctions définies par l’utilisateur dans une expression d’index augmentent le temps nécessaire à la création ou à la mise à jour de l’index. En outre, les mises à jour d’index peuvent ne pas se produire lorsqu’une fonction définie par l’utilisateur est utilisée pour une expression d’index.  
  
 Si vous utilisez une fonction définie par l’utilisateur dans une expression d’index, Visual FoxPro doit être en mesure de trouver la fonction définie par l’utilisateur. Lorsque Visual FoxPro crée un index, l’expression d’index est enregistrée dans le fichier d’index, mais seule une référence à la fonction définie par l’utilisateur est incluse dans l’expression d’index.  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE-commande SQL](../../odbc/microsoft/alter-table-sql-command.md)   
 [SUPPRIMER la BALIse, commande](../../odbc/microsoft/delete-tag-command.md)   
 [SET COLLATE, commande](../../odbc/microsoft/set-collate-command.md)   
 [SET UNIQUE, commande](../../odbc/microsoft/set-unique-command.md)
