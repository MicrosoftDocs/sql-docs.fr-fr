---
description: Paramètres du projet (Chargement d’objets système) (OracleToSQL)
title: Paramètres du projet (chargement des objets système) (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9418cb34-d869-4d24-95b3-6cb9db949bb0
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 6d9e13882e35ae40209a50be546ba15630d7a5fc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067904"
---
# <a name="project-settingsloading-system-objects-oracletosql"></a>Paramètres du projet (Chargement d’objets système) (OracleToSQL)
La page chargement des objets système de la boîte de dialogue **paramètres du projet** vous permet de spécifier les objets système Oracle que SSMA convertit et charge dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
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
  
Si vos objets Oracle font référence à des objets système supplémentaires, vous devez sélectionner ces objets. Si vous ne sélectionnez pas les objets système qui sont référencés par vos objets de base de données Oracle, SSMA signale des erreurs de conversion. Si vous recevez des erreurs de conversion causées par des objets système manquants, sélectionnez les objets manquants dans cette boîte de dialogue. Vous pouvez ensuite répéter la conversion si nécessaire.  
  
