---
description: Paramètres du projet (chargement des objets système) (DB2ToSQL)
title: Paramètres du projet (chargement des objets système) (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9a545233-1b0a-488a-a1ec-c33aa608dcc1
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 79f359fa2a45db75fd48a6f9bbcbb774044c942a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072244"
---
# <a name="project-settingsloading-system-objects-db2tosql"></a>Paramètres du projet (chargement des objets système) (DB2ToSQL)
La page chargement des objets système de la boîte de dialogue **paramètres du projet** vous permet de spécifier les objets système DB2 que SSMA convertit et charge dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Le volet chargement des objets système est disponible dans les boîtes de dialogue **paramètres du projet** et **paramètres du projet par défaut** :  
  
-   Pour spécifier les paramètres de tous les projets SSMA, dans le menu **Outils** , sélectionnez **paramètres du projet par défaut**, sélectionnez le type de projet de migration pour lequel les paramètres doivent être affichés ou modifiés dans la liste déroulante cible de la **migration** , cliquez sur **général** en bas du volet gauche, puis cliquez sur **chargement des objets système**.  
  
-   Pour spécifier les paramètres du projet actif, dans le menu **Outils** , sélectionnez **paramètres du projet**, cliquez sur **général** en bas du volet gauche, puis cliquez sur **chargement des objets système**.  
  
## <a name="default-settings"></a>Paramètres par défaut  
La conversion d’objets système consomme des ressources système et prend du temps. Pour améliorer les performances, SSMA sélectionne uniquement les objets système les plus fréquemment utilisés, comme indiqué dans la liste suivante :  
  
-   Table. DBMS_OUTPUT  
  
-   Table. DBMS_PIPE  
  
-   Table. DBMS_UTILITY  
  
-   Table. HIVER  
  
-   Table. UTL_FILE  
  
-   Table. DBMS_LOB  
  
-   Table. DBMS_SQL  
  
-   Table. DBMS_SESSION  
  
Si vos objets DB2 font référence à des objets système supplémentaires, vous devez sélectionner ces objets. Si vous ne sélectionnez pas les objets système qui sont référencés par vos objets de base de données DB2, SSMA signale des erreurs de conversion. Si vous recevez des erreurs de conversion causées par des objets système manquants, sélectionnez les objets manquants dans cette boîte de dialogue. Vous pouvez ensuite répéter la conversion si nécessaire.  
  
