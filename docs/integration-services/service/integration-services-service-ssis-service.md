---
description: Service Integration Services (Service SSIS)
title: Service Integration Services (Service SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssiseditserverregistration.connectionproperties.f1
- sql13.swb.connecttodts.connectionproperties.f1
- sql13.swb.connection.login.dtsserver.f1
- sql13.swb.connecttodts.login.f1
- sql13.swb.connecttodtsserver.login.f1
helpviewer_keywords:
- Integration Services service, about Integration Services service
- SQL Server Integration Services service
- service [Integration Services],about Integration Services service
- service [Integration Services]
- SQL Server Integration Services, service
ms.assetid: 2c785b3b-4a0c-4df7-b5cd-23756dc87842
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a665d78d180d87fe0d412b025bcce75d509285d4
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171201"
---
# <a name="integration-services-service-ssis-service"></a>Service Integration Services (Service SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Les rubriques de cette section décrivent le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , un service Windows de gestion des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Ce service n'est pas obligatoire pour créer, enregistrer et exécuter des packages Integration Services. [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] prend en charge le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour la compatibilité avec les versions antérieures de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 À compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] stocke des objets, des paramètres et des données opérationnelles dans la base de données **SSISDB** pour les projets que vous avez déployés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] à l’aide du modèle de déploiement de projet. Le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , qui est une instance du moteur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , héberge la base de données. Pour plus d’informations sur la base de données, consultez [Catalogue SSIS](../../integration-services/catalog/ssis-catalog.md). Pour plus d’informations sur le déploiement de projets sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], consultez [Déployer des projets et des packages Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
## <a name="management-capabilities"></a>Fonctionnalités de gestion  
 Le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est un service Windows pour la gestion des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] n'est disponible que dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 L’exécution du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] offre les fonctionnalités de gestion suivantes :  
  
-   Démarrage des packages stockés localement et à distance  
  
-   Arrêt des packages exécutés localement et à distance  
  
-   Surveillance des packages exécutés localement et à distance  
  
-   Importation et exportation de packages  
  
-   Gestion du stockage des packages  
  
-   Personnalisation des dossiers de stockage  
  
-   Arrêt des packages exécutés si le service est arrêté  
  
-   Affichage du journal des événements Windows  
  
-   Connexion à plusieurs serveurs [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]  
  
## <a name="startup-type"></a>Type de démarrage
 Le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est installé quand vous installez le composant [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par défaut, le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est démarré et le type de démarrage du service est défini comme étant automatique. Le service doit être en cours d'exécution pour surveiller les packages stockés dans le magasin de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Le magasin de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] peut aussi bien être la base de données msdb dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que les dossiers désignés dans le système de fichiers.  
  
 Le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] n’est pas nécessaire si vous souhaitez seulement concevoir et exécuter des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Cependant, le service est nécessaire pour répertorier et surveiller les packages à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  

