---
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
ms.openlocfilehash: e074f603586afa0322d62872c49abb52fe47f047
ms.sourcegitcommit: e8f6c51d4702c0046aec1394109bc0503ca182f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87933960"
---
# <a name="setting-conversion-and-migration-options-accesstosql"></a>Définition des options de conversion et de migration (AccessToSQL)
Pour chaque projet SSMA, vous pouvez définir des options au niveau du projet. Ces options spécifient la façon dont les objets sont convertis, la façon dont les données sont migrées et la manière dont les types de données sources sont mappés aux types de données cibles. Avant de convertir des objets en SQL Azure ou de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] migrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des données vers ou SQL Azure, vérifiez que les options de configuration sont appropriées pour le projet.  
  
## <a name="configuration-options-and-modes"></a>Options de configuration et modes  
SSMA possède quatre jeux de paramètres de configuration et quatre modes de configuration de ces paramètres : par défaut, optimiste, complet et personnalisé. Le mode par défaut est recommandé pour la plupart des utilisateurs. Utilisez le mode optimiste pour les conversions simples. Utilisez le mode complet si vous souhaitez afficher tous les messages. Dans le mode personnalisé, vous définissez les options.  
  
Les paramètres sont décrits dans la section « Référence de l’interface utilisateur » de cette documentation. Pour plus d’informations sur les paramètres et sur la façon dont les paramètres sont appliqués dans chaque mode, consultez les rubriques suivantes :  
  
-   [Paramètres du projet (Conversion)](https://msdn.microsoft.com/bcebc635-c638-4ddb-924c-b9ccfef86388)  
  
-   [Paramètres du projet (Migration)](https://msdn.microsoft.com/4caebc9c-8680-4b99-a8fa-89c43161c95d)  
  
-   [Paramètres du projet (GUI)](https://msdn.microsoft.com/cf06baf1-8714-48a3-95dc-781f6ca53693)  
  
-   [Paramètres du projet (Mappage de type)](https://msdn.microsoft.com/b87b9683-abed-4677-8c50-18bdba704655)  
  
-   [Paramètres du projet (SQL Azure)](https://msdn.microsoft.com/bbb8a204-d0e4-4f0b-9709-271feb1f136e)  
  
## <a name="setting-project-options"></a>Définition des options du projet  
Dans SSMA, vous pouvez configurer des paramètres par défaut pour tous les projets. Ces paramètres sont enregistrés dans le fichier de configuration SSMA et appliqués à tous les nouveaux projets que vous créez.  
  
**Pour définir les options de projet par défaut**  
  
1.  Dans le menu **Outils** , sélectionnez **paramètres du projet par défaut**.  
  
2.  Dans la boîte de dialogue **paramètres du projet par défaut** , effectuez l’une des opérations suivantes :  
  
    -   Sélectionnez le type de projet de migration pour lequel les paramètres doivent être affichés/modifiés dans la liste déroulante de la **version cible** de la migration, cliquez sur **général** en bas du volet gauche, puis sélectionnez **conversion ou migration ou SQL Azure**.  
  
        > [!NOTE]  
        > SQL Azure option est disponible sous l’onglet **général** uniquement si le type de projet créé est SQL Azure.  
  
    -   Pour sélectionner un mode prédéfini, sélectionnez **par défaut**, **optimiste**ou **complète** dans la zone de liste déroulante **mode** .  
  
    -   Pour spécifier un mode personnalisé, sélectionnez **personnalisé** dans la zone **mode** , sélectionnez une option dans le volet gauche, cliquez sur le paramètre ou la valeur dans le volet droit, puis sélectionnez ou entrez le nouveau paramètre ou la nouvelle valeur.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
Vous pouvez également personnaliser les paramètres du projet actuel. Ces paramètres sont enregistrés dans le fichier projet actif.  
  
**Pour personnaliser les paramètres du projet actif**  
  
1.  Dans le menu **Outils** , sélectionnez **paramètres du projet**.  
  
2.  Dans la boîte de dialogue **paramètres du projet** , effectuez l’une des opérations suivantes :  
  
    -   Pour sélectionner un mode prédéfini, sélectionnez **par défaut**, **optimiste**ou **complète** dans la zone de liste déroulante **mode** .  
  
    -   Pour spécifier un mode personnalisé, sélectionnez **personnalisé** dans la zone **mode** , sélectionnez une option dans le volet gauche, cliquez sur le paramètre ou la valeur dans le volet droit, puis sélectionnez ou entrez le nouveau paramètre ou la nouvelle valeur.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante de la migration dépend des besoins de votre projet :  
  
-   Pour personnaliser le mappage des types de données sources et cibles, consultez [mappage des types de données sources et cibles](mapping-source-and-target-data-types-accesstosql.md)  
  
-   Pour personnaliser le mappage des bases de données source et cible, consultez [mappage des bases de données source et cible](mapping-source-and-target-databases-accesstosql.md) .  
  
-   Dans le cas contraire, vous pouvez convertir les définitions d’objet de base de données Access en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL Azure définitions d’objets. Pour plus d’informations, consultez [conversion d’objets de base de données Access](converting-access-database-objects-accesstosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
