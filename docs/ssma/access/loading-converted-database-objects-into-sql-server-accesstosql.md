---
description: Chargement des objets de base de données convertis dans SQL Server (AccessToSQL)
title: Chargement des objets de base de données convertis dans SQL Server (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases, loading converted objects into SQL Azure
- Access databases, loading converted objects into SQL Server
- data, securing
- loading objects into SQL Azure
- loading objects into SQL Server
- migrating objects into SQL Azure
- migrating objects into SQL Server
- moving objects into SQL Azure
- moving objects into SQL Server
- schemas, loading into SQL Azure
- schemas, loading into SQL Server
- scripting converted objects
- securing data
- SQL Azure, loading objects into
- SQL Server, loading objects into
- synchronizing metadata with SQL Azure
- synchronizing metadata with SQL Server
- uploading objects into SQL Azure
- uploading objects into SQL Server
ms.assetid: 4e854eee-b10c-4f0b-9d9e-d92416e6f2ba
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 4fb81a05cebfaa1a2c59f7e2b90d8289412e140a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071117"
---
# <a name="loading-converted-database-objects-into-sql-server-accesstosql"></a>Chargement des objets de base de données convertis dans SQL Server (AccessToSQL)
Une fois que vous avez converti les objets de base de données Access vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, vous pouvez charger les objets de base de données résultants dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. Vous pouvez faire en sorte que SSMA crée les objets, ou vous pouvez générer un script pour les objets et exécuter les scripts vous-même. En outre, SSMA vous permet de mettre à jour les métadonnées cibles avec le contenu réel de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.  
  
## <a name="choosing-between-synchronization-and-scripts"></a>Choix entre la synchronisation et les scripts  
Si vous souhaitez charger les objets de base de données convertis dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure sans modification, vous pouvez demander à SSMA de créer ou de recréer directement les objets de base de données. Cette méthode est rapide et facile, mais elle n’autorise pas la personnalisation du [!INCLUDE[tsql](../../includes/tsql-md.md)] code qui définit les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets ou SQL Azure, à l’exception des procédures stockées.  
  
Si vous souhaitez modifier le [!INCLUDE[tsql](../../includes/tsql-md.md)] utilisé pour créer des objets, ou si vous souhaitez davantage de contrôle sur la création d’objets, utilisez SSMA pour créer des scripts. Vous pouvez ensuite modifier ces scripts, créer chaque objet individuellement et même utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’agent pour planifier la création de ces objets.  
  
## <a name="using-ssma-to-synchronize-objects-with-sql-server"></a>Utilisation de SSMA pour synchroniser des objets avec SQL Server  
Pour utiliser SSMA pour créer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database des objets, vous sélectionnez les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure Explorateur de métadonnées, puis vous synchronisez les objets avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, comme indiqué dans la procédure suivante. Par défaut, si les objets existent déjà dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, et si les métadonnées SSMA comportent des modifications locales ou des mises à jour de la définition de ces objets, SSMA modifie les définitions d’objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. Vous pouvez modifier le comportement par défaut en modifiant les **paramètres du projet**.  
  
> [!NOTE]  
> Vous pouvez sélectionner [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des objets existants ou Azure SQL Database qui n’ont pas été convertis à partir de bases de données Access. Toutefois, SSMA ne recrée pas ou ne modifie pas ces objets.  
  
**Pour synchroniser des objets avec SQL Server ou SQL Azure**  
  
1.  Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure l’Explorateur de métadonnées, développez le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nœud supérieur ou SQL Azure, puis développez **bases de données**.  
  
2.  Sélectionnez les objets à traiter :  
  
    -   Pour synchroniser une base de données complète, activez la case à cocher en regard du nom de la base de données.  
  
    -   Pour synchroniser ou omettre des objets individuels ou des catégories d’objets, activez ou désactivez la case à cocher en regard de l’objet ou du dossier.  
  
3.  Une fois que vous avez sélectionné les objets à traiter dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure Explorateur de métadonnées, cliquez avec le bouton droit sur **bases de données**, puis cliquez sur **synchroniser avec la base de données**.  
  
    Vous pouvez également synchroniser des objets individuels ou des catégories d’objets en cliquant avec le bouton droit sur l’objet ou son dossier parent, puis en cliquant sur **synchroniser avec la base de données**.  
  
    Ensuite, SSMA affiche la boîte **de dialogue synchroniser avec la base de données** , où vous pouvez voir deux groupes d’éléments. Sur le côté gauche, SSMA affiche les objets de base de données sélectionnés représentés dans une arborescence. Sur le côté droit, vous pouvez voir une arborescence qui représente les mêmes objets dans les métadonnées SSMA. Vous pouvez développer l’arborescence en cliquant sur le bouton « + » de droite ou de gauche. La direction de la synchronisation est indiquée dans la colonne action placée entre les deux arbres.  
  
    Un signe d’action peut être dans trois États :  
  
    -   Une flèche gauche signifie que le contenu des métadonnées est enregistré dans la base de données (valeur par défaut).  
  
    -   Une flèche droite signifie que le contenu de la base de données remplacera les métadonnées SSMA.  
  
    -   Un signe dièse signifie qu’aucune action n’est effectuée.  
  
    Cliquez sur le signe action pour modifier l’État. La synchronisation réelle est effectuée lorsque vous cliquez sur le bouton **OK** de la boîte de dialogue **synchroniser avec la base de données** .  
  
## <a name="scripting-objects"></a>Scripts d’objets  
Si vous souhaitez enregistrer les [!INCLUDE[tsql](../../includes/tsql-md.md)] définitions des objets de base de données convertis, ou si vous souhaitez modifier les définitions d’objet et exécuter vous-même les scripts, vous pouvez enregistrer les définitions d’objet de base de données converties dans des [!INCLUDE[tsql](../../includes/tsql-md.md)] scripts.  
  
**Pour enregistrer un ou plusieurs objets dans un script**  
  
1.  Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’Explorateur de métadonnées, développez le nœud supérieur (nom du serveur), puis développez **bases de données**.  
  
2.  Effectuez une ou plusieurs des actions suivantes :  
  
    -   Pour générer un script d’une base de données complète, activez la case à cocher en regard du nom de la base de données.  
  
    -   Pour générer un script ou omettre des vues individuelles, développez la base de données, développez **vues**, puis activez ou désactivez la case à cocher en regard de la vue.  
  
    -   Pour générer un script ou omettre des tables individuelles, développez la base de données, développez **tables**, puis activez ou désactivez la case à cocher en regard de la table.  
  
    -   Pour générer un script ou omettre des index individuels pour une table, développez la table, développez **index**, puis sélectionnez ou effacez l’index.  
  
3.  Cliquez avec le bouton droit sur **bases de données** , puis sélectionnez **enregistrer en tant que script**.  
  
    Vous pouvez également générer un script pour des objets individuels. Pour générer un script pour un objet, quels que soient les objets sélectionnés, cliquez avec le bouton droit sur l’objet et sélectionnez **enregistrer en tant que script**.  
  
4.  Dans la boîte de dialogue **Enregistrer sous** , localisez le dossier dans lequel vous souhaitez enregistrer le script, entrez un nom de fichier dans la zone **nom de fichier** , puis cliquez sur **OK**.  
  
    SSMA va ajouter l’extension de nom de fichier. Sql.  
  
### <a name="modifying-scripts"></a>Modification de scripts  
Une fois que vous avez enregistré les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définitions d’objets ou SQL Azure en tant que script, vous pouvez utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour modifier le script.  
  
**Pour modifier un script**  
  
1.  Dans le menu [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] **Fichier** , pointez sur **Ouvrir**, puis cliquez sur **Fichier**.  
  
2.  Dans la boîte de dialogue **ouvrir** , recherchez et sélectionnez votre fichier de script, puis cliquez sur **OK**.  
  
3.  Modifiez le fichier de script à l’aide de l’éditeur de requête.  
  
    Pour plus d’informations sur l’éditeur de requête, consultez « commandes et fonctionnalités pratiques de l’éditeur » dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentation en ligne de.  
  
4.  Pour enregistrer le script, dans le menu fichier, sélectionnez **Enregistrer**.  
  
### <a name="running-scripts"></a>Exécution de scripts  
Vous pouvez exécuter un script ou des instructions individuelles dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
**Pour exécuter un script**  
  
1.  Dans le [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] menu **fichier** , pointez sur **ouvrir** , puis cliquez sur **fichier**.  
  
2.  Dans la boîte de dialogue **ouvrir** , recherchez et sélectionnez votre fichier de script, puis cliquez sur **OK**.  
  
3.  Pour exécuter le script complet, appuyez sur la touche **F5** .  
  
4.  Pour exécuter un ensemble d’instructions, sélectionnez les instructions dans la fenêtre de l’éditeur de requête, puis appuyez sur la touche **F5** .  
  
Pour plus d’informations sur l’utilisation de l’éditeur de requête pour exécuter des scripts, consultez « [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] [!INCLUDE[tsql](../../includes/tsql-md.md)] requête » dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentation en ligne de.  
  
Vous pouvez également exécuter des scripts à partir de la ligne de commande à l’aide de l’utilitaire **sqlcmd** et à partir de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agent. Pour plus d’informations sur **sqlcmd**, consultez « utilitaire sqlcmd » dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentation en ligne de. Pour plus d’informations sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’agent, consultez « automatisation des tâches d’administration ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agent) » dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentation en ligne de.  
  
## <a name="securing-objects-in-sql-server"></a>Sécurisation des objets dans SQL Server  
Une fois que vous avez chargé les objets de base de données convertis dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez accorder et refuser des autorisations sur ces objets. Il est judicieux de le faire avant de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations sur la sécurisation des objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez « Considérations sur la sécurité pour les bases de données et les applications de base de données » dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentation en ligne de.  
  
## <a name="next-step"></a>étape suivante  
L’étape suivante du processus de migration consiste à [migrer des données vers SQL Server](migrating-access-data-into-sql-server-azure-sql-db-accesstosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
