---
title: Modifier la plateforme cible et publier un projet de base de données
description: Découvrez comment modifier la plateforme d’un projet de base de données SQL Server Data Tools en une instance prise en charge de SQL Server. Découvrez comment publier un projet de base de données.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.publish.dialog
- sql.data.tools.publishdacproject
ms.assetid: 6012e120-5f72-4f4f-ae6e-f9a57ae1dea7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 8a5a9cec35620f408182af3df75702eb85ce4e27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018459"
---
# <a name="how-to-change-target-platform-and-publish-a-database-project"></a>Procédure : Modifier la plateforme cible et publier un projet de base de données

Vous pouvez remplacer la version cible de SQL Server de votre projet de base de données SQL Server Data Tools (SSDT) par n’importe quelle instance prise en charge de SQL Server (SQL Server 2005, 2008, 2008 R2, Microsoft SQL Server 2012 ou SQL Azure). Cela vous permettra de centraliser le développement de votre base de données dans un seul projet, tout en la publiant dans plusieurs instances SQL Server en cas de besoin.  
  
SSDT simplifie aussi cette tâche en tenant compte de votre plateforme cible et en détectant automatiquement les erreurs dans votre code (par exemple, lorsque vous utilisez des fonctionnalités non prises en charge pour un projet qui va être publié sur SQL Azure).  
  
