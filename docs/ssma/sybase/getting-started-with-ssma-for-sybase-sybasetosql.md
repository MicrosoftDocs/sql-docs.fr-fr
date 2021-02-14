---
title: Prise en main avec SSMA pour SAP ASE (SybaseToSQL) | Microsoft Docs
description: Découvrez le processus d’installation de Assistant Migration SQL Server (SSMA) pour SAP ASE et familiarisez-vous avec l’interface utilisateur SSMA.
ms.custom: ''
ms.date: 09/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: c4098516-f0fc-4690-97bb-3766dfd43156
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2ed1698af612a3bd960b7c16011dc4f95e33f61a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078860"
---
# <a name="getting-started-with-ssma-for-sap-ase-sybasetosql"></a>Prise en main avec SSMA pour SAP ASE (SybaseToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration (SSMA) pour SAP ASE vous permet de convertir rapidement les schémas de base de données SAP Adaptive Server Enterprise (ASE) vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database schémas, de charger les schémas résultants dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou de Azure SQL Database et de migrer les données de SAP ASE vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.  
  
Cette rubrique présente le processus d’installation de, puis vous aide à vous familiariser avec l’interface utilisateur SSMA.  
  
## <a name="installing-and-licensing-ssma"></a>Installation et gestion des licences SSMA  
Pour utiliser SSMA, vous devez d’abord installer le programme client SSMA sur un ordinateur qui peut accéder à la fois à l’instance source de SAP ASE et à l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. Pour utiliser la migration de données côté serveur, vous devez installer le pack d’extension et au moins l’un des fournisseurs SAP ASE (OLE DB ou ADO.NET) sur l’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ces composants prennent en charge la migration des données et l’émulation des fonctions système de SAP ASE. Pour obtenir des instructions d’installation, consultez [installation de SSMA pour SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-sybasetosql.md).  
  