## <a name="manage-the-service"></a>Gérer le service
  
 Lorsque vous installez le composant [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est également installé. Par défaut, le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est démarré et le type de démarrage du service est défini comme étant automatique. Toutefois, vous devez également installer [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour utiliser le service afin de gérer les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] stockés et en cours d'exécution.  
  
> [!NOTE]
> Pour vous connecter directement à une instance du service Integration Services existant, vous devez utiliser la version de SQL Server Management Studio (SSMS) correspondant à la version de SQL Server sur lequel s’exécute le service Integration Services. Par exemple, pour vous connecter au service Integration Services existant s’exécutant sur une instance de SQL Server 2016, vous devez utiliser la version de SSMS publiée pour SQL Server 2016. [Téléchargez SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).
>
>   Dans la boîte de dialogue **Se connecter au serveur** de SSMS, vous ne pouvez pas entrer le nom d’un serveur sur lequel une version antérieure du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] s’exécute. Toutefois, pour gérer des packages stockés sur un serveur distant, vous ne devez pas vous connecter à l’instance du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur ce serveur distant. Au lieu de cela, modifiez le fichier de configuration du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] afin que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] affiche les packages stockés sur le serveur distant.   
  
 Vous ne pouvez installer qu'une seule instance du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur un ordinateur. Le service n'est pas spécifique à une instance particulière du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Vous vous connectez au service en utilisant le nom de l'ordinateur sur lequel il s'exécute.  
  
 Vous pouvez gérer le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] à l’aide de l’un des composants logiciels enfichables MMC (Microsoft Management Console) suivants : Gestionnaire de configuration SQL Server ou Services. Avant de pouvoir gérer des packages dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous devez vérifier que le service a démarré.  
  
 Par défaut, le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est configuré pour gérer les packages dans la base de données msdb de l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] qui est installée au même moment que [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Si une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] n’est pas installée au même moment, le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est configuré pour gérer les packages dans la base de données msdb de l’instance locale par défaut du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Pour gérer des packages stockés dans une instance nommée ou une instance distante du [!INCLUDE[ssDE](../../includes/ssde-md.md)]ou dans plusieurs instances du [!INCLUDE[ssDE](../../includes/ssde-md.md)], vous devez modifier le fichier de configuration du service.
  
 Par défaut, le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est configuré pour arrêter l'exécution des packages à l'arrêt du service. Toutefois, le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] n'attend pas l'arrêt des packages et certains packages peuvent continuer à être exécutés après l'arrêt du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Si le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est arrêté, vous pouvez continuer à exécuter des packages par le biais de l’Assistant Importation et Exportation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , de l’utilitaire d’exécution de package et de l’utilitaire d’invite de commandes **dtexec** (dtexec.exe). Vous ne pouvez cependant pas surveiller les packages en cours d'exécution.  
  
 Par défaut, le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] s'exécute dans le contexte du compte SERVICE RESEAU. Il est recommandé d’exécuter le service SQL Server Integration Services sous un compte disposant d’autorisations limitées, comme le compte SERVICE RÉSEAU. L’exécution du service SQL Server Integration Services sous un compte disposant de privilèges élevés représente un risque potentiel de sécurité. 
  
 Le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] écrit dans le journal d'événements de Windows. Vous pouvez afficher les événements du service dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous pouvez également consulter les événements du service à l'aide de l'Observateur d'événements Windows.  
  
## <a name="set-the-properties-of-the-service"></a>Définir les propriétés du service
  
 Le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] gère et surveille les packages dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Lors de l'installation initiale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est démarré et le type de démarrage du service est un démarrage automatique.  
  
 Après avoir installé le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vous pouvez définir les propriétés du service en utilisant le composant logiciel enfichable Gestionnaire de configuration SQL Server ou Services MMC.  
  
 Pour configurer d'autres fonctionnalités importantes du service, y compris les emplacements auxquels il stocke et gère les packages, vous devez modifier le fichier de configuration du service.
  
### <a name="to-set-properties-of-the-integration-services-service-by-using-sql-server-configuration-manager"></a>Pour définir les propriétés du service Integration Services à l'aide du Gestionnaire de configuration SQL Server  
  
1.  Dans le menu **Démarrer** , pointez sur **Tous les programmes**, sur **Microsoft SQL Server**, sur **Outils de configuration**, puis cliquez sur **Gestionnaire de configuration SQL Server**.  
  
2.  Dans le composant logiciel enfichable **Gestionnaire de configuration SQL Server** , recherchez **SQL Server Integration Services** dans la liste des services, cliquez avec le bouton droit sur **SQL Server Integration Services**, puis cliquez sur **Propriétés**.  
  
3.  Dans la boîte de dialogue **Propriétés de SQL Server Integration Services** , vous pouvez effectuer les actions suivantes :  
  
    -   Cliquez sur l’onglet **Ouvrir une session** pour afficher les informations d’ouverture de session, comme le nom du compte.  
  
    -   Cliquez sur l’onglet **Service** pour afficher les informations relatives au service, comme le nom de l’ordinateur hôte, puis spécifiez le mode de démarrage du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
        > [!NOTE]  
        >  L’onglet **Avancé** ne contient aucune information sur le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
4.  Cliquez sur **OK**.  
  
5.  Dans le menu **Fichier** , cliquez sur **Quitter** pour fermer le composant logiciel enfichable **Gestionnaire de configuration SQL Server** .  
  
### <a name="to-set-properties-of-the-integration-services-service-by-using-services"></a>Pour définir les propriétés du service Integration Services à l’aide de Services  
  
1.  Dans le **Panneau de configuration**, si vous utilisez l’affichage classique, cliquez sur **Outils d’administration**. Si vous utilisez l’affichage des catégories, cliquez sur **Performance et maintenance** puis sur **Outils d’administration**.  
  
