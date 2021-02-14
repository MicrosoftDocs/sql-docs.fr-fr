---
title: Chargement des objets de base de données convertis dans SQL Server (OracleToSQL) | Microsoft Docs
description: Découvrez comment charger les objets de base de données que vous avez convertis à partir d’Oracle dans l’instance de SQL Server à l’aide de SSMA pour Oracle.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Synchronization, Securing Objects in SQL Server
- Synchronization,Scripting Objects
ms.assetid: a8ae33b2-1883-4785-922b-ea0e31c0b37a
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 4b315b0b26f771a2e215f03e1abcfeb5f829e86b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080800"
---
# <a name="loading-converted-database-objects-into-sql-server-oracletosql"></a>Chargement d’objets de base de données convertis dans SQL Server (OracleToSQL)
Après avoir converti les schémas Oracle en SQL Server, vous pouvez charger les objets de base de données résultants dans SQL Server. Vous pouvez faire en sorte que SSMA crée les objets, ou vous pouvez générer un script pour les objets et exécuter les scripts vous-même. En outre, SSMA vous permet de mettre à jour les métadonnées cibles avec le contenu réel de SQL Server base de données.  
  
## <a name="choosing-between-synchronization-and-scripts"></a>Choix entre la synchronisation et les scripts  
Si vous souhaitez charger les objets de base de données convertis dans SQL Server sans modification, vous pouvez demander à SSMA de créer ou de recréer directement les objets de base de données. Cette méthode est rapide et facile, mais elle n’autorise pas la personnalisation du [!INCLUDE[tsql](../../includes/tsql-md.md)] code qui définit les objets SQL Server, à l’exception des procédures stockées.  
  
Si vous souhaitez modifier le [!INCLUDE[tsql](../../includes/tsql-md.md)] utilisé pour créer des objets, ou si vous souhaitez davantage de contrôle sur la création d’objets, utilisez SSMA pour créer des scripts. Vous pouvez ensuite modifier ces scripts, créer chaque objet individuellement et même utiliser SQL Server Agent pour planifier la création de ces objets.  
  
## <a name="using-ssma-to-synchronize-objects-with-sql-server"></a>Utilisation de SSMA pour synchroniser des objets avec SQL Server  
Pour utiliser SSMA pour créer SQL Server objets de base de données, sélectionnez les objets dans SQL Server Explorateur de métadonnées, puis synchronisez les objets avec SQL Server, comme indiqué dans la procédure suivante. Par défaut, si les objets existent déjà dans SQL Server, et si les métadonnées SSMA sont plus récentes que l’objet dans SQL Server, SSMA modifie les définitions d’objets dans SQL Server. Vous pouvez modifier le comportement par défaut en modifiant les **paramètres du projet**.  
  
> [!NOTE]  
> Vous pouvez sélectionner des objets de base de données SQL Server existants qui n’ont pas été convertis à partir des bases de données Oracle. Toutefois, ces objets ne seront pas recréés ou modifiés par SSMA.  
  
**Pour synchroniser des objets avec SQL Server**  
  
1.  Dans SQL Server l’Explorateur de métadonnées, développez le nœud du haut SQL Server, puis développez **bases de données**.  
  
2.  Sélectionnez les objets à traiter :  
  
    -   Pour synchroniser une base de données complète, activez la case à cocher en regard du nom de la base de données.  
  
    -   Pour synchroniser ou omettre des objets individuels ou des catégories d’objets, activez ou désactivez la case à cocher en regard de l’objet ou du dossier.  
  
3.  Une fois que vous avez sélectionné les objets à traiter dans SQL Server Explorateur de métadonnées, cliquez avec le bouton droit sur **bases de données**, puis cliquez sur **synchroniser avec la base de données**.  
  
    Vous pouvez également synchroniser des objets individuels ou des catégories d’objets en cliquant avec le bouton droit sur l’objet ou son dossier parent, puis en cliquant sur  **synchroniser avec la base de données**.  
  
    Ensuite, SSMA affiche la boîte **de dialogue synchroniser avec la base de données** , où vous pouvez voir deux groupes d’éléments. Sur le côté gauche, SSMA affiche les objets de base de données sélectionnés représentés dans une arborescence. Sur le côté droit, vous pouvez voir une arborescence qui représente les mêmes objets dans les métadonnées SSMA. Vous pouvez développer l’arborescence en cliquant sur le bouton « + » de droite ou de gauche. La direction de la synchronisation est indiquée dans la colonne action placée entre les deux arbres.  
  
    Un signe d’action peut être dans trois États :  
  
    -   Une flèche gauche signifie que le contenu des métadonnées est enregistré dans la base de données (valeur par défaut).  
  
    -   Une flèche droite signifie que le contenu de la base de données remplacera les métadonnées SSMA.  
  
    -   Un signe dièse signifie qu’aucune action n’est effectuée.  
  
