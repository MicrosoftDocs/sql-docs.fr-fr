---
title: Ajout et suppression de fichiers de base de données Access (AccessToSQL) | Microsoft Docs
description: Découvrez comment ajouter ou supprimer des bases de données Access vers ou à partir du projet SSMA pour migrer des données d’accès vers SQL Server ou Azure SQL Database.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases
- adding Access files
- adding and removing Access databases
- browsing Access metadata
- browsing metadata, Access databases
- connecting, Access databases
- databases
- files, adding and removing
- finding Access databases
- finding database files
- locating database files
- metadata
- metadata, browsing
- metadata, refreshing
- refreshing metadata
- removing Access files
- scanning for database files
- searching for database files
ms.assetid: e944c740-4c8a-4bc1-b0ed-be57bc06dced
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d8443eaceb150bd0cf2c936e44cb29fc2292f89c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042020"
---
# <a name="adding-and-removing-access-database-files-accesstosql"></a>Ajout et suppression de fichiers de base de données Access (AccessToSQL)
Pour migrer des données Access vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, vous devez ajouter une ou plusieurs bases de données Access au projet SSMA. Ces bases de données doivent être Access 97 ou versions ultérieures. Si vous avez des bases de données d’une version antérieure d’Access, vous devez convertir les bases de données vers une version plus récente. Pour ce faire, vous devez ouvrir et enregistrer les bases de données dans Access 97 ou une version ultérieure avant de les ajouter à SSMA.  
  
## <a name="what-happens-when-you-add-access-database-files"></a>Que se passe-t-il lorsque vous ajoutez des fichiers de base de données Access ?  
Lorsque vous ajoutez une base de données Access à un projet SSMA, SSMA lit les métadonnées de la base de données, puis ajoute ces métadonnées au fichier projet. Ces métadonnées décrivent la base de données et ses objets. SSMA utilise les métadonnées lors de la conversion d’objets vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure syntaxe, et lors de la migration de données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. Vous pouvez parcourir ces métadonnées dans l’Explorateur de métadonnées Access et consulter les propriétés des objets de base de données individuels.  
  
> [!NOTE]  
> Une base de données Access peut être fractionnée en plusieurs fichiers : une base de données principale qui contient des tables et des bases de données frontales qui contiennent des requêtes, des formulaires, des rapports, des macros, des modules et des raccourcis. Si vous souhaitez migrer une base de données fractionnée vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, ajoutez la base de données frontale à SSMA.  
  
## <a name="permissions-that-are-required-by-ssma"></a>Autorisations requises par SSMA  
Pour migrer une base de données Access vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, le groupe utilisateurs et l’utilisateur administrateur doivent disposer des autorisations administrer. Pour plus d’informations sur la façon de migrer des bases de données avec la protection de groupe de travail, consultez [préparation des bases de données Access pour la migration](preparing-access-databases-for-migration-accesstosql.md).  
  
## <a name="selecting-databases-to-add"></a>Sélection de bases de données à ajouter  
Si vous souhaitez ajouter une ou plusieurs bases de données à un projet SSMA et que les fichiers se trouvent tous dans un emplacement connu, vous pouvez ajouter les fichiers à l’aide de la procédure suivante.  
  
**Pour ajouter des fichiers de base de données individuels**  
  
1.  Dans le menu **fichier** , cliquez sur **Ajouter des bases de données**.  
  
2.  Dans la boîte de dialogue **ouvrir** , localisez le dossier qui contient le ou les fichiers de la base de données.  
  
3.  Sélectionnez les fichiers que vous souhaitez ajouter, puis cliquez sur **ouvrir**.  
  
## <a name="finding-databases-to-add"></a>Recherche de bases de données à ajouter  
Si vous souhaitez ajouter plusieurs bases de données Access de différents dossiers à un projet SSMA, ou ajouter un seul fichier, mais que vous devez rechercher le fichier, vous pouvez suivre ces étapes pour localiser un ou plusieurs fichiers et les ajouter au projet.  
  
**Pour rechercher et ajouter des bases de données**  
  
