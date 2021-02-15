---
description: Définitions de rôles - Rôles prédéfinis
title: Définitions de rôles - Rôles prédéfinis | Microsoft Docs
ms.date: 02/04/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- security [Reporting Services], defaults
- default security
- role-based security [Reporting Services], defaults
ms.assetid: 6b46db51-7c30-467d-a251-50f50647fe21
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 19846d90b09ce660d2ffbd2e1ef996984057d420
ms.sourcegitcommit: 6f4fb9cfd0cad06127a6328adc745e2ba7c191d1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99570443"
---
# <a name="role-definitions---predefined-roles"></a>Définitions de rôles - Rôles prédéfinis

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est installé avec des rôles prédéfinis que vous pouvez utiliser pour accorder l'accès aux opérations du serveur de rapports. Chaque rôle prédéfini décrit une collection de tâches associées. Vous pouvez assigner des groupes et des comptes d'utilisateurs à des rôles prédéfinis pour fournir l'accès immédiat aux opérations du serveur de rapports.  
  
## <a name="how-to-use-predefined-roles"></a>Comment utiliser des rôles prédéfinis  
  
1. Examinez les rôles prédéfinis pour déterminer si vous pouvez les utiliser en l'état. Si vous devez ajuster les tâches ou définir des rôles supplémentaires, vous devez le faire avant de commencer à assigner des utilisateurs à des rôles spécifiques. Pour créer ou modifier des rôles personnalisés, utilisez SQL Server Management Studio. Pour plus d’informations, consultez [Créer, supprimer ou modifier un rôle &#40;Management Studio&#41;](../../reporting-services/security/role-definitions-create-delete-or-modify.md).
  
2. Identifiez les utilisateurs et les groupes qui doivent accéder au serveur de rapports, et à quel niveau. Le rôle **Lecteur** ou le rôle **Générateur de rapports** doit être attribué à la plupart des utilisateurs. Le rôle **Serveur de publication** doit être attribué à un nombre restreint d'utilisateurs. Le rôle **Gestionnaire de contenu** ne doit être attribué qu'à un nombre très limité d'utilisateurs.  

3. Lorsque vous êtes prêt à attribuer des comptes d'utilisateurs et de groupes à des rôles spécifiques, utilisez le portail web. Pour plus d’informations, consultez [Accorder à un utilisateur l’accès à un serveur de rapports](../../reporting-services/security/grant-user-access-to-a-report-server.md).  
  
##  <a name="predefined-role-definitions"></a><a name="bkmk_rolelist"></a> Définitions de rôles prédéfinis  
 Les rôles prédéfinis sont définis par les tâches qu'ils prennent en charge. Vous pouvez modifier ces rôles ou les remplacer par des rôles personnalisés.  
  
 La *portée* définit les limites dans lesquelles les rôles sont utilisés. Les rôles au niveau élément fournissent des niveaux d'accès variés aux éléments du serveur de rapports et aux opérations qui affectent ces éléments. Les rôles au niveau élément sont définis sur le nœud racine (de base) et sur tous les éléments de l'arborescence des dossiers du serveur de rapports. Les rôles au niveau système autorisent l'accès au niveau du site. Les rôles au niveau élément et au niveau système sont mutuellement exclusifs mais sont utilisés ensemble pour fournir des autorisations complètes au contenu et aux opérations du serveur de rapports.  
  
 Le tableau suivant décrit la portée prédéfinie des rôles :  
  