2.  Cliquez sur **Services**.  
  
3.  Dans le composant logiciel enfichable **Services** , recherchez **SQL Server Integration Services** dans la liste des services, cliquez avec le bouton droit sur **SQL Server Integration Services**, puis cliquez sur **Propriétés**.  
  
4.  Dans la boîte de dialogue **Propriétés de SQL Server Integration Services** , vous pouvez effectuer les actions suivantes :  
  
    -   Cliquez sur l’onglet **General** (Général). Pour activer le service, sélectionnez le type de démarrage manuel ou automatique. Pour désactiver le service, sélectionnez Désactiver dans la zone **Type de démarrage** . Le fait de sélectionner Désactiver n'arrête pas le service s'il est en cours d'exécution.  
  
         Si le service est déjà activé, vous pouvez cliquer sur **Arrêter** pour arrêter le service ou sur **Démarrer** pour le démarrer.  
  
    -   Cliquez sur l’onglet **Ouvrir une session** pour afficher ou modifier les informations d’ouverture de session.  
  
    -   Cliquez sur l’onglet **Récupération** pour afficher les réponses par défaut de l’ordinateur à l’échec du service. Vous pouvez modifier ces options pour les adapter à votre environnement.  
  
    -   Cliquez sur l’onglet **Dépendances** pour afficher la liste des services dépendants. Le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] n’a pas de dépendances.  
  
5.  Cliquez sur **OK**.  
  
6.  Si le type de démarrage est manuel ou automatique, vous pouvez si vous le souhaitez cliquer avec le bouton droit sur **SQL Server Integration Services** , puis cliquer sur **Démarrer, Arrêter ou Redémarrer**.  
  
7.  Dans le menu **Fichier** , cliquez sur **Quitter** pour fermer le composant logiciel enfichable **Services** .  

## <a name="grant-permissions-to-the-service"></a>Accorder des autorisations au service
  Dans les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], par défaut, lorsque vous installiez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , tous les utilisateurs du groupe Utilisateurs avaient accès au service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Lorsque vous installez la version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les utilisateurs n'ont pas accès au service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Ce service est sécurisé par défaut. Une fois [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installé, l'administrateur doit accorder l'accès au service.  
  
### <a name="to-grant-access-to-the-integration-services-service"></a>Pour accorder l'accès au service Integration Services  
  
1.  Exécutez Dcomcnfg.exe. Dcomcnfg.exe fournit une interface utilisateur qui permet de modifier certains paramètres du Registre.  
  
2.  Dans la boîte de dialogue **Services de composants**, développez le nœud Services de composants > Ordinateurs > Poste de travail > Configuration DCOM.  
  
3.  Cliquez avec le bouton droit sur **Microsoft SQL Server Integration Services 13.0**, puis cliquez sur **Propriétés**.  
  
4.  Sous l'onglet **Sécurité** , cliquez sur **Modifier** dans la zone **Autorisations d'exécution et d'activation** .  
  
5.  Ajoutez des utilisateurs et affectez les autorisations appropriées, puis cliquez sur OK.  
  
6.  Répétez les étapes 4 à 5 pour les autorisations d'accès.  
  
7.  Redémarrez SQL Server Management Studio.  
  
8.  Redémarrez le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  

### <a name="event-logged-when-permissions-are-missing"></a>Événement journalisé en l’absence d’autorisations

Si le compte de service de l’agent SQL Server n’a pas le modèle DCOM (Distributed Component Object Model) Integration Services **[Autorisations de lancement et d’activation]**, l’événement suivant est ajouté aux journaux des événements système quand l’agent SQL Server exécute les travaux du package SSIS :

```
Log Name: System
Source: **Microsoft-Windows-DistributedCOM**
Date: 1/9/2019 5:42:13 PM
Event ID: **10016**
Task Category: None
Level: Error
Keywords: Classic
User: NT SERVICE\SQLSERVERAGENT
Computer: testmachine
Description:
The application-specific permission settings do not grant Local Activation permission for the COM Server application with CLSID
{xxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
and APPID
{xxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
to the user NT SERVICE\SQLSERVERAGENT SID (S-1-5-80-344959196-2060754871-2302487193-2804545603-1466107430) from address LocalHost (Using LRPC) running in the application container Unavailable SID (Unavailable). This security permission can be modified using the Component Services administrative tool.
```

