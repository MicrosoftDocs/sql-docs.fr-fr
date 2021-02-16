---
description: Journalisation d'Integration Services (SSIS)
title: Journalisation d’Integration Services (SSIS) | Microsoft Docs
ms.custom: supportability
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.configuredtslogs.containers.f1
- sql13.dts.designer.configuredtslogs.loggingdetails.f1
- sql13.dts.designer.configuredtslogs.providersandlogs.f1
- SQL13.SSIS.SSMS.ISMANAGECUSTOMIZEDLOGGINGLEVEL.F1
helpviewer_keywords:
- containers [Integration Services], logs
- Windows Event log provider [Integration Services]
- XML File log provider [Integration Services]
- SQL Server Profiler, log provider
- SQL Server Integration Services packages, logs
- SSIS packages, logs
- tasks [Integration Services], logs
- logs [Integration Services], log providers
- Integration Services packages, logs
- log providers [Integration Services]
- packages [Integration Services], logs
- Text File log provider
- SQL Server log provider
ms.assetid: 65e17889-371f-4951-9a7e-9932b2d0dcde
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8ac0f913188b23b85548f87a4f458d1c2b754d15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347451"
---
# <a name="integration-services-ssis-logging"></a>Journalisation d'Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contient des modules fournisseur d'informations que vous pouvez utiliser pour implémenter la journalisation dans les packages, les conteneurs et les tâches. Avec la journalisation, vous pouvez capturer des informations d'exécution sur un package qui vous aideront à auditer et à résoudre les problèmes d'un package à chaque exécution. Un journal peut ainsi capturer le nom de l'opérateur ayant exécuté le package et l'heure à laquelle le package a débuté et s'est terminé.  
  
 Vous pouvez configurer l'étendue des informations enregistrées lors d'une exécution de package sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d’informations, consultez [Activer la journalisation des exécutions de package sur le serveur SSIS](#server_logging).  
  
 Vous pouvez également inclure la journalisation lorsque vous exécutez un package à l'aide de l'utilitaire d'invite de commandes **dtexec** . Pour plus d'informations sur les arguments d'invite de commandes prenant en charge la journalisation, consultez [dtexec Utility](../../integration-services/packages/dtexec-utility.md).  
  
## <a name="configure-logging-in-sql-server-data-tools"></a>Configurer la journalisation dans les Outils de données SQL Server  
 Les journaux sont associés aux packages et configurés au niveau du package. Chaque tâche ou conteneur dans un package peut journaliser des informations dans n'importe quel journal de package. Les tâches et conteneurs d'un package peuvent être activés pour la journalisation même si le package lui-même ne l'est pas. Par exemple, vous pouvez activer la journalisation sur une tâche d'exécution SQL sans activer la journalisation sur le package parent. Un package, un conteneur ou une tâche peuvent écrire dans plusieurs journaux. Vous pouvez activer la journalisation sur le package uniquement, ou choisir d'activer la journalisation sur une tâche ou un conteneur individuel contenu dans le package.  
  
 Lorsque vous ajoutez le journal à un package, vous choisissez le module fournisseur d'informations et l'emplacement du journal. Le module fournisseur d'informations spécifie le format des données du journal, par exemple, une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou un fichier texte.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] comprend les modules fournisseurs d'informations suivants :  
  
-   Le module fournisseur d'informations pour les fichiers texte, qui enregistre les entrées du journal dans des fichiers texte ASCII au format CSV. L'extension de nom de fichier par défaut de ce fournisseur est .log.  
  
-   Le module fournisseur d'informations pour le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] , qui enregistre des traces que vous pouvez ensuite afficher à l'aide de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler. L'extension de nom de fichier par défaut de ce fournisseur est .trc.  
  
    > [!NOTE]  
    >  Vous ne pouvez pas utiliser le module fournisseur d’informations [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] dans un package exécuté en mode 64 bits.  
  
-   Le module fournisseur d'informations pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , qui enregistre les entrées du journal dans la table **sysssislog** d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Le module fournisseur d'informations pour les événements Windows, qui enregistre les entrées du journal des applications dans le journal des événements Windows sur l'ordinateur local.  
  
-   Le module fournisseur d'informations pour les fichiers XML, qui enregistre les fichiers journaux dans un fichier XML. L'extension de nom de fichier par défaut de ce fournisseur est .xml.  
  
 Si vous ajoutez un module fournisseur d'informations à un package ou configurez la journalisation par programmation, vous pouvez utiliser un ProgID ou un ClassID pour identifier ce module, au lieu d'utiliser les noms affichés par le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] dans la boîte de dialogue **Configurer les journaux SSIS** .  
  
 Le tableau suivant énumère les ProgID et ClassID des modules fournisseurs d'informations inclus dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , ainsi que l'emplacement des journaux dans lesquels les modules fournisseurs d'informations écrivent.  
  
|Module fournisseur d'informations|ProgID|ClassID|Location|  
|------------------|------------|-------------|--------------|  
|Fichier texte|DTS.LogProviderTextFile|{0A039101-ACC1-4E06-943F-279948323883}|Le gestionnaire de connexions de fichiers utilisé par le module fournisseur d'informations spécifie le chemin d'accès du fichier texte.|  
|SQL Server Profiler|DTS.LogProviderSQLProfiler|{E93F6300-AE0C-4916-A7BF-A8D0CE12C77A}|Le gestionnaire de connexions de fichiers utilisé par le module fournisseur d'informations spécifie le chemin d'accès du fichier utilisé par [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|  
|SQL Server|DTS.LogProviderSQLServer|{94150B25-6AEB-4C0D-996D-D37D1C4FDEDA}|Le gestionnaire de connexions OLE DB utilisé par le module fournisseur d’informations spécifie la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contenant la table sysssislog avec les entrées de journal.|  
|Journal des événements Windows|DTS.LogProviderEventLog|{071CC8EB-C343-4CFF-8D58-564B92FCA3CF}|Le journal des applications dans l'Observateur d'événements Windows contient les informations de journalisation de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
|Fichier XML|DTS.LogProviderXMLFile|{440945A4-2A22-4F19-B577-EAF5FDDC5F7A}|Le gestionnaire de connexions de fichiers utilisé par le module fournisseur d'informations spécifie le chemin d'accès du fichier XML.|  
  
 Vous pouvez également créer des modules fournisseurs d'informations personnalisés. Pour plus d’informations, consultez [Creating a Custom Log Provider](../../integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider.md).  
  
 Les modules fournisseurs d'informations d'un package sont membres de la collection des modules fournisseurs d'informations du package. Si vous créez un package et implémentez la journalisation à l'aide du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vous pouvez afficher la liste des membres de la collection dans les dossiers **Module fournisseur d'informations** de l'onglet **Explorateur de package** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Un module fournisseur d'informations est configuré en indiquant le nom et la description du module et en spécifiant le gestionnaire de connexions qu'il utilise. Le module fournisseur d'informations [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise un gestionnaire de connexions OLE DB. Le fichier texte, [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]et les modules fournisseurs d'informations pour les fichiers XML utilisent tous des gestionnaires de connexions de fichiers. Le module fournisseur d'informations pour les événements Windows n'utilise pas de gestionnaire de connexions, car il écrit directement dans le journal des événements Windows. Pour plus d'informations, consultez [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md) et [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md).  
  
### <a name="logging-customization"></a>Personnalisation de la journalisation  
 Pour personnaliser la journalisation d'un événement ou d'un message personnalisé, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] fournit un schéma d'informations fréquemment journalisées à inclure dans les entrées de journal. Le schéma de journal [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] définit les informations que vous pouvez journaliser. Vous pouvez sélectionner les éléments à partir du schéma de journal pour chaque entrée de journal.  
  
 Un package et ses conteneurs et tâches ne doivent pas journaliser la même information, et les tâches au sein du même package ou conteneur peuvent journaliser des informations différentes. Par exemple, un package peut journaliser des informations d'opérateur lorsque le package démarre, une tâche peut journaliser la source de l'échec de la tâche et une autre tâche peut journaliser les informations lorsque des erreurs surviennent. Si un package et ses conteneurs et tâches utilisent plusieurs journaux, les mêmes informations sont écrites dans tous les journaux.  
  
 Vous pouvez sélectionner un niveau de journalisation adapté à vos besoins en spécifiant les événements à journaliser et les informations à journaliser pour chaque événement. Vous pouvez estimer que certains événements fournissent des informations plus utiles que d'autres. Par exemple, vous pouvez ne journaliser que les noms d'ordinateur et d'opérateur pour l'événement **PreExecute** tout en journalisant toutes les informations disponibles pour l'événement **Error** .  
  
 Pour empêcher les fichiers journaux d'occuper de grands volumes sur l'espace disque ou pour éviter une journalisation excessive qui pourrait nuire aux performances, vous pouvez limiter la journalisation en sélectionnant des informations et événements spécifiques à journaliser. Par exemple, vous pouvez configurer un journal pour qu'il ne capture que la date et le nom de l'ordinateur pour chaque erreur.  
  
 Dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vous pouvez définir les options de journalisation en utilisant la boîte de dialogue **Configurer les journaux SSIS** .  
  
