---
title: Configurer des éditeurs (SQL Server Management Studio)
description: Découvrez comment personnaliser le fonctionnement des éditeurs SQL Server Management Studio en définissant des options dans la boîte de dialogue Options.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: e7c7a8ef-f561-4258-a7b6-c445dba69f87
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 33ddc2599ae030e61059c23a462fb68f9f814966
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354740"
---
# <a name="configure-editors-sql-server-management-studio"></a>Configurer des éditeurs (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Vous pouvez personnaliser le fonctionnement des éditeurs [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] en configurant les options pour chaque éditeur.  
  
## <a name="setting-editor-options"></a>Définition des options d'éditeur  
 La plupart des options d’éditeur peuvent être définies dans le menu **Outils**, en sélectionnant **Options...** pour afficher la boîte de dialogue **Options**. Dans la boîte de dialogue **Options** , ouvrez le nœud **Éditeur de texte** dans le volet gauche pour définir les options d'édition de code et de texte. Les nœuds sous Éditeur de texte s'appliquent à des éditeurs spécifiques :  
  
1.  **Tous les langages** : les options définies à l’aide de ce nœud s’appliquent à tous les éditeurs [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Vous pouvez remplacer ces paramètres en utilisant les autres nœuds pour définir différentes options pour un éditeur spécifique.  
  
2.  **Texte brut** : les options définies à l’aide de ce nœud s’appliquent aux éditeurs MDX, DMX ainsi qu’aux éditeurs de texte.  
  
3.  **Transact-SQL** : les options définies à l’aide de ce nœud s’appliquent à l’éditeur de requête du moteur de base de données.  
  
4.  **XML** : les options définies à l’aide de ce nœud s’appliquent à l’éditeur XML for Analysis.  
  
 Ouvrez les nœuds **Exécution de la requête** ou **Résultats de la requête** pour personnaliser l'exécution des requêtes et la façon dont les résultats sont affichés.  
  
## <a name="editor-configuration-tasks"></a>Tâches de configuration d'éditeur  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Décrit comment spécifier qu'un éditeur doit être ouvert par un double-clic sur un fichier avec une extension spécifiée dans l'Explorateur Windows.|[Associer des extensions de fichier à un éditeur de code](./associate-file-extensions-to-a-code-editor.md)|  
|Décrit comment personnaliser les polices pour rendre le code et le texte plus lisibles.|[Modifier la couleur, la taille et le style de la police](./change-font-color-size-and-style.md)|  
|Décrit comment afficher des propriétés.|[Utiliser la fenêtre Propriétés dans Management Studio](./use-the-properties-window-in-management-studio.md)|  
|Emplacement des pages d'aide F1 concernant les boîtes de dialogue d'options d'éditeur.|[Aide (F1) des pages Options de requête](../f1-help/f1-help-for-server-connections-sql-server-management-studio.md)|