---
description: Sécurité du collecteur de données
title: Sécurité du collecteur de données | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- data collection [SQL Server]
- security [data collector]
- data collector [SQL Server], security
ms.assetid: e75d6975-641e-440a-a642-cb39a583359a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a5e968978dd641f78dfb17d515ab07e3b7e27485
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250138"
---
# <a name="data-collector-security"></a>Sécurité du collecteur de données
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Le collecteur de données utilise le modèle de sécurité basée sur les rôles implémenté par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ce modèle permet à l'administrateur de base de données d'exécuter les différentes tâches du collecteur de données dans un contexte de sécurité qui ne dispose que des autorisations requises pour effectuer ces tâches. Cette méthode est également utilisée pour les opérations qui impliquent des tables internes, uniquement accessibles à l'aide d'une procédure stockée ou d'une vue. Aucune autorisation n'est accordée aux tables internes. En revanche, il est procédé à une vérification des autorisations de l'utilisateur de la procédure stockée ou de la vue utilisée pour accéder à une table.  
  
> [!IMPORTANT]  
>  Les autorisations concentriques constituent un autre aspect clé de ce modèle de sécurité. Dans le cadre des autorisations concentriques, les rôles les plus privilégiés héritent des autorisations des rôles les moins privilégiés sur des objets (notamment les alertes, les opérateurs, les travaux, les planifications et les proxys). Pour plus d'informations, consultez [SQL Server Agent Fixed Database Roles](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Les sections suivantes offrent une description générale de la sécurité de la collecte de données, ainsi que des rôles que vous devez attribuer aux utilisateurs pour qu'ils puissent configurer et utiliser le collecteur de données, et effectuer des tâches associées à l'entrepôt de données de gestion.  
  
