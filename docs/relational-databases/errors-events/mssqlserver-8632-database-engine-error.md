---
description: MSSQLSERVER_8632
title: MSSQLSERVER_8632
ms.custom: ''
ms.date: 10/27/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8632 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: b7c59634423bd6df623725720bf8bd0d262d5be0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348691"
---
# <a name="mssqlserver_8632"></a>MSSQLSERVER_8632
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|8632|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|QUERY_EXPRESSION_TOO_COMPLEX|
|Texte du message|Erreur interne : une limite des services d’expression est dépassée. Recherchez les expressions complexes dans votre requête et simplifiez-les.|
||

## <a name="explanation"></a>Explication

L’erreur 8632 est générée quand vous exécutez une requête dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contient un grand nombre d’identificateurs et de constantes dans une expression unique. Un message d’erreur semblable au suivant s’affiche :

> Serveur :  Message 8632, niveau 17, état 2, ligne 1  
Erreur interne : une limite des services d’expression est dépassée. Recherchez les expressions complexes dans votre requête et simplifiez-les.

## <a name="cause"></a>Cause

Ce problème se produit car [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] limite le nombre d’identificateurs et de constantes pouvant être contenus dans une seule expression d’une requête. Cette limite s’élève à 65 535. Par exemple, la requête suivante ne comprend qu’une seule expression :

```sql
select a, b + c, d + e
```

Cette expression récupère les cinq colonnes, calcule les opérateurs d’addition et envoie trois résultats projetés au client.

Le test du nombre d’identificateurs et de constantes est effectué une fois que après le développement, par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], de l’ensemble des identificateurs et constantes référencés. Par exemple, les éléments suivants peuvent être développés :

- L’astérisque (*) dans la liste de sélection
- Une vue
- Une définition de colonne calculée

Si le nombre après l’expansion dépasse la limite, la requête ne peut pas être exécutée.

## <a name="user-action"></a>Action requise

Pour contourner ce problème, réécrivez votre requête. Référencez moins d’identificateurs et de constantes dans la plus grande expression de la requête. Vous devez garantir que le nombre d’identificateurs et de constantes figurant dans chaque expression de la requête ne dépasse pas la limite. Pour ce faire, vous devrez peut-être décomposer une requête en plusieurs requêtes. Créez ensuite un résultat intermédiaire temporaire.