#### <a name="log-schema"></a>Schéma de journal  
 Le tableau suivant décrit les éléments du schéma de journal.  
  
|Élément|Description|  
|-------------|-----------------|  
|Computer|Le nom de l'ordinateur sur lequel l'événement du journal est survenu.|  
|Opérateur|L'identité de l'utilisateur ayant lancé le package.|  
|SourceName|Le nom du conteneur ou de la tâche dans laquelle l'événement du journal est survenu.|  
|SourceID|L'identificateur unique du package ; la boucle Foreach, la boucle For ou le conteneur de séquences ; ou la tâche dans laquelle l'événement du journal est survenu.|  
|ExecutionID|L'identificateur global unique (GUID) de l'instance d'exécution du package.<br /><br /> Remarque : l’exécution d’un package unique peut créer des entrées de journal avec des valeurs différentes pour l’élément ExecutionID. Par exemple, lorsque vous exécutez un package dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], la phase de validation peut créer des entrées de journal avec un élément ExecutionID correspondant à [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. Toutefois, la phase d'exécution peut créer des entrées de journal avec un élément ExecutionID correspondant à dtshost.exe. Autre exemple : lorsque vous exécutez un package qui contient des tâches d'exécution de package, chacune de ces tâches exécute un package enfant. Ces packages enfants peuvent créer des entrées de journal comportant un élément ExecutionID différent de celui des entrées de journal créées par le package parent.|  
|MessageText|Un message associé à l'entrée de journal.|  
|DataBytes|Un tableau d'octets spécifique à l'entrée du journal. La signification de ce champ varie en fonction de l'entrée du journal.|  
  
 Le tableau suivant décrit trois éléments supplémentaires du schéma de journal qui ne sont pas disponibles sous l'onglet **Détails** de la boîte de dialogue **Configurer les journaux SSIS** .  
  
|Élément|Description|  
|-------------|-----------------|  
|StartTime|Heure à laquelle le conteneur ou la tâche commence à s'exécuter.|  
|EndTime|Heure à laquelle le conteneur ou la tâche arrête de s'exécuter.|  
|DataCode|Valeur entière facultative qui contient généralement une valeur de l’énumération <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult> qui indique le résultat de l’exécution du conteneur ou de la tâche :<br /><br /> 0 - Succès<br /><br /> 1 - Échec<br /><br /> 2 - Terminée<br /><br /> 3 - Annulée|  
  
#### <a name="log-entries"></a>Entrées du journal  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] prend en charge les entrées de journal sur des événements prédéfinis et fournit des entrées de journal personnalisées pour de nombreux objets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . La boîte de dialogue **Configurer les journaux SSIS** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] répertorie ces événements et ces entrées de journal personnalisées.  
  
 Le tableau suivant décrit les événements prédéfinis qui peuvent être activés pour écrire des entrées de journal lorsque des événements générés à l'exécution surviennent. Ces entrées de journal s'appliquent aux exécutables, au package et aux conteneurs et tâches inclus dans le package. Le nom de l'entrée de journal est le même que l'événement généré à l'exécution et qui a provoqué l'écriture de l'entrée de journal.  
  
