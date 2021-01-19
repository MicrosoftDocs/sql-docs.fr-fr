---
title: Installer SQL Server sur Server Core | Microsoft Docs
description: Vous pouvez installer SQL Server sur une installation Server Core. L’option d’installation Server Core offre l’environnement minimal requis pour l’exécution de certains rôles de serveurs spécifiques.
ms.custom: ''
ms.date: 06/29/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 1dd294cc-5b69-4d0c-9005-3e307b75678b
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: ab5a2352358fdb678241425e0ca5e8f999d8a2f2
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170171"
---
# <a name="install-sql-server-on-server-core"></a>Installer SQL Server sur Server Core

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Vous pouvez installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur une installation Server Core.   
  
L’option d’installation Server Core offre l’environnement minimal requis pour l’exécution de certains rôles de serveurs spécifiques. Cela permet de réduire les besoins en maintenance et gestion et l'exposition aux attaques de ces rôles de serveur. Pour plus d’informations sur Server Core, consultez [Installer Server Core](/windows-server/get-started/getting-started-with-server-core). Pour plus d’informations sur Server Core implémenté sur [!INCLUDE[win8srv](../../includes/win8srv-md.md)], consultez [Server Core for Windows Server 2012](https://msdn.microsoft.com/library/hh846323\(VS.85\).aspx) (https://msdn.microsoft.com/library/hh846323(VS.85).aspx).  
  
 Pour obtenir la liste des systèmes d’exploitation pris en charge, consultez [Configurations matérielle et logicielle requises pour l’installation de SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).

## <a name="prerequisites"></a>Prérequis  
  
|Condition requise|Procédure d'installation|  
|-----------------|--------------------|  
|[!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 4.6.1 |Pour toutes les éditions de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] sauf [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)], le programme d’installation requiert [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 4.6.1 Server Core Profile. Le programme d’installation de SQL Server l’installe automatiquement s’il ne l’est pas déjà. L’installation requiert un redémarrage. Vous pouvez installer [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] avant d’exécuter le programme d’installation pour éviter un redémarrage.|  
|Windows Installer 4.5|Inclus dans l’installation Server Core.|  
|Windows PowerShell|Inclus dans l’installation Server Core.|  
|Java Runtime |Pour utiliser PolyBase, vous devez installer le Java Runtime approprié. Pour plus d’informations, consultez [Installation de PolyBase](../../relational-databases/polybase/polybase-installation.md).|
  
##  <a name="supported-features"></a><a name="BK_SupportedFeatures"></a> Fonctionnalités prises en charge  
 Utilisez le tableau suivant pour rechercher les fonctionnalités prises en charge dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] sur une installation Server Core.  
  
|Fonctionnalité|Prise en charge|Informations supplémentaires|  
|-------------|---------------|----------------------------|  
|du[!INCLUDE[ssDE](../../includes/ssde-md.md)]|Oui||  
|Réplication[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Oui||  
|Recherche en texte intégral|Oui||  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|Oui||  
|[!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)]|Oui||  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|Non||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Data Tools (SSDT)|Non||  
|Connectivité des outils clients|Oui||  
|Integration Services, serveur|Oui||  
|Compatibilité descendante des outils clients|Non||  
|Kit de développement logiciel (SDK) des outils clients|Non||  
|Documentation en ligne[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Non||  
|Outils de gestion - Base|À distance uniquement|L’installation de ces fonctionnalités sur Server Core n’est pas prise en charge. Ces composants peuvent être installés sur un serveur autre que Server Core et être connectés aux services de [!INCLUDE[ssDE](../../includes/ssde-md.md)] installés sur Server Core.|  
|Outils de gestion - Complet|À distance uniquement|L’installation de ces fonctionnalités sur Server Core n’est pas prise en charge. Ces composants peuvent être installés sur un serveur autre que Server Core et être connectés aux services de [!INCLUDE[ssDE](../../includes/ssde-md.md)] installés sur Server Core.|  
|Distributed Replay Controller|Non||  
|Distributed Replay Client|À distance uniquement|L’installation de ces fonctionnalités sur Server Core n’est pas prise en charge. Ces composants peuvent être installés sur un serveur autre que Server Core et être connectés aux services de [!INCLUDE[ssDE](../../includes/ssde-md.md)] installés sur Server Core.|  
|Kit de développement logiciel (SDK) de l'option Connectivité client de SQL|Oui||  
|Microsoft Sync Framework|Oui|Microsoft Sync Framework n’est pas inclus dans le package d’installation de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] . Vous pouvez télécharger la version appropriée de Sync Framework à partir du [Centre de téléchargement Microsoft](https://go.microsoft.com/fwlink/?LinkId=221788) (page https://go.microsoft.com/fwlink/?LinkId=221788) ) et l’installer sur un ordinateur exécutant Server Core.|  
|[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]|Non||  
|[!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)]|Non||  
  
