---
title: Effectuer une mise à niveau vers une autre édition
description: Le programme d’installation de SQL Server prend en charge la mise à niveau d’édition entre les différentes éditions de SQL Server. Avant de commencer une mise à niveau d’édition, passez en revue les ressources de cet article.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 31d16820-d126-4c57-82cc-27701e4091bc
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a1787dd26f51b6b712c253270bfbc14641d19b21
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813208"
---
# <a name="upgrade-to-a-different-edition-of-sql-server-setup"></a>Mettre à niveau vers une autre édition de SQL Server 2016 (Installation)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

L'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge la mise à niveau d'édition entre les différentes éditions de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur les chemins de mise à niveau d’édition pris en charge, consultez [Mises à niveau de version et d’édition prises en charge](../../database-engine/install-windows/supported-version-and-edition-upgrades-2017.md). Avant de lancer la mise à niveau d’édition d’une instance de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], consultez les articles suivants :  

  [Éditions et fonctionnalités prises en charge de SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md)
- [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)  
- [Éditions et fonctionnalités prises en charge de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)  
- [Limites de capacité de calcul par édition de SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md)  
- [Configurations matérielle et logicielle requises pour l’installation de SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)  
  
> [!NOTE]  
> **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur une instance de cluster de basculement :** exécuter une mise à niveau de l’édition sur l’un des nœuds de l’instance du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suffit. Le nœud peut être actif ou passif, et le moteur ne met pas les ressources hors connexion pendant la mise à niveau de l’édition. Après la mise à niveau de l'édition, il est nécessaire de redémarrer l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou de la basculer sur un nœud différent.  
  
## <a name="prerequisites"></a>Prérequis  
Pour des installations locales, vous devez exécuter le programme d'installation en tant qu'administrateur. Si vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'un partage distant, vous devez utiliser un compte de domaine qui dispose des autorisations de lecture sur le partage distant.  
  
> [!IMPORTANT]  
> Pour activer la modification de l'édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez redémarrer les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L'application n'est ainsi pas disponible pendant que les services sont hors connexion.  
  
## <a name="procedure"></a>Procédure  
  
### <a name="to-upgrade-to-a-different-edition-of-ssnoversion"></a>Pour effectuer une mise à niveau vers une autre édition de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)]  
  
1.  Insérez le support d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Au niveau du dossier racine, double-cliquez sur setup.exe ou lancez le Centre d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir des Outils de configuration. Pour effectuer l'installation à partir d'un partage réseau, recherchez le dossier racine sur le partage, puis double-cliquez sur Setup.exe.  
  
2.  Pour mettre à niveau une instance existante de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] vers une autre édition, dans le Centre d’installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cliquez sur **Maintenance**, puis sélectionnez **Mise à niveau d’édition**.  
  
3.  Si les fichiers de support du programme d'installation sont requis, le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les installe. Redémarrez votre ordinateur si vous êtes invité à le faire avant de continuer.  
  
4.  L'Outil d'analyse de configuration système effectue une opération de découverte sur votre ordinateur. Pour continuer, cliquez sur **OK**.  
  
5.  Dans la page Clé de produit, sélectionnez une case d'option pour indiquer si vous effectuez une mise à niveau vers une édition gratuite de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ou si vous disposez d'une clé PID pour une version de production du produit. Pour plus d’informations, consultez [Éditions et composants de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2017.md) et [Mises à niveau de version et d’édition prises en charge](../../database-engine/install-windows/supported-version-and-edition-upgrades.md).  
  
6.  Dans la page Termes du contrat de licence, prenez connaissance du contrat de licence et activez la case à cocher indiquant que vous en acceptez les termes et conditions. Pour continuer, cliquez sur **Suivant**. Pour mettre fin au programme d'installation, cliquez sur **Annuler**.  
  
7.  Dans la page Sélectionner une instance, spécifiez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à mettre à niveau.  
  
8.  La page Règles de mise à niveau d'édition valide la configuration de votre ordinateur avant le début de la mise à niveau de l'édition.  
  
9. La page Prêt pour la mise à niveau de l'édition affiche une arborescence des options d'installation spécifiées durant l'exécution du programme d'installation. Pour continuer, cliquez sur **Mettre à niveau**.  
  
10. Pendant la mise à niveau de l'édition, les services doivent redémarrer pour prendre en compte la nouvelle configuration. Après la mise à niveau de l'édition, la page Terminé fournit un lien vers le fichier journal résumé de la mise à niveau de l'édition. Pour fermer l’Assistant, cliquez sur **Fermer**.  
  
11. La page Terminé fournit un lien vers le fichier journal résumé pour l'installation et d'autres remarques importantes.  
  
12. Redémarrez l'ordinateur si vous êtes invité à le faire. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d’informations sur les fichiers journaux d’installation, consultez [Afficher et lire les fichiers journaux d’installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
13. Si vous avez effectué une mise à niveau à partir de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)], vous devez exécuter des étapes supplémentaires avant de pouvoir utiliser votre instance mise à niveau de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
    -   Activez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans Windows GCL.  
  
    -   Configurez le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent en utilisant le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Outre les étapes ci-dessus, vous devrez peut-être effectuer les opérations suivantes, si vous avez exécuté une mise à niveau à partir de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]:  
  
-   Les utilisateurs qui ont été configurés dans [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] conservent leur configuration après la mise à niveau. En particulier, le groupe BUILTIN\Users conserve sa configuration. Désactivez, supprimez ou reconfigurez ces comptes en fonction des besoins. Pour plus d’informations, consultez [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
-   Les tailles et le mode de récupération des bases de données système tempdb et model restent inchangés après la mise à niveau. Reconfigurez ces paramètres en fonction des besoins. Pour plus d’informations, consultez [Sauvegarder et restaurer des bases de données système &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md).  
  
-   Les bases de données modèles demeurent sur l'ordinateur après la mise à niveau.  

> [!NOTE]  
> Si la procédure échoue sur la règle Engine_SqlEngineHealthCheck, vous pouvez utiliser l’option d’installation en ligne de commande pour ignorer cette règle spécifique et ainsi permettre au processus de mise à niveau de se terminer correctement. Pour ignorer la vérification de cette règle, ouvrez une invite de commandes, puis entrez le chemin du programme d’installation de SQL Server (Setup.exe). Ensuite, tapez la commande suivante : 

```console
setup.exe /q /ACTION=editionupgrade /InstanceName=MSSQLSERVER /PID=<appropriatePid> /SkipRules=Engine_SqlEngineHealthCheck
```


## <a name="see-also"></a>Voir aussi  
 [Mettre à niveau SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)   
 [Compatibilité descendante_supprimé](/previous-versions/sql/sql-server-2016/cc280407(v=sql.130))  
  