> [!WARNING]  
> Les procédures suivantes utilisent les entités créées dans les procédures précédentes des sections [Développement de base de données connectée](../ssdt/connected-database-development.md) et [Développement de base de données hors connexion orienté projet](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-change-a-projects-target-platform"></a>Pour modifier la plateforme cible d’un projet  
  
1.  Cliquez avec le bouton droit sur votre projet dans **l’Explorateur de solutions**, puis sélectionnez **Propriétés**. Cliquez sur l'onglet **Paramètres du projet** à gauche pour accéder à la page de propriétés **Paramètres du projet** .  
  
2.  La liste déroulante **Plateforme cible** sur cette page contient toutes les plateformes SQL Server prises en charge sur lesquelles un projet de base de données peut être publié. Pour cette procédure, sélectionnez **SQL Azure**.  
  
### <a name="to-use-platform-validation-when-editing-scripts"></a>Pour utiliser la fonctionnalité de validation de plateforme lors de la modification des scripts  
  
1.  Cliquez avec le bouton droit sur la table **Products** dans l’Explorateur de solutions, puis sélectionnez **Afficher le code** pour l’ouvrir dans l’Éditeur Transact\-SQL.  
  
2.  Ajoutez `ON [PRIMARY]` à la fin de l'instruction `CREATE TABLE` .  
  
3.  L’erreur suivante s’affiche dans le volet **Liste d’erreurs** : SQL70015 : Le « schéma de partition et de référence du groupe de fichiers » n’est pas pris en charge dans SQL Azure.  
  
    SSDT valide automatiquement votre script en fonction de la plateforme cible. Dans ce cas, étant donné que le groupe de fichiers n'est pas pris en charge dans SQL Azure, SSDT retourne une erreur. Pour connaître la liste des instructions Transact\-SQL non prises en charge dans SQL Azure, voir [Instructions Transact-SQL partiellement prises en charge (Microsoft Azure SQL Database)](/previous-versions/azure/ee336267(v=azure.100)).  
  
4.  Supprimez la clause `ON` . Notez que l'erreur disparaît immédiatement de la **Liste d'erreurs**.  
  
### <a name="to-publish-a-database-project"></a>Pour publier un projet de base de données  
  
1.  Si vous avez accès à une instance SQL Azure, vous pouvez passer à l'étape suivante. Sinon, cliquez avec le bouton droit sur le projet **TradeDev** dans **l’Explorateur de solutions** et sélectionnez **Propriétés** pour accéder à la page de propriétés **Paramètres du projet**. Utilisez la liste déroulante **Plateforme cible** pour sélectionner la plateforme SQL Server sur laquelle vous souhaitez publier le projet.  
  
2.  Cliquez avec le bouton droit sur le projet **TradeDev** dans **l’Explorateur de solutions**, puis sélectionnez **Publier**. SSDT commence à générer votre projet. Si aucune erreur de build n'est détectée, la boîte de dialogue **Publier la base de données** s'affiche.  
  
3.  Dans la boîte de dialogue **Publier la base de données** , cliquez sur **Modifier** pour modifier la connexion de la base de données cible.  
  
4.  Dans la boîte de dialogue **Propriétés de connexion**, entrez le nom de votre instance SQL Server et vos informations d’identification pour l’authentification. Sous **Connexion à la base de données**, entrez **NewTrade**. Vous tentez ainsi de publier votre projet de base de données dans une nouvelle base de données. Vous pouvez également choisir une base de données existante sur laquelle effectuer la publication. Par exemple, si vous choisissez la base de données **TradeDev** existante, toutes les modifications apportées aux objets (comme les scripts) dans le projet **TradeDev** hors connexion seront propagées sur la base de données **TradeDev** active.  
  
    Si vous disposez des autorisations nécessaires pour apporter des modifications à la base de données dans laquelle vous souhaitez publier, cliquez sur le bouton **Publier** . Si, au contraire, vous n’avez pas accès en écriture à une base de données de production, vous pouvez cliquer sur le bouton **Générer un script** pour produire un script de publication Transact\-SQL, qui pourra ensuite être remis à un Administrateur de base de données. L'administrateur de base de données peut ensuite exécuter le script pour mettre à jour le serveur de production afin que son schéma soit synchronisé avec le projet de base de données.  
  
5.  La fenêtre **Opérations des outils de données**  affiche la progression de l'opération de publication et vous avertit qu'une erreur s'est produite. Dans cette fenêtre, vous pouvez également choisir d'afficher l'aperçu de déploiement, le script généré ou les résultats de la publication complète, si vous le souhaitez.  
  
6.  Vous pouvez aussi enregistrer les paramètres de publication dans un profil, afin que vous puissiez réutiliser les mêmes paramètres à des fins de publication ultérieure. Pour cela, cliquez sur le bouton **Enregistrer le profil sous** dans la boîte de dialogue **Publier la base de données** . À l'avenir, vous pourrez cliquer sur le bouton **Charger le profil** lorsque vous souhaiterez recharger des paramètres existants.  
  
7.  Notez les messages dans la fenêtre **Opérations des outils de données** . Cliquez sur le lien « Afficher l’aperçu » situé à droite de **Création de l’aperçu de la publication…** . Le rapport d'aperçu du déploiement s'ouvre. Si la plateforme cible de votre projet n’est pas identique au serveur de base de données sur lequel le projet est publié, SSDT émet un avertissement dans ce rapport.  Par exemple, si la plateforme cible de votre projet est Microsoft SQL Server 2012 et que vous tentez de publier le projet sur une instance de serveur SQL Server 2008 R2, l’avertissement suivant s’affiche dans la fenêtre **Sortie** :  
  
**Un projet qui spécifie Microsoft SQL Server 2012 comme plateforme cible risque de rencontrer des problèmes de compatibilité avec SQL Server 2008.** S’il contient des entités (par exemple, un objet Séquence) introduites dans Microsoft SQL Server 2012, l’opération de publication échouera.  
  
Le déploiement échoue si les prédicats d'objet utilisent **CONTAINS** OU **FREETEXT** sur un index de recherche en texte intégral nouvellement créé et que des scripts transactionnels sont utilisés. Si l'option consistant à inclure des scripts transactionnels est activée au cours du déploiement, les procédures et vues sont définies au sein d'une transaction, alors qu'un index de texte intégral est défini à l'extérieur d'une transaction à la fin du script de déploiement. En raison de cette organisation dans le script, les procédures ou les vues utilisant CONTAINS ou FREETEXT ne sont pas résolues par rapport à l'index de texte intégral dans une erreur de déploiement.  
