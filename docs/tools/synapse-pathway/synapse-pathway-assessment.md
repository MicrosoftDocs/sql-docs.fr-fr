---
title: Évaluation Azure Synapse Pathway Preview.
description: Effectuer une traduction du code d’un entrepôt de données avec Azure Synapse Pathway
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: tools-other
ms.topic: tutorial
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-tutorial
ms.openlocfilehash: 3e5e8536c135244288d022879764d021c53e67ca
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770489"
---
# <a name="tutorial-to-perform-your-first-code-translation-with-azure-synapse-pathway-preview"></a>Tutoriel pour effectuer votre première traduction de code avec Azure Synapse Pathway Preview
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Azure Synapse Pathway Preview introduit la prise en charge de la traduction des schémas, des tables, des vues, des fonctions, etc. à partir de **Netezza**, **Snowflake** et **Microsoft SQL Server** en code conforme à T-SQL qui automatise la migration vers Azure Synapse Analytics.

Pour plus d’informations, consultez [Vue d’ensemble d’Azure Synapse Pathway Preview](azure-synapse-pathway-overview.md).

Dans ce tutoriel, vous allez apprendre à :

> [!div class="checklist"]
> * Exécuter votre première traduction des scripts SQL de votre entrepôt de données existant en scripts T-SQL pour Azure Synapse SQL 
> * Choisir une des sources disponibles
> * Visualiser les erreurs et les avertissements relatifs aux objets qui n’ont pas été traduits

## <a name="prerequisites"></a>Prérequis

Pour suivre ce tutoriel, vérifiez que vous avez installé [Azure Synapse Pathway](synapse-pathway-download.md). Si vous avez besoin d’une présentation, consultez [Vue d’ensemble d’Azure Synapse Pathway](azure-synapse-pathway-overview.md).

## <a name="run-the-translation"></a>Exécuter la traduction

1. Lancez le fichier MSI d’Azure Synapse Pathway. 

1. Sélectionnez une des sources disponibles ; celles qui seront ajoutées bientôt apparaissent en grisé.
1. Dans le dossier du répertoire Entrée, sélectionnez Parcourir et faites pointer l’outil vers l’emplacement des scripts **DDL** et **DML** qui doivent être traduits.

    > [!Note]
    > Seuls des fichiers avec l’extension .sql peuvent être fournis comme source d’entrée. Si l’utilisateur fournit des scripts DDL et DML dans un fichier .txt, l’outil n’effectue pas de traduction.

1. Lors de la traduction du code Netezza vers Azure Synapse Analytics, choisissez IBM Netezza dans la liste déroulante Type de traduction.
  ![Entrée d’évaluation Azure Synapse.](./media/synapse-pathway-assessment/assessment-input.png)

1. Pour sélectionner le répertoire de sortie, sélectionnez Parcourir pour spécifier l’emplacement où la sortie sera générée.
 ![Répertoire de sortie d’Azure Synapse.](./media/synapse-pathway-assessment/output-directory.png)

1. Sélectionnez **Traduire** pour démarrer la traduction.

## <a name="view-results"></a>Afficher les résultats

1. La durée de l’évaluation dépend du nombre de bases de données ajoutées et de la taille du schéma de chaque base de données. Les résultats sont affichés pour chaque base de données dès qu’ils sont disponibles.
 ![Rapport d’évaluation Azure Synapse.](./media/synapse-pathway-assessment/assessment-report-rendering.png)

1. Quand vous sélectionnez Afficher les résultats, vous êtes dirigé vers le répertoire de sortie spécifié à l’étape précédente et vous voyez le ou les fichiers de script traduits en fonction de la structure de votre répertoire d’entrée.

1. Il comprend la structure du projet, qui peut être facilement commitée dans votre dépôt GitHub.
  
1. Un fichier de résultats avec une liste d’erreurs et d’avertissements est chargé dans le même répertoire de sortie.

## <a name="run-the-translation-using-command-line"></a>Exécuter la traduction en utilisant la ligne de commande
1. Au moment de l’installation, AspCmd.exe est disponible dans C:\Program Files (x86) \Mise Synapse (préversion)
1. Lancez l’invite de commandes et accédez à l’emplacement du fichier 
1. Tapez aspcmd.exe --help pour obtenir la liste des commandes

  ![Commandes d’aide de la ligne de commande de l’évaluation d’Azure Synapse.](./media/synapse-pathway-assessment/command-line-help.png)


4. Vous pouvez lancer l’exécution des traductions en utilisant la ligne de commande

 ![Évaluation d’Azure Synapse avec la ligne de commande.](./media/synapse-pathway-assessment/command-line-assessment.png)

## <a name="next-steps"></a>Étapes suivantes

[Découvrir comment enregistrer et charger l’évaluation](tutorial-save-load-assessment.md)
