---
description: Paramètres du projet (migration) (AccessToSQL)
title: Paramètres du projet (migration) (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Migration settings
- Project Settings dialog box, Migration
ms.assetid: 4caebc9c-8680-4b99-a8fa-89c43161c95d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e1771bebb7619cbbc752485fb649331dab62d0dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066411"
---
# <a name="project-settings-migration-accesstosql"></a>Paramètres du projet (migration) (AccessToSQL)
Les paramètres du projet de migration vous permettent de configurer la façon dont les données sont migrées vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure.  
  
Le volet migration est disponible dans les boîtes de dialogue **paramètres du projet** et **paramètres du projet par défaut** .  
  
-   Utilisez la boîte de dialogue **paramètres du projet** pour définir les options de configuration du projet actif. Pour accéder aux paramètres de migration, dans le menu **Outils** , sélectionnez **paramètres du projet**, cliquez sur **général** en bas du volet gauche, puis cliquez sur **migration**.  
  
-   Utilisez la boîte de dialogue **paramètres du projet par défaut** pour définir les options de configuration de tous les projets. Pour accéder aux paramètres de migration, dans le menu **Outils** , sélectionnez **paramètres du projet par défaut**, sélectionnez le type de projet dans la zone de liste déroulante cible de la **migration** dont vous souhaitez accéder aux paramètres, cliquez sur **général** en bas du volet gauche, puis cliquez sur **migration**.  
  
## <a name="options"></a>Options  
**Contraintes de validation**  
Spécifie si SSMA doit vérifier les contraintes lorsqu’il ajoute des données aux tables.  
  
-   **Mode par défaut**: false  
  
-   **Mode optimiste**: true  
  
-   **Mode complet**: false  
  
**Exécuter les déclencheurs**  
Spécifie si SSMA doit déclencher les déclencheurs d’insertion lorsqu’il ajoute des données aux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tables.  
  
-   **Mode par défaut**: false  
  
-   **Mode optimiste**: true  
  
-   **Mode complet**: false  
  
**Conserver l'identité**  
Spécifie si SSMA conserve les valeurs d’identité d’accès lorsqu’il ajoute des données à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si cette valeur est false, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assigne des valeurs d’identité.  
  
-   **Mode par défaut**: true  
  
-   **Mode optimiste**: true  
  
-   **Mode complet**: false  
  
**Conserver les valeurs NULL**  
Spécifie si SSMA conserve les valeurs NULL dans les données sources lorsqu’il ajoute des données à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , quelles que soient les valeurs par défaut spécifiées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   **Mode par défaut**: true  
  
-   **Mode optimiste**: false  
  
-   **Mode complet**: true  
  
**Verrous de table**  
Spécifie si SSMA verrouille des tables lorsqu’il ajoute des données aux tables pendant la migration des données. Si la valeur est false, SSMA utilise des verrous de ligne.  
  
-   **Mode par défaut**: true  
  
-   **Mode optimiste**: true  
  
-   **Mode complet**: true  
  
**Remplacer les dates non prises en charge**  
Spécifie si SSMA doit corriger les dates d’accès antérieures à la date du DateTime la plus ancienne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (01 janvier 1753).  
  
-   Pour conserver les valeurs de date actuelles, sélectionnez **ne rien faire**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’accepte pas les dates antérieures au 1er janvier 1753 dans une colonne DateTime. Si vous utilisez des dates antérieures, vous devez convertir les valeurs DateTime en valeurs de caractères.  
  
-   Pour convertir les dates antérieures au 1er janvier 1753 en NULL, sélectionnez **remplacer par null**.  
  
-   Pour remplacer les dates antérieures au 1er janvier 1753 par une Date prise en charge, sélectionnez **remplacer par la date de prise en charge la plus proche**. Si vous sélectionnez cette valeur, par défaut, la date de prise en charge la plus proche sera sélectionnée en tant que 01 janvier 1753.  
  
**Taille du lot**  
Taille de lot utilisée lors de la migration des données. Une transaction est journalisée après chaque lot. Par défaut, la taille de lot pour tous les schémas est 10000.  
  
## <a name="see-also"></a>Voir aussi  
[Référence de l’interface utilisateur (accès)](./user-interface-reference-accesstosql.md)  
