---
description: Rapport d’évaluation (OracleToSQL)
title: Rapport d’évaluation (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 168b7465-a6d6-4329-b46e-fc6c5a3f2d9d
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: b4f66820e4745d16484b81a5723e03c0acad5cbe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058734"
---
# <a name="assessment-report-oracletosql"></a>Rapport d’évaluation (OracleToSQL)
La fenêtre rapport d’évaluation affiche les résultats de la conversion des objets de base de données en [!INCLUDE[tsql](../../includes/tsql-md.md)] syntaxe et peut également vous aider à estimer la complexité et le coût de vos projets de migration.  
  
Pour accéder au rapport d’évaluation, sélectionnez les objets à convertir dans l’Explorateur de métadonnées sources, cliquez avec le bouton droit sur **schémas** ou **synonymes**, puis sélectionnez **créer un rapport**.  
  
## <a name="options"></a>Options  
  
|Terme|Définition|  
|-|-|  
|**Statistiques de conversion**|Affiche les statistiques de conversion par type d’instruction. Ce volet est visible lorsqu’un objet de groupe, tel qu’un schéma, ou un objet sans code est sélectionné dans le volet gauche.|  
|**Objets par catégories**|Affiche le nombre d’objets par catégorie. Ce volet est visible uniquement lorsqu’un objet de groupe, tel qu’un schéma, ou un objet sans code est sélectionné dans le volet gauche.|  
|**Statistiques**|Affiche les statistiques de conversion pour l’objet sélectionné. Ce volet est visible uniquement lorsqu’un objet individuel avec code est sélectionné dans le volet gauche. Vous devrez peut-être développer les **statistiques**, qui est immédiatement au-dessus du volet **source** , pour afficher ce volet.|  
|**Source**|Affiche le code Oracle pour l’objet sélectionné et met en surbrillance le code qui n’a pas été converti en [!INCLUDE[tsql](../../includes/tsql-md.md)] . Ce volet est visible uniquement lorsqu’un objet individuel avec code est sélectionné dans le volet gauche.<br /><br />Cliquez sur les numéros de ligne pour définir ou supprimer des signets. Utilisez les boutons en haut du volet pour naviguer dans le code.|  
|**Cible**|Affiche le code résultant de [!INCLUDE[tsql](../../includes/tsql-md.md)] la conversion pour l’objet sélectionné, ainsi que les messages d’erreur pour le code qui n’a pas été converti. Ce volet est visible uniquement lorsqu’un objet individuel avec code est sélectionné dans le volet gauche.<br /><br />Cliquez sur les numéros de ligne pour définir ou supprimer des signets. Utilisez les boutons en haut du volet pour naviguer dans le code.|  
|**Volet Messages**|Affiche les erreurs, les avertissements et les messages d’information qui ont été générés lors de la création du rapport d’évaluation. Les messages sont regroupés par nombre. Pour afficher le code à l’origine de l’erreur, cliquez sur **Erreurs**, **avertissements** ou **informations**, développez la catégorie des messages, puis cliquez sur un message.|  
  