Cliquez sur le signe action pour modifier l’État. La synchronisation réelle est effectuée lorsque vous cliquez sur le bouton **OK** de la boîte de dialogue **synchroniser avec la base de données** .  
  
## <a name="scripting-objects"></a>Scripts d’objets  
Pour enregistrer les définitions [!INCLUDE[tsql](../../includes/tsql-md.md)] des objets de base de données convertis, ou pour modifier les définitions d’objet et exécuter vous-même les scripts, vous pouvez enregistrer les définitions d’objet de base de données converties dans des [!INCLUDE[tsql](../../includes/tsql-md.md)] scripts.  
  
**Pour enregistrer des objets en tant que scripts**  
  
1.  Une fois que vous avez sélectionné les objets à enregistrer dans un script, cliquez avec le bouton droit sur **bases de données**, puis cliquez sur **enregistrer en tant que script**.  
  
    Vous pouvez également générer un script pour des objets individuels ou des catégories d’objets en cliquant avec le bouton droit sur l’objet ou son dossier parent, puis en cliquant sur **enregistrer en tant que script**.  
  
2.  Dans la boîte de dialogue **Enregistrer sous** , localisez le dossier dans lequel vous souhaitez enregistrer le script, entrez un nom de fichier dans la zone **nom de fichier** , puis cliquez sur OK. l’extension de nom de fichier. SQL est alors ajoutée.  
  
### <a name="modifying-scripts"></a>Modification de scripts  
Une fois que vous avez enregistré les définitions d’objets SQL Server sous la forme d’un ou de plusieurs scripts, vous pouvez utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour afficher et modifier les scripts.  
  
**Pour modifier un script**  
  
1.  Dans le menu [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Fichier** , pointez sur **Ouvrir**, puis cliquez sur **Fichier**.  
  
2.  Dans la boîte de dialogue **ouvrir** , sélectionnez votre fichier de script, puis cliquez sur OK.
  
3.  Modifiez le fichier de script à l’aide de l’éditeur de requête.  
  
    Pour plus d’informations sur l’éditeur de requête, consultez « commandes et fonctionnalités pratiques de l’éditeur » dans Documentation en ligne de SQL Server.  
  
4.  Pour enregistrer le script, dans le menu fichier, cliquez sur **Enregistrer**.  
  
### <a name="running-scripts"></a>Exécution de scripts  
Vous pouvez exécuter un script ou des instructions individuelles dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
**Pour exécuter un script**  
  
1.  Dans le menu [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Fichier** , pointez sur **Ouvrir**, puis cliquez sur **Fichier**.  
  
2.  Dans la boîte de dialogue **ouvrir** , sélectionnez votre fichier de script, puis cliquez sur OK.  
  
3.  Pour exécuter le script complet, appuyez sur la touche **F5** .  
  
4.  Pour exécuter un ensemble d’instructions, sélectionnez les instructions dans la fenêtre de l’éditeur de requête, puis appuyez sur la touche **F5** .  
  
Pour plus d’informations sur l’utilisation de l’éditeur de requête pour exécuter des scripts, consultez « [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] [!INCLUDE[tsql](../../includes/tsql-md.md)] requête » dans documentation en ligne de SQL Server.  
  
Vous pouvez également exécuter des scripts à partir de la ligne de commande à l’aide de l’utilitaire **sqlcmd** et à partir de l’SQL Server Agent. Pour plus d’informations sur **sqlcmd**, consultez « utilitaire sqlcmd » dans documentation en ligne de SQL Server. Pour plus d’informations sur les SQL Server Agent, consultez « automatisation des tâches d’administration (SQL Server Agent) » dans Documentation en ligne de SQL Server.  
  
## <a name="securing-objects-in-sql-server"></a>Sécurisation des objets dans SQL Server  
Une fois que vous avez chargé les objets de base de données convertis dans SQL Server, vous pouvez accorder et refuser des autorisations sur ces objets. Il est judicieux de le faire avant de migrer des données vers SQL Server. Pour plus d’informations sur la sécurisation des objets dans SQL Server, consultez « Considérations sur la sécurité pour les bases de données et les applications de base de données » dans Documentation en ligne de SQL Server.  
  
## <a name="next-step"></a>étape suivante  
L’étape suivante du processus de migration consiste à [migrer des données vers SQL Server](migrating-oracle-data-into-sql-server-oracletosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Oracle vers SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