## <a name="configure-the-service"></a>Configurer le service
 
Lorsque vous installez [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], le processus d'installation crée et installe le fichier de configuration pour le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Ce fichier de configuration par défaut contient les paramètres suivants :  
  
-   Les packages reçoivent une commande d'arrêt lorsque le service s'arrête.  
  
-   Les dossiers racine à afficher pour [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] dans l'Explorateur d'objets de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] sont les dossiers MSDB et File System.  
  
-   Dans le système de fichiers géré par le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], les packages se trouvent à l’emplacement %ProgramFiles%\Microsoft SQL Server\130\DTS\Packages.  
  
 Ce fichier de configuration spécifie également quelle base de données msdb contient les packages que le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] gère. Par défaut, le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est configuré pour gérer les packages dans la base de données msdb de l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] qui est installée au même moment que [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Si une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] n’est pas installée au même moment, le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est configuré pour gérer les packages dans la base de données msdb de l’instance locale par défaut du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
### <a name="default-configuration-file-example"></a>Exemple de fichier de configuration par défaut  
 L'exemple suivant présente un fichier de configuration par défaut qui spécifie les paramètres suivants :  
  
-   Arrêt des packages exécutés si le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est arrêté.  
  
-   Les dossiers racine pour le stockage de package dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sont les dossiers MSDB et File System.  
  
-   Le service gère les packages qui sont stockés dans la base de données msdb de l'instance locale par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Le service gère des packages qui sont stockés dans le système de fichiers du dossier Packages.  
  
 **Exemple de fichier de configuration par défaut**  
  
```xml
\<?xml version="1.0" encoding="utf-8"?>  
\<DtsServiceConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <StopExecutingPackagesOnShutdown>true</StopExecutingPackagesOnShutdown>  
  <TopLevelFolders>  
    \<Folder xsi:type="SqlServerFolder">  
      <Name>MSDB</Name>  
      <ServerName>.</ServerName>  
    </Folder>  
    \<Folder xsi:type="FileSystemFolder">  
      <Name>File System</Name>  
      <StorePath>..\Packages</StorePath>  
    </Folder>  
  </TopLevelFolders>    
</DtsServiceConfiguration>  
```  
  
### <a name="modify-the-configuration-file"></a>Modifiez le fichier de configuration  
 Vous pouvez modifier le fichier de configuration de manière à permettre aux packages de poursuivre leur exécution en cas d'arrêt du service, à afficher des dossiers racine supplémentaires dans l'Explorateur d'objets ou à spécifier un dossier différent ou des dossiers supplémentaires dans le système de fichiers que doit gérer le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Par exemple, vous pouvez créer des dossiers racines supplémentaires de type, **SqlServerFolder**, pour gérer des packages dans les bases de données msdb d’instances supplémentaires du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
