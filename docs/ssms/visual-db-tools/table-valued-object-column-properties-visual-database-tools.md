---
description: Propriétés de l’objet table (colonne) (Visual Database Tools)
title: Propriétés des objets table (colonne)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.designers.properties.QueryViewColumn
ms.assetid: 212d9bcd-aded-4313-a6b9-d7e2270e5954
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: f33c6df2e1acfa5c014b6739acd61c4d0153631c
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88312705"
---
# <a name="table-valued-object-column-properties-visual-database-tools"></a>Propriétés de l’objet table (colonne) (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 Ces propriétés s’affichent quand vous sélectionnez une colonne d’un objet table dans le volet **Schéma** du Concepteur de requêtes et de vues.  
  
> [!NOTE]  
> Les propriétés mentionnées dans cette rubrique sont classées par catégorie et non par ordre alphabétique.  
  
> [!NOTE]  
> Les boîtes de dialogue et les commandes de menu affichées peuvent différer de celles décrites dans l'Aide selon les paramètres actifs ou le mode d'édition. Pour modifier vos paramètres, choisissez **Paramètres d'importation et d'exportation** dans le menu **Outils** .  
  
**Catégorie Identité**  
Développe pour afficher la propriété **Nom** .  
  
**Nom**  
Affiche le nom de la colonne sélectionnée.  
  
**Catégorie Concepteur de requêtes**  
Peut être développée pour afficher les propriétés de **Autoriser les valeurs NULL**, **Classement**, **Type de données**, **Longueur**, **Précision**, **Échelle**et **Taille**.  
  
**Null autorisé**  
Précise si le type de données de la colonne autorise les valeurs NULL.  
  
**Classement**  
Affiche le paramètre de classement pour la colonne sélectionnée. Le classement peut être défini dans l'onglet Propriétés de la colonne du Concepteur de tables.  
  
**Type de données**  
Affiche le type de données de la colonne sélectionnée.  
  
**Longueur**  
Affiche le nombre de caractères ou de chiffres autorisé par le type de données de la colonne sélectionnée. Cette propriété est disponible uniquement pour les types de données basés sur les caractères.  
  
> [!NOTE]  
>  Pour afficher la taille en octets, consultez la propriété **Taille** ci-dessous.  
  
**Précision**  
Affiche le nombre maximal de chiffres autorisés pour les types de données numériques. Cette propriété affiche **0** pour les types de données non numériques.  
  
**Mise à l’échelle**  
Affiche le nombre maximal de chiffres qui peuvent apparaître à droite de la virgule décimale pour les types de données numériques. Cette valeur doit être inférieure ou égale à la précision. Cette propriété affiche **0** pour les types de données non numériques.  
  
**Taille**  
Affiche la taille en octets autorisée par le type de données de la colonne. Par exemple, un type de données nchar peut avoir une longueur de 10 (nombre de caractères), mais une taille de 20 pour tenir compte des jeux de caractères Unicode.  
  