1.  Dans le menu **fichier** , cliquez sur **Rechercher des bases de données**.  
  
2.  Dans l’Assistant Rechercher des bases de données, entrez le nom du lecteur, le chemin d’accès du fichier ou le chemin d’accès UNC que vous souhaitez rechercher. Vous pouvez également cliquer sur **Parcourir** pour localiser le lecteur ou le dossier réseau.  
  
3.  Cliquez sur **Ajouter** pour ajouter l’emplacement à la liste.  
  
    Répétez les deux étapes précédentes pour ajouter d’autres emplacements de recherche.  
  
4.  Si vous le souhaitez, ajoutez des critères de recherche pour affiner la liste des bases de données retournées.  
  
    > [!IMPORTANT]  
    > La zone **de texte tout ou partie du nom de fichier** ne prend pas en charge les caractères génériques.  
  
5.  Cliquez sur **Analyse**.  
  
    La page d’analyse s’affiche. Cela indique les bases de données qui ont été trouvées et la progression de la recherche. Pour arrêter la recherche, cliquez sur **arrêter**.  
  
6.  Dans la page Sélectionner des fichiers, sélectionnez les bases de données que vous souhaitez ajouter au projet.  
  
    Vous pouvez utiliser les boutons **Sélectionner tout** et **Effacer tout** en haut de la liste pour sélectionner ou effacer toutes les bases de données. Vous pouvez maintenir la touche CTRL enfoncée pour sélectionner plusieurs bases de données, ou maintenir la touche Maj enfoncée pour sélectionner une plage de bases de données.  
  
7.  Cliquez sur **Suivant**.  
  
8.  Dans la page vérifier, cliquez sur **Terminer**.  
  
## <a name="browsing-access-metadata"></a>Navigation dans les métadonnées d’accès  
Une fois que vous avez ajouté une base de données Access à un projet, les métadonnées du projet s’affichent dans l’Explorateur de métadonnées Access. Vous pouvez parcourir la hiérarchie des bases de données et des objets de base de données dans l’Explorateur.  
  
**Pour parcourir les métadonnées**  
  
1.  Dans l’Explorateur de métadonnées Access, développez **accès-métabase**, puis développez **bases de données**.  
  
2.  Développez la base de données que vous souhaitez examiner, puis développez **requêtes**.  
  
    Notez la liste des requêtes. Si vous sélectionnez une requête, un onglet **SQL** et un onglet **Propriétés** s’affichent dans le volet droit.  
  
3.  Développez **tables** , puis sélectionnez une table.  
  
    Notez que quatre onglets s’affichent : **table**, **mappage de type**, **Propriétés** et **données**.  
  
4.  Développez une table, développez **clés**, puis sélectionnez une clé.  
  
    Les propriétés de clé s’affichent dans le volet droit.  
  
5.  Développez **index**, puis sélectionnez un index.  
  
    Les propriétés de l’index s’affichent dans le volet droit.  
  
## <a name="refreshing-databases"></a>Actualisation des bases de données  
Si une base de données Access change après l’ajout de son fichier, vous pouvez mettre à jour les métadonnées de la base de données Access.  
  
**Pour mettre à jour les métadonnées d’accès**  
  
-   Dans l’Explorateur de métadonnées Access, cliquez avec le bouton droit sur la base de données, puis sélectionnez **Actualiser à partir de la base de données**.  
  
## <a name="removing-databases"></a>Suppression de bases de données  
Vous pouvez supprimer une base de données Access d’un projet en procédant comme suit.  
  
**Pour supprimer une base de données d’un projet**  
  
1.  Dans l’Explorateur de métadonnées Access, développez **accès-métabase**, puis développez **bases de données**.  
  
2.  Cliquez avec le bouton droit sur la base de données, puis sélectionnez **Supprimer la base de données**.  
  
## <a name="next-step"></a>étape suivante  
L’étape suivante du processus de migration consiste à [se connecter à SQL Server](../sybase/connecting-to-sql-server-sybasetosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[Création et gestion de projets](creating-and-managing-projects-accesstosql.md)  
