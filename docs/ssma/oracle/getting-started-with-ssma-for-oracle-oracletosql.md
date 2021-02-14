---
title: Prise en main avec SSMA pour Oracle (OracleToSQL) | Microsoft Docs
description: Découvrez le Assistant Migration SQL Server (SSMA) pour le processus d’installation d’Oracle et familiarisez-vous avec l’interface utilisateur SSMA.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- SSMA for Oracle, Metadata Explorers
- SSMA for Oracle, Toolbars
ms.assetid: df79664c-972e-4bef-865a-ce609789fee7
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: ce9d4908113daddf51707b4b7a80c4085defc228
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076710"
---
# <a name="getting-started-with-ssma-for-oracle-oracletosql"></a>Bien démarrer avec SSMA pour Oracle (OracleToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration (SSMA) pour Oracle vous permet de convertir rapidement les schémas de base de données Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schémas, de charger les schémas résultants dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de migrer les données d’Oracle vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Cette rubrique présente le processus d’installation de, puis vous aide à vous familiariser avec l’interface utilisateur SSMA.  
  
## <a name="installing-ssma"></a>Installation de SSMA  
Pour utiliser SSMA, vous devez d’abord installer le programme client SSMA sur un ordinateur qui peut accéder à la base de données Oracle source et à l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous devez ensuite installer un pack d’extension et au moins l’un des fournisseurs Oracle (OLE DB ou [!INCLUDE[vstecado](../../includes/vstecado_md.md)] ) sur l’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ces composants prennent en charge la migration des données et l’émulation des fonctions système Oracle. Pour obtenir des instructions d’installation, consultez [installation de SSMA pour Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/installing-ssma-for-oracle-oracletosql.md).  
  
Pour démarrer SSMA, cliquez sur **Démarrer**, pointez sur **tous les programmes**, sur **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration pour Oracle**, puis cliquez sur **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration pour Oracle**.  
  
## <a name="ssma-for-oracle-user-interface"></a>SSMA pour l’interface utilisateur Oracle  
Après l’installation de SSMA, vous pouvez utiliser SSMA pour migrer des bases de données Oracle vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Il vous permet de vous familiariser avec l’interface utilisateur SSMA avant de commencer. Le diagramme suivant illustre l’interface utilisateur pour SSMA, y compris les explorateurs de métadonnées, les métadonnées, les barres d’outils, le volet de sortie et le volet Liste d’erreurs :  
  
![SSMA pour l'interface utilisateur d'Oracle](../../ssma/oracle/media/ssma_oracle_ui.jpg "SSMA pour l'interface utilisateur d'Oracle")  
  
Pour démarrer une migration, vous devez d’abord créer un nouveau projet. Ensuite, vous vous connectez à une base de données Oracle. Après une connexion réussie, les schémas Oracle s’affichent dans l’Explorateur de métadonnées Oracle. Vous pouvez ensuite cliquer avec le bouton droit sur des objets dans l’Explorateur de métadonnées Oracle pour effectuer des tâches telles que créer des rapports qui évaluent les conversions vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez également effectuer ces tâches à l’aide des barres d’outils et des menus.  
  
Vous devez également vous connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Une fois la connexion établie, une hiérarchie de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données s’affiche dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées. Après avoir converti les schémas Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schémas, sélectionnez les schémas convertis dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées, puis synchronisez les schémas avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Après avoir synchronisé les schémas convertis avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez revenir à l’Explorateur de métadonnées Oracle et migrer des données à partir de schémas Oracle vers des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données.  
  
Pour plus d’informations sur ces tâches et sur la façon de les effectuer, consultez [migration de bases de données Oracle vers SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md).  
  
Les sections suivantes décrivent les fonctionnalités de l’interface utilisateur SSMA.  
  
### <a name="metadata-explorers"></a>Explorateurs de métadonnées  
SSMA contient deux explorateurs de métadonnées permettant de parcourir et d’effectuer des actions sur Oracle et les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données.  
  
#### <a name="oracle-metadata-explorer"></a>Explorateur de métadonnées Oracle  
L’Explorateur de métadonnées Oracle affiche des informations sur les schémas Oracle. À l’aide de l’Explorateur de métadonnées Oracle, vous pouvez effectuer les tâches suivantes :  
  
-   Parcourez les objets dans chaque schéma.  
  
-   Sélectionnez les objets à convertir, puis convertissez les objets en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe. Pour plus d’informations, consultez [conversion de schémas Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/converting-oracle-schemas-oracletosql.md).  
  
-   Sélectionnez tables pour la migration des données, puis migrez les données de ces tables vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [migration de données Oracle vers SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-data-into-sql-server-oracletosql.md).  
  
#### <a name="sql-server-metadata-explorer"></a>Explorateur de métadonnées SQL Server  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] L’Explorateur de métadonnées affiche des informations sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quand vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA récupère les métadonnées relatives à cette instance et les stocke dans le fichier projet.  
  
Vous pouvez utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’Explorateur de métadonnées pour sélectionner des objets de base de données Oracle convertis, puis synchroniser ces objets avec l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Pour plus d’informations, consultez [chargement des objets de base de données convertis dans SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/loading-converted-database-objects-into-sql-server-oracletosql.md).  
  