> [!NOTE]  
>  Certains caractères ne sont pas valides dans les noms de dossiers. Les caractères valides des noms de dossiers sont déterminés par la classe [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]**System.IO.Path** et le champ **GetInvalidFilenameChars** . Le champ **GetInvalidFilenameChars** fournit un tableau de caractères propre à la plateforme, qui ne peuvent pas être spécifiés dans des arguments de chaîne de chemin transmis aux membres de la classe **Path** . Le jeu des caractères non valides peut varier selon le système de fichiers. En général, les caractères non valides sont le guillemet ("), le caractère « inférieur à » (<) et la barre verticale (|).  
  
 Cependant, pour gérer des packages stockés dans une instance nommée ou une instance distante du [!INCLUDE[ssDE](../../includes/ssde-md.md)], vous devez modifier le fichier de configuration. Si vous ne mettez pas à jour le fichier de configuration, vous ne pouvez pas utiliser **l’Explorateur d’objets** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour consulter des packages stockés dans la base de données msdb sur l’instance nommée ou l’instance distante. Si vous essayez d'utiliser l' **Explorateur d'objets** pour consulter ces packages, le message d'erreur suivant apparaît :  
  
 `Failed to retrieve data for this request. (Microsoft.SqlServer.SmoEnum)`  
  
 `The SQL Server specified in Integration Services service configuration is not present or is not available. This might occur when there is no default instance of SQL Server on the computer. For more information, see the topic "Configuring the Integration Services Service" in SQL Server 2008 Books Online.`  
  
 `Login Timeout Expired`  
  
 `An error has occurred while establishing a connection to the server. When connecting to SQL Server 2008, this failure may be caused by the fact that under the default settings SQL Server does not allow remote connections.`  
  
 `Named Pipes Provider: Could not open a connection to SQL Server [2]. (MsDtsSvr).`  
  
 Pour modifier le fichier de configuration pour le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , utilisez un éditeur de texte.  
  
> [!IMPORTANT]  
>  Après avoir modifié le fichier de configuration de service, vous devez redémarrer le service afin d'utiliser la configuration de service mise à jour.  
  
### <a name="modified-configuration-file-example"></a>Exemple de fichier de configuration modifié  
 L'exemple suivant illustre un fichier de configuration modifié pour [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Ce fichier concerne une instance nommée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appelée `InstanceName` , située sur un serveur nommé `ServerName`.  
  
 **Exemple de fichier de configuration modifié pour une instance nommée de SQL Server**  
  
```xml
\<?xml version="1.0" encoding="utf-8"?>  
\<DtsServiceConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <StopExecutingPackagesOnShutdown>true</StopExecutingPackagesOnShutdown>  
  <TopLevelFolders>  
    \<Folder xsi:type="SqlServerFolder">  
      <Name>MSDB</Name>  
      <ServerName>ServerName\InstanceName</ServerName>  
    </Folder>  
    \<Folder xsi:type="FileSystemFolder">  
      <Name>File System</Name>  
      <StorePath>..\Packages</StorePath>  
    </Folder>  
  </TopLevelFolders>    
</DtsServiceConfiguration>  
```  
  
### <a name="modify-the-configuration-file-location"></a>Modifier l’emplacement du fichier de configuration  
 La clé de Registre **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\SSIS\ServiceConfigFile** spécifie l’emplacement et le nom du fichier de configuration utilisé par le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. La valeur par défaut de la clé de Registre est **C:\Program Files\Microsoft SQL Server\130\DTS\Binn\MsDtsSrvr.ini.xml**. Vous pouvez mettre à jour la valeur de la clé de Registre pour utiliser un nom et un emplacement différents pour le fichier de configuration. Notez que le numéro de version présent dans le chemin (120 pour SQL Server [!INCLUDE[ssSQL14_md](../../includes/sssql14-md.md)], 130 pour [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], etc.) varie selon la version de SQL Server.
  
> [!CAUTION]  
>  La modification incorrecte du Registre peut entraîner de graves problèmes et nécessiter la réinstallation du système d'exploitation. [!INCLUDE[msCoName](../../includes/msconame-md.md)] ne garantit pas que les problèmes résultant d'une modification incorrecte du Registre peuvent être résolus. Avant de modifier le Registre, sauvegardez toutes vos données importantes. Pour plus d'informations sur la méthode de sauvegarde, de restauration et de modification du Registre, consultez l'article [!INCLUDE[msCoName](../../includes/msconame-md.md)] Description du Registre de Microsoft Windows [de la Base de connaissances](https://support.microsoft.com/kb/256986).  
  
 Lorsqu'il démarre, le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] charge le fichier de configuration. Toute modification de l'entrée de Registre nécessite le redémarrage du service.  

## <a name="connect-to-the-local-service"></a>Se connecter au service local
  Avant de vous connecter au service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , l'administrateur doit vous accorder l'accès. 
  
### <a name="to-connect-to-the-integration-services-service"></a>Pour se connecter au service Integration Services  
  
1.  Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Cliquez sur **Explorateur d'objets** dans le menu **Affichage** .  
  
3.  Sur la barre d'outils de l'Explorateur d'objets, cliquez sur **Connecter**, puis sur **Integration Services**.  
  
4.  Dans la boîte de dialogue **Se connecter au serveur** , entrez un nom de serveur. Vous pouvez utiliser un point (.), (local) ou **localhost** pour indiquer le serveur local.  
  
5.  Cliquez sur **Connecter**.  

## <a name="connect-to-a-remote-ssis-server"></a>Se connecter à un serveur SSIS distant
  
 Pour se connecter à une instance de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur un serveur distant, que ce soit à partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou d’une autre application de gestion, les utilisateurs de l’application ont besoin d’un ensemble de droits sur le serveur.  
  
> [!IMPORTANT]
> Pour vous connecter directement à une instance du service Integration Services existant, vous devez utiliser la version de SQL Server Management Studio (SSMS) correspondant à la version de SQL Server sur lequel s’exécute le service Integration Services. Par exemple, pour vous connecter au service Integration Services existant s’exécutant sur une instance de SQL Server 2016, vous devez utiliser la version de SSMS publiée pour SQL Server 2016. [Téléchargez SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).
>
>  Pour gérer les packages stockés sur un serveur distant, vous n’avez pas besoin de vous connecter à l’instance du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de ce serveur distant. Au lieu de cela, modifiez le fichier de configuration du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] afin que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] affiche les packages stockés sur le serveur distant.
  
