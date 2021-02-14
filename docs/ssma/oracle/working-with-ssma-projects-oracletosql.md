---
title: Utilisation de projets SSMA (OracleToSQL) | Microsoft Docs
description: Apprenez à créer un projet SSMA qui contient des métadonnées pour les bases de données Oracle à migrer et SQL Server, ainsi que les paramètres et les informations de connexion.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Customizing Project Settings
ms.assetid: ee5d94c0-c7a6-4779-bd32-729bdaf61e1b
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 32a35ace86beea46d79f8eff1ea9a3ba094903fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073433"
---
# <a name="working-with-ssma-projects-oracletosql"></a>Utilisation de projets SSMA (OracleToSQL)
Pour migrer des bases de données Oracle vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez d’abord créer un projet SSMA. Le projet est un fichier qui contient les informations suivantes :  
  
-   Métadonnées sur les bases de données Oracle vers lesquelles vous souhaitez effectuer la migration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Métadonnées relatives à l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui recevront les objets et les données migrés.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] informations de connexion.  
  
-   Paramètres du projet.  
  
Lorsque vous ouvrez un projet, il est déconnecté d’Oracle et de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cela vous permet de travailler hors connexion. Pour plus d’informations sur la reconnexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [connexion à SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-sql-server-oracletosql.md).  
  
## <a name="reviewing-default-project-settings"></a>Vérification des paramètres de projet par défaut  
SSMA contient plusieurs paramètres permettant de convertir et de charger des objets de base de données, de migrer des données et de synchroniser SSMA avec Oracle et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les paramètres par défaut conviennent à de nombreux utilisateurs. Toutefois, avant de créer un nouveau projet SSMA, vous devez passer en revue les paramètres. Si vous le souhaitez, vous pouvez modifier les paramètres par défaut qui seront utilisés pour tous vos nouveaux projets.  
  
**Pour examiner les paramètres de projet par défaut**  
  
1.  Dans le menu **Outils** , cliquez sur **paramètres de projet par défaut**.  
  
2.  Sélectionnez le type de projet dans la liste déroulante **version cible** de la migration pour laquelle vous devez afficher ou modifier les paramètres, puis cliquez sur l’onglet **général** .  
  
3.  Dans le volet gauche, cliquez sur **conversion**.  
  
4.  Dans le volet droit, examinez et modifiez les paramètres selon vos besoins. Pour plus d’informations sur ces paramètres, consultez [paramètres du projet &#40;Conversion&#41; &#40;&#41;OracleToSQL ](../../ssma/oracle/project-settings-conversion-oracletosql.md).  
  
5.  Répétez les étapes 1-3 pour la migration, la synchronisation, le chargement des objets système, l’interface utilisateur graphique et les pages de mappage de type.  
  
    -   Pour plus d’informations sur les paramètres de migration, consultez [Project settings &#40;migration&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-migration-oracletosql.md).  
  
    -   Pour plus d’informations sur les paramètres de l’objet système, consultez [paramètres du projet&#40;chargement des objets système&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-loading-system-objects-oracletosql.md).  
  
    -   Pour plus d’informations sur les paramètres de synchronisation dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [paramètres du projet&#40;synchronisation&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-synchronization-oracletosql.md).  
  
    -   Pour plus d’informations sur les paramètres de l’interface utilisateur graphique, consultez [paramètres du projet &#40;interface graphique&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-gui-oracletosql.md).  
  
    -   Pour plus d’informations sur les paramètres de mappage de type de données, consultez [Project settings &#40;type mapping&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-type-mapping-oracletosql.md).  
  
## <a name="creating-new-projects"></a>Création de projets  
Pour migrer des données depuis des bases de données Oracle vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez d’abord créer un projet.  
  
**Pour créer un projet**  
  
1.  Dans le menu **Fichier**, cliquez sur **Nouveau projet**.  
  
    La boîte de dialogue **Nouveau projet** apparaît.  
  
2.  Dans la zone **Nom** , tapez le nom de votre projet.  
  
3.  Dans la zone **emplacement** , entrez ou sélectionnez un dossier pour le projet, puis cliquez sur **OK**.  
  
4.  Dans la liste déroulante **migration vers** , sélectionnez la version de la cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisée pour la migration. Voici les options disponibles :  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2005  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016  
  
    -   Azure SQL Database  
  
## <a name="customizing-project-settings"></a>Personnalisation des paramètres du projet  
En plus de définir les paramètres de projet par défaut qui s’appliquent à tous les nouveaux projets SSMA, vous pouvez personnaliser les paramètres de chaque projet. Pour plus d’informations, consultez [définition des options de projet &#40;OracleToSQL&#41;](../../ssma/oracle/setting-project-options-oracletosql.md).  
  
Lorsque vous personnalisez des mappages de types de données entre des bases de données source et cible, vous pouvez définir des mappages au niveau du projet, de la base de données ou de l’objet. Pour plus d’informations, consultez [mappage des types de données Oracle et SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/mapping-oracle-and-sql-server-data-types-oracletosql.md).  
  
## <a name="saving-projects"></a>Enregistrement des projets  
Lorsque vous enregistrez un projet, SSMA conserve les paramètres du projet et éventuellement les métadonnées de la base de données dans le fichier projet.  
  
**Pour enregistrer un projet**  
  
-   Dans le menu **fichier** , cliquez sur **enregistrer le projet**.  
  
    Si les schémas du projet ont été modifiés ou n’ont pas été convertis, SSMA vous invite à charger et enregistrer les métadonnées. Le chargement et l’enregistrement des métadonnées vous permettent de travailler hors connexion. Il vous permet également d’envoyer un fichier projet complet à d’autres personnes, telles que le personnel du support technique. Si vous êtes invité à enregistrer les métadonnées, procédez comme suit :  
  
    1.  Pour chaque schéma qui affiche l’état des **métadonnées manquantes**, activez la case à cocher en regard du nom de la base de données.  
  
        L’enregistrement des métadonnées peut prendre plusieurs minutes. Si vous ne souhaitez pas enregistrer les métadonnées, n’activez pas les cases à cocher.  
  
    2.  Cliquez sur le bouton **Enregistrer** .  
  
        SSMA analyse les schémas Oracle et enregistre les métadonnées dans le fichier projet.  
  
## <a name="opening-projects"></a>Ouverture de projets  
Lorsque vous ouvrez un projet, il est déconnecté d’Oracle et de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cela vous permet de travailler hors connexion. Pour mettre à jour les métadonnées, chargez les objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour migrer des données, vous devez vous reconnecter à Oracle et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**Pour ouvrir un projet**  
  
1.  Utilisez l’une des procédures suivantes :  
  
    -   Dans le menu **fichier** , pointez sur **projets récents**, puis cliquez sur le projet que vous souhaitez ouvrir.  
  
    -   Dans le menu **fichier** , sélectionnez **ouvrir un projet**, recherchez le fichier projet. o2ssproj, sélectionnez le fichier, puis cliquez sur **ouvrir**.  
  
2.  Pour vous reconnecter à Oracle, dans le menu **fichier** , cliquez sur **se reconnecter à Oracle**.  
  
3.  Pour vous reconnecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , dans le menu **fichier** , cliquez sur **se reconnecter à SQL Server**.  
  
## <a name="next-step"></a>étape suivante  
L’étape suivante du processus de migration consiste à [se connecter à Oracle Database (OracleToSQL)](./connecting-to-oracle-database-oracletosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Oracle vers SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
[Connexion à Oracle Database &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-oracle-database-oracletosql.md)  
[Connexion à SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-sql-server-oracletosql.md)  
