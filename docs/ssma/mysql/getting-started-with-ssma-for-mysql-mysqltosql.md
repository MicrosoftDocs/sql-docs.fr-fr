---
title: Prise en main avec SSMA pour MySQL (MySQLToSQL) | Microsoft Docs
description: Découvrez le processus d’installation de Assistant Migration SQL Server (SSMA) pour MySQL et familiarisez-vous avec l’interface utilisateur SSMA.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Getting started, MySQL metadata explorer
- Getting started, SQL Server or SQL Azure metadata explorer
- Getting started,Installing and licensing
ms.assetid: 8ebfa061-be6f-4a07-923f-8dc832a82f70
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9847b7a709fe64f3fcb7fca92ce1f8a4003843e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070254"
---
# <a name="getting-started-with-ssma-for-mysql-mysqltosql"></a>Bien démarrer avec SSMA pour MySQL (MySQLToSQL)
Assistant Migration SQL Server (SSMA) pour MySQL vous permet de convertir rapidement les schémas de base de données MySQL en SQL Server ou Azure SQL Database schémas, de charger les schémas résultants dans SQL Server ou Azure SQL Database, et de migrer les données de MySQL vers SQL Server ou Azure SQL Database.  
  
Cette rubrique présente le processus d’installation de, puis vous aide à vous familiariser avec l’interface utilisateur SSMA.  
  
