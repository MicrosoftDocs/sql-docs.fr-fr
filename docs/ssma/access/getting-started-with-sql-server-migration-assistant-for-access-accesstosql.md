---
title: Prise en main de Assistant Migration SQL Server pour l’accès | Microsoft Docs
description: Commencez à utiliser SSMA pour convertir des objets de base de données Access en SQL Server ou Azure SQL Database des objets, charger les objets résultants et migrer les données.
ms.prod: sql
ms.custom: ''
ms.date: 08/15/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- error list pane
- getting started
- menus
- metadata explorers
- output pane
- toolbars
- user interface
- user interface overview
ms.assetid: 462a731f-08f1-44e1-9eeb-4deac6d2f6c5
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: e05bc860ec1cc9f3703a1d1c17441c4e1ad3de15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044609"
---
# <a name="getting-started-with-sql-server-migration-assistant-for-access-accesstosql"></a>Prise en main de Assistant Migration SQL Server pour Access (AccessToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration (SSMA) pour Access vous permet de convertir rapidement des objets de base de données Access en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets ou Azure SQL Database, de charger les objets résultants dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database, et de migrer des données d’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. Si nécessaire, vous pouvez également lier des tables d’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des tables ou Azure SQL Database pour pouvoir continuer à utiliser vos applications frontales Access existantes avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.  
  
Cette rubrique présente le processus d’installation et vous aide à vous familiariser avec l’interface utilisateur SSMA.  
  
