---
description: Utilisation de projets SSMA (MySQLToSQL)
title: Utilisation de projets SSMA (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Working with SSMA projects, create new project
- Working with SSMA projects, customize settings
- Working with SSMA projects, Open project
- Working with SSMA projects, Save project
ms.assetid: 9e4394e9-f177-41d9-839e-5d53a9c9b840
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 92fb1bec694ca29ffe0591eb03012e1ed2ab3f2f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064364"
---
# <a name="working-with-ssma-projects-mysqltosql"></a>Utilisation de projets SSMA (MySQLToSQL)
Pour migrer des bases de données MySQL vers SQL Server ou SQL Azure, vous devez d’abord créer un projet SSMA. Le projet est un fichier qui contient les informations suivantes :  
  
-   Métadonnées relatives aux bases de données MySQL que vous souhaitez migrer vers SQL Server ou SQL Azure.  
  
-   Métadonnées relatives à l’instance cible de SQL Server ou SQL Azure qui recevront les objets et les données migrés.  
  
-   SQL Server ou les informations de connexion SQL Azure.  
  
-   Paramètres du projet.  
  
Lorsque vous ouvrez un projet, il est déconnecté de MySQL et SQL Server ou SQL Azure. Cela vous permet de travailler hors connexion. Pour plus d’informations sur la reconnexion à SQL Server, consultez [connexion à SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
## <a name="reviewing-default-project-settings"></a>Vérification des paramètres de projet par défaut  
SSMA contient plusieurs paramètres pour la conversion et le chargement de la base de données, la migration des données et la synchronisation de SSMA avec MySQL et SQL Server ou SQL Azure. Les paramètres par défaut conviennent à de nombreux utilisateurs. Toutefois, avant de créer un nouveau projet SSMA, vous devez passer en revue les paramètres. Si nécessaire, vous pouvez modifier les paramètres par défaut qui seront utilisés pour tous vos nouveaux projets.  
  
##### <a name="to-review-default-project-settings"></a>Pour examiner les paramètres de projet par défaut  
  
1.  Dans le menu **Outils** , sélectionnez **paramètres du projet par défaut** .  
  
2.  Sélectionnez le type de projet dans la liste déroulante **version cible** de la migration pour laquelle les paramètres doivent être affichés/modifiés, puis cliquez sur l’onglet **général** .  
  
3.  Dans le volet gauche, cliquez sur **conversion**.  
  
4.  Dans le volet droit, examinez et modifiez les paramètres selon vos besoins. Pour plus d’informations sur ces paramètres, consultez [paramètres du projet &#40;Conversion&#41; &#40;&#41;MySQLToSQL ](../../ssma/mysql/project-settings-conversion-mysqltosql.md) .  
  
5.  Répétez les étapes 1-3 pour la migration, la synchronisation, les SQL Azure, l’interface utilisateur graphique et les pages de mappage de type.  
  
-   Pour plus d’informations sur les paramètres de migration, consultez [Project settings &#40;migration&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-migration-mysqltosql.md).  
  
-   Pour plus d’informations sur les paramètres de synchronisation à SQL Server, consultez [paramètres du projet &#40;synchronisation&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-synchronization-mysqltosql.md).  
  
-   Pour plus d’informations sur les paramètres de l’interface utilisateur graphique, consultez [paramètres du projet (interface utilisateur) (SSMA commun)](../sybase/project-settings-gui-sybasetosql.md).  
  
-   Pour plus d’informations sur les paramètres de mappage de type de données, consultez [Project settings &#40;type mapping&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-type-mapping-mysqltosql.md).  
  
-   Pour plus d’informations sur les paramètres de SQL Azure, consultez [paramètres du projet &#40;Azure SQL Database&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-azure-sql-db-mysqltosql.md).  
  
> [!NOTE]  
> Les paramètres de SQL Azure s’affichent uniquement lorsque vous sélectionnez **migration vers SQL Azure lors de** la création d’un projet.  
  
## <a name="creating-new-projects"></a>Création de projets  
Pour migrer des données de bases de données MySQL vers SQL Server ou SQL Azure, vous devez créer un projet.  
  
##### <a name="to-create-a-new-project"></a>Pour créer un projet  
  
1.  Dans le menu **fichier** , sélectionnez **nouveau projet** . La boîte de dialogue **Nouveau projet** apparaît. Dans le menu **Fichier**, sélectionnez **Nouveau projet**. La boîte de dialogue **Nouveau projet** apparaît.  
  
2.  Dans la zone **Nom** , tapez le nom de votre projet.  
  
3.  Dans la zone **emplacement** , entrez ou sélectionnez un dossier pour le projet.  
  
4.  Dans la liste déroulante **migration vers** , sélectionnez la version de la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisée pour la migration. Voici les options disponibles :  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2005  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014  
  
    -   Azure SQL Database  
  
Puis cliquez sur **OK** .  
  
SSMA crée le fichier projet.  
  
## <a name="customizing-project-settings"></a>Personnalisation des paramètres du projet  
En plus de définir les paramètres de projet par défaut qui s’appliquent à tous les nouveaux projets SSMA, vous pouvez également personnaliser les paramètres de chaque projet. Pour plus d’informations, consultez [définition des options de projet &#40;MySQLToSQL&#41;](../../ssma/mysql/setting-project-options-mysqltosql.md).  
  
Lorsque vous personnalisez les mappages de types de données entre les bases de données source et cible, vous pouvez définir des mappages au niveau du projet, de la base de données ou de l’objet. Pour plus d’informations, consultez [mappage des types de données MySQL et SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md).  
  
## <a name="saving-projects"></a>Enregistrement des projets  
La fonctionnalité d’enregistrement de projets permet à l’utilisateur d’enregistrer essentiellement les paramètres du projet et, éventuellement, les métadonnées de la base de données dans le fichier projet SSMA.  
  
##### <a name="to-save-a-project"></a>Pour enregistrer un projet  
  
-   Dans le menu **fichier** , sélectionnez **Enregistrer** le projet.  
  
Si les bases de données dans le projet ont été modifiées ou n’ont pas été converties, SSMA vous invite à charger et enregistrer les métadonnées. Le chargement et l’enregistrement des métadonnées vous permettent de travailler hors connexion. Il vous permet également d’envoyer un fichier projet complet à d’autres personnes, telles que le personnel du support technique. Si vous êtes invité à enregistrer les métadonnées, procédez comme suit :  
  
1.  Pour chaque base de données qui affiche l’état des **métadonnées manquantes**, activez la case à cocher en regard du nom de la base de données. L’enregistrement des métadonnées peut prendre plusieurs minutes. Si vous ne souhaitez pas enregistrer les métadonnées à ce stade, n’activez pas les cases à cocher.  
  
2.  Cliquez sur **Enregistrer**.  
  
SSMA analyse les schémas MySQL et enregistre les métadonnées dans le fichier projet.  
  
## <a name="opening-projects"></a>Ouverture de projets  
Lorsque vous ouvrez un projet, il est déconnecté de MySQL et de SQL Server ou SQL Azure. Cela vous permet de travailler hors connexion. Pour mettre à jour les métadonnées, chargez les objets de base de données dans SQL Server ou SQL Azure. Pour migrer des données, vous devez vous reconnecter à SQL Server ou SQL Azure.  
  
##### <a name="to-open-a-project"></a>Pour ouvrir un projet  
  
1.  Utilisez l’une des procédures suivantes :  
  
    1.  Dans le menu **fichier** , pointez sur **projets récents**.  
  
    2.  Sélectionnez le projet que vous souhaitez ouvrir.  
  
    3.  Dans le menu **fichier** , sélectionnez **ouvrir un projet**, recherchez le fichier projet. m2ssproj, sélectionnez le fichier, puis cliquez sur **ouvrir**.  
  
2.  Pour vous reconnecter à MySQL, dans le menu **fichier** , sélectionnez **se reconnecter à MySQL**.  
  
3.  Pour vous reconnecter à SQL Server, dans le menu **fichier** , sélectionnez **se reconnecter à SQL Server**.  
  
4.  Pour vous reconnecter à SQL Azure, dans le menu **fichier** , sélectionnez **se reconnecter à SQL Azure.**  
  
## <a name="next-step"></a>étape suivante  
L’étape suivante du processus de migration consiste [à se connecter à MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Connexion à MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
[Migration de bases de données MySQL vers SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
[Connexion à SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
[Connexion à Azure SQL Database &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md)  
