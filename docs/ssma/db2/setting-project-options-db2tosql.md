---
description: Définition des options du projet (DB2ToSQL)
title: Définition des options du projet (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: f325a606-97ac-48bc-b344-b55f5e086a48
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 72e4fadcd1422ea0afacc0af5e8766ebff9e6386
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071943"
---
# <a name="setting-project-options-db2tosql"></a>Définition des options du projet (DB2ToSQL)
Pour chaque projet SSMA, vous pouvez définir des options au niveau du projet. Ces options spécifient la conversion d’objet, le chargement d’objet, l’interface utilisateur et les paramètres de migration de données. Avant de convertir des objets vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vérifiez que les options de configuration sont appropriées pour le projet.  
  
SSMA vous permet de configurer les options par défaut pour tous les projets. Ces options sont appliquées à tous les nouveaux projets que vous créez. Vous pouvez ensuite personnaliser les options pour chaque projet.  
  
## <a name="configuration-options-and-modes"></a>Options de configuration et modes  
SSMA comporte cinq jeux de paramètres de projet :  
  
-   Informations sur le projet  
  
-   Général (conversion, migration, chargement d’objets)  
  
-   Synchronization  
  
-   Interface graphique utilisateur  
  
-   Mappage de type  
  
Il comporte également quatre modes de configuration de ces paramètres :  
  
-   Default  
  
-   Optimistic  
  
-   Complète  
  
-   Personnalisé  
  
Le mode par défaut est recommandé pour la plupart des utilisateurs. Le mode optimiste conserve la syntaxe DB2 actuelle et est plus facile à lire. Toutefois, il se peut que la conservation de la syntaxe actuelle ne soit pas exacte. Si la syntaxe DB2 doit être convertie en syntaxe équivalente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le mode complet effectue la conversion la plus complète, mais le code obtenu peut être plus difficile à lire. Dans le mode personnalisé, vous définissez les options.  
  
Pour plus d’informations sur les paramètres et sur la façon dont les paramètres sont appliqués dans chaque mode, consultez les rubriques suivantes :  
  
-   [Paramètres du projet &#40;conversion&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-conversion-db2tosql.md)  
  
-   [Paramètres du projet &#40;&#41; &#40;de migration DB2ToSQL&#41;](../../ssma/db2/project-settings-migration-db2tosql.md)  
  
-   [Paramètres du projet&#40;synchronisation&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-synchronization-db2tosql.md)  
  
-   [Paramètres du projet &#40;GUI&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-gui-db2tosql.md)  
  
-   [Paramètres du projet &#40;le mappage de type&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-type-mapping-db2tosql.md)  
  
## <a name="setting-project-options"></a>Définition des options du projet  
Dans SSMA, vous pouvez configurer des paramètres par défaut pour tous les projets. Ces paramètres sont enregistrés dans le fichier de configuration SSMA et appliqués à tous les nouveaux projets que vous créez.  
  
**Pour définir les options de projet par défaut**  
  
1.  Dans le menu **Outils** , cliquez sur **paramètres de projet par défaut**.  
  
2.  Dans la boîte de dialogue **paramètres du projet par défaut** , utilisez l’une des procédures suivantes :  
  
    -   Sélectionnez le type de projet de migration pour lequel les paramètres doivent être affichés ou modifiés dans la liste déroulante **version cible** de la migration cliquez sur **général** en bas du volet gauche, puis sélectionnez conversion ou migration.  
  
    -   Pour sélectionner un mode prédéfini, dans la zone de liste déroulante **mode** , sélectionnez **par défaut**, **optimiste** ou **complète**.  
  
    -   Pour spécifier des paramètres personnalisés, sélectionnez ou entrez les nouveaux paramètres ou valeurs.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
Vous pouvez également personnaliser les paramètres du projet actuel. Ces paramètres sont enregistrés dans le fichier projet actif.  
  
**Pour personnaliser les paramètres du projet actif**  
  
1.  Dans le menu **Outils** , cliquez sur **paramètres du projet**.  
  
2.  Dans la boîte de dialogue **paramètres du projet** , utilisez l’une des procédures suivantes :  
  
    -   Pour sélectionner un mode prédéfini, dans la zone de liste déroulante **mode** , sélectionnez **par défaut**, **optimiste** ou **complète**.  
  
    -   Pour spécifier un mode personnalisé, dans la zone **mode** , sélectionnez **personnalisé**, puis sélectionnez les paramètres de projet appropriés.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante de la migration dépend des besoins de votre projet :  
  
-   Pour personnaliser le mappage des types de données sources et cibles, consultez [mappage des types de données DB2 et SQL Server &#40;les&#41;DB2ToSQL ](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md).  
  
-   Dans le cas contraire, vous pouvez convertir les définitions d’objet de base de données DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définitions d’objets. Pour plus d’informations, consultez [conversion de schémas DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Mappage des types de données DB2 et SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md)  
  