## <a name="supported-scenarios"></a>Scénarios pris en charge  
 Le tableau suivant indique la matrice de scénario prise en charge pour l’installation de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] sur Server Core.  
  
| Installation | Cible valide |  
|-|-|  
|Éditions de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Tous les éditions 64 bits de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] |  
|Langue de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Toutes les langues|  
|Langage[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le langage du système d'exploitation/paramètres régionaux (combinaison)|ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Windows JPN (japonais)<br /><br /> ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Windows GER (allemand)<br /><br /> ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Windows CHS (chinois-Chine)<br /><br /> ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Windows ARA (Arabe (Arabie-Saoudite))<br /><br /> ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Windows THA (thaïlandais)<br /><br /> ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Windows TRK (turque)<br /><br /> ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Windows pt-PT (portugais Portugal)<br /><br /> ENG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Windows ENG (anglais)|  
|Édition Windows|Windows Server 2019 Datacenter <br/><br/> Windows Server 2019 Standard <br /><br />  [!INCLUDE[winserver2016_datacenter_md](../../includes/winserver2016-datacenter-md.md)]<br/><br/>[!INCLUDE[winserver2016_standard_md](../../includes/winserver2016-standard-md.md)]<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2 Foundation<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Datacenter<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Standard<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Essentials<br/><br/>[!INCLUDE[win8srv](../../includes/win8srv-md.md)] Foundation|  
  
## <a name="upgrade"></a>Mettre à niveau 
 Sur les installations Server Core, la mise à niveau depuis [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] vers [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] n'est pas prise en charge.  
  
## <a name="install"></a>Installer  
 [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] ne prend pas en charge l'installation avec l'Assistant d'installation sur le système d'exploitation de Server Core. Lors de l'installation sous Server Core, le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge le mode silencieux complet via le paramètre /Q ou le mode silencieux simple via le paramètre /QS. Pour plus d’informations, consultez [Installer SQL Server à partir de l’invite de commandes](./install-sql-server-from-the-command-prompt.md).  
  
 Indépendamment de la méthode d'installation, vous êtes invité à confirmer l'acceptation des termes de la licence de logiciel en tant que personne physique ou pour le compte d'une entité, sauf si votre utilisation du logiciel est régie par un accord distinct, tel qu'un accord de concession de licence en volume de [!INCLUDE[msCoName](../../includes/msconame-md.md)] ou un accord tiers avec un éditeur de logiciels ou un fabricant OEM.  
  
 Les termes du contrat de licence sont affichés afin que vous puissiez les consulter et les accepter dans l'interface utilisateur du programme d'installation. Les installations sans assistance (à l'aide du paramètre /Q ou /QS) doivent inclure le paramètre /IACCEPTSQLSERVERLICENSETERMS. Vous pouvez consulter les termes du contrat de licence séparément sur la page [Termes du contrat de licence logiciel Microsoft](https://go.microsoft.com/fwlink/?LinkId=148209).  
  
> [!NOTE]  
>  Selon la façon dont vous avez reçu le logiciel (par exemple, via le programme de licence en volume [!INCLUDE[msCoName](../../includes/msconame-md.md)] ), votre utilisation du logiciel peut être soumise à des termes et conditions supplémentaires.  
  
 Pour installer des fonctionnalités spécifiques, utilisez le paramètre /FEATURES et spécifiez la fonctionnalité parent ou les valeurs de fonctionnalités. Pour plus d'informations sur les paramètres de fonctionnalités et leur utilisation, consultez les sections suivantes.  
  
### <a name="feature-parameters"></a>Paramètres de fonctionnalités  
  
|Paramètre de fonctionnalité|Description|  
|-----------------------|-----------------|  
|SQLENGINE|Installe uniquement [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|RÉPLICATION|Installe le composant Replication avec le [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|FULLTEXT|Installe le composant FullText avec [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|AS|Installe tous les composants [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|  
|IS|Installe tous les composants [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
|CONN|Installe les composants de connectivité.| 
|ADVANCEDANALYTICS |Installe R Services, nécessite le moteur de base de données. Les installations sans assistance nécessitent le paramètre /IACCEPTROPENLICENSETERMS.  |


 Consultez les exemples suivants de l'utilisation de paramètres de fonctionnalités :  
  
|Paramètre et valeurs|Description|  
|--------------------------|-----------------|  
|/FEATURES=SQLEngine|Installe uniquement [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|/FEATURES=SQLEngine,FullText|Installe le [!INCLUDE[ssDE](../../includes/ssde-md.md)] et la recherche en texte intégral.|  
|/FEATURES=SQLEngine,Conn|Installe les [!INCLUDE[ssDE](../../includes/ssde-md.md)] et composants de connectivité.|  
|/FEATURES=SQLEngine,AS,IS,Conn|Installe les [!INCLUDE[ssDE](../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]et composants de connectivité.|  
|/FEATURES=SQLENGINE,ADVANCEDANALYTICS /IACCEPTROPENLICENSETERMS |Installe le  [!INCLUDE[ssDE](../../includes/ssde-md.md)] et [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)].|  

  
### <a name="installation-options"></a>Options d'installation  
 L'installation prend en charge les options d'installation suivantes lors de l'installation de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] sur un système d'exploitation Server Core :  
  
1.  **Installation à partir de la ligne de commande**  
  
     Pour installer des fonctionnalités spécifiques à l'aide de l'option d'installation de l'invite de commande, utilisez le paramètre /FEATURES et spécifiez la fonctionnalité parent ou les valeurs de fonctionnalités répertoriées dans le tableau suivant. Voici un exemple d'utilisation des paramètres de la ligne de commande :  
  
    ```console
    Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,Replication /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="<StrongPassword>" /SQLSYSADMINACCOUNTS="<DomainName\UserName>" /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /TCPENABLED=1 /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
2.  **Installation à l’aide du fichier de configuration**  
  
     Le programme d'installation prend en charge l'utilisation du fichier de configuration uniquement via l'invite de commandes. Le fichier de configuration est un fichier texte avec une structure de base d'un paramètre (paire nom/valeur) et d'un commentaire descriptif. Le fichier de configuration spécifié à l'invite de commande doit avoir une extension de nom de fichier .INI. Consultez les exemples suivants de ConfigurationFile.INI :  
  
    - Installation du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. 
    
    L’exemple suivant montre comment installer une nouvelle instance autonome qui inclut [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssDE](../../includes/ssde-md.md)] :  

    ```console
    ; SQL Server Configuration File  
    [OPTIONS]  
    
    ; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE. This is a required parameter.   
    
    ACTION="Install"  
    
    ; Specifies features to install, uninstall, or upgrade. The lists of features include SQLEngine, FullText, Replication, AS, IS, and Conn.   
    
    FEATURES=SQLENGINE  
    
    ; Specify a default or named instance. MSSQLSERVER is the default instance for non-Express editions and SQLExpress for Express editions. This parameter is required when installing the ssNoVersion Database Engine, and Analysis Services (AS).  
    
    INSTANCENAME="MSSQLSERVER"  
    
    ; Specify the Instance ID for the ssNoVersion features you have specified. ssNoVersion directory structure, registry structure, and service names will incorporate the instance ID of the ssNoVersion instance.   
    
    INSTANCEID="MSSQLSERVER"  
    
    ; Account for ssNoVersion service: Domain\User or system account.   
    
    SQLSVCACCOUNT="NT Service\MSSQLSERVER"  
    
    ; Windows account(s) to provision as ssNoVersion system administrators.   
    
    SQLSYSADMINACCOUNTS="\<DomainName\UserName>"  
    
    ; Accept the License agreement to continue with Installation  
    
    IAcceptSQLServerLicenseTerms="True"  
    
    ```

    -   Installation des composants de connectivité. L'exemple suivant montre comment installer les composants de connectivité :  
  
        ```console
        ; SQL Server Configuration File  
        [OPTIONS]  
  
        ; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE. This is a required parameter.   
  
        ACTION="Install"  
  
        ; Specifies features to install, uninstall, or upgrade. The lists of features include SQLEngine, FullText, Replication, AS, IS, and Conn.   
  
        FEATURES=Conn  
  
        ; Specifies acceptance of License Terms  
  
        IAcceptSQLServerLicenseTerms="True  
  
        ```  
  
    -   Installation de toutes les fonctionnalités prises en charge  
  
        L'exemple suivant montre comment installer toutes les fonctionnalités prises en charge de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] sur Server Core :  
  
        ```console
        ; SQL Server Configuration File  
        [OPTIONS]  
        ; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE. This is a required parameter.   
  
        ACTION="Install"  
  
        ; Specifies features to install, uninstall, or upgrade. The lists of features include SQLEngine, FullText, Replication, AS, IS, and Conn.   
  
        FEATURES=SQLENGINE,FullText,Replication,AS,IS,Conn  
  
        ; Specify a default or named instance. MSSQLSERVER is the default instance for non-Express editions and SQLExpress for Express editions. This parameter is required when installing the ssNoVersion Database Engine (SQL), or Analysis Services (AS).  
  
        INSTANCENAME="MSSQLSERVER"  
  
        ; Specify the Instance ID for the ssNoVersion features you have specified. ssNoVersion directory structure, registry structure, and service names will incorporate the instance ID of the ssNoVersion instance.   
  
        INSTANCEID="MSSQLSERVER"  
  
        ; Account for ssNoVersion service: Domain\User or system account.   
  
        SQLSVCACCOUNT="NT Service\MSSQLSERVER"  
  
        ; Windows account(s) to provision as ssNoVersion system administrators.   
  
        SQLSYSADMINACCOUNTS="\<DomainName\UserName>"  
  
        ; The name of the account that the Analysis Services service runs under.   
  
        ASSVCACCOUNT= "NT Service\MSSQLServerOLAPService"  
  
        ; Specifies the list of administrator accounts that need to be provisioned.   
  
        ASSYSADMINACCOUNTS="\<DomainName\UserName>"  
  
        ; Specifies the server mode of the Analysis Services instance. Valid values are MULTIDIMENSIONAL, POWERPIVOT or TABULAR. ASSERVERMODE is case-sensitive. All values must be expressed in upper case.   
  
        ASSERVERMODE="MULTIDIMENSIONAL"  
  
        ; Optional value, which specifies the state of the TCP protocol for the ssNoVersion service. Supported values are: 0 to disable the TCP protocol, and 1 to enable the TCP protocol.  
  
        TCPENABLED=1  
  
        ;Specifies acceptance of License Terms  
  
        IAcceptSQLServerLicenseTerms="True"  
        ```  
  
     L’exemple suivant montre comment lancer l’installation à l’aide d’un fichier de configuration par défaut ou personnalisé :  
  
    -   Démarrage de l’installation à l’aide d’un fichier de configuration personnalisé :  
  
         Pour spécifier le fichier de configuration à l'invite de commandes :  
  
        ```console
        Setup.exe /QS /ConfigurationFile=MyConfigurationFile.INI  
        ```  
  
         Pour spécifier des mots de passe à l'invite de commandes plutôt que dans le fichier de configuration :  
  
        ```console
        Setup.exe /QS /SQLSVCPASSWORD="************" /ASSVCPASSWORD="************"  /ConfigurationFile=MyConfigurationFile.INI  
        ```  
  
    -   Lancement de l’installation à l’aide du fichier DefaultSetup.ini :  
  
         Si vous le fichier DefaultSetup.ini figure dans les dossiers \x86 et \x64 au niveau de la racine du média source [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ouvrez le fichier DefaultSetup.ini, puis ajoutez le paramètre *Features* au fichier.  
  
         Si le fichier DefaultSetup.ini n'existe pas, vous pouvez le créer et le copier dans les dossiers \x86 et \x64 au niveau de la racine du média source [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="configure-remote-access-of-ssnoversion-on-server-core"></a>Configurer l’accès à distance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Server Core  
 Effectuez les actions décrites ci-dessous pour configurer l’accès à distance d’une instance de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)] qui s’exécute sur Server Core.  
  
### <a name="enable-remote-connections-on-the-instance-of-ssnoversion"></a>Activer les connexions distantes sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  

Pour activer les connexions distantes, utilisez SQLCMD.exe localement et exécutez les instructions suivantes sur l'instance de Server Core :  

```sql
EXEC sys.sp_configure N'remote access', N'1'  
GO
RECONFIGURE WITH OVERRIDE
GO
```  
  
### <a name="enable-and-start-the-ssnoversion-browser-service"></a>Activer et démarrer le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] browser service  
 Par défaut, le service Browser est désactivé.  Si elle est désactivée sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécutant sur Server Core, exécutez la commande suivante à partir de l'invite de commandes pour l'activer :  
  
 `sc config SQLBROWSER start= auto`  
  
 Après activation, exécutez la commande suivante à partir de l'invite de commandes pour démarrer le service :  
  
 `net start SQLBROWSER`  
  
### <a name="create-exceptions-in-windows-firewall"></a>Créer des exceptions dans le pare-feu Windows  
 Pour créer des exceptions pour l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le Pare-feu Windows, suivez les étapes spécifiées dans [Configurer le Pare-feu Windows pour autoriser l’accès à SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
### <a name="enable-tcpip-on-the-instance-of-ssnoversion"></a>Activer TCP/IP sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Le protocole TCP/IP peut être activé via Windows PowerShell pour une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Server Core. Procédez comme suit :  
  
1.  Dans PowerShell : Import-Module SQLPS.  
  
2.  Dans la fenêtre **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Powershell**, exécutez le script suivant pour activer le protocole TCP/IP :  
  
```powershell  
$smo = 'Microsoft.SqlServer.Management.Smo.'  
$wmi = new-object ($smo + 'Wmi.ManagedComputer')  
# Enable the TCP protocol on the default instance.  If the instance is named, replace MSSQLSERVER with the instance name in the following line.  
$uri = "ManagedComputer[@Name='" + (get-item env:\computername).Value + "']/ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
$Tcp = $wmi.GetSmoObject($uri)  
$Tcp.IsEnabled = $true  
$Tcp.Alter()  
$Tcp  
```  
  
## <a name="uninstall"></a>Désinstaller l’interface

 Après avoir ouvert une session sur un ordinateur qui exécute Server Core, vous disposez d’un environnement de bureau limité avec une invite de commandes d’administrateur. Vous pouvez utiliser cette invite de commandes pour lancer la désinstallation de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)]. Pour désinstaller une instance de [!INCLUDE[ssCurrent](../../includes/ssnoversion-md.md)], lancez la désinstallation à partir de l'invite de commandes en mode silencieux complet à l'aide du paramètre /Q ou en mode silencieux simple à l'aide du paramètre /QS. Le paramètre /QS indique la progression via l'interface utilisateur, mais n'accepte aucune entrée. /Q s'exécute en mode silencieux sans interface utilisateur.  
  
 Pour désinstaller une instance existante de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
```console
Setup.exe /Q /Action=Uninstall /FEATURES=SQLEngine,AS,IS /INSTANCENAME=MSSQLSERVER  
```  
  
 Pour supprimer une instance nommée, spécifiez le nom de l’instance au lieu de `MSSQLSERVER` dans l’exemple précédent.  
  
## <a name="start-a-new-command-prompt"></a>Démarrer une nouvelle invite de commandes

Si vous fermez accidentellement l'invite de commandes, vous pouvez démarrer une nouvelle invite de commandes en suivant ces étapes :  
 
1.  Appuyez sur Ctrl+Shift+Esc pour afficher le Gestionnaire des tâches.  
2.  Dans l'onglet **Applications** , cliquez sur **Nouvelle tâche**.  
3.  Dans la boîte de dialogue **Créer une nouvelle tâche** , tapez **cmd** dans le champ **Ouvrir** , [!INCLUDE[clickOK](../../includes/clickok-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [Installer SQL Server à l’aide d’un fichier de configuration](./install-sql-server-using-a-configuration-file.md)   
 [Installer SQL Server à partir de l’invite de commandes](./install-sql-server-from-the-command-prompt.md)   
 [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)   
 [Installer Server Core](/windows-server/get-started/getting-started-with-server-core)   
 [Configurer une installation Server Core de Windows Server 2016 avec Sconfig.cmd](/windows-server/get-started/sconfig-on-ws2016)   
 [Applets de commande de cluster de basculement dans Windows PowerShell](/powershell/module/failoverclusters/)

  
