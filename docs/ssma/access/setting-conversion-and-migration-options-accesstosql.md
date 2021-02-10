---
description: Définition des options de conversion et de migration (AccessToSQL)
title: Définition des options de conversion et de migration (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- conversion, setting options
- migration options
- modes
- options, conversion settings
- project settings
- schemas
ms.assetid: 0a7304df-2f35-4453-96ef-7ac83dea1167
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 154eaa42bf08f622f3e08359f025284752134bc5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066314"
---
# <a name="setting-conversion-and-migration-options-accesstosql"></a>Définition des options de conversion et de migration (AccessToSQL)
Pour chaque projet SSMA, vous pouvez définir des options au niveau du projet. Ces options spécifient la façon dont les objets sont convertis, la façon dont les données sont migrées et la manière dont les types de données sources sont mappés aux types de données cibles. Avant de convertir des objets en SQL Azure ou de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] migrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des données vers ou SQL Azure, vérifiez que les options de configuration sont appropriées pour le projet.  
  
## <a name="configuration-options-and-modes"></a>Options de configuration et modes  
SSMA possède quatre jeux de paramètres de configuration et quatre modes de configuration de ces paramètres : par défaut, optimiste, complet et personnalisé. Le mode par défaut est recommandé pour la plupart des utilisateurs. Utilisez le mode optimiste pour les conversions simples. Utilisez le mode complet si vous souhaitez afficher tous les messages. Dans le mode personnalisé, vous définissez les options.  
  
Les paramètres sont décrits dans la section « Référence de l’interface utilisateur » de cette documentation. Pour plus d’informations sur les paramètres et sur la façon dont les paramètres sont appliqués dans chaque mode, consultez les rubriques suivantes :  
  
-   [Paramètres du projet (Conversion)](./project-settings-conversion-accesstosql.md)  
  
-   [Paramètres du projet (Migration)](./project-settings-migration-accesstosql.md)  
  
-   [Paramètres du projet (GUI)](../sybase/project-settings-gui-sybasetosql.md)  
  
-   [Paramètres du projet (Mappage de type)](./project-settings-type-mapping-accesstosql.md)  
  
-   [Paramètres du projet (SQL Azure)](./project-settings-azure-sql-db-accesstosql.md)  
  
## <a name="setting-project-options"></a>Définition des options du projet  
Dans SSMA, vous pouvez configurer des paramètres par défaut pour tous les projets. Ces paramètres sont enregistrés dans le fichier de configuration SSMA et appliqués à tous les nouveaux projets que vous créez.  
  
**Pour définir les options de projet par défaut**  
  
1.  Dans le menu **Outils** , sélectionnez **paramètres du projet par défaut**.  
  
2.  Dans la boîte de dialogue **paramètres du projet par défaut** , effectuez l’une des opérations suivantes :  
  
    -   Sélectionnez le type de projet de migration pour lequel les paramètres doivent être affichés/modifiés dans la liste déroulante de la **version cible** de la migration, cliquez sur **général** en bas du volet gauche, puis sélectionnez **conversion ou migration ou SQL Azure**.  
  
        > [!NOTE]  
        > SQL Azure option est disponible sous l’onglet **général** uniquement si le type de projet créé est SQL Azure.  
  
    -   Pour sélectionner un mode prédéfini, sélectionnez **par défaut**, **optimiste** ou **complète** dans la zone de liste déroulante **mode** .  
  
    -   Pour spécifier un mode personnalisé, sélectionnez **personnalisé** dans la zone **mode** , sélectionnez une option dans le volet gauche, cliquez sur le paramètre ou la valeur dans le volet droit, puis sélectionnez ou entrez le nouveau paramètre ou la nouvelle valeur.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
Vous pouvez également personnaliser les paramètres du projet actuel. Ces paramètres sont enregistrés dans le fichier projet actif.  
  
**Pour personnaliser les paramètres du projet actif**  
  
1.  Dans le menu **Outils** , sélectionnez **paramètres du projet**.  
  
2.  Dans la boîte de dialogue **paramètres du projet** , effectuez l’une des opérations suivantes :  
  
    -   Pour sélectionner un mode prédéfini, sélectionnez **par défaut**, **optimiste** ou **complète** dans la zone de liste déroulante **mode** .  
  
    -   Pour spécifier un mode personnalisé, sélectionnez **personnalisé** dans la zone **mode** , sélectionnez une option dans le volet gauche, cliquez sur le paramètre ou la valeur dans le volet droit, puis sélectionnez ou entrez le nouveau paramètre ou la nouvelle valeur.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante de la migration dépend des besoins de votre projet :  
  
-   Pour personnaliser le mappage des types de données sources et cibles, consultez [mappage des types de données sources et cibles](mapping-source-and-target-data-types-accesstosql.md)  
  
-   Pour personnaliser le mappage des bases de données source et cible, consultez [mappage des bases de données source et cible](mapping-source-and-target-databases-accesstosql.md) .  
  
-   Dans le cas contraire, vous pouvez convertir les définitions d’objet de base de données Access en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL Azure définitions d’objets. Pour plus d’informations, consultez [conversion d’objets de base de données Access](converting-access-database-objects-accesstosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
