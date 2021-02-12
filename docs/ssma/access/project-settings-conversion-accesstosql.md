---
description: Paramètres du projet (conversion) (AccessToSQL)
title: Paramètres du projet (conversion) (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- conversion, options described
- Project Settings dialog box, Conversion
ms.assetid: bcebc635-c638-4ddb-924c-b9ccfef86388
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a7390c30cd245a3f8a46784ec12e97752173a629
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066474"
---
# <a name="project-settings-conversion-accesstosql"></a>Paramètres du projet (conversion) (AccessToSQL)
Les paramètres du projet de conversion vous permettent de configurer la façon dont les objets sont convertis à partir des objets de base de données Access vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database objets.  
  
Le volet conversion est disponible dans les boîtes de dialogue **paramètres du projet** et **paramètres du projet par défaut** .  
  
-   Utilisez la boîte de dialogue **paramètres du projet** pour définir les options de configuration du projet actif. Pour accéder aux paramètres de conversion, dans le menu **Outils** , sélectionnez **paramètres du projet**, cliquez sur **général** en bas du volet gauche, puis sélectionnez **conversion**.  
  
-   Utilisez la boîte de dialogue **paramètres du projet par défaut** pour définir les options de configuration de tous les projets. Pour accéder aux paramètres de conversion, dans le menu **Outils** , sélectionnez **paramètres du projet par défaut**, sélectionnez le type de projet de migration pour lequel les paramètres doivent être affichés/Changed à partir de la liste déroulante de la **version cible** de la migration, cliquez sur **général** en bas du volet gauche, puis sélectionnez **conversion**.  
  
## <a name="options"></a>Options  
**Ajouter une clé primaire**  
Crée une nouvelle clé primaire dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table ou SQL Azure si une table Access n’a pas de clé primaire ou d’index unique.  
  
-   **Mode par défaut**: false  
  
-   **Mode optimiste**: false  
  
-   **Mode complet**: true  
  
En cas de connexion à SQL Azure, la valeur par défaut est true. **Ajouter des colonnes timestamp**  
Spécifie si SSMA doit créer une valeur d’horodatage si nécessaire.  
  
-   **Mode par défaut**: laisser SSMA décider  
  
-   **Mode optimiste**: jamais  
  
-   **Mode complet**: laisser SSMA décider  
  
**Inclure un rapport d’évaluation des données avec des rapports d’évaluation de la conversion**  
Comprend une évaluation des données dans le rapport d’évaluation.  
  
-   **Mode par défaut**: true  
  
-   **Mode optimiste**: false  
  
-   **Mode complet**: true  
  
**Type de message lorsqu’une clé primaire contient des colonnes Nullable**  
Spécifie le type de message (avertissement, erreur ou rien) que SSMA affiche dans le volet de sortie lorsqu’il trouve des clés primaires avec des colonnes Nullable.  
  
-   **Mode par défaut**: AVERTISSEMENT  
  
-   **Mode optimiste**: aucun message  
  
-   **Mode complet**: erreur  
  
**Type de message lorsque les colonnes clés étrangères sont de tailles différentes**  
Spécifie le type de message (avertissement, erreur ou rien) que SSMA affiche dans le volet de sortie lorsqu’il trouve une clé étrangère de texte incorrecte.  
  
-   **Mode par défaut**: AVERTISSEMENT  
  
-   **Mode optimiste**: aucun message  
  
-   **Mode complet**: erreur  
  
**Type de message lorsque les colonnes mémo sont indexées**  
Spécifie le type de message (avertissement, erreur ou rien) que SSMA affiche dans le volet sortie lorsqu’il trouve un index qui contient une colonne **Mémo** .  
  
-   **Mode par défaut**: AVERTISSEMENT  
  
-   **Mode optimiste**: aucun message  
  
-   **Mode complet**: erreur  
  
**Avertir quand une requête complexe utilise un caractère générique ( \& #42 ;)**  
Affiche un avertissement dans le volet de sortie et Liste d’erreurs lorsqu’un nom de colonne dans une instruction SELECT est un caractère générique (*).  
  
-   **Mode par défaut**: true  
  
-   **Mode optimiste**: false  
  
-   **Mode complet**: true  
  
**Avertir lors de la modification du nom de l’identificateur**  
Affiche un message dans le rapport d’évaluation et dans le volet sortie lorsqu’un nom d’identificateur d’objet est modifié par SSMA.  
  
-   **Mode par défaut**: true  
  
-   **Mode optimiste**: false  
  
-   **Mode complet**: true  
  
**Avertir quand l’identificateur sera placé entre guillemets**  
Affiche un message dans le rapport d’évaluation et dans le volet sortie lorsqu’un nom d’identificateur d’objet est mis entre guillemets par SSMA. Les identificateurs de guillemets sont nécessaires lorsque le nom est un mot clé ou qu’il contient des caractères spéciaux.  
  
-   **Mode par défaut**: true  
  
-   **Mode optimiste**: false  
  
-   **Mode complet**: true  
  
## <a name="see-also"></a>Voir aussi  
[Référence de l’interface utilisateur (accès)](./user-interface-reference-accesstosql.md)  