## <a name="installing-ssma"></a>Installation de SSMA  
Pour utiliser SSMA, vous devez d’abord installer le programme client SSMA sur un ordinateur qui peut accéder aux bases de données que vous souhaitez migrer et à l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. Pour obtenir des instructions d’installation, consultez [installation de Assistant Migration SQL Server pour Access &#40;AccessToSQL&#41;](../../ssma/access/installing-sql-server-migration-assistant-for-access-accesstosql.md).  
  
Pour démarrer SSMA, cliquez sur **Démarrer**, pointez sur **tous les programmes**, sur **Assistant Migration SQL Server pour accès**, puis sélectionnez **Assistant Migration SQL Server pour l’accès**.  
  
## <a name="using-ssma"></a>Utilisation de SSMA  
Après l’installation de SSMA, vous pouvez vous familiariser avec l’interface utilisateur SSMA avant d’utiliser l’outil pour migrer des bases de données Access vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. L’interface utilisateur SSMA, y compris les explorateurs de métadonnées, les métadonnées, les barres d’outils, le volet de sortie et le volet Liste d’erreurs, sont affichées dans le diagramme suivant :  
  
![SSMA pour l'interface d'utilisateur d'accès graphique](../../ssma/access/media/ssmaforaccessgui.gif "SSMA pour l'interface d'utilisateur d'accès graphique")  
  
Pour démarrer une migration, créez un projet, puis ajoutez des bases de données Access pour accéder à l’Explorateur de métadonnées. Vous pouvez ensuite cliquer avec le bouton droit sur objets dans l’Explorateur de métadonnées Access pour effectuer des tâches telles que :
- Exportation d’un inventaire des objets de base de données Access vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.
- Création de rapports qui évaluent les conversions vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.
- Conversion de schémas d’accès à des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schémas ou Azure SQL Database.

Vous pouvez également effectuer ces tâches à l’aide des barres d’outils et des menus.  
  
Vous devez également vous connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Une fois la connexion établie, une hiérarchie de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données s’affiche dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées. Après avoir converti les schémas d’accès en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schémas, vous pouvez sélectionner ces schémas convertis dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées, puis charger les schémas dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Si vous avez sélectionné Azure SQL Database dans la boîte de dialogue migrer vers la liste déroulante dans le nouveau projet, vous devez vous connecter à Azure SQL Database. Une fois la connexion établie, une hiérarchie de Azure SQL Database bases de données apparaît dans Azure SQL Database Explorateur de métadonnées. Après avoir converti les schémas d’accès en Azure SQL Database schémas, vous pouvez sélectionner les schémas convertis dans Azure SQL Database Explorateur de métadonnées, puis charger les schémas dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Une fois que vous avez chargé les schémas convertis dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database, vous pouvez revenir à l’Explorateur de métadonnées Access et migrer les données à partir des bases de données Access vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database bases de données. Si nécessaire, vous pouvez également lier des tables d’accès à des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tables ou Azure SQL Database.  
  
Pour plus d’informations sur ces tâches et leur exécution, consultez les rubriques suivantes :  
  
-   [Préparation des bases de données Access pour la migration](preparing-access-databases-for-migration-accesstosql.md)  
  
-   [Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
-   [Liaison des applications Access à SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md)  
  
Les sections suivantes décrivent les fonctionnalités de l’interface utilisateur SSMA.  
  
### <a name="metadata-explorers"></a>Explorateurs de métadonnées  
SSMA contient deux explorateurs de métadonnées que vous pouvez utiliser pour parcourir et effectuer des actions sur les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données Access et Azure SQL Database.  
  
#### <a name="access-metadata-explorer"></a>Accéder à l'explorateur de métadonnées  
L’Explorateur de métadonnées Access affiche des informations sur les bases de données Access qui ont été ajoutées au projet. Lorsque vous ajoutez une base de données Access, SSMA récupère les métadonnées de cette base de données, qui sont les métadonnées qui sont disponibles dans l’Explorateur de métadonnées Access.  
  
Vous pouvez utiliser l’Explorateur de métadonnées Access pour effectuer les tâches suivantes :  
  
-   Parcourez les tables de chaque base de données Access.  
  
-   Sélectionnez les objets à convertir et convertissez les objets en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe. Pour plus d’informations, consultez [conversion d’objets de base de données Access](converting-access-database-objects-accesstosql.md).  
  
-   Sélectionnez des objets pour la migration des données et migrez les données de ces objets vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [migration de données Access vers SQL Server](migrating-access-data-into-sql-server-azure-sql-db-accesstosql.md).  
  
-   Liez et dissociez l’accès et les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tables.  
  
#### <a name="sql-server-or-azure-sql-database-metadata-explorer"></a>SQL Server ou Azure SQL Database l’Explorateur de métadonnées  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database Explorateur de métadonnées affiche des informations sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. Quand vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database, SSMA récupère les métadonnées relatives à cette instance et les stocke dans le fichier projet.  
  
Vous pouvez utiliser la SQL Server ou Azure SQL Database Explorateur de métadonnées pour sélectionner les objets de base de données Access convertis et charger (synchroniser) ces objets dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.  
  
Pour plus d’informations, consultez [chargement d’objets de base de données convertis dans SQL Server](loading-converted-database-objects-into-sql-server-accesstosql.md).  
  
### <a name="metadata"></a>Métadonnées  
À droite de chaque Explorateur de métadonnées se trouvent des onglets qui décrivent l’objet sélectionné. Par exemple, si vous sélectionnez une table dans l’Explorateur de métadonnées Access, quatre onglets s’affichent : **table**, **mappage de type**, **Propriétés** et **données**. Si vous sélectionnez une table dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées, trois onglets s’affichent : **table**, **SQL** et **données**.  
  
La plupart des paramètres de métadonnées sont en lecture seule. Toutefois, vous pouvez modifier les métadonnées suivantes :  
  
-   Dans l’Explorateur de métadonnées Access, vous pouvez modifier les mappages de types. Veillez à effectuer ces modifications avant de créer des rapports ou de convertir des schémas.  
  
-   Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’Explorateur de métadonnées, vous pouvez modifier les propriétés des tables et des index sous l’onglet **table** . Effectuez ces modifications avant de charger les schémas dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [conversion d’objets de base de données Access](converting-access-database-objects-accesstosql.md).  
  
### <a name="toolbars"></a>Barres d'outils  
SSMA comporte deux barres d’outils : une barre d’outils de projet et une barre d’outils de migration.  
  
#### <a name="the-project-toolbar"></a>Barre d’outils du projet  
La barre d’outils projet contient des boutons permettant d’utiliser des projets, d’ajouter des fichiers de base de données Access et de se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. Ces boutons sont semblables aux commandes du menu **fichier** .  
  
#### <a name="the-migration-toolbar"></a>Barre d’outils de migration  
La barre d’outils de migration contient les commandes suivantes :  
  
|Bouton|Fonction|  
|----------|------------|  
|**Convertir, charger et migrer**|Convertit les bases de données Access, charge les objets convertis dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database et migre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les données vers ou Azure SQL Database, le tout en une seule étape.|  
|**Créer un rapport**|Convertit le schéma d’accès sélectionné en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure SQL Database syntaxe, puis crée un rapport qui indique la réussite de la conversion.<br /><br />Cette commande est disponible uniquement lorsque des objets sont sélectionnés dans l’Explorateur de métadonnées Access.|  
|**Convertir le schéma**|Convertit le schéma d’accès sélectionné en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure SQL Database schémas.<br /><br />Cette commande est disponible uniquement lorsque des objets sont sélectionnés dans l’Explorateur de métadonnées Access.|  
|**Migrer des données**|Migre les données de la base de données Access vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. Avant d’exécuter cette commande, vous devez convertir les schémas d’accès en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database schémas, puis charger les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.<br /><br />Cette commande est disponible uniquement lorsque des objets sont sélectionnés dans l’Explorateur de métadonnées Access.|  
|**Stop**|Arrête le processus en cours, par exemple la conversion d’objets en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure SQL Database syntaxe.|  
  
### <a name="menus"></a>Menus  
SSMA contient les menus suivants :  
  
|Menu|Description|  
|--------|---------------|  
|**File**|Contient des commandes pour l’Assistant Migration, l’utilisation de projets, l’ajout et la suppression de fichiers de base de données Access, et la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.|  
|**Modifier**|Contient des commandes pour rechercher et utiliser du texte dans les pages de détails, telles que [!INCLUDE[tsql](../../includes/tsql-md.md)] la copie à partir du volet d’informations SQL. Pour ouvrir la boîte de dialogue **gérer les signets** , dans le menu modifier, cliquez sur gérer les signets. Dans la boîte de dialogue, vous verrez une liste de signets existants. Vous pouvez utiliser les boutons sur le côté droit de la boîte de dialogue pour gérer les signets.|  
|**Visualiser**|Contient la commande **synchroniser les explorateurs de métadonnées** . Cela synchronise les objets entre l’Explorateur de métadonnées d’accès et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database l’Explorateur de métadonnées. Contient également des commandes permettant d’afficher et de masquer les volets **sortie** et **liste d’erreurs** , ainsi que les **mises en page** d’options à gérer avec les dispositions.|  
|**outils**|Contient des commandes permettant de créer des rapports, d’exporter des données, de migrer des objets et des données, de lier des tables et d’accéder aux boîtes de dialogue des paramètres globaux et des projets.|  
|**Aide**|Fournit l’accès à l’aide de SSMA et à la boîte de dialogue **à propos** de.|  
  
### <a name="output-pane-and-error-list-pane"></a>Volet de sortie et volet de Liste d’erreurs  
Le menu **affichage** fournit des commandes pour activer/désactiver la visibilité du volet de sortie et du volet de liste d’erreurs :  
  
-   Le volet sortie affiche les messages d’état de SSMA lors de la conversion d’objets, de la synchronisation d’objets et de la migration des données.  
  
-   Le volet Liste d’erreurs affiche les messages d’erreur, d’avertissement et d’information dans une liste que vous pouvez trier.  
  
## <a name="see-also"></a>Voir également  
[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
