---
title: Exécuter le débogueur Transact-SQL
description: Découvrez comment personnaliser le débogueur Transact-SQL et l’utiliser pour déboguer votre code Transact-SQL. Vous pouvez exécuter le débogueur sur une instance du moteur de base de données qui se trouve sur un autre ordinateur.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, sysadmin requirement
- Transact-SQL debugger, supported versions
- Query Editor [Database Engine], right-click menu
- debugging [SQL Server], T-SQL debugger
- Transact-SQL debugger, Query Editor shortcut menu
- Transact-SQL debugger, stopping
- Transact-SQL debugger, Debug menu
- debugging [SQL Server]
- Transact-SQL debugger, Debug toolbar
- Transact-SQL debugger, keyboard shortcuts
- Transact-SQL debugger, starting
ms.assetid: 386f6d09-dbec-4dc7-9e8a-cd9a4a50168c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cb075a71ec6a4004de4691a53b8b487eadbd65c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354532"
---
# <a name="run-the-transact-sql-debugger"></a>Exécuter le débogueur Transact-SQL

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Vous pouvez démarrer le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] après avoir ouvert une fenêtre de l’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Vous pouvez ensuite exécuter votre code [!INCLUDE[tsql](../../includes/tsql-md.md)] en mode débogage jusqu'à ce que vous arrêtiez le débogueur. Vous pouvez définir des options permettant de personnaliser la façon dont le débogueur s'exécute.

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="starting-and-stopping-the-debugger"></a>Démarrage et arrêt du débogueur

La configuration requise pour démarrer le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] est la suivante :

- Si votre éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est connecté à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] sur un autre ordinateur, vous avez dû configurer le débogueur de façon qu’il accepte le débogage distant. Pour plus d’informations, consultez [Configurer des règles de pare-feu avant d’exécuter le débogueur TSQL](./configure-firewall-rules-before-running-the-tsql-debugger.md).
  
- [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] doit s'exécuter sous un compte Windows qui est membre du rôle serveur fixe sysadmin.

- La fenêtre de l’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit être connectée à l’aide d’une connexion via l’authentification Windows ou l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui est membre du rôle serveur fixe sysadmin.
  
- La fenêtre de l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit être connectée à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 (SP2) ou version ultérieure. Vous ne pouvez pas exécuter le débogueur lorsque la fenêtre de l'éditeur de requête est connectée à une instance en mode mono-utilisateur.  
  
 Nous vous recommandons de déboguer le code [!INCLUDE[tsql](../../includes/tsql-md.md)] sur un serveur test, et non sur un serveur de production, pour les raisons suivantes :
  
- Le débogage est une opération hautement privilégiée. Par conséquent, seuls les membres du rôle serveur fixe sysadmin sont autorisés à déboguer dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
- Les sessions de débogage s'exécutent en général assez longtemps lorsque vous étudiez les opérations de plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] . Les verrous, tels que les verrous de mise à jour, qui sont acquis par la session peuvent être appliqués pendant des périodes prolongées, jusqu'à ce que la session soit terminée ou jusqu'à ce que la transaction soit validée ou restaurée.  
  
 Au démarrage du débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] , la fenêtre de l'éditeur de requête passe en mode débogage. Lorsque la fenêtre de l'éditeur de requête entre en mode débogage, le débogueur s'arrête à la première ligne de code. Vous pouvez ensuite parcourir le code, suspendre l'exécution au niveau d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] spécifiques et utiliser les fenêtres du débogueur pour examiner l'état d'exécution actuel. Vous pouvez démarrer le débogueur en cliquant sur le bouton **Déboguer** dans la barre d'outils **Requête** ou en cliquant sur **Démarrer le débogage** dans le menu **Déboguer** .  
  
 La fenêtre de l'éditeur de requête reste en mode débogage jusqu'à la fin de la dernière instruction dans la fenêtre de l'éditeur de requête ou jusqu'à l'arrêt du mode débogage. Vous pouvez arrêter le mode débogage et l'exécution d'instructions en utilisant l'une des méthodes suivantes :  
  