### <a name="connecting-to-integration-services-on-a-remote-server"></a>Connexion à Integration Services sur un serveur distant  
  
#### <a name="to-connect-to-integration-services-on-a-remote-server"></a>Pour se connecter à Integration Services sur un serveur distant  
  
1.  Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Sélectionnez **Fichier** et **Connecter l’Explorateur d’objets** pour afficher la boîte de dialogue **Se connecter au serveur** .  
  
3.  Sélectionnez **Integration Services** dans la liste **Type de serveur** .  
  
4.  Tapez le nom d’un serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] dans la zone de texte **Nom du serveur**.  
  
    > [!NOTE]  
    >  Le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] n'est pas spécifique à l'instance. Vous vous connectez au service en utilisant le nom de l'ordinateur sur lequel le service Integration Services s'exécute.  
  
5.  Cliquez sur **Connecter**.  
  
> [!NOTE]  
>  La boîte de dialogue **Rechercher les serveurs** n’affiche pas les instances distantes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. De plus, les options disponibles sous l’onglet **Options de connexion** de la boîte de dialogue **Se connecter au serveur** (cliquez sur le bouton **Options** pour les afficher) ne s’appliquent pas aux connexions [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
### <a name="eliminating-the-access-is-denied-error"></a>Suppression de l'erreur « Accès refusé »  
 Lorsqu'un utilisateur muni des droits suffisants tente de se connecter à une instance de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur un serveur distant, ce dernier répond par un message d'erreur lui indiquant que l'accès est refusé. Vous pouvez éviter ce message d'erreur en vous assurant que les utilisateurs bénéficient des autorisations DCOM requises.  
  
#### <a name="to-configure-rights-for-remote-users-on-windows-server-2003-or-windows-xp"></a>Pour configurer les droits des utilisateurs distants dans Windows Server 2003 ou Windows XP  
  
1.  Si l'utilisateur n'est pas membre du groupe Administrateurs local, ajoutez l'utilisateur au groupe des utilisateurs Distributed COM. Pour ce faire, vous pouvez utiliser le composant logiciel enfichable MMC Gestion de l’ordinateur accessible à partir du menu **Outils d’administration** .  
  
2.  Ouvrez le Panneau de configuration, double-cliquez sur **Outils d’administration** , puis sur **Services de composants** pour démarrer le composant logiciel enfichable MMC Services de composants.  
  
3.  Développez le nœud **Services de composants** dans le volet gauche de la console. Développez le nœud **Ordinateurs** et **Poste de travail**, puis cliquez sur le nœud **Configuration DCOM** .  
  
4.  Sélectionnez le nœud **Configuration DCOM** , puis sélectionnez SQL Server Integration Services 11.0 dans la liste des applications configurables.  
  
5.  Cliquez avec le bouton droit sur SQL Server Integration Services 11.0, puis sélectionnez **Propriétés**.  
  
6.  Dans la boîte de dialogue **Propriétés de SQL Server Integration Services 11.0** , sélectionnez l’onglet **Sécurité** .  
  
7.  Sous **Autorisations d’exécution et d’activation**, sélectionnez **Personnaliser**, puis cliquez sur **Modifier** pour ouvrir la boîte de dialogue **Autorisation d’exécution** .  
  
8.  Dans la boîte de dialogue **Autorisation d’exécution** , ajoutez ou supprimez des utilisateurs, puis affectez les autorisations appropriées aux utilisateurs et groupes concernés. Les autorisations disponibles sont Exécution locale, Exécution à distance, Activation locale et Activation à distance. Les droits d'exécution octroient ou refusent l'autorisation de démarrer et d'arrêter le service ; les droits d'activation octroient ou refusent l'autorisation de se connecter au service.  
  
9. Cliquez sur OK pour fermer la boîte de dialogue.  
  
10. Sous **Autorisations d’accès**, répétez les étapes 7 et 8 pour assigner les autorisations appropriées aux utilisateurs et groupes concernés.  
  
11. Fermez le composant logiciel enfichable MMC.  
  
12. Redémarrez le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
#### <a name="to-configure-rights-for-remote-users-on-windows-2000-with-the-latest-service-packs"></a>Pour configurer les droits des utilisateurs distants dans Windows 2000 avec les derniers Service Packs  
  
1.  Exécutez **dcomcnfg.exe** à l’invite de commandes.  
  
2.  Dans la page **Applications** de la boîte de dialogue des **propriétés de configuration de Distributed COM** , sélectionnez SQL Server Integration Services 11.0, puis cliquez sur **Propriétés**.  
  
3.  Sélectionnez la page **Security** .  
  
4.  Utilisez les deux boîtes de dialogue distinctes pour configurer les **autorisations d’accès** et les **autorisations d’exécution**. Aucune distinction entre accès local et accès à distance n'est possible : les autorisations d'accès englobent l'accès local et l'accès à distance, et les autorisations d'exécution incluent l'exécution locale et l'exécution à distance.  
  
5.  Fermez les boîtes de dialogue et **dcomcnfg.exe**.  
  
6.  Redémarrez le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
### <a name="connecting-by-using-a-local-account"></a>Connexion à l'aide d'un compte local

Si vous travaillez sous un compte Windows local sur un ordinateur client, vous pouvez vous connecter au service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur un ordinateur distant uniquement si un compte local porte le même nom et mot de passe et si les droits appropriés existent sur l'ordinateur distant.  
  
### <a name="ssis-windows-service-doesnt-support-delegation"></a>Le service Windows SSIS ne prend pas en charge la délégation

SSIS ne prend pas en charge la délégation d’informations d'identification, parfois appelée « double saut ». Dans ce scénario, vous opérez sur un ordinateur client, SSIS est installé sur un deuxième ordinateur et SQL Server sur un troisième ordinateur. Bien que SSMS transmette correctement vos informations d’identification de l’ordinateur client au deuxième ordinateur (où SSIS est en cours d’exécution), SSIS ne peut pas déléguer vos informations d’identification du deuxième ordinateur au troisième ordinateur (où SQL Server est en cours d’exécution).

## <a name="configure-the-firewall"></a>Configurer le pare-feu
  
 Le système de Pare-feu Windows permet d’empêcher l’accès non autorisé à des ressources informatiques via une connexion réseau. Pour accéder à [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] à travers ce pare-feu, vous devez configurer le pare-feu de façon à autoriser l’accès.  
  
> [!IMPORTANT]  
>  Pour gérer les packages stockés sur un serveur distant, vous n’avez pas besoin de vous connecter à l’instance du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de ce serveur distant. Au lieu de cela, modifiez le fichier de configuration du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] afin que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] affiche les packages stockés sur le serveur distant.
  
 Le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilise le protocole DCOM.
  
 De nombreux systèmes de pare-feu sont disponibles. Si vous exécutez un autre pare-feu que le Pare-feu Windows, consultez la documentation de votre pare-feu pour obtenir des informations spécifiques au système utilisé.  
  
 Si le pare-feu prend en charge le filtrage au niveau application, vous pouvez utiliser l'interface utilisateur fournie par Windows pour spécifier les exceptions qui sont autorisées à traverser le pare-feu, telles que certains programmes ou services. Autrement, vous devez configurer DCOM de façon à utiliser un jeu de ports TCP limité. Le lien vers le site Web de Microsoft fourni ci-dessus contient des informations sur la façon de spécifier les ports TCP à utiliser.  
  
 Le service Integration Services utilise le port 135 ; ce port ne peut pas être modifié. Vous devez ouvrir le port TCP 135 pour accéder au gestionnaire de contrôle de services. Celui-ci effectue des tâches telles que le démarrage et l’arrêt des services [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , et la transmission de demandes de contrôle au service en cours d’exécution.  
  
 Les informations de la section suivante sont spécifiques au Pare-feu Windows. Vous pouvez configurer le système de Pare-feu Windows en exécutant une commande à l’invite de commandes, ou en définissant des propriétés dans la boîte de dialogue Pare-feu Windows.  
  
 Pour plus d’informations sur les paramètres par défaut du Pare-feu Windows et pour obtenir une description des ports TCP qui affectent le moteur de base de données, Analysis Services, Reporting Services et Integration Services, consultez [Configurer le Pare-feu Windows pour autoriser l’accès à SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
### <a name="configuring-a-windows-firewall"></a>Configuration d’un Pare-feu Windows  
 Vous pouvez utiliser les commandes suivantes pour ouvrir le port TCP 135, ajouter MsDtsSrvr.exe à la liste d'exceptions et spécifier la portée du déblocage pour le pare-feu.  
  
#### <a name="to-configure-a-windows-firewall-using-the-command-prompt-window"></a>Pour configurer un Pare-feu Windows à l'aide de la fenêtre d'invite de commandes  
  
1.  Exécutez la commande suivante :

    ```dos
    netsh firewall add portopening protocol=TCP port=135 name="RPC (TCP/135)" mode=ENABLE scope=SUBNET
    ```
  
2.  Exécutez la commande suivante :

    ```dos
    netsh firewall add allowedprogram program="%ProgramFiles%\Microsoft SQL Server\100\DTS\Binn\MsDtsSrvr.exe" name="SSIS Service" scope=SUBNET
    ```
  
    > [!NOTE]  
    >  Pour ouvrir le pare-feu pour tous les ordinateurs et également pour ceux sur Internet, remplacez scope=SUBNET par scope=ALL.  
  
 La procédure suivante décrit la façon d'utiliser l'interface utilisateur Windows pour ouvrir le port TCP 135, ajouter MsDtsSrvr.exe à la liste d'exceptions et spécifier la portée du déblocage pour le pare-feu.  
  
#### <a name="to-configure-a-firewall-using-the-windows-firewall-dialog-box"></a>Pour configurer un pare-feu à l’aide de la boîte de dialogue Pare-feu Windows  
  
1.  Dans le Panneau de configuration, double-cliquez sur **Pare-feu Windows**.  
  
2.  Dans la boîte de dialogue **Pare-feu Windows** , cliquez sur l’onglet **Exceptions** , puis sur **Ajouter un programme**.  
  
3.  Dans la boîte de dialogue **Ajouter un programme** , cliquez sur **Parcourir**, naviguez jusqu’au dossier Program Files\Microsoft SQL Server\100\DTS\Binn, cliquez sur MsDtsSrvr.exe, puis sur **Ouvrir**. Cliquez sur **OK** pour fermer la boîte de dialogue **Ajouter un programme** .  
  
4.  Sous l’onglet **Exceptions** , cliquez sur **Ajouter un port**.  
  
5.  Dans la boîte de dialogue **Ajouter un port** , tapez **RPC(TCP/135)** ou un autre nom descriptif dans la zone **Nom**, tapez **135** dans la zone **Numéro de port** , puis sélectionnez **TCP**.  
  
    > [!IMPORTANT]  
    >  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Le service utilise toujours le port 135. Vous ne pouvez pas spécifier un autre port.  
  
6.  Dans la boîte de dialogue **Ajouter un port** , vous pouvez éventuellement cliquer sur **Modifier l’étendue** pour modifier l’étendue par défaut.  
  
7.  Dans la boîte de dialogue **Modifier l’étendue** , sélectionnez **Uniquement mon réseau (ou sous-réseau)** ou tapez une liste personnalisée, puis cliquez sur **OK**.  
  
8.  Pour fermer la boîte de dialogue **Ajouter un port** , cliquez sur **OK**.  
  
9. Pour fermer la boîte de dialogue **Pare-feu Windows** , cliquez sur **OK**.  
  
    > [!NOTE]  
    >  Pour configurer le Pare-feu Windows, cette procédure utilise l’élément **Pare-feu Windows** du Panneau de configuration. L’élément **Pare-feu Windows** configure uniquement le pare-feu du profil d’emplacement réseau actuel. Toutefois, vous pouvez également configurer le Pare-feu Windows à l’aide de l’outil en ligne de commande **netsh** ou du composant logiciel enfichable MMC ([!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console) appelé Pare-feu Windows avec fonctions avancées de sécurité. Pour plus d’informations sur ces outils, consultez [Configurer le Pare-feu Windows pour autoriser l’accès à SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