## <a name="installing-ssma"></a>Installation de SSMA  
Pour utiliser SSMA, vous devez d’abord installer le programme client SSMA sur un ordinateur qui peut accéder à la base de données MySQL source et à l’instance cible de SQL Server ou Azure SQL Database. Ensuite, installez les fournisseurs MySQL (pilote ODBC 5,1 MySQL (approuvé)) sur l’ordinateur qui exécute le programme client SSMA. Pour obtenir des instructions d’installation, consultez [installation de SSMA pour MySQL &#40;MySqlToSql&#41;](../../ssma/mysql/installing-ssma-for-mysql-mysqltosql.md)  
  
Pour démarrer SSMA, cliquez sur **Démarrer**, pointez sur **tous les programmes**, sur **Assistant Migration SQL Server pour mysql**, puis cliquez sur **Assistant Migration SQL Server pour MySQL**.  
  
## <a name="ssma-for-mysql-user-interface"></a>SSMA pour l'interface d'utilisateur MySQL  
Une fois SSMA installé et sous licence, vous pouvez utiliser SSMA pour migrer des bases de données MySQL vers SQL Server ou Azure SQL Database. Il vous permet de vous familiariser avec l’interface utilisateur SSMA avant de commencer. Le diagramme suivant illustre l’interface utilisateur pour SSMA, y compris les explorateurs de métadonnées, les métadonnées, les barres d’outils, le volet de sortie et le volet Liste d’erreurs :  
  
![SSMA pour l'interface d'utilisateur d'accès graphique MySql](../../ssma/mysql/media/ssmaformysqlgui.gif "SSMA pour l'interface d'utilisateur d'accès graphique MySql")  
  
Pour démarrer une migration, vous devez effectuer les opérations suivantes :  
  
1.  Créez un projet.  
  
2.  Connectez-vous à une base de données MySQL.  
  
3.  Après une connexion réussie, les schémas MySQL s’affichent dans l’Explorateur de métadonnées MySQL. Dans l’Explorateur de métadonnées MySQL, cliquez avec le bouton droit sur objets pour effectuer des tâches telles que créer des rapports qui évaluent les conversions en SQL Server/Azure SQL Database.  
  
Vous pouvez également effectuer ces tâches à l’aide des barres d’outils et des menus.  
  
Vous devez également vous connecter à une instance de SQL Server. Une fois la connexion établie, une hiérarchie de SQL Server bases de données apparaît dans SQL Server Explorateur de métadonnées. Après avoir converti les schémas MySQL en SQL Server schémas, sélectionnez les schémas convertis dans SQL Server Explorateur de métadonnées, puis synchronisez les schémas avec SQL Server.  
  
Vous devez vous connecter à Azure SQL Database si vous avez sélectionné Azure SQL Database dans la boîte de dialogue migrer vers la liste déroulante dans le nouveau projet. Une fois la connexion établie, une hiérarchie de Azure SQL Database bases de données apparaît dans Azure SQL Database Explorateur de métadonnées. Après avoir converti les schémas MySQL en Azure SQL Database schémas, sélectionnez les schémas convertis dans Azure SQL Database Explorateur de métadonnées, puis synchronisez les schémas avec Azure SQL Database.  
  
Une fois que vous avez synchronisé les schémas convertis avec SQL Server ou Azure SQL Database, vous pouvez revenir à l’Explorateur de métadonnées MySQL et migrer des données à partir de schémas MySQL vers des bases de données SQL Server ou Azure SQL Database.  
  
Pour plus d’informations sur ces tâches et sur la façon de les effectuer, consultez [migration de bases de données MySQL vers SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md).  
  
Les sections suivantes décrivent les fonctionnalités de l’interface utilisateur SSMA.  
  
### <a name="metadata-explorers"></a>Explorateurs de métadonnées  
SSMA contient deux explorateurs de métadonnées permettant de parcourir et d’exécuter des actions sur MySQL et les bases de données de SQL Server.  
  
### <a name="mysql-metadata-explorer"></a>Explorateur de métadonnées MySQL  
L’Explorateur de métadonnées MySQL affiche des informations sur les schémas MySQL. À l’aide de l’Explorateur de métadonnées MySQL, vous pouvez effectuer les tâches suivantes :  
  
-   Parcourez les objets dans chaque schéma.  
  
-   Sélectionnez les objets à convertir, puis convertissez les objets en SQL Server syntaxe. Pour plus d’informations, consultez [conversion de bases de données MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
-   Sélectionnez tables pour la migration des données, puis migrez les données de ces tables vers SQL Server. Pour plus d’informations, consultez [migration de données MySQL vers SQL Server Azure SQL Database &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
  
### <a name="sql-server-or-azure-sql-database-metadata-explorer"></a>SQL Server ou Azure SQL Database l’Explorateur de métadonnées  
SQL Server ou Azure SQL Database Explorateur de métadonnées affiche des informations sur une instance de SQL Server ou Azure SQL Database. Quand vous vous connectez à une instance de SQL Server ou Azure SQL Database, SSMA récupère les métadonnées relatives à cette instance et les stocke dans le fichier projet.  
  
Vous pouvez utiliser cet Explorateur de métadonnées pour sélectionner les objets de base de données MySQL convertis, puis synchroniser ces objets avec l’instance de SQL Server ou Azure SQL Database.  
  
Pour plus d’informations, consultez [Synchronization (MySQL to SQL Server/Azure SQL Database)](./loading-converted-database-objects-into-sql-server-mysqltosql.md)  
  
### <a name="metadata"></a>Métadonnées  
À droite de chaque Explorateur de métadonnées se trouvent des onglets qui décrivent l’objet sélectionné. Par exemple, si vous sélectionnez une table dans l’Explorateur de métadonnées MySQL, neuf onglets s’affichent : **table**, **SQL**, mappage de type, **données**, **paramètres**, **mappage de jeux** de **caractères**, **modes SQL**, **Propriétés** et **rapport**. L’onglet **rapport** contient des informations uniquement après que vous avez créé un rapport qui contient l’objet sélectionné. Si vous sélectionnez une table dans SQL Server Explorateur de métadonnées, trois onglets s’affichent : **table**, **SQL** et **données**.  
  
La plupart des paramètres de métadonnées sont en lecture seule. Toutefois, vous pouvez modifier les métadonnées suivantes :  
  
-   Dans l’Explorateur de métadonnées MySQL, vous pouvez modifier les mappages de types, le mappage de jeux de caractères, les modes SQL. Pour convertir les mappages de types ou de jeux de caractères modifiés ou les modes SQL, apportez les modifications nécessaires avant de convertir les schémas.  
  
-   Dans SQL Server Explorateur de métadonnées, vous pouvez modifier les propriétés de table et d’index dans l’onglet table. Pour voir ces modifications dans SQL Server, effectuez ces modifications avant de charger les schémas dans SQL Server.  
  
Les modifications apportées dans un Explorateur de métadonnées sont reflétées dans les métadonnées du projet, et non dans les bases de données source ou cible.  
  
### <a name="toolbars"></a>Barres d'outils  
SSMA comporte deux barres d’outils : une barre d’outils de projet et une barre d’outils de migration.  
  
### <a name="the-project-toolbar"></a>Barre d’outils du projet  
La barre d’outils projet contient des boutons permettant d’utiliser des projets, de se connecter à MySQL et de se connecter à SQL Server ou Azure SQL Database. Ces boutons sont semblables aux commandes du menu **fichier** .  
  
### <a name="migration-toolbar"></a>Barre d’outils migration  
Le tableau suivant présente les commandes de la barre d’outils migration :  
  
|||  
|-|-|  
|**Button**|**Fonction**|  
|**Créer un rapport**|Convertit les objets MySQL sélectionnés en objets SQL Server ou Azure SQL Database, puis crée un rapport qui indique la réussite de la conversion.<br /><br />Cette commande est désactivée, à moins que les objets ne soient sélectionnés dans l’Explorateur de métadonnées MySQL.|  
|**Convertir le schéma**|Convertit les objets MySQL sélectionnés en objets SQL Server ou Azure SQL Database.<br /><br />Cette commande est désactivée, à moins que les objets ne soient sélectionnés dans l’Explorateur de métadonnées MySQL.|  
|**Migrer des données**|Migre les données de la base de données MySQL vers SQL Server ou Azure SQL Database. Avant d’exécuter cette commande, vous devez convertir les schémas MySQL en SQL Server ou Azure SQL Database schémas, puis charger les objets dans SQL Server ou Azure SQL Database.<br /><br />Cette commande est désactivée, à moins que les objets ne soient sélectionnés dans l’Explorateur de métadonnées MySQL.|  
|**Stop**|Arrête le processus en cours.|  
  
### <a name="menus"></a>Menus  
Le tableau suivant présente les menus SSMA.  
  
|||  
|-|-|  
|**Menu**|**Description**|  
|**Fichier**|Contient des commandes pour travailler avec des projets, se connecter à MySQL et se connecter à SQL Server ou Azure SQL Database.|  
|**Modifier**|Contient des commandes pour rechercher et utiliser du texte dans les pages de détails. Pour ouvrir la boîte de dialogue **gérer les signets** , dans le menu Edition, cliquez sur gérer les signets. Dans la boîte de dialogue, vous verrez une liste de signets existants. Vous pouvez utiliser les boutons sur le côté droit de la boîte de dialogue pour gérer les signets.|  
|**Visualiser**|Contient la commande **synchroniser les explorateurs de métadonnées** . Cela synchronise les objets entre l’Explorateur de métadonnées MySQL et SQL Server ou Azure SQL Database Explorateur de métadonnées. Contient également des commandes permettant d’afficher et de masquer les volets de **sortie** et de **liste d’erreurs** , ainsi que les **mises en page** d’options à gérer avec les dispositions.|  
|**outils**|Contient des commandes pour créer des rapports, convertir un schéma, actualiser à partir de la base de données, migrer des objets et des données, et enregistrer en tant que script. Permet également d’accéder aux boîtes de dialogue paramètres **globaux, paramètres du projet par défaut** et **paramètres du projet** .|  
|**Aide**|Fournit l’accès à l’aide de SSMA et à la boîte de dialogue **à propos** de.|  
  
### <a name="output-pane-and-error-list-pane"></a>Volet de sortie et volet de Liste d’erreurs  
Le menu **affichage** fournit des commandes pour activer/désactiver la visibilité du volet de sortie et du volet de liste d’erreurs :  
  
-   Le volet sortie affiche les messages d’état de SSMA lors de la conversion d’objets, de la synchronisation d’objets et de la migration des données.  
  
-   Le volet Liste d’erreurs affiche les messages d’erreur, d’avertissement et d’information dans une liste pouvant être triée.  
  
## <a name="see-also"></a>Voir aussi  
[Référence de l’interface utilisateur &#40;MySQLToSQL&#41;](../../ssma/mysql/user-interface-reference-mysqltosql.md)  
[Migration de données MySQL vers SQL Server Azure SQL Database &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
