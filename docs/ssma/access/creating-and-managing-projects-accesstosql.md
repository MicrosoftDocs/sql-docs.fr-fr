---
description: Création et gestion de projets (AccessToSQL)
title: Création et gestion de projets (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- creating projects
- new projects
- opening projects
- projects
- projects, creating and managing
- saving metadata
- saving projects
ms.assetid: f2d1f0b0-5394-4adb-b3f3-abd71eb68ca7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: ba2e567c5675e602cc80382d42adfcbb89bce90a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045039"
---
# <a name="creating-and-managing-projects-accesstosql"></a>Création et gestion de projets (AccessToSQL)
Pour migrer des bases de données Access vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, vous devez d’abord créer un projet SSMA. Le projet est un fichier qui contient des métadonnées sur les bases de données Access que vous souhaitez migrer vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, des métadonnées sur l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure qui recevront les objets migrés et les données, les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] informations de connexion et les paramètres du projet.  
  
## <a name="reviewing-default-project-settings"></a>Vérification des paramètres de projet par défaut  
SSMA contient plusieurs options pour la conversion et la synchronisation des objets de base de données et pour la conversion de données. Le paramètre par défaut pour ces options est approprié pour de nombreux utilisateurs. Toutefois, avant de créer un nouveau projet SSMA, vous devez passer en revue les options et, si vous le souhaitez, modifier les paramètres par défaut qui seront utilisés pour tous vos nouveaux projets.  
  
**Pour examiner les paramètres de projet par défaut**  
  
1.  Dans le menu **Outils** , sélectionnez **paramètres du projet par défaut**.  
  
2.  Sélectionnez le type de projet dans la liste déroulante **version cible** de la migration pour laquelle les paramètres doivent être affichés/modifiés, puis cliquez sur l’onglet **général** .  
  
3.  Dans le volet gauche, cliquez sur **conversion**.  
  
4.  Dans le volet droit, passez en revue les options. Pour plus d’informations sur ces options, consultez [paramètres du projet (conversion)](./project-settings-conversion-accesstosql.md).  
  
5.  Modifiez les options selon vos besoins.  
  
6.  Répétez les étapes précédentes pour la **migration**, l' **interface utilisateur graphique** et les pages de **mappage de type** .  
  
    -   Pour plus d’informations sur les options de migration, consultez [paramètres du projet (migration)](./project-settings-migration-accesstosql.md).  
  
    -   Pour plus d’informations sur les options d’interface utilisateur, consultez [paramètres du projet (GUI)](../sybase/project-settings-gui-sybasetosql.md).  
  
    -   Pour plus d’informations sur les paramètres de mappage de type de données, consultez [paramètres du projet (mappage de type)](./project-settings-type-mapping-accesstosql.md).  
  
    -   Pour plus d’informations sur les paramètres de SQL Azure, consultez [paramètres du projet (SQL Azure)](./project-settings-azure-sql-db-accesstosql.md).  
  
**Remarque** Les paramètres de SQL Azure ne sont disponibles que lorsque vous sélectionnez migration vers SQL Azure lors de la création d’un projet.  
  
## <a name="creating-new-projects"></a>Création de projets  
SSMA démarre sans charger de projet par défaut. Pour migrer des données à partir de bases de données Access vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, vous devez créer un projet.  
  
**Pour créer un nouveau projet**  
  
1.  Dans le menu **Fichier**, sélectionnez **Nouveau projet**.  
  
    La boîte de dialogue **Nouveau projet** apparaît.  
  
2.  Dans la zone **Nom** , tapez le nom de votre projet.  
  
3.  Dans la zone **emplacement** , entrez ou sélectionnez un dossier pour le projet.  
  
4.  Dans la liste déroulante migration vers, sélectionnez l’une des SQL Server 2005/SQL Server 2008/SQL Server 2012/SQL Server 2014/SQL Server 2016/Azure SQL Database, puis cliquez sur **OK**.  
  
SSMA crée le fichier projet. Vous pouvez maintenant effectuer l’étape suivante pour [Ajouter une ou plusieurs bases de données Access](adding-and-removing-access-database-files-accesstosql.md).  
  
## <a name="customizing-project-settings"></a>Personnalisation des paramètres du projet  
En plus de définir les paramètres de projet par défaut, qui s’appliquent à tous les nouveaux projets SSMA, vous pouvez également personnaliser les paramètres de chaque projet. Pour plus d’informations, consultez [définition des options de conversion et de migration](setting-conversion-and-migration-options-accesstosql.md).  
  
Lorsque vous personnalisez des mappages de types de données entre des bases de données source et cible, vous pouvez définir des mappages au niveau du projet, de la base de données ou de l’objet. Pour plus d’informations sur le mappage de type, consultez [mappage des types de données source et cible](mapping-source-and-target-data-types-accesstosql.md).  
  
## <a name="saving-projects"></a>Enregistrement des projets  
Lorsque vous enregistrez un projet, SSMA conserve les paramètres du projet et éventuellement les métadonnées de la base de données dans le fichier projet.  
  
**Pour enregistrer un projet**  
  
-   Dans le menu **fichier** , sélectionnez **enregistrer le projet**.  
  
    Si les bases de données dans le projet ont été modifiées ou n’ont pas été converties, SSMA vous invite à enregistrer les métadonnées dans le projet. L’enregistrement des métadonnées vous permet de travailler hors connexion. Il vous permet également d’envoyer un fichier projet complet à d’autres personnes, y compris le personnel de support technique. Si vous êtes invité à enregistrer les métadonnées, procédez comme suit :  
  
    1.  Pour chaque base de données qui affiche l’état des **métadonnées manquantes**, activez la case à cocher en regard du nom de la base de données.  
  
        L’enregistrement des métadonnées peut prendre plusieurs minutes. Si vous ne souhaitez pas enregistrer les métadonnées à ce stade, n’activez pas les cases à cocher.  
  
    2.  Cliquez sur **Enregistrer**.  
  
        SSMA analyse les schémas d’accès et enregistre les métadonnées dans le fichier projet.  
  
## <a name="opening-projects"></a>Ouverture de projets  
Lorsque vous ouvrez un projet, il est déconnecté de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. Cela vous permet de travailler hors connexion. Pour mettre à jour les métadonnées, chargez des objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. Pour migrer des données, vous devez vous reconnecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure.  
  
**Pour ouvrir un projet**  
  
1.  Utilisez l’une des procédures suivantes :  
  
    -   Dans le menu **fichier** , pointez sur **projets récents**, puis sélectionnez le projet que vous souhaitez ouvrir.  
  
    -   Dans le menu **fichier** , sélectionnez **ouvrir un projet**, recherchez le fichier projet. a2ssproj, sélectionnez le fichier, puis cliquez sur **ouvrir**.  
  
2.  Pour vous reconnecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , dans le menu **fichier** , sélectionnez **se reconnecter à SQL Server**.  
  
3.  Pour vous reconnecter à SQL Azure, dans le menu **fichier** , sélectionnez **se reconnecter à SQL Azure.**  
  
## <a name="next-step"></a>étape suivante  
L’étape suivante du processus de migration consiste à [Ajouter une ou plusieurs bases de données Access](adding-and-removing-access-database-files-accesstosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[Ajout et suppression de fichiers de base de données Access](adding-and-removing-access-database-files-accesstosql.md)  
