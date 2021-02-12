---
description: Rapport d’évaluation (AccessToSQL)
title: Rapport d’évaluation (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Assessment Report dialog box
- Conversion Report dialog box
ms.assetid: ba6f53aa-0049-4c49-8bb8-607a8bfaa737
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 7c7e8b8e282a59144628f70e0b2efc06a1f4b59f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076390"
---
# <a name="assessment-report-accesstosql"></a>Rapport d’évaluation (AccessToSQL)
La fenêtre rapport d’évaluation affiche les résultats de la conversion des objets de base de données en [!INCLUDE[tsql](../../includes/tsql-md.md)] syntaxe et peut également vous aider à estimer la complexité et le coût de vos projets de migration.  
  
Pour créer un rapport d’évaluation, sélectionnez les objets à convertir dans l’Explorateur de métadonnées source, cliquez avec le bouton droit sur **bases de données**, puis sélectionnez **créer un rapport**. Vous pouvez également afficher ce rapport automatiquement après la conversion de schémas. Toutefois, le nom du rapport sera un rapport de conversion. Pour plus d’informations, consultez [paramètres du projet (GUI) (SSMA commun)](../sybase/project-settings-gui-sybasetosql.md).  
  
## <a name="options"></a>Options  
**Volet Explorateur**  
Contient une hiérarchie d’objets dans le rapport d’évaluation. Développez les dossiers pour afficher des objets et des sous-composants individuels. Lorsque vous cliquez sur une catégorie ou un objet, les statistiques de conversion de cette catégorie ou de cet objet s’affichent dans le volet d’informations.  
  
**Volet Détails**  
Affiche les statistiques de conversion ou les messages d’erreur et d’avertissement pour l’objet sélectionné. Par exemple, si le dossier tables est sélectionné, le volet Détails affiche le nombre de clés étrangères, d’index, de clés primaires et de tables qui ont été convertis.  
  
**Volet Messages**  
Affiche les erreurs, avertissements et messages d’information qui ont été générés lors de la création du rapport d’évaluation. Les messages sont regroupés par nombre.  
  
Pour afficher les détails d’un message, cliquez sur **Erreurs**, **avertissements** ou **messages**, puis développez un message. SSMA affiche la liste des objets qui ont cette erreur. Cliquez sur un objet pour afficher tous les détails de la conversion de l’objet.  
  
## <a name="see-also"></a>Voir aussi  
[Référence de l’interface utilisateur (accès)](./user-interface-reference-accesstosql.md)  