|Événements|Description|  
|------------|-----------------|  
|**OnError**|Écrit une entrée de journal lorsqu'une erreur se produit.|  
|**OnExecStatusChanged**|Écrit une entrée de journal lorsqu'une tâche (et non un conteneur) est interrompue ou reprise pendant le débogage.|  
|**OnInformation**|Écrit une entrée de journal pendant la validation et l'exécution d'un exécutable pour rapporter des informations.|  
|**OnPostExecute**|Écrit une entrée de journal immédiatement après la fin de l'exécution de l'exécutable.|  
|**OnPostValidate**|Écrit une entrée de journal lorsque la validation de l'exécutable est terminée.|  
|**OnPreExecute**|Écrit une entrée de journal immédiatement avant le début de l'exécution de l'exécutable.|  
|**OnPreValidate**|Écrit une entrée de journal lorsque la validation de l'exécutable débute.|  
|**OnProgress**|Écrit une entrée de journal lorsqu'une progression quantifiable a été réalisée par l'exécutable.|  
|**OnQueryCancel**|Écrit une entrée de journal pour chaque point de jonction dans le traitement de la tâche où il est possible d'annuler l'exécution.|  
|**OnTaskFailed**|Écrit une entrée de journal lorsqu'une tâche échoue.|  
|**OnVariableValueChanged**|Écrit une entrée de journal lorsque la valeur d'une variable est modifiée.|  
|**OnWarning**|Écrit une entrée de journal lorsqu'un avertissement survient.|  
|**PipelineComponentTime**|Pour chaque composant de flux de données, écrit une entrée de journal pour chaque phase de validation et d'exécution. L'entrée de journal spécifie le temps de traitement de chaque phase.|  
|**Diagnostic**<br /><br /> **DiagnosticEx**|Écrit une entrée de journal qui fournit des informations de diagnostic.<br /><br /> Par exemple, vous pouvez enregistrer un message avant et après chaque appel à un fournisseur de données externes. Pour plus d’informations, consultez [Outils de dépannage pour l’exécution des packages](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).<br /><br /> Consignez l’événement **DiagnosticEx** lorsque vous souhaitez rechercher les noms des colonnes dans le flux de données qui contiennent des erreurs. Cet événement consigne un mappage de lignage de flux de données dans le journal. Vous pouvez alors rechercher le nom de colonne dans ce mappage de lignage à l’aide de l’identificateur de colonne capturé par une sortie d’erreur. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).<br /><br /> Notez que l’événement **DiagnosticEx** ne conserve pas l’espace blanc dans sa sortie XML afin réduire la taille du journal. Pour améliorer la lisibilité, copiez le journal dans un éditeur XML (dans Visual Studio, par exemple) prenant en charge la mise en forme XML et la mise en surbrillance de la syntaxe.<br /><br /> Remarque : si vous consignez l’événement **DiagnosticEx** avec le fournisseur de journaux SQL Server, la sortie risque d’être tronquée. Le champ **message** du module fournisseur d’informations SQL Server est de type nvarchar(2048). Pour prévenir le risque de troncation, utilisez un fournisseur de journaux différent lorsque vous consignez l’événement **DiagnosticEx** .|  
  
 Le package ainsi que de nombreuses tâches contiennent des entrées du journal personnalisées qui peuvent être activées pour la journalisation. Par exemple, la tâche d’envoi de courrier fournit l’entrée de journal personnalisée **SendMailTaskBegin** , qui journalise les informations quand l’exécution de la tâche démarre, mais avant d’envoyer un message électronique. Pour plus d’informations, consultez [Custom Messages for Logging](#custom_messages).  
  
### <a name="differentiating-package-copies"></a>Différentiation entre des copies de packages  
 Les données de journalisation incluent le nom et le GUID du package auxquels les entrées de journal appartiennent. Si vous créez un package par copie d'un package existant, le nom et le GUID du package existant sont également copiés. Il est donc possible que deux packages aient le même nom et le même GUID, ce qui les rend difficiles à distinguer dans les données du journal.  
  
 Pour lever l'ambiguïté, vous devez mettre à jour le nom et le GUID des nouveaux packages. Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], vous pouvez régénérer le GUID dans la propriété **ID** et mettre à jour la valeur de la propriété **Name** dans la fenêtre Propriétés. Vous pouvez également modifier le GUID et le nom par programmation ou en utilisant l'invite de commandes **dtutil** . Pour plus d’informations, consultez [Définir les propriétés d’un package](../../integration-services/set-package-properties.md) et [Utilitaire dtutil](../../integration-services/dtutil-utility.md).  
  
### <a name="parent-logging-options"></a>Options de journalisation des parents  
 Souvent, les options de journalisation des tâches et conteneurs de séquences et de boucles For ou Foreach correspondent à celle du package ou du conteneur parent. Dans ce cas, vous pouvez configurer ces éléments pour qu'ils héritent des options de journalisation de leur conteneur parent. Par exemple, dans un conteneur de boucles For qui contient une tâche d'exécution SQL, cette tâche peut utiliser les options de journalisation définies pour le conteneur de boucles For. Pour utiliser les options de journalisation des parents, définissez la propriété LoggingMode du conteneur avec la valeur **UseParentSetting**. Vous pouvez définir cette propriété dans la fenêtre **Propriétés** de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ou via la boîte de dialogue **Configurer les journaux SSIS** dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
### <a name="logging-templates"></a>Modèles de journalisation  
 Dans la boîte de dialogue **Configurer les journaux SSIS** , vous pouvez également créer et enregistrer comme modèles les configurations de journalisation fréquemment utilisées, puis utiliser ces modèles dans différents packages. Cette procédure facilite l'application d'une stratégie de journalisation cohérente à différents packages et la modification des paramètres de journaux par la mise à jour et l'application des modèles. Les modèles sont stockés sous forme de fichiers XML.  
  
 **Pour configurer la journalisation à l'aide de la boîte de dialogue Configurer les journaux SSIS**  
  
1.  Activez le package et ses tâches pour la journalisation. La journalisation peut s'effectuer au niveau du package, du conteneur et de la tâche. Vous pouvez spécifier différents journaux pour les packages, conteneurs et tâches.  
  
2.  Sélectionnez un module fournisseur d'informations et ajoutez un journal pour le package. Les journaux ne peuvent être créés qu'au niveau du package, et une tâche ou un conteneur doit utiliser un des journaux créés pour le package. Chaque journal est associé à l'un des modules fournisseurs d'informations suivants : fichier texte, [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], journal des événements Windows ou fichier XML. Pour plus d’informations, consultez [Activer la journalisation des packages dans les outils de données SQL Server](#ssdt).  
  
3.  Sélectionnez les événements et les informations de schéma de journal pour chaque événement que vous voulez capturer dans le journal. Pour plus d’informations, consultez [Configurer la journalisation à l’aide d’un fichier de configuration enregistré](#saved_config).  
  
### <a name="configuration-of-log-provider"></a>Configuration du module fournisseur d'informations  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Un module fournisseur d'informations est créé et configuré dans le cadre de l'implémentation d'un package.  
  
 Après avoir créé un module fournisseur d'informations, vous pouvez l'afficher et modifier ses propriétés dans la fenêtre Propriétés de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
 Pour plus d’informations sur la définition par programmation de ces propriétés, consultez la documentation relative à la classe <xref:Microsoft.SqlServer.Dts.Runtime.LogProvider> .  
  
### <a name="logging-for-data-flow-tasks"></a>Journalisation des tâches de flux de données  
 La tâche de flux de données fournit un grand nombre d'entrées de journal personnalisées à l'aide desquelles vous pouvez analyser et améliorer les performances. Vous pouvez, par exemple, analyser les composants susceptibles de provoquer des fuites de mémoire ou contrôler le temps nécessaire à l'exécution d'une tâche en particulier. Pour obtenir une liste de ces entrées de journal personnalisées et un exemple de sortie de journalisation, consultez [Data Flow Task](../../integration-services/control-flow/data-flow-task.md).  
  
#### <a name="capture-the-names-of-columns-in-which-errors-occur"></a>Capturez les noms des colonnes dans lesquelles des erreurs se produisent  
 Lorsque vous configurez une sortie d’erreur dans le flux de données, par défaut, la sortie d’erreur fournit uniquement l’identificateur numérique de la colonne dans laquelle l’erreur s’est produite. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).  
  
 Vous pouvez rechercher des noms de colonne en activant la journalisation et en sélectionnant l’événement **DiagnosticEx** . Cet événement consigne un mappage de lignage de flux de données dans le journal. Vous pouvez ensuite rechercher le nom de colonne à partir de son identificateur dans ce mappage de lignage. Notez que l’événement **DiagnosticEx** ne conserve pas l’espace blanc dans sa sortie XML afin réduire la taille du journal. Pour améliorer la lisibilité, copiez le journal dans un éditeur XML (dans Visual Studio, par exemple) prenant en charge la mise en forme XML et la mise en surbrillance de la syntaxe.  
  
#### <a name="use-the-pipelinecomponenttime-event"></a>Utiliser l'événement PipelineComponentTime  
 L'entrée de journal personnalisée la plus utile est peut-être l'événement PipelineComponentTime. Cette entrée de journal signale le nombre de millisecondes que chaque composant dans le flux de données passe sur chacune des cinq étapes de traitement majeures. Le tableau suivant décrit ces étapes de traitement. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Les développeurs reconnaîtront ces étapes comme étant les méthodes principales d’un <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>.  
  
|Étape|Description|  
|----------|-----------------|  
|Valider|Le composant recherche des valeurs de propriété et des paramètres de configuration valides.|  
|PreExecute|Le composant effectue un traitement unique avant de commencer à traiter les lignes de données.|  
|PostExecute|Le composant effectue un traitement unique après avoir traité toutes les lignes de données.|  
|ProcessInput|Le composant de transformation ou de destination traite les lignes de données entrantes qu'une source ou une transformation en amont lui a passées.|  
|PrimeOutput|Le composant source ou de transformation remplit les tampons de données à passer à un composant de transformation ou de destination en aval.|  
  
 Lorsque vous activez l'événement PipelineComponentTime, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] consigne un message pour chaque étape de traitement effectuée par chaque composant. Les entrées de journal suivantes illustrent un sous-ensemble des messages consignés par l'exemple de package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] CalculatedColumns :  
  
 `The component "Calculate LineItemTotalCost" (3522) spent 356 milliseconds in ProcessInput.`  
  
 `The component "Sum Quantity and LineItemTotalCost" (3619) spent 79 milliseconds in ProcessInput.`  
  
 `The component "Calculate Average Cost" (3662) spent 16 milliseconds in ProcessInput.`  
  
 `The component "Sort by ProductID" (3717) spent 125 milliseconds in ProcessInput.`  
  
 `The component "Load Data" (3773) spent 0 milliseconds in ProcessInput.`  
  
 `The component "Extract Data" (3869) spent 688 milliseconds in PrimeOutput filling buffers on output "OLE DB Source Output" (3879).`  
  
 `The component "Sum Quantity and LineItemTotalCost" (3619) spent 141 milliseconds in PrimeOutput filling buffers on output "Aggregate Output 1" (3621).`  
  
 `The component "Sort by ProductID" (3717) spent 16 milliseconds in PrimeOutput filling buffers on output "Sort Output" (3719).`  
  
 Ces entrées de journal montrent que la tâche de flux de données a passé la plupart du temps dans les étapes suivantes, répertoriées ici dans l'ordre décroissant :  
  
-   La source OLE DB nommée « Extract Data » a passé 688 ms à charger des données.  
  
-   La transformation de la colonne dérivée nommée « Calculate LineItemTotalCost » a passé 356 ms à exécuter des calculs sur les lignes entrantes.  
  
-   La transformation d’agrégation nommée « Sum Quantity and LineItemTotalCost » a passé un total de 220 ms (141 ms dans PrimeOutput et 79 ms dans ProcessInput) à effectuer des calculs et à passer les données à la transformation suivante.  

## <a name="enable-package-logging-in-sql-server-data-tools"></a><a name="ssdt"></a> Activer la journalisation des packages dans les outils de données SQL Server
  Cette procédure décrit comment ajouter des journaux à un package, configurer la journalisation au niveau du package et enregistrer la configuration dans un fichier XML. Vous ne pouvez ajouter des journaux qu'au niveau du package, mais le package n'a pas besoin d'effectuer une journalisation pour activer la journalisation dans les conteneurs inclus dans ce package.  
  
> [!IMPORTANT]  
>  Si vous déployez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , le niveau de journalisation que vous définissez pour l'exécution du package remplace l'enregistrement du package que vous configurez dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
 Par défaut, les conteneurs du package utilisent la même configuration de journalisation que leur conteneur parent. Pour plus d’informations sur la définition des options de journalisation pour les conteneurs individuels, consultez [Configurer la journalisation à l’aide d’un fichier de configuration enregistré](#saved_config).  
  
### <a name="to-enable-logging-in-a-package"></a>Pour activer la journalisation dans un package  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans le menu **SSIS** , cliquez sur **Enregistrement**.  
  
3.  Sélectionnez un module fournisseur d'informations dans la liste **Type de fournisseur** , puis cliquez sur **Ajouter**.  
  
4.  Dans la colonne **Configuration**, sélectionnez un gestionnaire de connexions ou cliquez sur **\<New connection>** afin de créer un gestionnaire de connexions du type approprié pour le module fournisseur d’informations. En fonction du module fournisseur sélectionné, utilisez l'un des gestionnaires de connexions suivants :  
  
    -   Pour les fichiers texte, utilisez un gestionnaire de connexions de fichiers. Pour plus d’informations, consultez [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md)  
  
    -   Pour [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], utilisez un gestionnaire de connexions de fichiers.  
  
    -   Pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez un gestionnaire de connexions OLE DB. Pour plus d’informations, consultez [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
    -   Pour le journal des événements Windows, ne faites rien : [!INCLUDE[ssIS](../../includes/ssis-md.md)] crée automatiquement le journal.  
  
    -   Pour les fichiers XML, utilisez un gestionnaire de connexions de fichiers.  
  
5.  Répétez les étapes 3 et 4 pour chaque journal à utiliser dans le package.  
  
    > [!NOTE]  
    >  Un package peut utiliser plusieurs journaux du même type.  
  
6.  Vous pouvez également cocher la case du niveau du package, sélectionner les journaux à utiliser pour la journalisation au niveau du package, puis cliquer sur l’onglet **Détails** .  
  
7.  Sur l'onglet **Détails** , activez **Événements** pour enregistrer toutes les entrées de journal ou désactivez **Événements** pour sélectionner des événements individuels.  
  
8.  Éventuellement, cliquez sur **Avancé** pour spécifier les informations à journaliser.  
  
    > [!NOTE]  
    >  Par défaut, toutes les informations sont journalisées.  
  
9. Sous l’onglet **Détails** , cliquez sur **Enregistrer**. La boîte de dialogue **Enregistrer sous** s’affiche. Recherchez l'emplacement où enregistrer la configuration de journalisation, tapez un nom de fichier pour la nouvelle configuration de journal, puis cliquez sur **Enregistrer**.  
  
10. Cliquez sur **OK**.  
  
11. Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  

## <a name="configure-ssis-logs-dialog-box"></a><a name="configure_logs"></a> Boîte de dialogue Configurer les journaux SSIS
  Utilisez la boîte de dialogue **Configurer les journaux SSIS** pour définir les options de journalisation d’un package.  
  
 **Que voulez-vous faire ?**  
  
1.  [Ouvrir la boîte de dialogue Configurer les journaux SSIS](#open_dialog)  
  
2.  [Configurer les options du volet Conteneurs](#container)  
  
3.  [Configurer les options sous l'onglet Fournisseurs et journaux](#provider)  
  
4.  [Configurer les options sous l'onglet Détails](#detail)  
  
###  <a name="open-the-configure-ssis-logs-dialog-box"></a><a name="open_dialog"></a> Ouvrir la boîte de dialogue Configurer les journaux SSIS  
 **Pour ouvrir la boîte de dialogue Configurer les journaux SSIS**  
  
-   Dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , dans le menu **SSIS** , cliquez sur **Enregistrement** .  
  
###  <a name="configure-the-options-in-the-containers-pane"></a><a name="container"></a> Configurer les options du volet Conteneurs  
 Utilisez le volet **Conteneurs** de la boîte de dialogue **Configurer les journaux SSIS** pour activer l’enregistrement du fichier journal du package et de ses conteneurs.  
  
#### <a name="options"></a>Options  
 **Containers**  
 Activez les cases à cocher dans la vue hiérarchique pour activer l'enregistrement du fichier journal du package et de ses conteneurs :  
  
-   Si la case à cocher est désactivée, l'enregistrement du container dans le fichier journal n'est pas activé. Activez-la pour permettre l'enregistrement.  
  
-   Si elle est grisée, le conteneur utilise les options d'enregistrement dans le journal de son parent. Cette option n'est pas disponible pour le package.  
  
-   Si elle est activée, le conteneur définit ses propres options d'enregistrement dans le journal.  
  
 Si un conteneur est grisé alors que vous voulez définir ses options d'enregistrement dans le journal, cliquez sur sa case à cocher deux fois. Le premier clic désactive la case à cocher et le deuxième l'active, vous permettant ainsi de choisir le module fournisseur d'informations à utiliser et de sélectionner les informations à enregistrer.  
  
###  <a name="configure-the-options-on-the-providers-and-logs-tab"></a><a name="provider"></a> Configurer les options sous l'onglet Fournisseurs et journaux  
 Utilisez l’onglet **Fournisseurs et journaux** de la boîte de dialogue **Configurer les journaux SSIS** pour créer et configurer des journaux permettant de capturer des événements à l’exécution.  
  
#### <a name="options"></a>Options  
 **Type de fournisseur**  
 Sélectionnez un type de module fournisseur d'informations dans la liste.  
  
 **Ajouter**  
 Ajoutez un journal du type spécifié à la collection de modules fournisseurs d'informations du package.  
  
 **Nom**  
 Activez ou désactivez les journaux pour les conteneurs ou les tâches sélectionnés dans le volet **Conteneurs** de la boîte de dialogue **Configurer les journaux SSIS** à l’aide des cases à cocher. Le champ de nom est modifiable. Utilisez le nom par défaut du fournisseur ou tapez un nom descriptif unique.  
  
 **Description**  
 Le champ de description est modifiable. Cliquez sur cette option, puis modifiez la description par défaut du journal.  
  
 **Configuration**  
 Sélectionnez un gestionnaire de connexions existant dans la liste ou cliquez sur \<**New connection...**> pour en créer un. En fonction du type de module fournisseur d'informations, vous pouvez configurer un gestionnaire de connexions OLE DB ou un gestionnaire de connexions de fichiers. Le module fournisseur d’informations du journal des événements [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows ne nécessite aucune connexion.  
  
 Rubriques connexes : [Gestionnaire de connexions OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md) , [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md)  
  
 **Supprimer**  
 Sélectionnez un module fournisseur d’informations, puis cliquez sur **Supprimer**.  
  
###  <a name="configure-the-options-on-the-details-tab"></a><a name="detail"></a> Configurer les options sous l'onglet Détails  
 Utilisez l’onglet **Détails** de la boîte de dialogue **Configurer les journaux SSIS** pour spécifier les événements à enregistrer dans le journal et les détails des informations à consigner. Les informations sélectionnées s'appliquent à tous les modules fournisseurs d'informations du package. Par exemple, vous ne pouvez pas écrire d’informations dans l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et écrire des informations différentes dans un fichier texte.  
  
#### <a name="options"></a>Options  
 **Événements**  
 Activez ou désactivez les événements à enregistrer dans le journal.  
  
 **Description**  
 Affichez la description de l'événement.  
  
 **Avancée**  
 Sélectionnez ou désélectionnez les événements à enregistrer dans le journal et les informations à enregistrer pour chaque événement. Cliquez sur **Simple** pour masquer tous les détails de l’enregistrement dans le journal à l’exception de la liste des événements. Les informations suivantes peuvent être enregistrées dans le journal :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Ordinateur**|Nom de l'ordinateur sur lequel s'est produit l'événement enregistré.|  
|**Opérateur**|Nom de l'utilisateur qui a démarré le package.|  
|**SourceName**|Nom du package, du conteneur ou de la tâche dans lequel s'est produit l'événement enregistré.|  
|**SourceID**|GUID (Global Unique IDentifier) du package, du conteneur ou de la tâche dans lequel s'est produit l'événement enregistré.|  
|**ExecutionID**|GUID de l'instance d'exécution du package.|  
|**MessageText**|Un message associé à l'entrée de journal.|  
|**DataBytes**|Réservé pour un usage futur.|  
  
 **De base**  
 Sélectionnez ou désélectionnez les événements à enregistrer dans le journal. Cette option masque les détails d'enregistrement à l'exception de la liste des événements. Si vous sélectionnez un événement, tous les détails d'enregistrement dans le journal sont sélectionnés pour l'événement par défaut. Cliquez sur **Avancé** pour afficher tous les détails d’enregistrement.  
  
 **Load**  
 Spécifiez un fichier XML existant à utiliser comme modèle de configuration des options d'enregistrement dans le journal.  
  
 **Save**  
 Enregistrez les détails de la configuration en tant que modèle dans un fichier XML.  

## <a name="configure-logging-by-using-a-saved-configuration-file"></a><a name="saved_config"></a> Configurer la journalisation à l’aide d’un fichier de configuration enregistré
  Cette procédure permet de configurer la journalisation de nouveaux conteneurs dans un package en chargeant un fichier de configuration de journalisation déjà enregistré.  
  
 Par défaut, tous les conteneurs d'un package utilisent la même configuration de journalisation que leur conteneur parent. Par exemple, les tâches d'une boucle Foreach utilisent la même configuration de journalisation que la boucle Foreach.  
  
### <a name="to-configure-logging-for-a-container"></a>Pour configurer la journalisation pour un conteneur  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans le menu **SSIS** , cliquez sur **Enregistrement**.  
  
3.  Développez l'arborescence du package et sélectionnez le conteneur à configurer.  
  
4.  Sous l’onglet **Fournisseurs et journaux** , sélectionnez les journaux à utiliser pour le conteneur.  
  
    > [!NOTE]  
    >  Vous ne pouvez créer des journaux qu'au niveau du package. Pour plus d’informations, consultez [Activer la journalisation des packages dans les outils de données SQL Server](#ssdt).  
  
5.  Cliquez sur l’onglet **Détails** , puis sur **Charger**.  
  
6.  Recherchez le fichier de configuration de journalisation que vous voulez utiliser et cliquez sur **Ouvrir**.  
  
7.  Si vous le souhaitez, sélectionnez une entrée de journal différente à consigner en cochant la case correspondante dans la colonne **Événements** . Cliquez sur **Avancé** pour sélectionner le type d’information à consigner pour cette entrée.  
  
    > [!NOTE]  
    >  Le nouveau conteneur peut inclure des entrées de journal supplémentaires qui ne sont pas disponibles pour le conteneur utilisé à l'origine pour créer la configuration de journalisation. Ces entrées de journal supplémentaires doivent être sélectionnées manuellement si vous voulez qu'elles soient journalisées.  
  
8.  Pour enregistrer la version mise à jour de la configuration de journalisation, cliquez sur **Enregistrer**.  
  
9. Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  

## <a name="enable-logging-for-package-execution-on-the-ssis-server"></a><a name="server_logging"></a>Activer la journalisation des exécutions de package sur le serveur SSIS
  Cette rubrique explique comment définir ou modifier le niveau de journalisation d'un package lorsque vous exécutez un package déployé sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Le niveau de journalisation que vous définissez lorsque vous exécutez le package remplace la journalisation du package configurée lors de la création dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Pour plus d’informations, consultez [Activer la journalisation des packages dans les outils de données SQL Server](#ssdt) .  
  
 Dans **Propriétés du serveur** de SQL Server, sous la propriété **Niveau de journalisation du serveur** , vous pouvez sélectionner un niveau de journalisation par défaut au niveau du serveur. Vous pouvez choisir parmi les niveaux de journalisation intégrés décrits dans cette rubrique, ou choisir un niveau de journalisation personnalisé existant. Le niveau de journalisation sélectionné s'applique par défaut à tous les packages déployés sur le catalogue SSIS. Il s'applique également par défaut à une étape de travail de l'Agent SQL qui exécute un package SSIS.  
  
 Vous pouvez également spécifier le niveau de journalisation d’un package individuel en procédant selon l'une des méthodes suivantes. Cette rubrique présente la première méthode.  
  
-   Configuration d'une instance d'un package d'exécution à l'aide de la boîte de dialogue Exécuter le package  
  
-   Définition des paramètres pour une instance d’exécution à l’aide de [catalog.set_execution_parameter_value &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md)  
  
-   Configuration d'un travail de l'Agent SQL Server pour une exécution de package à l'aide de la boîte de dialogue Nouvelle étape de travail.  
  
### <a name="set-the-logging-level-for-a-package-by-using-the-execute-package-dialog-box"></a>Définir le niveau de journalisation d'un package à l'aide de la boîte de dialogue Exécuter le package  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], naviguez jusqu'au package dans l'Explorateur d'objets.  
  
2.  Cliquez avec le bouton droit sur le package, puis sélectionnez **Exécuter**.  
  
3.  Dans la boîte de dialogue **Exécuter le package** , sélectionnez l'onglet **Avancé** .  
  
4.  Sous **Niveau de journalisation**, sélectionnez le niveau de journalisation. Cette rubrique contient une description des valeurs disponibles.  
  
5.  Terminez toutes les autres configurations du package, puis cliquez sur **OK** pour l'exécuter.  
  
### <a name="select-a-logging-level"></a>Sélectionner un niveau de journalisation  
 Les niveaux de journalisation intégrés suivants sont disponibles. Vous pouvez également sélectionner un niveau de journalisation personnalisé existant. Cette rubrique contient une description des niveaux de journalisation personnalisés.  
  
|Niveau de journalisation|Description|  
|-------------------|-----------------|  
|None|La journalisation est désactivée. Seul l'état d'exécution du package est enregistré.|  
|De base|Tous les événements sont enregistrés, sauf les événements personnalisés et de diagnostic. Il s’agit de la valeur par défaut.|  
|RuntimeLineage|Collecte les données nécessaires au suivi des informations de lignage dans le flux de données. Vous pouvez analyser ces informations de lignage pour mapper la relation de lignage entre différentes tâches. Les éditeurs de logiciels indépendants et les développeurs peuvent créer des outils de mappage de lignage personnalisés à l’aide de ces informations.|  
|Performances|Seules les statistiques de performances, et les événements OnError et OnWarning, sont enregistrés.<br /><br /> Le rapport **Performances de l'exécution** affiche le temps d'activité et le temps total écoulé des composants de flux de données du package. Ces informations sont disponibles si le niveau de journalisation de la dernière exécution du package a été défini sur **Performances** ou **Commentaires**. Pour plus d'informations, consultez [Reports for the Integration Services Server](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports).<br /><br /> La vue [catalog.execution_component_phases](../../integration-services/system-views/catalog-execution-component-phases.md) affiche les heures de début et de fin des composants de flux de données, pour chaque phase d’exécution. Cette vue affiche ces informations pour ces composants uniquement lorsque le niveau de journalisation de l'exécution du package est défini sur **Performances** ou **Commentaires**.|  
|Commentaires|Tous les événements sont enregistrés, y compris les événements personnalisés et de diagnostic.<br /><br /> Les événements personnalisés sont notamment les événements consignés par les tâches [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d'informations sur les événements personnalisés, consultez [Custom Messages for Logging](#custom_messages).<br /><br /> L'événement **DiagnosticEx** est un exemple d'événement de diagnostic. Chaque fois qu'une tâche d'exécution de package exécute un package enfant, cet événement capture les valeurs de paramètres passées aux packages enfants.<br /><br /> L’événement **DiagnosticEx** vous permet également d’obtenir les noms des colonnes dans lesquelles des erreurs se produisent au niveau des lignes. Cet événement consigne un mappage de lignage de flux de données dans le journal. Vous pouvez alors rechercher le nom de colonne dans ce mappage de lignage à l’aide de l’identificateur de colonne capturé par une sortie d’erreur.  Pour plus d’informations, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md).<br /><br /> La valeur de la colonne de message pour **DiagnosticEx** est du texte XML. Pour afficher le texte du message pour une exécution de package, interrogez la vue [catalog.operation_messages &#40;base de données SSISDB&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md). Notez que l’événement **DiagnosticEx** ne conserve pas l’espace blanc dans sa sortie XML afin réduire la taille du journal. Pour améliorer la lisibilité, copiez le journal dans un éditeur XML (dans Visual Studio, par exemple) prenant en charge la mise en forme XML et la mise en surbrillance de la syntaxe.<br /><br /> La vue [catalog.execution_data_statistics](../../integration-services/system-views/catalog-execution-data-statistics.md) affiche une ligne chaque fois qu’un composant de flux de données envoie des données à un composant en aval, pour une exécution de package. Le niveau de journalisation doit avoir la valeur **Commentaires** pour capturer ces informations dans la vue.|  
  
### <a name="create-and-manage-customized-logging-levels-by-using-the-customized-logging-level-management-dialog-box"></a>Créer et gérer des niveaux de journalisation personnalisés à l'aide de la boîte de dialogue de gestion des niveaux de journalisation personnalisés  
 Vous pouvez créer des niveaux de journalisation personnalisés qui collectent uniquement les statistiques et les événements que vous souhaitez. Vous pouvez également capturer le contexte des événements, notamment les valeurs de variables, les chaînes de connexion et les propriétés de composants. Lorsque vous exécutez un package, vous pouvez sélectionner un niveau de journalisation personnalisé partout où vous pouvez sélectionner un niveau de journalisation intégré.  
  
> [!TIP]  
>  Pour capturer les valeurs des variables d’un package, la propriété **IncludeInDebugDump** des variables doit être définie sur **True**.  
  
1.  Pour créer et gérer des niveaux de journalisation personnalisés, dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cliquez avec le bouton droit sur la base de données SSISDB, sélectionnez **Niveau de journalisation personnalisé** pour ouvrir la boîte de dialogue **Gestion du niveau de journalisation personnalisée** . La liste **Niveaux de journalisation personnalisés** contient tous les niveaux de journalisation personnalisés existants.  
  
2.  Pour **créer** un niveau de journalisation personnalisé, cliquez sur **Créer**, puis indiquez un nom et une description. Dans les onglets **Statistiques** et **Événements** , sélectionnez les statistiques et les événements que vous souhaitez collecter. Dans l’onglet **Événements** , vous pouvez également sélectionner l’option **Inclure le contexte** pour des événements individuels. Ensuite, cliquez sur **Enregistrer**.  
  
3.  Pour **mettre à jour** un niveau de journalisation personnalisé existant, sélectionnez-le dans la liste, reconfigurez-le, puis cliquez sur **Enregistrer**.  
  
4.  Pour **supprimer** un niveau de journalisation personnalisé existant, sélectionnez-le dans la liste, puis cliquez sur **Supprimer**.  
  
 **Autorisations des niveaux de journalisation personnalisés.**  
  
-   Tous les utilisateurs de la base de données SSISDB peuvent voir les niveaux de journalisation personnalisés et sélectionner un niveau de journalisation personnalisé lorsqu'ils exécutent des packages.  
  
-   Seuls les utilisateurs avec un rôle ssis_admin ou sysadmin peuvent créer, mettre à jour ou supprimer des niveaux de journalisation personnalisés.  

## <a name="custom-messages-for-logging"></a><a name="custom_messages"></a> Custom Messages for Logging
SQL Server Integration Services fournit un ensemble complet d’événements personnalisés permettant d’écrire des entrées de journal pour des packages et bon nombre de tâches. Vous pouvez utiliser ces entrées pour enregistrer des informations détaillées sur l'avancement, les résultats et les problèmes d'exécution en enregistrant des événements prédéfinis ou des messages définis par l'utilisateur en vue d'une analyse ultérieure. Vous pouvez ainsi enregistrer l'heure de début et de fin d'une insertion en bloc pour identifier des problèmes de performances lors de l'exécution du package.  
  
 Les entrées de journal personnalisées constituent un ensemble qui se distingue de l'ensemble des événements de journalisation standard, disponibles pour les packages et tous les conteneurs et tâches. Ces entrées sont conçues pour capturer des informations utiles sur une tâche spécifique d'un package. Par exemple, l'une des entrées de journal personnalisées pour la tâche d'exécution de requêtes SQL consigne l'instruction SQL que la tâche exécute dans le journal.  
  
 Toutes les entrées de journal contiennent des informations de date et d'heure, y compris les entrées qui sont écrites automatiquement au début et à la fin d'un package. La plupart des événements de journal écrivent plusieurs entrées dans le journal. C'est généralement le cas lorsque l'événement comprend plusieurs phases. Par exemple, l’événement du journal **ExecuteSQLExecutingQuery** consigne trois entrées : la première une fois que la tâche a acquis une connexion à la base de données, la seconde une fois que la tâche a commencé à préparer l’instruction SQL et la troisième à la fin de l’exécution de l’instruction SQL.  
  
 Les objets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] suivants possèdent des entrées de journal personnalisées :  
  
 [Package](#Package)  
  
 [Tâche d’insertion en bloc](#BulkInsert)  
  
 [tâche de flux de données](#DataFlow)  
  
 [Tâche d'exécution DTS 2000](#ExecuteDTS200)  
  
 [Tâche d’exécution de processus](#ExecuteProcess)  
  
 [Tache d’exécution de requêtes SQL](#ExecuteSQL)  
  
 [Tâches du système de fichiers](#FileSystem)  
  
 [Tâche FTP](#FTP)  
  
 [Tâche MSMQ](#MessageQueue)  
  
 [Tâche de script](#Script)  
  
 [Tache Envoyer un message](#SendMail)  
  
 [Tâche de transfert de bases de données](#TransferDatabase)  
  
 [Tâche de transfert de messages d’erreur](#TransferErrorMessages)  
  
 [Tâche de transfert de travaux](#TransferJobs)  
  
 [Tâche de transfert de connexions](#TransferLogins)  
  
 [Tâche de transfert de procédures stockées de master](#TransferMasterStoredProcedures)  
  
 [Tâche de transfert d’objets SQL Server](#TransferSQLServerObjects)  
  
 [Tâche de services Web](#WebServices)  
  
 [Tâche Lecteur de données WMI](#WMIDataReader)  
  
 [Tâche Observateur d’événement WMI](#WMIEventWatcher)  
  
 [Tâche XML](#XML)  
  
### <a name="log-entries"></a>Entrées du journal  
  
####  <a name="package"></a><a name="Package"></a> Package  
 Le tableau suivant répertorie les entrées de journal personnalisées pour les packages.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**PackageStart**|Indique que le package a commencé à s'exécuter. Cette entrée de journal est automatiquement écrite au journal. Vous ne pouvez pas l'exclure.|  
|**PackageEnd**|Indique que le package est terminé. Cette entrée de journal est automatiquement écrite au journal. Vous ne pouvez pas l'exclure.|  
|**Diagnostic**|Fournit des informations sur la configuration système qui affecte l'exécution du package, notamment le nombre d'exécutables pouvant s'exécuter simultanément.<br /><br /> L’entrée de journal **Diagnostic** inclut également des entrées avant et après les appels effectués auprès des fournisseurs de données externes.|  
  
####  <a name="bulk-insert-task"></a><a name="BulkInsert"></a> Tâche d'insertion en bloc  
 Le tableau suivant répertorie les entrées de journal personnalisées de la tâche d'insertion en bloc.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**DTSBulkInsertTaskBegin**|Indique que l'insertion en bloc a commencé.|  
|**DTSBulkInsertTaskEnd**|Indique que l'insertion en bloc est terminée.|  
|**DTSBulkInsertTaskInfos**|Fournit des informations détaillées concernant la tâche.|  
  
####  <a name="data-flow-task"></a><a name="DataFlow"></a> Data Flow Task  
 Le tableau suivant répertorie les entrées de journal personnalisées pour la tâche de flux de données.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**BufferSizeTuning**|Indique que la tâche de flux de données a modifié la taille du tampon. L'entrée de journal décrit les raisons de cette modification de taille et indique la nouvelle taille temporaire du tampon.|  
|**OnPipelinePostEndOfRowset**|Indique qu’un composant a reçu son signal de fin d’ensemble de lignes, qui est défini par le dernier appel de la méthode **ProcessInput** . Une entrée est écrite pour chaque composant du flux de données qui traite l'entrée. L'entrée inclut le nom du composant.|  
|**OnPipelinePostPrimeOutput**|Indique que le composant a terminé son dernier appel de la méthode **PrimeOutput** . Selon le flux de données, plusieurs entrées de journal peuvent être écrites. Si le composant est une source, cela signifie que le composant a terminé le traitement des lignes.|  
|**OnPipelinePreEndOfRowset**|Indique qu’un composant est sur le point de recevoir son signal de fin d’ensemble de lignes, qui est défini par le dernier appel de la méthode **ProcessInput** . Une entrée est écrite pour chaque composant du flux de données qui traite l'entrée. L'entrée inclut le nom du composant.|  
|**OnPipelinePrePrimeOutput**|Indique que le composant est sur le point de recevoir son appel de la méthode **PrimeOutput** . Selon le flux de données, plusieurs entrées de journal peuvent être écrites.|  
|**OnPipelineRowsSent**|Indique le nombre de lignes fournies à une entrée de composant par un appel de la méthode **ProcessInput** . L'entrée du journal inclut le nom du composant.|  
|**PipelineBufferLeak**|Donne des informations sur tout composant qui maintient l'activité des tampons après la fermeture du gestionnaire de tampons. Cela signifie que des ressources des tampons n'ont pas été libérées et qu'elles peuvent provoquer des fuites de mémoire. L'entrée du journal fournit le nom du composant et l'ID du tampon.|  
|**PipelineExecutionPlan**|Indique le plan d'exécution du flux de données. L'entrée du journal donne des informations sur la façon dont les tampons sont envoyés aux composants. Ces informations, conjuguées à l'entrée PipelineExecutionTrees, décrivent ce qui se passe dans la tâche.|  
|**PipelineExecutionTrees**|Indique les arborescences d'exécution de la disposition du flux de données. Le planificateur du moteur du flux de données utilise les arborescences pour construire le plan d’exécution du flux de données.|  
|**PipelineInitialization**|Donne des informations d'initialisation relatives à la tâche. Ces informations incluent les répertoires à utiliser pour le stockage temporaire des données blob, la taille par défaut de la mémoire tampon, ainsi que le nombre de lignes contenues dans une mémoire tampon. Selon la configuration de la tâche de flux de données, plusieurs entrées de journal peuvent être écrites.|  
  
####  <a name="execute-dts-2000-task"></a><a name="ExecuteDTS200"></a> Tâche d'exécution DTS 2000  
 Le tableau suivant répertorie les entrées de journal personnalisées pour la tâche d'exécution DTS 2000.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**ExecuteDTS80PackageTaskBegin**|Indique que la tâche a commencé l'exécution d'un package DTS 2000.|  
|**ExecuteDTS80PackageTaskEnd**|Indique que la tâche est terminée.<br /><br /> Remarque : il est possible que le package DTS 2000 continue à s’exécuter à la fin de la tâche.|  
|**ExecuteDTS80PackageTaskTaskInfo**|Fournit des informations détaillées concernant la tâche.|  
|**ExecuteDTS80PackageTaskTaskResult**|Indique le résultat d'exécution du package DTS 2000 que la tâche a exécuté.|  
  
####  <a name="execute-process-task"></a><a name="ExecuteProcess"></a> Tâche d'exécution de processus  
 Le tableau suivant répertorie les entrées de journal personnalisées pour la tâche d'exécution de processus.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**ExecuteProcessExecutingProcess**|Donne des informations sur le processus d'exécution de l'exécutable dont est chargé la tâche.<br /><br /> Deux entrées de journal sont écrites. La première contient des informations sur le nom et l'emplacement de l'exécutable que la tâche exécute, tandis que la seconde enregistre la sortie de l'exécutable.|  
|**ExecuteProcessVariableRouting**|Fournit des informations sur les variables qui doivent être acheminées vers l'entrée et les sorties de l'exécutable. Les entrées du journal sont écrites pour stdin (l'entrée), stdout (la sortie) et stderr (la sortie des erreurs).|  
  
####  <a name="execute-sql-task"></a><a name="ExecuteSQL"></a> Tache d'exécution de requêtes SQL  
 Le tableau suivant décrit les entrées de journal personnalisées pour la tâche d'exécution SQL.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**ExecuteSQLExecutingQuery**|Fournit des informations sur les phases d'exécution de l'instruction SQL. Des entrées de journal sont écrites lorsque la tâche acquiert la connexion à la base de données, lorsqu'elle commence à préparer l'instruction SQL et à la fin de l'exécution de l'instruction SQL. L'entrée de journal concernant la phase de préparation inclut l'instruction SQL que la tâche utilise.|  
  
####  <a name="file-system-task"></a><a name="FileSystem"></a> Tâches du système de fichiers  
 Le tableau suivant décrit l'entrée de journal personnalisée pour la tâche de système de fichiers.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**FileSystemOperation**|Indique l'opération que la tâche effectue. L'entrée de journal est écrite au démarrage de l'opération du système de fichiers et inclut des informations sur la source et la destination.|  
  
####  <a name="ftp-task"></a><a name="FTP"></a> Tâche FTP  
 Le tableau suivant répertorie les entrées de journal personnalisées pour la tâche FTP.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**FTPConnectingToServer**|Indique que la tâche a lancé une connexion au serveur FTP.|  
|**FTPOperation**|Indique le démarrage et le type d'une opération FTP effectuée par la tâche.|  
  
####  <a name="message-queue-task"></a><a name="MessageQueue"></a> Tâche MSMQ  
 Le tableau suivant répertorie les entrées de journal personnalisées de la tâche MSMQ.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**MSMQAfterOpen**|Indique que la tâche a fini d'ouvrir la file d'attente des messages.|  
|**MSMQBeforeOpen**|Indique que la tâche a commencé l'ouverture de la file d'attente des messages.|  
|**MSMQBeginReceive**|Indique que la tâche a commencé la réception d'un message.|  
|**MSMQBeginSend**|Indique que la tâche a commencé l'envoi d'un message.|  
|**MSMQEndReceive**|Indique que la tâche a terminé la réception d'un message.|  
|**MSMQEndSend**|Indique que la tâche a terminé l’envoi d’un message.|  
|**MSMQTaskInfo**|Fournit des informations détaillées concernant la tâche.|  
|**MSMQTaskTimeOut**|Indique que le délai de la tâche a expiré.|  
  
####  <a name="script-task"></a><a name="Script"></a> Tâche de script  
 Le tableau suivant décrit l'entrée de journal personnalisée pour la tâche de script.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**ScriptTaskLogEntry**|Indique les résultats de l'implémentation de la journalisation dans le script. Une entrée de journal est écrite pour chaque appel de la méthode **Log** de l’objet **Dts** . L'entrée est écrite à l'exécution du code. Pour plus d’informations, consultez [Journalisation dans la tâche de script](../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md).|  
  
####  <a name="send-mail-task"></a><a name="SendMail"></a> Tâche Envoyer un message  
 Le tableau suivant répertorie les entrées de journal personnalisées pour la tâche Envoyer un message.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**SendMailTaskBegin**|Indique que la tâche a commencé l'envoi d'un message électronique.|  
|**SendMailTaskEnd**|Indique que la tâche a terminé l'envoi d'un message électronique.|  
|**SendMailTaskInfo**|Fournit des informations détaillées concernant la tâche.|  
  
####  <a name="transfer-database-task"></a><a name="TransferDatabase"></a> Tâche de transfert de bases de données  
 Le tableau suivant répertorie les entrées de journal personnalisées pour la tâche de transfert de bases de données.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**SourceDB**|Spécifie la base de données que la tâche a copiée.|  
|**SourceSQLServer**|Spécifie l'ordinateur à partir duquel la base de données a été copiée.|  
  
####  <a name="transfer-error-messages-task"></a><a name="TransferErrorMessages"></a> Tâche de transfert de messages d'erreur  
 Le tableau suivant répertorie les entrées de journal personnalisées pour la tâche de transfert de messages d'erreur.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**TransferErrorMessagesTaskFinishedTransferringObjects**|Indique que la tâche a terminé le transfert des messages d'erreur.|  
|**TransferErrorMessagesTaskStartTransferringObjects**|Indique que la tâche a commencé le transfert des messages d'erreur.|  
  
####  <a name="transfer-jobs-task"></a><a name="TransferJobs"></a> Tâche de transfert de travaux  
 Le tableau suivant répertorie les entrées de journal personnalisées pour la tâche de transfert de travaux.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**TransferJobsTaskFinishedTransferringObjects**|Indique que la tâche a terminé le transfert des travaux de l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**TransferJobsTaskStartTransferringObjects**|Indique que la tâche a commencé le transfert des travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
  
####  <a name="transfer-logins-task"></a><a name="TransferLogins"></a> Tâche de transfert de connexions  
 Le tableau suivant répertorie les entrées de journal personnalisées pour la tâche de transfert de connexions.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**TransferLoginsTaskFinishedTransferringObjects**|Indique que la tâche a terminé le transfert des connexions.|  
|**TransferLoginsTaskStartTransferringObjects**|Indique que la tâche a commencé le transfert des connexions.|  
  
####  <a name="transfer-master-stored-procedures-task"></a><a name="TransferMasterStoredProcedures"></a> Tâche de transfert de procédures stockées de master  
 Le tableau suivant répertorie les entrées de journal personnalisées pour la tâche de transfert de procédures stockées de master.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**TransferStoredProceduresTaskFinishedTransferringObjects**|Indique que la tâche a terminé le transfert des procédures stockées définies par l’utilisateur qui existent dans la base de données **master** .|  
|**TransferStoredProceduresTaskStartTransferringObjects**|Indique que la tâche a commencé le transfert des procédures stockées définies par l’utilisateur qui existent dans la base de données **master** .|  
  
####  <a name="transfer-sql-server-objects-task"></a><a name="TransferSQLServerObjects"></a> Tâche de transfert d'objets SQL Server  
 Le tableau suivant répertorie les entrées de journal personnalisées pour la tâche de transfert d'objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**TransferSqlServerObjectsTaskFinishedTransferringObjects**|Indique que la tâche a terminé le transfert des objets de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**TransferSqlServerObjectsTaskStartTransferringObjects**|Indique que la tâche a commencé le transfert des objets de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
  
####  <a name="web-services-task"></a><a name="WebServices"></a> Tâche de services Web  
 Le tableau suivant répertorie les entrées de journal personnalisées pour la tâche de services Web.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**WSTaskBegin**|La tâche a commencé à accéder à un service Web.|  
|**WSTaskEnd**|La tâche a terminé une méthode de service Web.|  
|**WSTaskInfo**|Donne des informations détaillées relatives à la tâche.|  
  
####  <a name="wmi-data-reader-task"></a><a name="WMIDataReader"></a> Tâche Lecteur de données WMI  
 Le tableau suivant répertorie les entrées de journal personnalisées de la tâche Lecteur de données WMI.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**WMIDataReaderGettingWMIData**|Indique que la tâche a commencé la lecture des données WMI.|  
|**WMIDataReaderOperation**|Indique la requête WQL que la tâche a exécutée.|  
  
####  <a name="wmi-event-watcher-task"></a><a name="WMIEventWatcher"></a> Tâche Observateur d'événement WMI  
 Le tableau suivant répertorie les entrées de journal personnalisées de la tâche Observateur d'événement WMI.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**WMIEventWatcherEventOccurred**|Indique que l'événement surveillé par la tâche s'est produit.|  
|**WMIEventWatcherTimedout**|Indique que le délai de la tâche a expiré.|  
|**WMIEventWatcherWatchingForWMIEvents**|Indique que la tâche a commencé l'exécution de la requête WQL. L'entrée inclut la requête.|  
  
####  <a name="xml-task"></a><a name="XML"></a> Tâche XML  
 Le tableau suivant décrit l'entrée de journal personnalisée de la tâche XML.  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**XMLOperation**|Fournit des informations sur l'opération que la tâche effectue.|  

## <a name="related-tasks"></a>Tâches associées  
 La liste suivante contient des liens vers les rubriques qui indiquent comment effectuer les tâches relatives à la fonctionnalité de journalisation.  
  
-   [Événements journalisés par un package Integration Services](../../integration-services/performance/events-logged-by-an-integration-services-package.md)  
  
## <a name="related-content"></a>Contenu associé  
 [Outil DTLoggedExec pour un enregistrement complet et détaillé (projet CodePlex)](https://go.microsoft.com/fwlink/?LinkId=150579)  
