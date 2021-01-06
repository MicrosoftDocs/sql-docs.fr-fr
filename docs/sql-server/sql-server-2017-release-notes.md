---
title: Notes de publication de SQL Server 2017 | Microsoft Docs
description: Cet article décrit les limitations et les problèmes liés à SQL Server 2017 et propose des liens vers des informations connexes.
ms.custom: ''
ms.date: 11/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
ms.assetid: 13942af8-5a40-4cef-80f5-918386767a47
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = sql-server-2017
ms.openlocfilehash: 0c70b2a7202ea7bcd14128a544487ec011c9c7b8
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643315"
---
# <a name="sql-server-2017-release-notes"></a>Notes de publication de SQL Server 2017
[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]
Cet article décrit les limitations et les problèmes de SQL Server 2017. Pour plus d’informations, consultez :
- [Nouveautés de SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md)
- [Notes de publication de SQL Server sur Linux](../linux/sql-server-linux-release-notes.md)
- [Mises à jour cumulatives de SQL Server 2017](https://aka.ms/sql2017cu) pour plus d’informations sur la dernière version des mises à jour cumulatives

**Essayer SQL Server**
- [![Télécharger à partir du Centre d’évaluation](../includes/media/download2.png)](https://go.microsoft.com/fwlink/?LinkID=829477) [Télécharger SQL Server 2017](https://go.microsoft.com/fwlink/?LinkID=829477)
- [![Créer une machine virtuelle ](../includes/media/azure-vm.png)](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm) [Lancer une machine virtuelle avec SQL Server 2017](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm)

> [!NOTE]
> La préversion de SQL Server 2019 est maintenant disponible. Pour plus d’informations, consultez [Nouveautés de SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15&preserve-view=true).

## <a name="sql-server-2017---general-availability-release-october-2017"></a>SQL Server 2017 - version de disponibilité générale (octobre 2017)
### <a name="database-engine"></a>Moteur de base de données

- **Problème et impact sur le client :** après la mise à niveau, le partage réseau FILESTREAM peut ne plus être disponible.

- **Solution de contournement :** redémarrez d’abord l’ordinateur, puis vérifiez si le partage réseau FILESTREAM est disponible. Si le partage n’est toujours pas disponible, effectuez les étapes suivantes :

    1. Dans le Gestionnaire de configuration SQL Server, cliquez avec le bouton droit sur l’instance SQL Server, puis cliquez sur **Propriétés**. 
    2. Sous l’onglet **FILESTREAM**, décochez **Activer FILESTREAM pour l’accès aux E/S de fichier**, puis cliquez sur **Appliquer**.
    3. Recochez **Activer FILESTREAM pour l’accès aux E/S de fichier** avec le nom du partage d’origine, puis cliquez sur **Appliquer**.

### <a name="master-data-services-mds"></a>Master Data Services (MDS)
- **Problème et impact sur le client :**  dans la page des autorisations des utilisateurs, quand vous accordez une autorisation au niveau racine dans l’arborescence des entités, vous voyez l’erreur suivante : `"The model permission cannot be saved. The object guid is not valid"`

- **Solution de contournement :** 
  - Accordez l’autorisation sur les sous-nœuds de l’arborescence au lieu de le faire au niveau racine.

### <a name="analysis-services"></a>Analysis Services
- **Problème et impact sur le client :** les connecteurs de données pour les sources suivantes ne sont pas encore disponibles pour les modèles tabulaires au niveau de compatibilité 1400.
  - Amazon Redshift
  - IBM Netezza
  - Impala
- **Solution de contournement :** Aucun.   

- **Problème et impact sur le client :** les modèles Direct Query au niveau de compatibilité 1400 avec des perspectives peuvent échouer lors de l’interrogation ou la découverte des métadonnées.
- **Solution de contournement :** supprimez les perspectives et redéployez.

### <a name="tools"></a>Outils
- **Problème et impact sur le client :** En cours d’exécution *DReplay* échoue avec le message suivant : « Une erreur DReplay inattendue s'est produite ! ».
- **Solution de contournement :** Aucun.

![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-release-candidate-rc2---august-2017"></a>SQL Server 2017 Release Candidate (RC2 - Août 2017)
Il n’y a aucune note de publication sur cette version de SQL Server sur Windows. Consultez [Notes de publication de SQL Server sur Linux](../linux/sql-server-linux-release-notes.md).


![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-release-candidate-rc1---july-2017"></a>SQL Server 2017 Release Candidate (RC1, juillet 2017)
### <a name="sql-server-integration-services-ssis-rc1---july-2017"></a>SQL Server Integration Services (SSIS) (RC1, juillet 2017)
- **Problème et impact sur le client :** Le paramètre *runincluster* de la procédure stockée **[catalog].[create_execution]** est renommé en *runinscaleout* pour des raisons de cohérence et de lisibilité.
- **Solution de contournement :** Si vous avez des scripts existants pour exécuter des packages dans Scale-out, vous devez remplacer le nom du paramètre *runincluster* par *runinscaleout* pour que les scripts fonctionnent dans la version RC1.

- **Problème et impact sur le client :** SQL Server Management Studio (SSMS) 17.1 et versions antérieures ne peuvent pas déclencher l’exécution du package dans Scale Out dans la version RC1. Le message d’erreur est : « *\@runincluster* n’est pas un paramètre valide pour la procédure **create_execution** ». Ce problème est résolu dans la prochaine version de SSMS, la version 17.2. Les versions 17.2 et ultérieures de SSMS prennent en charge le nouveau nom de paramètre et l’exécution de package dans Scale-out. 
- **Solution de contournement :** jusqu’à ce que SSMS version 17.2 soit disponible :
  1. Utilisez votre version existante de SSMS pour générer le script d’exécution du package.
  2. Modifiez le nom du paramètre *runincluster* sur *runinscaleout* dans le script.
  3. Exécutez le script.

![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-ctp-21-may--2017"></a>SQL Server 2017 CTP 2.1 (mai 2017)
### <a name="documentation-ctp-21"></a>Documentation (CTP 2.1)
- **Problème et impact sur le client :** la documentation pour [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] est limitée et le contenu est inclus avec l’ensemble de documentation [!INCLUDE[ssSQL15_md](../includes/sssql15-md.md)].  Dans les articles, le contenu propre à [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] est noté avec **S’applique à**. 
- **Problème et impact sur le client :** aucun contenu hors connexion n’est disponible pour [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)].

### <a name="sql-server-reporting-services-ctp-21"></a>SQL Server Reporting Services (CTP 2.1)

- **Problème et impact sur le client :** si vous disposez de SQL Server Reporting Services et de Power BI Report Server sur la même machine et que vous désinstallez un des deux, vous ne pouvez pas vous connecter au serveur de rapports restant avec le Gestionnaire de configuration du serveur de rapports.
- **Solution de contournement** : pour contourner ce problème, vous devez effectuer les opérations suivantes après la désinstallation d’un des serveurs.

    1. Lancez une invite de commandes en mode administrateur.
    2. Accédez au répertoire où est installé le serveur de rapports restant.

        *Emplacement par défaut pour Power BI Report Server : C:\Program Files\Microsoft Power BI Report Server*

        *Emplacement par défaut pour SQL Server Reporting Services : C:\Program Files\Microsoft SQL Server Reporting Services*

    3. Puis accédez au dossier suivant, qui est soit *SSRS* ou *PBIRS* selon ce qui reste.
    4. Accédez au dossier WMI.
    5. Exécutez la commande suivante :

        ```console
        regsvr32 /i ReportingServicesWMIProvider.dll
        ```

        Si vous voyez l’erreur suivante, ignorez-la.

        ```
        The module "ReportingServicesWMIProvider.dll" was loaded but the entry-point DLLInstall was not found. Make sure that "ReportingServicesWMIProvider.dll" is a valid DLL or OCX file and then try again.
        ```

### <a name="tsqllanguageservicemsi-ctp-21"></a>TSqlLanguageService.msi (CTP 2.1)

- **Problème et impact sur le client :** après l’installation sur un ordinateur où la version 2016 de *TSqlLanguageService.msi* est installée (via le programme d’installation de SQL ou en tant que composant redistribuable autonome), les versions v13.* (SQL 2016) de *Microsoft.SqlServer.Management.SqlParser.dll* et *Microsoft.SqlServer.Management.SystemMetadataProvider.dll* sont supprimées. Toute application qui dépend des versions 2016 de ces assemblys arrête de fonctionner et génèrer une erreur similaire à : *erreur : Impossible de charger le fichier ou l’assembly « Microsoft.SqlServer.Management.SqlParser, Version=13.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91 » ou l’une de ses dépendances. Le système ne peut pas trouver le fichier spécifié.*

   En outre, les tentatives pour réinstaller une version 2016 de TSqlLanguageService.msi échouent avec le message : *L’installation du service de langage T-SQL de Microsoft SQL Server 2016 a échoué parce qu’une version supérieure existe déjà sur l’ordinateur*.

- **Solution de contournement** : pour contourner ce problème et corriger une application qui dépend de la version v13 des assemblys, procédez comme suit :

   1. Accédez à **Ajout/Suppression de programmes**.
   2. Recherchez *Service de langage T-SQL Microsoft SQL Server 2019 CTP2.1*, cliquez avec le bouton droit et sélectionnez **Désinstaller**.
   3. Une fois le composant supprimé, réparez l’application endommagée ou réinstallez la version appropriée de *TSqlLanguageService.MSI*.

   Cette solution de contournement supprime la version v14 de ces assemblys : toutes les applications qui dépendent des versions v14 ne fonctionneront donc plus. Si ces assemblys sont nécessaires, une installation distincte sans aucune installation 2016 côte à côte est obligatoire.

![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-ctp-20-april--2017"></a>SQL Server 2017 CTP 2.0 (April 2017)
### <a name="documentation-ctp-20"></a>Documentation (CTP 2.0)
- **Problème et impact sur le client :** la documentation pour [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] est limitée et le contenu est inclus avec l’ensemble de documentation [!INCLUDE[ssSQL15_md](../includes/sssql15-md.md)].  Dans les articles, le contenu propre à [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] est noté avec **S’applique à**. 
- **Problème et impact sur le client :** aucun contenu hors connexion n’est disponible pour [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)].

### <a name="always-on-availability-groups"></a>Groupes de disponibilité Always On

- **Problème et impact sur le client :** une instance de SQL Server hébergeant un réplica secondaire du groupe de disponibilité se bloque si la version majeure de SQL Server est inférieure à celle de l’instance qui héberge le réplica principal. Affecte les mises à niveau à partir de toutes les versions prises en charge de SQL Server hébergeant des groupes de disponibilité pour SQL Server [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] CTP 2.0. Le problème se produit dans les conditions suivantes. 

> 1. L’utilisateur met à niveau une instance de SQL Server hébergeant un réplica secondaire conformément aux [bonnes pratiques](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md).
> 2. Après la mise à niveau, un basculement se produit et le réplica secondaire qui vient d’être mis à niveau devient le réplica principal avant la fin de la mise à niveau pour tous les réplicas secondaires du groupe de disponibilité. L’ancien réplica principal est maintenant un réplica secondaire d’une version inférieure au réplica principal.
> 3. Le groupe de disponibilité est dans une configuration non prise en charge et tous les réplicas secondaires deviennent vulnérables en cas de blocage. 

- **Solution de contournement** Connectez-vous à l’instance de SQL Server qui héberge le nouveau réplica principal et supprimez de la configuration le réplica secondaire défectueux.

   ```sql
   ALTER AVAILABILITY GROUP agName REMOVE REPLICA ON NODE instanceName;
   ```

   L’instance de SQL Server qui hébergeait le réplica secondaire revient à un état normal.

## <a name="more-information"></a>Informations complémentaires
- [Notes de publication de SQL Server Reporting Services](../reporting-services/release-notes-reporting-services.md).
- [Problèmes connus dans Machine Learning Services](../machine-learning/troubleshooting/known-issues-for-sql-server-machine-learning-services.md)
- [Centre de mise à jour SQL Server - liens et informations pour toutes les versions prises en charge](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![MS_Logo_X-Small](../sql-server/media/ms-logo-x-small.png)