## <a name="general-security"></a>Sécurité générale  
 Le collecteur de données est installé en fonction des normes documentées spécifiées pour [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
### <a name="network-security"></a>Sécurité réseau  
 Des informations sensibles peuvent être transmises entre des instances cibles, l'instance relationnelle associée au serveur de configuration, les jeux d'éléments de collection en cours d'exécution et le serveur hébergeant l'entrepôt de données de gestion.  
  
 Pour protéger des données quelconques transférées sur un réseau, les mécanismes de sécurité standard sont implémentés, tels que le chiffrement de protocole pour [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="permissions-for-configuring-and-using-the-data-collector"></a>Autorisations de configuration et d'utilisation du collecteur de données  
 En fonction de la tâche, les utilisateurs doivent être membres d'un ou plusieurs des rôles de base de données fixes fournis pour le collecteur de données. Ces rôles, présentés ici du plus privilégié au moins privilégié en termes d'accès, sont les suivants :  
  
-   **dc_admin**  
  
-   **dc_operator**  
  
-   **dc_proxy**  
  
 Ces rôles sont stockés dans la base de données msdb. Par défaut, aucun utilisateur n'est membre de ces rôles de base de données. L'appartenance d'un utilisateur à ces rôles doit être accordée explicitement.  
  
 Les utilisateurs membres du rôle serveur fixe **sysadmin** disposent d'un accès complet aux objets de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et aux vues du collecteur de données. Ils doivent toutefois être ajoutés explicitement aux rôles de collecteur de données.  
  
> [!IMPORTANT]  
>  Les membres du rôle db_ssisadmin et du rôle dc_admin peuvent être en mesure d'élever leurs privilèges à sysadmin. Cette élévation de privilège peut se produire, car ces rôles peuvent modifier les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] peuvent être exécutés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide du contexte de sécurité sysadmin de l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour vous prémunir contre cette élévation de privilège lors de l'exécution de plans de maintenance, de jeux d'éléments de collecte de données et d'autres packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , configurez des travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui exécutent des packages pour l'utilisation d'un compte proxy doté de privilèges limités ou ajoutez uniquement des membres sysadmin aux rôles db_ssisadmin et dc_admin.  
  
### <a name="dc_admin-role"></a>Rôle dc_admin  
 Les utilisateurs dotés du rôle **dc_admin** disposent d’un accès administrateur complet (en création, lecture, mise à jour et suppression) à la configuration du collecteur de données sur une instance de serveur. Les membres de ce rôle peuvent effectuer les opérations suivantes :  
  
-   définir des propriétés au niveau du collecteur ;  
  
-   ajouter des jeux d'éléments de collection ;  
  
-   installer de nouveaux types de collections ;  
  
-   effectuer toutes les opérations autorisées pour le rôle **dc_operator** .  
  
 Le rôle **dc_admin** est membre des rôles suivants :  
  
-   **SQLAgentUserRole**. Ce rôle est requis pour créer des planifications et exécuter des travaux.  
  
    > [!NOTE]  
    >  Les proxys créés pour le collecteur de données doivent accorder l’accès au rôle **dc_admin** pour les créer et les utiliser dans toutes les étapes de travail nécessitant un proxy.  
  
-   **dc_operator**. Les membres du rôle **dc_admin** héritent des autorisations accordées au rôle **dc_operator**.  
  
### <a name="dc_operator-role"></a>Rôle dc_operator  
 Les membres du rôle **dc_operator** disposent d’un accès en lecture et mise à jour. Ce rôle prend en charge les tâches d'opérations liées à l'exécution et la configuration des jeux d'éléments de collection. Les membres de ce rôle peuvent effectuer les opérations suivantes :  
  
-   démarrer ou arrêter un jeu d'éléments de collection ;  
  
-   énumérer les jeux d'éléments de collection existants ;  
  
-   afficher les informations détaillées (par exemple, les éléments de collection et la fréquence de collecte) associées à un jeu d'éléments de collection ;  
  
-   modifier la fréquence de téléchargement des jeux d'éléments de collection existants ;  
  
-   modifier la fréquence de collecte des éléments de collecte appartenant à un jeu d'éléments de collecte existant.  
  
 Le rôle **dc_operator** est membre des rôles suivants, requis pour énumérer et afficher les packages du collecteur de données :  
  
-   **db_ssisltduser**  
  
-   **db_ssisoperator**  
  
 Pour plus d’informations, consultez [Rôles Integration Services &#40;Service SSIS&#41;](../../integration-services/security/integration-services-roles-ssis-service.md).  
  
### <a name="dc_proxy-role"></a>Rôle dc_proxy  
 Les membres du rôle **dc_proxy** disposent d’un accès en lecture aux jeux d’éléments de collecte du collecteur de données et aux propriétés au niveau du collecteur. Les membres de ce rôle peuvent également exécuter les travaux dont ils sont propriétaires, et créer des étapes de travail qui s'exécutent comme un compte proxy existant.  
  
 Les membres de ce rôle peuvent effectuer les opérations suivantes :  
  
-   consulter des informations sur la configuration des jeux d'éléments de collection (par exemple, les paramètres d'entrée des éléments de collection et la fréquence de collecte de ces éléments) ;  
  
-   obtenir des informations chiffrées internes uniquement accessibles par l'intermédiaire d'une procédure stockée signée (par exemple, les informations de connexion d'entrepôt de données utilisées pour les téléchargements de données) ;  
  
-   enregistrer des événements d'exécution de jeu d'éléments de collecte.  
  
 Le rôle **dc_proxy** est membre des rôles suivants requis pour énumérer et afficher les packages du collecteur de données :  
  
-   **db_ssisltduser**.  
  
-   **db_ssisoperator**  
  
 Pour plus d’informations, consultez [Rôles Integration Services &#40;Service SSIS&#41;](../../integration-services/security/integration-services-roles-ssis-service.md).  
  
## <a name="permissions-for-configuring-and-using-the-management-data-warehouse"></a>Autorisations de configuration et d'utilisation de l'entrepôt de données de gestion  
 En fonction de la tâche, les utilisateurs doivent être membres d'un ou plusieurs des rôles de base de données fixes fournis pour accéder à l'entrepôt de données de gestion. Ces rôles, présentés ici du plus privilégié au moins privilégié en termes d'accès, sont les suivants :  
  
-   **mdw_admin**  
  
-   **mdw_writer**  
  
-   **mdw_reader**  
  
 Ces rôles sont stockés dans la base de données msdb. Par défaut, aucun utilisateur n'est membre de ces rôles de base de données. L'appartenance d'un utilisateur à ces rôles doit être accordée explicitement.  
  
 Les utilisateurs qui sont membres du rôle serveur fixe **sysadmin** ont un accès complet aux vues du collecteur de données. Toutefois, ils doivent être ajoutés explicitement aux rôles de base de données pour effectuer d'autres opérations.  
  
### <a name="mdw_admin-role"></a>Rôle mdw_admin  
 Les membres du rôle **mdw_admin** disposent d’un accès en lecture, écriture, mise à jour et suppression à l’entrepôt de données de gestion.  
  
 Les membres de ce rôle peuvent effectuer les opérations suivantes :  
  
-   modifier le schéma de l'entrepôt de données de gestion lorsque cela est nécessaire (par exemple, ajouter une table lorsqu'un nouveau type de collecte est installé) ;  
  
    > [!NOTE]  
    >  En cas de modification de schéma, l’utilisateur doit également être membre du rôle **dc_admin** pour installer un nouveau type de collecteur, car cette action requiert l’autorisation de mise à jour de la configuration du collecteur de données dans la base de données msdb.  
  
-   exécuter des travaux de maintenance sur l'entrepôt de données de gestion, tels que l'archivage ou le nettoyage.  
  
### <a name="mdw_writer-role"></a>Rôle mdw_writer  
 Les membres du rôle **mdw_writer** peuvent télécharger et écrire des données dans l’entrepôt de données de gestion. Tout collecteur de données qui stocke des données dans l'entrepôt de données de gestion doit être membre de ce rôle.  
  
### <a name="mdw_reader-role"></a>Rôle mdw_reader  
 Les membres du rôle **mdw_reader** disposent d’un accès en lecture à l’entrepôt de données de gestion. Comme ce rôle a pour but de prendre en charge la résolution de problèmes en fournissant l'accès aux données historiques, les membres de ce rôle ne peuvent pas consulter les autres éléments du schéma de l'entrepôt de données de gestion.  
  
## <a name="see-also"></a>Voir aussi  
 [Implémenter la sécurité de l'Agent SQL Server](../../ssms/agent/implement-sql-server-agent-security.md)  
  
  