- Dans le menu **Déboguer** , cliquez sur **Arrêter le débogage**.  
  
- Dans la barre d'outils **Déboguer** , cliquez sur le bouton **Arrêter le débogage** .  
  
- Dans le menu **Requête** , cliquez sur **Annuler l'exécution de la requête**.  
  
- Dans la barre d'outils **Requête** , cliquez sur le bouton **Annuler l'exécution de la requête** .  
  
 Vous pouvez également arrêter le mode débogage et laisser l'exécution des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] restantes se terminer en cliquant sur **Détacher tout** dans le menu **Déboguer** .  
  
## <a name="controlling-the-debugger"></a>Contrôle du débogueur

 Vous pouvez contrôler le mode de fonctionnement du débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] à l'aide des commandes de menu, des barres d'outils et des raccourcis suivants :  
  
- Menu **Déboguer** et barre d'outils **Déboguer** . Le menu **Déboguer** et la barre d'outils **Déboguer** sont inactifs tant que le focus ne se trouve pas sur une fenêtre ouverte de l'éditeur de requête. Ils restent actifs jusqu'à la fermeture du projet actif.  
  
- Raccourcis clavier du débogueur.  
  
- Menu contextuel Éditeur de requête. Le menu contextuel s'affiche lorsque vous cliquez avec le bouton droit sur une ligne dans une fenêtre de l'éditeur de requête. Lorsque la fenêtre de l'éditeur de requête est en mode débogage, le menu contextuel affiche les commandes du débogueur qui s'appliquent à la ligne ou à la chaîne sélectionnée.  
  
- Éléments de menu et commandes contextuelles dans les fenêtres ouvertes par le débogueur, telles que les fenêtres **Espion** ou **Points d'arrêt** .  
  
 Le tableau suivant répertorie les commandes de menu, les boutons de barre d'outils et les raccourcis clavier du débogueur.  
  
