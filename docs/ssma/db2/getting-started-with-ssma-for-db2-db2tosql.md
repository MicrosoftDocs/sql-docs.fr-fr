---
description: Prise en main avec SSMA pour DB2 (DB2ToSQL)
title: Prise en main avec SSMA pour DB2 (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 48ca32fc-1830-4d1f-add7-480ba5ad02e8
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 1829101a62a3f2602e32b6e4d3535292a90a140d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069514"
---
# <a name="getting-started-with-ssma-for-db2-db2tosql"></a>Prise en main avec SSMA pour DB2 (DB2ToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration (SSMA) pour DB2 vous permet de convertir rapidement les schémas de base de données DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schémas, de charger les schémas résultants dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de migrer des données de DB2 vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Cette rubrique présente le processus d’installation de, puis vous aide à vous familiariser avec l’interface utilisateur SSMA.  
  
## <a name="installing-ssma"></a>Installation de SSMA  
Pour utiliser SSMA, vous devez d’abord installer le programme client SSMA sur un ordinateur qui peut accéder à la base de données DB2 source et à l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Fournisseurs OLEDB DB2 sur l’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ces composants prennent en charge la migration des données et l’émulation des fonctions système DB2. Pour obtenir des instructions d’installation, consultez [installation de SSMA pour DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/installing-ssma-for-db2-db2tosql.md).  
  
Pour démarrer SSMA, cliquez sur **Démarrer**, pointez sur **tous les programmes**, sur **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration pour DB2**, puis cliquez sur **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration pour DB2**.  
  
## <a name="ssma-for-db2-user-interface"></a>SSMA pour l’interface utilisateur DB2  
Après l’installation de SSMA, vous pouvez utiliser SSMA pour migrer des bases de données DB2 vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Il vous permet de vous familiariser avec l’interface utilisateur SSMA avant de commencer. Le diagramme suivant illustre l’interface utilisateur pour SSMA, y compris les explorateurs de métadonnées, les métadonnées, les barres d’outils, le volet de sortie et le volet Liste d’erreurs :  
  
![Interface utilisateur SSMA](../../ssma/db2/media/ssma_db2_ui.png "Interface utilisateur SSMA")  
  
Pour démarrer une migration, vous devez d’abord créer un nouveau projet. Ensuite, vous vous connectez à une base de données DB2. Après une connexion réussie, les schémas DB2 s’affichent dans l’Explorateur de métadonnées DB2. Vous pouvez ensuite cliquer avec le bouton droit sur des objets dans l’Explorateur de métadonnées DB2 pour effectuer des tâches telles que créer des rapports qui évaluent les conversions vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez également effectuer ces tâches à l’aide des barres d’outils et des menus.  
  
Vous devez également vous connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Une fois la connexion établie, une hiérarchie de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données s’affiche dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées. Après avoir converti les schémas DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schémas, sélectionnez les schémas convertis dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées, puis synchronisez les schémas avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Après avoir synchronisé les schémas convertis avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez revenir à l’Explorateur de métadonnées DB2 et migrer des données à partir de schémas DB2 vers des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données.  
  
Pour plus d’informations sur ces tâches et sur la façon de les effectuer, consultez [migration de bases de données DB2 vers SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md).  
  
Les sections suivantes décrivent les fonctionnalités de l’interface utilisateur SSMA.  
  
### <a name="metadata-explorers"></a>Explorateurs de métadonnées  
SSMA contient deux explorateurs de métadonnées permettant de parcourir et d’effectuer des actions sur DB2 et les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données.  
  
#### <a name="db2-metadata-explorer"></a>Explorateur de métadonnées DB2  
L’Explorateur de métadonnées DB2 affiche des informations sur les schémas DB2. À l’aide de l’Explorateur de métadonnées DB2, vous pouvez effectuer les tâches suivantes :  
  
-   Parcourez les objets dans chaque schéma.  
  
-   Sélectionnez les objets à convertir, puis convertissez les objets en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe. Pour plus d’informations, consultez [conversion de schémas DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md).  
  
-   Sélectionnez tables pour la migration des données, puis migrez les données de ces tables vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [migration de bases de données DB2 vers SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md).  
  
#### <a name="sql-server-metadata-explorer"></a>Explorateur de métadonnées SQL Server  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] L’Explorateur de métadonnées affiche des informations sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quand vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA récupère les métadonnées relatives à cette instance et les stocke dans le fichier projet.  
  
Vous pouvez utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’Explorateur de métadonnées pour sélectionner les objets de base de données DB2 convertis, puis synchroniser ces objets avec l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="metadata"></a>Métadonnées  
À droite de chaque Explorateur de métadonnées se trouvent des onglets qui décrivent l’objet sélectionné. Par exemple, si vous sélectionnez une table dans l’Explorateur de métadonnées DB2, six onglets s’affichent : **table**, **SQL**, **mappage de type, rapport**, **Propriétés** et **données**. L’onglet **rapport** contient des informations uniquement après que vous avez créé un rapport qui contient l’objet sélectionné. Si vous sélectionnez une table dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées, trois onglets s’affichent : **table**, **SQL** et **données**.  
  
La plupart des paramètres de métadonnées sont en lecture seule. Toutefois, vous pouvez modifier les métadonnées suivantes :  
  
-   Dans l’Explorateur de métadonnées DB2, vous pouvez modifier les procédures et les mappages de type. Pour convertir les procédures et les mappages de type modifiés, apportez les modifications nécessaires avant de convertir les schémas.  
  
-   Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’Explorateur de métadonnées, vous pouvez modifier les [!INCLUDE[tsql](../../includes/tsql-md.md)] procédures stockées. Pour voir ces modifications dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , effectuez ces modifications avant de charger les schémas dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Les modifications apportées dans un Explorateur de métadonnées sont reflétées dans les métadonnées du projet, et non dans les bases de données source ou cible.  
  
### <a name="toolbars"></a>Barres d'outils  
SSMA comporte deux barres d’outils : une barre d’outils de projet et une barre d’outils de migration.  
  
#### <a name="the-project-toolbar"></a>Barre d’outils du projet  
La barre d’outils projet contient des boutons permettant d’utiliser des projets, de se connecter à DB2 et de se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ces boutons sont semblables aux commandes du menu **fichier** .  
  
#### <a name="migration-toolbar"></a>Barre d’outils migration  
Le tableau suivant présente les commandes de la barre d’outils migration :  
  
|Bouton|Fonction|  
|------|--------|  
|**Créer un rapport**|Convertit les objets DB2 sélectionnés en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe, puis crée un rapport qui indique la réussite de la conversion.<br /><br />Cette commande est désactivée, à moins que les objets ne soient sélectionnés dans l’Explorateur de métadonnées DB2.|  
|**Convertir le schéma**|Convertit les objets DB2 sélectionnés en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets.<br /><br />Cette commande est désactivée, à moins que les objets ne soient sélectionnés dans l’Explorateur de métadonnées DB2.|  
|**Migrer des données**|Migre les données de la base de données DB2 vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Avant d’exécuter cette commande, vous devez convertir les schémas DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schémas, puis charger les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br />Cette commande est désactivée, à moins que les objets ne soient sélectionnés dans l’Explorateur de métadonnées DB2.|  
|**Stop**|Arrête le processus en cours.|  
  
### <a name="menus"></a>Menus  
Le tableau suivant présente les menus SSMA.  
  
|Menu|Description|  
|----|-----------|  
|**File**|Contient des commandes pour travailler avec des projets, se connecter à DB2 et se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Modifier**|Contient des commandes pour rechercher et utiliser du texte dans les pages de détails, telles que [!INCLUDE[tsql](../../includes/tsql-md.md)] la copie à partir du volet d’informations SQL. Contient également l’option **gérer les signets** , dans laquelle vous pouvez afficher la liste des signets existants. Vous pouvez utiliser les boutons sur le côté droit de la boîte de dialogue pour gérer les signets.|  
|**Visualiser**|Contient la commande **synchroniser les explorateurs de métadonnées** . Qui synchronise les objets entre l’Explorateur de métadonnées DB2 et l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées. Contient également des commandes pour afficher et masquer les volets **sortie** et **liste d’erreurs** , ainsi qu’une **disposition** d’options pour gérer les dispositions.|  
|**outils**|Contient des commandes pour créer des rapports et migrer des objets et des données. Permet également d’accéder aux boîtes de dialogue **paramètres globaux** et **paramètres du projet** .|  
|**Aide**|Fournit l’accès à l’aide de SSMA et à la boîte de dialogue **à propos** de.|  
  
### <a name="output-pane-and-error-list-pane"></a>Volet de sortie et volet de Liste d’erreurs  
Le menu **affichage** fournit des commandes pour activer/désactiver la visibilité du volet de sortie et du volet de liste d’erreurs :  
  
-   Le volet sortie affiche les messages d’état de SSMA lors de la conversion d’objets, de la synchronisation d’objets et de la migration des données.  
  
-   Le volet Liste d’erreurs affiche les messages d’erreur, d’avertissement et d’information dans une liste pouvant être triée.  
  
## <a name="see-also"></a>Voir aussi  
[Migration de données DB2 vers SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-data-into-sql-server-db2tosql.md)  
[Référence de l’interface utilisateur &#40;DB2ToSQL&#41;](../../ssma/db2/user-interface-reference-db2tosql.md)  
  
