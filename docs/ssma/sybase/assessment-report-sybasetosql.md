---
title: Rapport d’évaluation (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: af24f2c4-5e86-4135-a4f3-a24faaeeefe7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 11b47065fe180956d58361ec80eda1dac25fa4b1
ms.sourcegitcommit: e8f6c51d4702c0046aec1394109bc0503ca182f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87932320"
---
# <a name="assessment-report-sybasetosql"></a>Rapport d’évaluation (SybaseToSQL)
La fenêtre rapport d’évaluation affiche les résultats de la conversion des objets de base de données en [!INCLUDE[tsql](../../includes/tsql-md.md)] syntaxe et peut également vous aider à estimer la complexité et le coût de vos projets de migration.  
  
Pour accéder au rapport d’évaluation, sélectionnez les objets à convertir dans l’Explorateur de métadonnées sources, cliquez avec le bouton droit sur **bases de données**, puis sélectionnez **créer un rapport**.  
  
## <a name="options"></a>Options  
**Statistiques de conversion**  
Affiche les statistiques de conversion par type d’instruction. Ce volet est visible uniquement lorsqu’un objet de groupe, tel qu’un schéma, ou un objet sans code est sélectionné dans le volet gauche.  
  
**Objets par catégorie**  
Affiche les statistiques de conversion par type d’objet. Ce volet est visible uniquement lorsqu’un objet de groupe, tel qu’un schéma, ou un objet sans code est sélectionné dans le volet gauche.  
  
**Statistiques**  
Affiche les statistiques de conversion pour l’objet sélectionné. Ce volet est visible uniquement lorsqu’un objet individuel avec code est sélectionné dans le volet gauche. Vous devrez peut-être développer les **statistiques** pour afficher ce volet.  
  
**Navigation source**  
Affiche le code de l’ASE pour l’objet sélectionné et met en surbrillance le code qui n’a pas été converti en [!INCLUDE[tsql](../../includes/tsql-md.md)] . Ce volet est visible uniquement lorsqu’un objet individuel avec code est sélectionné dans le volet gauche.  
  
Cliquez sur les numéros de ligne pour définir ou supprimer des signets. Utilisez les boutons en haut du volet pour naviguer dans le code.  
  
**Navigation cible**  
Affiche le code résultant de [!INCLUDE[tsql](../../includes/tsql-md.md)] la conversion pour l’objet sélectionné, ainsi que les messages d’erreur pour le code qui n’a pas été converti. Ce volet est visible uniquement lorsqu’un objet individuel avec code est sélectionné dans le volet gauche.  
  
Cliquez sur les numéros de ligne pour définir ou supprimer des signets. Utilisez les boutons en haut du volet pour naviguer dans le code.  
  
**Volet Messages**  
Affiche les erreurs, avertissements et messages d’information générés lors de la création du rapport d’évaluation. Les messages sont regroupés par nombre. Pour afficher le code à l’origine de l’erreur, cliquez sur **erreur**, **Avertissement**ou **informations**, développez la catégorie des messages, puis cliquez sur un message.  
  