|Commande du menu Déboguer|Commande de raccourci de l'éditeur|Bouton de la barre d'outils|Raccourci clavier|Action|  
|------------------------|-----------------------------|--------------------|-----------------------|------------|  
|**Fenêtres/Points d'arrêt**|Non disponible|**Points d'arrêt**|Ctrl+Alt+B|Affiche la fenêtre **Points d'arrêt** , dans laquelle vous pouvez afficher et gérer les points d'arrêt.|  
|**Fenêtres/Espion/Espion 1**|Non disponible|**Points d'arrêt/Espion/Espion 1**|Ctrl+Alt+W, 1|Affiche la fenêtre **Espion 1** .|  
|**Fenêtres/Espion/Espion 2**|Non disponible|**Points d'arrêt/Espion/Espion 2**|Ctrl+Alt+W, 2|Affiche la fenêtre **Espion 2** .|  
|**Fenêtres/Espion/Espion 3**|Non disponible|**Points d'arrêt/Espion/Espion 3**|Ctrl+Alt+W, 3|Affiche la fenêtre **Espion 3** .|  
|**Fenêtres/Espion/Espion 4**|Non disponible|**Points d'arrêt/Espion/Espion 4**|Ctrl+Alt+W, 4|Affiche la fenêtre **Espion 4** .|  
|**Fenêtres/Variables locales**|Non disponible|**Points d'arrêt/Variables locales**|Ctrl+Alt+V, L|Affiche la fenêtre **Variables locales** .|  
|**Fenêtres/Pile des appels**|Non disponible|**Points d'arrêt/Pile des appels**|Ctrl+Alt+C|Affiche la fenêtre **Pile des appels** .|  
|**Fenêtres/Threads**|Non disponible|**Points d'arrêt/Threads**|Ctrl+Alt+H|Affiche la fenêtre **Threads** .|  
|**Continuer**|Non disponible|**Continuer**|Alt+F5|Exécuter le code jusqu'au point d'arrêt suivant. **Continuer** est inactif tant que le focus ne se trouve pas sur une fenêtre de l'éditeur de requête en mode débogage.|  
|**Démarrer le débogage**|Non disponible|**Démarrer le débogage**|Alt+F5|Fait passer une fenêtre de l'éditeur de requête en mode débogage et exécute le code jusqu'au premier point d'arrêt. Si le focus se trouve sur une fenêtre de l'éditeur de requête en mode débogage, **Démarrer le débogage** est remplacé par **Continuer**.|  
|**Interrompre tout**|Non disponible|**Interrompre tout**|Ctrl+Alt+Pause|Cette fonctionnalité n'est pas utilisée par le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**Arrêter le débogage**|Non disponible|**Arrêter le débogage**|Maj+F5|Fait sortir une fenêtre de l'éditeur de requête du mode débogage et restaure le mode normal.|  
|**Détacher tout**|Non disponible|Non disponible|Non disponible|Arrête le mode débogage, mais exécute les instructions restantes dans la fenêtre de l'éditeur de requête.|  
|**Pas à pas détaillé**|Non disponible|**Pas à pas détaillé**|F11|Exécute l'instruction suivante et ouvre une nouvelle fenêtre de l'éditeur de requête en mode débogage si l'instruction suivante exécute une procédure stockée, un déclencheur ou une fonction.|  
|**Pas à pas principal**|Non disponible|**Pas à pas principal**|F10|Identique à **Pas à pas détaillé**, à la différence près que les fonctions, les procédures stockées ou les déclencheurs ne font pas l'objet d'un débogage.|  
|**Pas à pas sortant**|Non disponible|**Pas à pas sortant**|Maj+F11|Exécute le code restant dans un déclencheur, une fonction ou une procédure stockée sans stopper aux points d'arrêt. Le mode débogage normal reprend lorsque le contrôle est retourné au code ayant appelé le module.|  
|Non disponible|**Exécuter jusqu'au curseur**|Non disponible|Ctrl+F10|Exécute la totalité du code du dernier emplacement d'arrêt à l'emplacement du curseur actuel sans stopper aux points d'arrêt.|  
|**Espion express**|**Espion express**|Non disponible|Ctrl+Alt+Q|Affiche la fenêtre **Espion express** .|  
|**Basculer le point d’arrêt**|**Point d'arrêt/Insérer un point d'arrêt**|Non disponible|F9|Positionne un point d'arrêt sur l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] actuelle ou sélectionnée.|  
|Non disponible|**Point d'arrêt/Supprimer un point d'arrêt**|Non disponible|Non disponible|Supprime le point d'arrêt de la ligne sélectionnée.|  
|Non disponible|**Point d'arrêt/Désactiver un point d'arrêt**|Non disponible|Non disponible|Désactive le point d'arrêt sur la ligne sélectionnée. Le point d'arrêt reste sur la ligne de code, mais il n'arrête pas l'exécution tant qu'il n'est pas réactivé.|  
|Non disponible|**Point d'arrêt/Activer le point d'arrêt**|Non disponible|Non disponible|Active le point d'arrêt sur la ligne sélectionnée.|  
|**Supprimer tous les points d'arrêt**|Non disponible|Non disponible|Ctrl+Maj+F9|Supprime tous les points d'arrêt.|  
|**Désactiver tous les points d'arrêt**|Non disponible|Non disponible|Non disponible|Désactive tous les points d'arrêt.|  
|Non disponible|**Ajouter un espion**|Non disponible|Non disponible|Ajoute l'expression sélectionnée à la fenêtre **Espion** .|  
  
## <a name="see-also"></a>Voir aussi

- [Débogueur Transact-SQL](./transact-sql-debugger.md)
- [Exécuter pas à pas du code Transact-SQL](./step-through-transact-sql-code.md)
- [Informations du débogueur Transact-SQL](./transact-sql-debugger-information.md)
- [Éditeur de requête du moteur de base de données &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Statistiques des requêtes dynamiques](../../relational-databases/performance/live-query-statistics.md)