Pour démarrer SSMA, cliquez sur **Démarrer**, pointez sur **tous les programmes**, sur **[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration pour Sybase**, puis sélectionnez **[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration pour Sybase**. La première fois que vous démarrez SSMA, une boîte de dialogue de licence s’affiche. Vous devez disposer d’une licence SSMA à l’aide d’un identifiant Windows Live ID avant de pouvoir utiliser SSMA. Les instructions d’installation sont fournies avec les instructions d’installation de la rubrique [installation de SSMA pour le Client &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md) .  
  
## <a name="ssma-for-sap-ase-user-interface"></a>SSMA pour l’interface utilisateur SAP ASE  
Une fois SSMA installé et concédé sous licence, vous pouvez utiliser SSMA pour migrer des bases de données SAP ASE vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. Il vous permet de vous familiariser avec l’interface utilisateur SSMA avant de commencer. Le diagramme suivant illustre l’interface utilisateur pour SSMA, y compris les explorateurs de métadonnées, les métadonnées, les barres d’outils, le volet de sortie et le volet Liste d’erreurs :  
  
![SSMA pour l’interface utilisateur SAP ASE](../../ssma/sybase/media/ssmaforsybaseuserinterface.jpg "SSMA pour l’interface utilisateur SAP ASE")  
  
Pour démarrer une migration, vous devez d’abord créer un nouveau projet. Ensuite, vous vous connectez à SAP ASE. Une fois la connexion établie, une hiérarchie de bases de données SAP ASE s’affiche dans l’Explorateur de métadonnées Sybase. Vous pouvez ensuite cliquer avec le bouton droit sur des objets dans l’Explorateur de métadonnées Sybase pour effectuer des tâches telles que créer des rapports qui évaluent les conversions vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. Vous pouvez également effectuer ces tâches à l’aide des barres d’outils et des menus.  
  
Vous devez également vous connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. Une fois la connexion établie, une hiérarchie de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou des bases de données SQL Azure s’affiche dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure l’Explorateur de métadonnées. Après avoir converti les schémas SAP ASE vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database schémas, sélectionnez les schémas convertis dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure Explorateur de métadonnées, puis chargez les schémas dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.  
  
Après avoir chargé des schémas convertis dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database, vous pouvez revenir à l’Explorateur de métadonnées Sybase et migrer des données à partir de bases de données SAP ASE vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou bases de données SQL Azure.  
  
Pour plus d’informations sur ces tâches et sur la façon de les effectuer, consultez [migration de bases de données SAP ASE vers SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md).  
  
Les sections suivantes décrivent les fonctionnalités de l’interface utilisateur SSMA.  
  
### <a name="metadata-explorers"></a>Explorateurs de métadonnées  
SSMA contient deux explorateurs de métadonnées permettant de parcourir et d’effectuer des actions sur SAP ASE et les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données SQL Azure.  
  
#### <a name="sybase-metadata-explorer"></a>Explorateur de métadonnées Sybase  
L’Explorateur de métadonnées Sybase affiche des informations sur les bases de données sur l’instance source de SAP ASE.  
  
À l’aide de l’Explorateur de métadonnées Sybase, vous pouvez effectuer les tâches suivantes :  
  
-   Parcourez les tables de chaque base de données.  
  
-   Sélectionnez les objets à convertir, puis convertissez les objets vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database syntaxe. Pour plus d’informations, consultez [conversion d’objets de base de données SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md).  
  
-   Sélectionnez objets pour la migration des données, puis migrez les données à partir de ces objets vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. Pour plus d’informations, consultez [migration de données SAP ASE vers SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-data-into-sql-server-azure-sql-db-sybasetosql.md).  
  
#### <a name="sql-server-or-sql-azure-metadata-explorer"></a>SQL Server ou SQL Azure l’Explorateur de métadonnées  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure Explorateur de métadonnées affiche des informations sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. Quand vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database, SSMA récupère les métadonnées relatives à cette instance et les stocke dans le fichier projet.  
  
Vous pouvez utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure Explorateur de métadonnées pour sélectionner les objets de base de données SAP ASE convertis, puis charger (synchroniser) ces objets dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.  
  
Pour plus d’informations, consultez [chargement des objets de base de données convertis dans SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/loading-converted-database-objects-into-sql-server-sybasetosql.md).  
  
### <a name="metadata"></a>Métadonnées  
À droite de chaque Explorateur de métadonnées se trouvent des onglets qui décrivent l’objet sélectionné. Par exemple, si vous sélectionnez une table dans l’Explorateur de métadonnées Sybase, six onglets s’affichent : **table**, **SQL**, **mappage de type**, **données**, **Propriétés** et **rapport**. L’onglet **rapport** contient des informations uniquement après que vous avez créé un rapport contenant l’objet sélectionné. Si vous sélectionnez une table dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure Explorateur de métadonnées, trois onglets s’affichent : **table**, **SQL** et **données**.  
  
La plupart des paramètres de métadonnées sont en lecture seule. Toutefois, vous pouvez modifier les métadonnées suivantes :  
  
-   Dans l’Explorateur de métadonnées Sybase, vous pouvez modifier les procédures et les mappages de type. Apportez ces modifications avant de convertir les schémas.  
  
-   Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure l’Explorateur de métadonnées, vous pouvez modifier les [!INCLUDE[tsql](../../includes/tsql-md.md)] procédures stockées. Apportez ces modifications avant de charger les schémas dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Les modifications apportées dans un Explorateur de métadonnées sont reflétées dans les métadonnées du projet, et non dans les bases de données source ou cible.  
  
### <a name="toolbars"></a>Barres d'outils  
SSMA comporte deux barres d’outils : une barre d’outils de projet et une barre d’outils de migration.  
  
#### <a name="the-project-toolbar"></a>Barre d’outils du projet  
La barre d’outils du projet contient des boutons permettant d’utiliser des projets, de se connecter à SAP ASE et de se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. Ces boutons sont semblables aux commandes du menu **fichier** .  
  
#### <a name="the-migration-toolbar"></a>Barre d’outils de migration  
La barre d’outils de migration contient les commandes suivantes :  
  
|Bouton|Fonction|  
|----------|------------|  
|**Créer un rapport**|Convertit les objets SAP ASE sélectionnés en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe, puis crée un rapport qui indique la réussite de la conversion.<br /><br />Cette commande est disponible uniquement lorsque des objets sont sélectionnés dans l’Explorateur de métadonnées Sybase.|  
|**Convertir le schéma**|Convertit les objets SAP ASE sélectionnés en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets ou Azure SQL Database.<br /><br />Cette commande est disponible uniquement lorsque des objets sont sélectionnés dans l’Explorateur de métadonnées Sybase.|  
|**Migrer des données**|Migre les données de la base de données SAP ASE vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database. Avant d’exécuter cette commande, vous devez convertir les schémas SAP ASE vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database schémas, puis charger les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.<br /><br />Cette commande est disponible uniquement lorsque des objets sont sélectionnés dans l’Explorateur de métadonnées Sybase.|  
|**Stop**|Arrête le processus en cours, par exemple la conversion d’objets en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure SQL Database syntaxe.|  
  
### <a name="menus"></a>Menus  
SSMA contient les menus suivants :  
  
|Menu|Description|  
|--------|---------------|  
|**File**|Contient des commandes pour travailler avec des projets, se connecter à SAP ASE et se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.|  
|**Modifier**|Contient des commandes pour rechercher et utiliser du texte dans les pages de détails, telles que [!INCLUDE[tsql](../../includes/tsql-md.md)] la copie à partir du volet d’informations SQL. Contient également l’option **gérer les signets** , dans laquelle vous pouvez voir une liste de signets existants. Vous pouvez utiliser les boutons sur le côté droit de la boîte de dialogue pour gérer les signets.|  
|**Visualiser**|Contient la commande **synchroniser les explorateurs de métadonnées** . Cela synchronise les objets entre l’Explorateur de métadonnées Sybase et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure l’Explorateur de métadonnées. Contient également des commandes pour afficher et masquer les volets de **sortie** et de **liste d’erreurs** , ainsi qu’une **disposition** d’options pour gérer les dispositions.|  
|**outils**|Contient des commandes pour créer des rapports, exporter des données et migrer des objets et des données. Permet également d’accéder aux boîtes de dialogue **paramètres globaux** et **paramètres du projet** .|  
|**Testeur**|Contient des commandes pour créer des cas de test, afficher les résultats des tests et les commandes pour la gestion des sauvegardes de base de données.|  
|**Aide**|Fournit l’accès à l’aide de SSMA et à la boîte de dialogue **à propos** de.|  
  
### <a name="output-pane-and-error-list-pane"></a>Volet de sortie et volet de Liste d’erreurs  
Le menu **affichage** fournit des commandes pour activer/désactiver la visibilité du volet de sortie et du volet de liste d’erreurs :  
  
-   Le volet sortie affiche les messages d’état de SSMA lors de la conversion d’objets, de la synchronisation d’objets et de la migration des données.  
  
-   Le volet Liste d’erreurs affiche les messages d’erreur, d’avertissement et d’information dans une liste que vous pouvez trier.  
  
## <a name="see-also"></a>Voir également  
[Migration de bases de données SAP ASE vers SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
[Référence de l’interface utilisateur &#40;SybaseToSQL&#41;](../../ssma/sybase/user-interface-reference-sybasetosql.md)  
