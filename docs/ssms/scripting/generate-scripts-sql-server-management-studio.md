---
title: Générer des scripts
description: Découvrez comment utiliser l’Assistant Générer et publier des scripts pour créer des scripts Transact-SQL pour plusieurs objets et comment utiliser le script en tant que menu dans l'Explorateur d'objets pour générer des scripts pour des objets individuels ou multiples.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 9711c617-3c68-4e5a-aea3-befc64d51524
author: markingmyname
ms.author: maghan
ms.reviewer: mathoma
ms.custom: seo-lt-2019
ms.date: 04/07/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 31907e51df24aaaee20b1fc27bbc97bf809abe03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271298"
---
# <a name="generate-scripts-sql-server-management-studio"></a>Générer des scripts (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] fournit deux mécanismes pour générer des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] . Vous pouvez créer des scripts pour plusieurs objets à l’aide de **l’Assistant Générer et publier des scripts**. Vous pouvez également générer un script pour des objets individuels ou plusieurs objets à l'aide du menu **Script en tant que** dans l' **Explorateur d'objets**.

Pour obtenir un didacticiel détaillé sur les scripts de différents objets à l’aide de SQL Server Management Studio (SSMS), consultez [Tutoriel : Écriture de scripts dans SSMS](../tutorials/scripting-ssms.md).

## <a name="before-you-begin"></a>Avant de commencer

Choisissez le mécanisme qui correspond le mieux à vos besoins. 

###  <a name="generate-and-publish-scripts-wizard"></a><a name="GenPubScriptWiz"></a> Assistant Générer et publier des scripts

Utilisez l' **Assistant Générer et publier des scripts** pour créer un script [!INCLUDE[tsql](../../includes/tsql-md.md)] pour plusieurs objets. L'Assistant génère un script de tous les objets d'une base de données, ou un sous-ensemble des objets que vous sélectionnez. L'Assistant propose de nombreuses options pour vos scripts et vous permet notamment d'inclure ou non des autorisations, un classement et des contraintes. Pour obtenir des instructions sur l'utilisation de l'Assistant, consultez [Generate and Publish Scripts Wizard](./generate-and-publish-scripts-wizard.md).
  
### <a name="object-explorer-script-as-menu"></a><a name="OEScriptAsMenu"></a> Menu Script en tant que de l'Explorateur d'objets

Vous pouvez utiliser le menu **« Script en tant que » de l’Explorateur d’objets** pour générer le script d’un objet unique, de plusieurs objets ou de plusieurs instructions pour un objet unique. Vous pouvez choisir un type de scripts parmi plusieurs ; par exemple pour créer, modifier ou supprimer l'objet. Vous pouvez enregistrer le script dans une fenêtre Éditeur de requête, dans un fichier ou dans le Presse-papiers. Le script est créé au format Unicode.

## <a name="to-generate-a-script-of-a-single-object"></a><a name="ScriptSingleObject"></a> Pour générer un script d'un objet unique

**Pour générer un script d'un objet unique**

1. Dans l'Explorateur d'objets, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et développez-la.

2. Développez **Bases de données**, puis développez la base de données contenant l'objet pour lequel un script doit être généré.

3. Développez la catégorie de l'objet. Par exemple, développez le nœud **Tables** ou **Vues** .

4. Cliquez avec le bouton de droite sur le l’objet, pointez la souris sur **Script \<object type> en tant que**, par exemple, pointez la souris sur **Générer un script de la table en tant que**.

5. Pointez sur le type de script, tel que **Create to** ou **Alter to**.

6. Sélectionnez l'emplacement dans lequel vous voulez enregistrer le script, tel que **Nouvelle fenêtre d'éditeur de requête** ou **Presse-papiers**.

    ![Générer un script de la table](media/generate-scripts-sql-server-management-studio/script-table.png)

Vous pouvez utiliser le volet **Détails de l’Explorateur d’objets** afin de générer un script pour plusieurs objets appartenant à la même catégorie.

1. Dans l'Explorateur d'objets, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et développez-la.

2. Développez **Bases de données**, puis développez la base de données contenant les objets pour lequel un script doit être généré.

3. Développez le nœud de catégorie des types d'objet pour lesquels vous voulez générer un script, tel que le nœud **Tables** .

4. Ouvrez le volet **Détails de l'Explorateur d'objets** en appuyant sur **F7**, ou en ouvrant le menu **Affichage** et en sélectionnant **Détails de l'Explorateur d'objets**.

    ![Menu Affichage](media/generate-scripts-sql-server-management-studio/object-explorer-details-view-menu.png)

5. Cliquez avec le bouton gauche sur l'un des objets pour lesquels vous voulez générer un script.

6. Appuyez sur Crtl + clic gauche sur le deuxième objet pour lequel vous voulez générer un script.

7. Cliquez avec le bouton de droite sur l'un des objets sélectionnés et sélectionnez **Script \<object type> en tant que**.

    ![Détails](media/generate-scripts-sql-server-management-studio/object-explorer-details.png)