|Rôle prédéfini|Étendue|Description|  
|---------------------|-----------|-----------------|  
|[Rôle du Gestionnaire de contenu](#bkmk_content)|Élément|Peut gérer le contenu dans le serveur de rapports. Cela inclut des dossiers, des rapports et des ressources.|  
|[Rôle Serveur de publication](#bkmk_publisher)|Élément|Peut publier des rapports et des rapports liés au serveur de rapports.|  
|[Rôle Lecteur](#bkmk_browser)|Élément|Peut afficher des dossiers, rapports et vous abonner aux rapports.|  
|[Rôle Générateur de rapports](#bkmk_reportbuilder)|Élément|Peut lire les définitions de rapport.|  
|[Rôle Mes Rapports](#bkmk_myreports)|Élément|Peut publier des rapports et des rapports liés ; gérer des dossiers, des rapports et des ressources dans un dossier d’utilisateurs Mes rapports.|  
|[Rôle Administrateur système](#bkmk_systemadministrator)|Système|Afficher et modifier des attributions de rôles système, définitions de rôles système, propriétés système et planifications partagées, en plus de créer des définitions de rôle et gérer des tâches dans Management Studio.|  
|[Rôle Utilisateur système](#bkmk_systemuser)|Système|Afficher les propriétés système, planifications partagées et autoriser l’utilisation du Générateur de rapports ou d’autres clients qui exécutent les définitions de rapport.|  
  
##  <a name="content-manager-role"></a><a name="bkmk_content"></a> Rôle Gestionnaire de contenu  
 Le rôle **Gestionnaire de contenu** est prédéfini et comprend des tâches qui permettent à un utilisateur de gérer des rapports et du contenu Web, sans nécessairement créer des rapports ni gérer un serveur Web ou une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un gestionnaire de contenu déploie des rapports, gère des connexions aux modèles de rapport et aux sources de données, et prend des décisions sur le mode d'utilisation des rapports. Toutes les tâches au niveau élément sont sélectionnées par défaut pour la définition de rôle **Gestionnaire de contenu** .  
  
 Le rôle **Gestionnaire de contenu** est souvent utilisé avec le rôle **Administrateur système** . Ces deux définitions de rôles fournissent un ensemble complet de tâches aux utilisateurs qui ont besoin d'un accès complet à tous les éléments se trouvant sur un serveur de rapports. Bien que le rôle **Gestionnaire de contenu** offre un accès complet aux rapports, modèles de rapports, dossiers et autres éléments au sein de la hiérarchie des dossiers, il ne permet pas d’accéder aux éléments de niveau site ni aux opérations. Des tâches telles que la création et la gestion des planifications partagées, la définition des propriétés de serveur et la gestion des définitions de rôles sont des tâches de niveau système qui sont incluses dans le rôle **Administrateur système** . Pour cette raison, nous vous recommandons de créer une deuxième attribution de rôle au niveau site, permettant d'accéder aux planifications partagées.  
  
### <a name="content-manager-tasks"></a>Tâches du Gestionnaire de contenu  
 Le tableau suivant répertorie les tâches qui sont comprises dans le rôle **Gestionnaire de contenu** :  
  
|Tâche|Description|  
|----------|-----------------|  
|Effectuer des commentaires sur les rapports|Créer, afficher, modifier et supprimer des commentaires sur les rapports.| 
|Lire les rapports|Lire les définitions de rapport.|  
|Créer des rapports liés|Créer des rapports liés qui sont basés sur un rapport non lié.|  
|Gérer tous les abonnements|Afficher, modifier et supprimer un abonnement à un rapport, lié ou non, quel que soit le propriétaire de l'abonnement. Cette tâche prend en charge la création d’abonnements pilotés par les données. Elle prend également en charge la modification et l’exécution de l’[actualisation planifiée pour les fichiers Power BI (.pbix) dans Power BI Report Server](/power-bi/report-server/configure-scheduled-refresh).|  
|Gérer les commentaires|Supprimer les commentaires d’autres utilisateurs sur les rapports.| 
|Gérer les sources de données|Créer et supprimer les éléments de source de données partagée, afficher et modifier les propriétés et le contenu des sources de données.|  
|Gérer les dossiers|Créer, afficher et supprimer des dossiers. Afficher et modifier des propriétés de dossier.| 
|Gérer les abonnements individuels|Créer, afficher, modifier et supprimer des abonnements - appartenant à des utilisateurs - à des rapports, liés ou non. Cette tâche prend également en charge la modification et l’exécution de l’[actualisation planifiée pour les fichiers Power BI (.pbix) dans Power BI Report Server](/power-bi/report-server/configure-scheduled-refresh).|  
|Gérer les modèles|Créer, afficher et supprimer des modèles. Afficher et modifier des propriétés du modèle.|  
|Gérer l'historique de rapport|Créer, afficher et supprimer l'historique de rapport, afficher les propriétés de l'historique de rapport. Afficher et modifier les paramètres qui déterminent les limites de l'historique des instantanés ainsi que le fonctionnement de la mise en cache.|  
|Gérer les rapports|Ajouter et supprimer des rapports, modifier des paramètres de rapport, afficher et modifier des propriétés de rapport, afficher et modifier des sources de données qui fournissent du contenu au rapport, afficher et modifier des définitions de rapports et définir des stratégies au niveau du rapport.|  
|Gestion des ressources|Créer, modifier et supprimer des ressources. Afficher et modifier des propriétés de ressource.|  
|Définir la sécurité pour des éléments individuels|Définir des stratégies de sécurité pour les rapports, les rapports liés, les dossiers, les ressources et les sources de données. Pour plus d’informations, consultez [Éléments sécurisables](../../reporting-services/security/securable-items.md).|  
|Afficher les sources de données|Afficher les éléments de source de données partagée dans la hiérarchie de dossiers.|  
|Afficher les dossiers|Afficher le contenu des dossiers et naviguer dans la hiérarchie des dossiers.|  
|Afficher les modèles|Afficher des modèles de l'arborescence des dossiers, utiliser des modèles comme sources de données pour un rapport et exécuter des requêtes sur le modèle pour récupérer des données.|  
|Afficher les rapports|Exécuter les rapports et afficher les propriétés de rapports.|  
|Afficher les ressources|Afficher les ressources et les propriétés des ressources.|  
  
### <a name="customizing-the-content-manager-role"></a>Personnalisation du rôle du Gestionnaire de contenu  
 Ce rôle est destiné aux utilisateurs approuvés qui ont la responsabilité générale de la gestion et de la maintenance du contenu du serveur de rapports. Vous pouvez supprimer des tâches de cette définition, mais vous risquez d'introduire de l'ambiguïté dans ce qui peut être géré. Par exemple, supprimer la tâche « Afficher les rapports » de cette définition de rôle interdirait à un **Gestionnaire de contenu** d'afficher du contenu de rapports, rendant ainsi impossible la vérification des modifications apportées aux paramètres et aux paramètres relatifs aux informations d'identification.  
  
 Le rôle **Gestionnaire de contenu** est utilisé dans la sécurité par défaut.  
  
##  <a name="publisher-role"></a><a name="bkmk_publisher"></a> Rôle Serveur de publication  
 Le rôle **Serveur de publication** est une définition de rôle intégrée qui inclut des tâches permettant aux utilisateurs d’ajouter du contenu à un serveur de rapports. Ce rôle est prédéfini pour plus de commodité. Il n'est pas utilisé tant que vous n'avez pas créé des attributions de rôles qui l'incluent. Ce rôle est destiné aux utilisateurs qui créent des rapports ou des modèles dans le Concepteur de rapports ou le Générateur de modèles avant de les publier sur un serveur de rapports.  
  
> [!CAUTION]  
> L'autorisation de publier des éléments sur un serveur de rapports doit être accordée uniquement à des utilisateurs approuvés. Le rôle Serveur de publication octroie un large éventail d'autorisations qui permettent aux utilisateurs de télécharger tout type de fichier sur un serveur de rapports. Si un fichier HTML ou un rapport téléchargé contient un script malveillant, l'utilisateur qui clique sur le rapport ou le document HTML exécutera le script sous ses informations d'identification.  
  
 Les définitions de rapport peuvent inclure du script et d'autres éléments qui sont vulnérables aux attaques par injection HTML lorsque le rapport est rendu au format HTML lors de l'exécution. Si un rapport publié contient un script malveillant, l'utilisateur qui exécute ce rapport provoquera accidentellement l'exécution du script lors de l'ouverture du rapport. Si l'utilisateur dispose d'autorisations élevées, le script sera exécuté sous ces autorisations.  
  
 Pour réduire le risque que des utilisateurs exécutent accidentellement des scripts malveillants, limitez le nombre d'utilisateurs autorisés à publier du contenu et assurez-vous que ces utilisateurs publient uniquement des documents et des rapports issus de sources approuvées. Lorsque vous ne savez pas si une définition de rapport peut être publiée en toute sécurité, vous devez ouvrir le fichier .rdl dans un éditeur de texte et rechercher des balises de script éventuelles. Un script malveillant peut être caché dans des expressions et des URL (par exemple, une URL dans une action de navigation).  
  
### <a name="publisher-tasks"></a>Tâches du serveur de publication  
 Le tableau ci-dessous répertorie les tâches qui sont comprises dans le rôle **Serveur de publication** :  
  
|Tâche|Description|  
|----------|-----------------|  
|Créer des rapports liés|Créer des rapports liés puis les publier dans un dossier de serveur de rapports.|  
|Gérer les commentaires|Supprimer les commentaires d’autres utilisateurs sur les rapports.| 
|Gérer les sources de données|Créer et supprimer les éléments de source de données partagée, afficher et modifier les propriétés et le contenu des sources de données.|  
|Gérer les dossiers|Créer, afficher et supprimer des dossiers ; afficher et modifier des propriétés de dossier.|  
|Gérer les modèles|Créer, afficher et supprimer des modèles de rapport ; afficher et modifier des propriétés de modèles de rapport.|  
|Gérer les rapports|Ajouter et supprimer des rapports, modifier des paramètres de rapport, afficher et modifier des propriétés de rapport, afficher et modifier des sources de données qui fournissent du contenu au rapport, afficher et modifier des définitions de rapports.|  
|Gestion des ressources|Créer, modifier et supprimer des ressources ; afficher et modifier des propriétés de ressource.|  
  
### <a name="customizing-the-publisher-role"></a>Personnalisation du rôle de serveur de publication  
 Modifiez le rôle **Serveur de publication** en fonction de vos besoins. Par exemple, vous pouvez supprimer la tâche « Créer des rapports liés » si vous ne souhaitez pas que les utilisateurs puissent créer et publier des rapports liés, ou ajouter la tâche « Afficher les dossiers » pour que les utilisateurs puissent naviguer dans la hiérarchie de dossiers lorsqu'ils sélectionnent un emplacement pour un nouvel élément.  
  
 Les utilisateurs qui publient des rapports à partir du Concepteur de rapports requièrent au minimum la tâche « Gérer les rapports » pour pouvoir ajouter un rapport sur le serveur de rapports. Si l'utilisateur doit publier des rapports qui utilisent des sources de données partagées ou des fichiers externes, vous devez également inclure les tâches « Gérer les sources de données » et « Gérer les ressources ». Si l'utilisateur doit également disposer de la capacité de créer un dossier au cours du processus de publication, vous devez également inclure la tâche « Gérer les dossiers ».  
  
##  <a name="browser-role"></a><a name="bkmk_browser"></a> Rôle Navigateur  
 Le rôle **Lecteur** est un rôle prédéfini qui comprend des tâches utiles pour les utilisateurs qui affichent des rapports sans nécessairement les créer ou les gérer. Ce rôle fournit des capacités de base pour un usage conventionnel d'un serveur de rapports. Sans ces tâches, il peut être difficile pour les utilisateurs d'utiliser un serveur de rapports.  
  
 Le rôle **Lecteur** doit être utilisé avec le rôle **Utilisateur système** . Ces deux définitions de rôles fournissent un ensemble complet de tâches aux utilisateurs qui interagissent avec des éléments sur un serveur de rapports. Bien que le rôle **Lecteur** permette d’afficher les rapports, les modèles de rapports, les dossiers et d’autres éléments au sein de la hiérarchie des dossiers, il ne permet pas d’accéder aux éléments de niveau site, tels que les planifications partagées, utiles lors de la création d’abonnements. Pour cette raison, nous vous recommandons de créer une deuxième attribution de rôle au niveau site, permettant d'accéder aux planifications partagées.  
  
### <a name="browser-tasks"></a>Tâches navigateur  
 Le tableau ci-dessous décrit les tâches qui sont comprises dans la définition du rôle **Navigateur** :  
  
|Tâche|Description|  
|----------|-----------------|  
|Effectuer des commentaires sur les rapports|Créer, afficher, modifier et supprimer des commentaires sur les rapports.| 
|Gérer les abonnements individuels|Créer, afficher, modifier et supprimer des abonnements - appartenant à des utilisateurs - à des rapports, liés ou non, et créer des planifications pour ces abonnements.| 
|Afficher les dossiers|Afficher le contenu des dossiers et naviguer dans la hiérarchie des dossiers.| 
|Afficher les modèles|Afficher des modèles de l'arborescence des dossiers, utiliser des modèles comme sources de données pour un rapport et exécuter des requêtes sur le modèle pour récupérer des données.| 
|Afficher les rapports|Exécuter un rapport et afficher les propriétés du rapport.|  
|Afficher les ressources|Afficher les ressources et les propriétés des ressources.|  
   
### <a name="customizing-the-browser-role"></a>Personnalisation du rôle Navigateur  
 Modifiez le rôle **Lecteur** en fonction de vos besoins. Par exemple, vous pouvez supprimer la tâche « Gérer les abonnements individuels » si vous ne voulez pas prendre en charge les abonnements, ou supprimer la tâche « Afficher les ressources » si vous ne voulez pas que les utilisateurs puissent consulter la documentation associée ou d'autres éléments pouvant être transférés dans le serveur de rapports.  
  
 Ce rôle doit comprendre au minimum les tâches « Afficher les rapports » et « Afficher les dossiers » pour permettre l'affichage et la navigation dans les dossiers. Vous ne devez pas supprimer la tâche « Afficher les dossiers » à moins de vouloir interdire la navigation dans les dossiers. De même, vous ne devez pas supprimer la tâche « Afficher les rapports », à moins de vouloir empêcher les utilisateurs d'afficher les rapports. Ces types de modifications suggèrent la nécessité d'une définition de rôle personnalisée appliquée sélectivement à un groupe spécifique d'utilisateurs.  
  
##  <a name="report-builder-role"></a><a name="bkmk_reportbuilder"></a> Rôle Générateur de rapports  
 Le rôle **Générateur de rapports** est un rôle prédéfini qui comprend des tâches pour le chargement des rapports dans le Générateur de rapports ainsi que pour l'affichage et l'exploration de la hiérarchie des dossiers. Pour créer et modifier des rapports dans le Générateur de rapports, vous devez par ailleurs posséder une attribution de rôle système qui comprend la tâche « Exécuter les définitions de rapport », nécessaire au traitement local des rapports dans le Générateur de rapports.  
  
### <a name="report-builder-tasks"></a>Tâches du Générateur de rapports  
 Le tableau ci-dessous décrit les tâches qui sont comprises dans la définition du rôle **Générateur de rapports** :  
  
|Tâche|Description|  
|----------|-----------------|  
|Effectuer des commentaires sur les rapports|Créer, afficher, modifier et supprimer des commentaires sur les rapports.| 
|Lire les rapports|Lire les définitions de rapport.|  
|Gérer les abonnements individuels|Créer, afficher, modifier et supprimer des abonnements - appartenant à des utilisateurs - à des rapports, liés ou non, et créer des planifications pour ces abonnements.|  
|Afficher les dossiers|Afficher le contenu des dossiers et naviguer dans la hiérarchie des dossiers.|  
|Afficher les modèles|Afficher des modèles de l'arborescence des dossiers, utiliser des modèles comme sources de données pour un rapport et exécuter des requêtes sur le modèle pour récupérer des données.|  
|Afficher les rapports|Exécuter un rapport et afficher les propriétés du rapport.|  
|Afficher les ressources|Afficher les ressources et les propriétés des ressources.|  
  
### <a name="customizing-the-report-builder-role"></a>Personnalisation du rôle Générateur de rapports  
 Modifiez le rôle **Générateur de rapports** en fonction de vos besoins. Les recommandations sont généralement les mêmes que celles liées au rôle **Lecteur** : supprimez la tâche « Gérer les abonnements individuels » si vous ne souhaitez pas prendre en charge les abonnements, supprimez la tâche « Afficher les ressources » si vous ne souhaitez pas que les utilisateurs visualisent les ressources et conservez les tâches « Afficher les rapports » et « Afficher les dossiers » pour prendre en charge l'affichage et l'exploration des dossiers.  
  
 La tâche la plus importante de cette définition de rôle est « Lire les rapports », car elle permet à un utilisateur de charger une définition de rapport à partir du serveur de rapports dans une instance locale du Générateur de rapports. Si vous ne souhaitez pas prendre en charge cette tâche, supprimez cette définition de rôle et utilisez le rôle **Lecteur** pour prendre en charge l'accès général à un serveur de rapports.  
  
##  <a name="my-reports-role"></a><a name="bkmk_myreports"></a> Rôle Mes Rapports  
 Le rôle **Mes Rapports** est un rôle prédéfini qui inclut un ensemble de tâches qui sont utiles pour les utilisateurs de la fonctionnalité Mes Rapports. Cette définition de rôle inclut des tâches qui accordent aux utilisateurs des autorisations administratives sur le dossier Mes Rapports qui leur appartient.  
  
 Bien que vous puissiez choisir un autre rôle à utiliser avec la fonctionnalité Mes Rapports, il est recommandé d'en choisir un qui serve exclusivement à la sécurité de Mes Rapports. Pour plus d’informations, consultez [Sécuriser Mes Rapports](../../reporting-services/security/secure-my-reports.md).  
                          
### <a name="my-reports-tasks"></a>Tâches du rôle Mes Rapports  
 Le tableau ci-dessous répertorie les tâches qui sont comprises dans le rôle **Mes Rapports** :  
  
|Tâche|Description|  
|----------|-----------------|  
|Effectuer des commentaires sur les rapports|Créer, afficher, modifier et supprimer des commentaires sur les rapports.| 
|Créer des rapports liés|Créer des rapports liés qui sont basés sur les rapports qui sont stockés dans le dossier Mes Rapports de l'utilisateur.|  
|Gérer les commentaires|Supprimer les commentaires d’autres utilisateurs sur les rapports.| 
|Gérer les sources de données|Créer et supprimer les éléments de source de données partagée, afficher et modifier les propriétés et le contenu des sources de données.|  
|Gérer les dossiers|Créer, afficher et supprimer des dossiers. Afficher et modifier des propriétés de dossier.|  
|Gérer les abonnements individuels|Créer, afficher, modifier et supprimer des abonnements à des rapports, liés ou non.|  
|Gérer l'historique de rapport|Créer, afficher et supprimer l'historique de rapport, afficher les propriétés de l'historique de rapport. Afficher et modifier les paramètres qui déterminent les limites de l'historique des instantanés ainsi que le fonctionnement de la mise en cache.|  
|Gérer les rapports|Ajouter et supprimer des rapports, modifier des paramètres de rapport, afficher et modifier des propriétés de rapport, afficher et modifier des sources de données qui fournissent du contenu au rapport, afficher et modifier des définitions de rapports et définir des stratégies au niveau du rapport.|  
|Gestion des ressources|Créer, modifier, puis supprimer des ressources et afficher-les. et modifier des propriétés de ressource.|  
|Afficher les sources de données|Afficher les éléments de source de données partagée dans la hiérarchie de dossiers.|  
|Afficher les dossiers|Afficher le contenu des dossiers.|  
|Afficher les rapports|Exécuter les rapports qui sont stockés dans le dossier Mes Rapports de l'utilisateur et afficher les propriétés des rapports.|  
|Afficher les ressources|Afficher les ressources et les propriétés des ressources.|  
  
### <a name="customizing-the-my-reports-role"></a>Personnalisation du rôle Mes Rapports  
 Vous pouvez modifier ce rôle en fonction de vos besoins. Toutefois, il est recommandé que vous conserviez les tâches « Gérer les rapports » et « Gérer les dossiers » pour permettre la gestion du contenu de base. En outre, ce rôle doit prendre en charge toutes les tâches d'affichage pour que les utilisateurs puissent voir le contenu des dossiers et exécuter les rapports qu'ils gèrent.  
  
 Bien que la tâche « Définir la sécurité pour des éléments individuels » ne fasse pas partie de la définition de rôle par défaut, vous pouvez ajouter cette tâche au rôle **Mes Rapports** pour que les utilisateurs puissent personnaliser les paramètres de sécurité pour les sous-dossiers et les rapports.  
  
##  <a name="system-administrator-role"></a><a name="bkmk_systemadministrator"></a> Rôle Administrateur système  
 Le rôle **Administrateur système** est un rôle prédéfini qui comprend des tâches utiles pour un administrateur qui a la responsabilité générale du serveur de rapports, mais pas nécessairement de son contenu.  
  
 Pour créer une attribution de rôle qui inclut ce rôle, utilisez la page Paramètres du site dans le portail web ou utilisez les commandes accessibles par clic droit sur le nœud du serveur de rapports dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 Le rôle **Administrateur système** n'implique pas le même éventail complet d'autorisations qu'un administrateur local peut posséder sur un ordinateur. À la place, le rôle **Administrateur système** inclut les opérations qui sont effectuées au niveau du site et non au niveau de l'élément. Pour les utilisateurs qui nécessitent l’accès aux opérations à l’échelle du site et aux éléments stockés sur le serveur de rapports, créez une seconde attribution de rôle sur le dossier de base incluant le rôle **Gestionnaire de contenu** . Ces deux définitions de rôles fournissent un ensemble complet de tâches aux utilisateurs qui ont besoin d'un accès complet à tous les éléments se trouvant sur un serveur de rapports.  
  
### <a name="system-administrator-tasks"></a>Tâches de l’Administrateur système  
 Le tableau ci-dessous répertorie les tâches qui sont comprises dans le rôle **Administrateur système** :  
  
|Tâche|Description|  
|----------|-----------------|  
|Exécuter les définitions de rapport|Démarrer l'exécution de la définition de rapport sans la publier sur un serveur de rapports.|  
|Gestion des travaux|Afficher et annuler les travaux en cours d'exécution. Pour plus d’informations, consultez [Gérer un processus en cours d’exécution](../../reporting-services/subscriptions/manage-a-running-process.md).|  
|Gérer les propriétés du serveur de rapports|Afficher et modifier les propriétés qui s'appliquent au serveur de rapports et aux éléments gérés par le serveur de rapports.<br /><br /> Cette tâche permet le changement de nom du portail web, l'activation de Mes Rapports et la définition des paramètres par défaut de l'historique de rapport.|  
|Gérer la sécurité du serveur de rapports|Afficher et modifier des attributions de rôles au niveau du système|  
|Gérer les rôles|Créer, afficher, modifier et supprimer les définitions de rôles.<br /><br /> Les membres du rôle **Administrateur système** peuvent utiliser la page Paramètres du site pour gérer les rôles.|  
|Gérer les planifications partagées|Créer, afficher, modifier et supprimer les planifications partagées qui sont utilisées pour exécuter ou actualiser les rapports.|  
  
 Le rôle **Administrateur système** est utilisé dans la sécurité par défaut.  
  
##  <a name="system-user-role"></a><a name="bkmk_systemuser"></a> Rôle Utilisateur système  
Le rôle **Utilisateur système** est un rôle prédéfini qui comprend des tâches permettant aux utilisateurs d'afficher des informations de base sur le serveur de rapports. Il prend également en charge le chargement d'un rapport dans le Générateur de rapports. Le Générateur de rapports est une application cliente qui peut traiter un rapport indépendamment d'un serveur de rapports. La tâche « Exécuter les définitions de rapport » est destinée à être utilisée avec le Générateur de rapports. Si vous n'utilisez pas le Générateur de rapports, vous pouvez supprimer cette tâche du rôle **Utilisateur système** .  

Le tableau ci-dessous répertorie les tâches qui sont comprises dans la définition du rôle **Utilisateur système** :  
  
### <a name="system-user-tasks"></a>Tâches Utilisateur système  
  
|Tâche|Description|  
|----------|-----------------|  
|Exécuter les définitions de rapport|Exécutez un rapport sans le publier sur un serveur de rapports.|  
|Afficher les propriétés du serveur de rapports|Afficher les propriétés qui s'appliquent au serveur de rapports, telles que le nom de l'application, l'état d'activation du paramètre Mes Rapports ainsi que les paramètres par défaut de l'historique de rapport.<br /><br /> Si vous supprimez cette tâche du rôle **Utilisateur système** , la page Paramètres du site n'est pas disponible. Par ailleurs, le titre de l'application n'est pas affiché en haut de chaque page. Par défaut, le titre du portail web est «[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. »|  
|Afficher les planifications partagées|Afficher les planifications partagées qui sont utilisées pour exécuter les rapports ou actualiser un rapport.<br /><br /> Si vous supprimez cette tâche du rôle **Utilisateur système** , les utilisateurs ne peuvent pas sélectionner les planifications partagées à utiliser avec les abonnements et autres opérations planifiées.|  
  
 Le rôle **Utilisateur système** peut être utilisé pour compléter la sécurité par défaut. Vous pouvez inclure le rôle dans de nouvelles attributions de rôles qui étendent l'accès du serveur de rapports aux utilisateurs des rapports. Pour plus d’informations, consultez [Octroi d'autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md).  
  
## <a name="see-also"></a>Voir aussi  
[Créer, supprimer ou modifier un rôle &#40;Management Studio&#41;](../../reporting-services/security/role-definitions-create-delete-or-modify.md)  
[Accorder un accès utilisateur l'accès à un serveur de rapports](../../reporting-services/security/grant-user-access-to-a-report-server.md)  
[Modifier ou supprimer une attribution de rôle &#40; portail web SSRS&#41;](../../reporting-services/security/role-assignments-modify-or-delete.md)  
[Octroi d'autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)  
[Tâches et autorisations](../../reporting-services/security/tasks-and-permissions.md)