### <a name="metadata"></a>Métadonnées  
À droite de chaque Explorateur de métadonnées se trouvent des onglets qui décrivent l’objet sélectionné. Par exemple, si vous sélectionnez une table dans l’Explorateur de métadonnées Oracle, six onglets s’affichent : **table**, **SQL**, **mappage de type, rapport**, **Propriétés** et **données**. L’onglet **rapport** contient des informations uniquement après que vous avez créé un rapport qui contient l’objet sélectionné. Si vous sélectionnez une table dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées, trois onglets s’affichent : **table**, **SQL** et **données**.  
  
La plupart des paramètres de métadonnées sont en lecture seule. Toutefois, vous pouvez modifier les métadonnées suivantes :  
  
-   Dans l’Explorateur de métadonnées Oracle, vous pouvez modifier les procédures et les mappages de types. Pour convertir les procédures et les mappages de type modifiés, apportez les modifications nécessaires avant de convertir les schémas.  
  
-   Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’Explorateur de métadonnées, vous pouvez modifier les [!INCLUDE[tsql](../../includes/tsql-md.md)] procédures stockées. Pour voir ces modifications dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , effectuez ces modifications avant de charger les schémas dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Les modifications apportées dans un Explorateur de métadonnées sont reflétées dans les métadonnées du projet, et non dans les bases de données source ou cible.  
  
### <a name="toolbars"></a>Barres d'outils  
SSMA comporte deux barres d’outils : une barre d’outils de projet et une barre d’outils de migration.  
  
#### <a name="the-project-toolbar"></a>Barre d’outils du projet  
La barre d’outils projet contient des boutons permettant d’utiliser des projets, de se connecter à Oracle et de se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ces boutons sont semblables aux commandes du menu **fichier** .  
  
#### <a name="migration-toolbar"></a>Barre d’outils migration  
Le tableau suivant présente les commandes de la barre d’outils migration :  
  
|Bouton|Fonction|  
|------|--------|  
|**Créer un rapport**|Convertit les objets Oracle sélectionnés en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe, puis crée un rapport qui indique la réussite de la conversion.<br /><br />Cette commande est désactivée, à moins que les objets ne soient sélectionnés dans l’Explorateur de métadonnées Oracle.|  
|**Convertir le schéma**|Convertit les objets Oracle sélectionnés en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets.<br /><br />Cette commande est désactivée, à moins que les objets ne soient sélectionnés dans l’Explorateur de métadonnées Oracle.|  
|**Migrer des données**|Migre les données de la base de données Oracle vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Avant d’exécuter cette commande, vous devez convertir les schémas Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schémas, puis charger les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br />Cette commande est désactivée, à moins que les objets ne soient sélectionnés dans l’Explorateur de métadonnées Oracle.|  
|**Stop**|Arrête le processus en cours.|  
  
### <a name="menus"></a>Menus  
Le tableau suivant présente les menus SSMA.  
  
|Menu|Description|  
|----|-----------|  
|**File**|Contient des commandes pour travailler avec des projets, se connecter à Oracle et se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Modifier**|Contient des commandes pour rechercher et utiliser du texte dans les pages de détails, telles que [!INCLUDE[tsql](../../includes/tsql-md.md)] la copie à partir du volet d’informations SQL. Contient également l’option **gérer les signets** , dans laquelle vous pouvez afficher la liste des signets existants. Vous pouvez utiliser les boutons sur le côté droit de la boîte de dialogue pour gérer les signets.|  
|**Visualiser**|Contient la commande **synchroniser les explorateurs de métadonnées** . Qui synchronise les objets entre l’Explorateur de métadonnées Oracle et l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées. Contient également des commandes pour afficher et masquer les volets **sortie** et **liste d’erreurs** , ainsi qu’une **disposition** d’options pour gérer les dispositions.|  
|**outils**|Contient des commandes pour créer des rapports et migrer des objets et des données. Permet également d’accéder aux boîtes de dialogue **paramètres globaux** et **paramètres du projet** .|  
|**Testeur**|Contient des commandes pour créer et utiliser des cas de test, le référentiel et le système de gestion de sauvegarde.|  
|**Aide**|Fournit l’accès à l’aide de SSMA et à la boîte de dialogue **à propos** de.|  
  
### <a name="output-pane-and-error-list-pane"></a>Volet de sortie et volet de Liste d’erreurs  
Le menu **affichage** fournit des commandes pour activer/désactiver la visibilité du volet de sortie et du volet de liste d’erreurs :  
  
-   Le volet sortie affiche les messages d’état de SSMA lors de la conversion d’objets, de la synchronisation d’objets et de la migration des données.  
  
-   Le volet Liste d’erreurs affiche les messages d’erreur, d’avertissement et d’information dans une liste pouvant être triée.  
  
## <a name="see-also"></a>Voir aussi  
[Migration des données Oracle vers SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-data-into-sql-server-oracletosql.md)  
[Référence de l’interface utilisateur &#40;OracleToSQL&#41;](../../ssma/oracle/user-interface-reference-oracletosql.md)  
  
