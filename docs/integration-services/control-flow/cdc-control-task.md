---
description: Tâche de contrôle de capture de données modifiées
title: Contrôle de capture de données modifiées, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.designer.cdccontroltask.f1
- sql13.ssis.designer.cdccontroltask.config.f1
ms.assetid: 6404dc7f-550c-47cc-b901-c072742f430a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 492a0cd4b510b91d1f5f17ad04de54b005502bba
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049112"
---
# <a name="cdc-control-task"></a>Tâche de contrôle de capture de données modifiées

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche de contrôle de capture de données modifiées permet de contrôler le cycle de vie des packages de capture de données modifiées. Elle gère la synchronisation des package de capture de données modifiées avec le package de charge initiale et la gestion des plages de numéros séquentiels dans le journal (NSE) qui sont traités lors de l'exécution d'un package de capture de données modifiées. En outre, la tâche de contrôle de capture de données modifiées traite les scénarios d'erreur et la récupération.  
  
 La tâche de contrôle de capture de données modifiées gère l'état du package de capture de données modifiées dans une variable de package SSIS et peut également le conserver de manière permanente dans une table de base de données afin qu'il soit conservé d'une activation de package à l'autre et entre plusieurs packages qui exécutent ensemble un processus de capture de données modifiées commun (par exemple, une tâche peut être responsable du chargement initial et l'autre des mises à jour du flux progressif).  
  
 La tâche de contrôle de capture de données modifiées prend en charge deux groupes d'opérations. Un groupe gère la synchronisation de la charge initiale et le traitement des modifications, tandis que l'autre gère la plage de traitement des modifications des NSE pour une exécution d'un package de source de données modifiées et garde une trace des actions correctement traitées.  
  
 Les opérations suivantes gèrent la synchronisation de la charge initiale et le traitement des modifications :  
  
|Opération|Description|  
|---------------|-----------------|  
|ResetCdcState|Cette opération est utilisée pour réinitialiser l'état permanent de capture de données modifiées associé au contexte de capture de données modifiées actuel. Une fois cette opération effectuée, le numéro LSN maximal actuel de la table d’horodatage des LSN `sys.fn_cdc_get_max_lsn` devient le début de la plage de traitement suivante. Cette opération requiert une connexion à la base de données source.|  
|MarkInitialLoadStart|Cette opération est utilisée au début d'un package de charge initiale pour enregistrer le NSE actuel dans la base de données source avant que le package de charge initiale commence à lire les tables sources. Cette opération requiert une connexion à la base de données source pour appeler `sys.fn_cdc_get_max_lsn`.<br /><br /> Si vous sélectionnez MarkInitialLoadStart quand vous travaillez sur la capture de données modifiées [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (autrement dit, hors d’Oracle), l’utilisateur spécifié dans le gestionnaire de connexions doit être db_owner ou sysadmin.|  
|MarkInitialLoadEnd|Cette opération est utilisée à la fin d'un package de charge initiale pour enregistrer le NSE actuel dans la base de données source une fois que le package de charge initiale a fini de lire les tables sources. Ce NSE est déterminé en enregistrant l'heure à laquelle cette opération s'est produite, puis en interrogeant la table de mappage `cdc.lsn_time_`dans la base de données de capture de données modifiées afin de rechercher une modification survenue après cette heure.<br /><br /> Si vous sélectionnez MarkInitialLoadEnd quand vous travaillez sur la capture de données modifiées [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (autrement dit, hors d’Oracle), l’utilisateur spécifié dans le gestionnaire de connexions doit être db_owner ou sysadmin.|  
|MarkCdcStart|Cette opération est utilisée lorsque la charge initiale est effectuée à partir d'une base de données par instantané. Dans ce cas, le traitement des modifications doit démarrer immédiatement après le NSE de l'instantané. Vous pouvez spécifier le nom de la base de données par instantané à utiliser et la tâche de contrôle de capture de données modifiées qui interroge [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] pour connaître le NSE de l'instantané. Vous avez également la possibilité de spécifier directement le NSE de l'instantané.<br /><br /> Si vous sélectionnez MarkCdcStart quand vous travaillez sur la capture de données modifiées [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (autrement dit, hors d’Oracle), l’utilisateur spécifié dans le gestionnaire de connexions doit être db_owner ou sysadmin.|  
  
 Les opérations suivantes sont utilisées pour gérer la plage de traitement :  
  
|Opération|Description|  
|---------------|-----------------|  
|GetProcessingRange|Cette opération est utilisée avant d'appeler le flux de données qui utilise le flux de données de la source CDC. Elle établit une plage de numéros LSN que le flux de données de la source CDC lit lorsqu'il est appelé. La plage est stockée dans une variable de package SSIS qui est utilisée par la source CDC pendant le traitement du flux de données.<br /><br /> Pour plus d’informations sur les états stockés, consultez [Définir une variable d’état](../../integration-services/data-flow/define-a-state-variable.md).|  
|MarkProcessedRange|Cette opération est exécutée après que chaque exécution de la capture de données modifiées (une fois le flux de données de capture de données modifiées terminé avec succès) pour consigner le dernier NSE qui a été entièrement traité dans le cadre de l'exécution de la capture de données modifiées. Lors de la prochaine exécution de GetProcessingRange, cette position constitue le début de la plage de traitement.|  
  
## <a name="handling-cdc-state-persistency"></a>Gestion de la permanence de l'état de capture de données modifiées  
 La tâche de contrôle de capture de données modifiées conserve un état permanent entre les activations. Les informations stockées dans l'état de capture de données modifiées sont utilisées pour déterminer et gérer la plage de traitement pour le package de capture de données modifiées et pour détecter les conditions d'erreur. L'état permanent est stocké sous forme de chaîne. Pour plus d’informations, consultez [Définir une variable d’état](../../integration-services/data-flow/define-a-state-variable.md).  
  
 La tâche de contrôle de capture de données modifiées prend en charge deux types de permanence d'état :  
  
-   Permanence d'état manuelle : dans ce cas, la tâche de contrôle de capture de données modifiées gère l'état stocké dans une variable de package, mais le développeur de package doit lire la variable dans un stockage permanent avant d'appeler le contrôle de capture de données modifiées, puis la réécrire dans ce stockage permanent après le dernier appel du contrôle de capture de données modifiées et une fois l'exécution de la capture de données modifiées terminée.  
  
-   Permanence d'état automatique : l'état de capture de données modifiées est stocké dans une table dans une base de données. L’état est stocké sous un nom fourni par la propriété **StateName** dans une table nommée de la propriété **Table à utiliser pour le stockage de l’état** , qui se trouve dans un gestionnaire de connexions sélectionné pour le stockage de l’état. La valeur par défaut est le gestionnaire de connexions source, mais la pratique courante consiste à utiliser le gestionnaire de connexions cible. La tâche de contrôle de capture de données modifiées met à jour la valeur d'état dans la table d'état et celle-ci est validée dans le cadre de la transaction en cours.  
  
## <a name="error-handling"></a>Gestion des erreurs  
 La tâche de contrôle de capture de données modifiées peut signaler une erreur dans les conditions suivantes :  
  
-   Elle ne parvient pas à lire l'état de capture de données modifiées permanent ou la mise à jour de l'état permanent échoue.  
  
-   Elle ne parvient pas à lire les informations du NSE actuel dans la base de données source.  
  
-   La lecture de l'état de capture de données modifiées n'est pas cohérente.  
  
 Dans toutes ces situations, la tâche de contrôle de capture de données modifiées signale une erreur qui peut être gérée de la manière dont SSIS gère habituellement les erreurs de flux de contrôle.  
  
 La tâche de contrôle de capture de données modifiées peut également signaler un avertissement lorsque l'opération Obtenir la plage de traitement est appelée directement après une autre opération Obtenir la plage de traitement sans que Marquer la plage traitée soit appelé. Ceci indique que l'exécution précédente a échoué ou qu'un autre package de capture de données modifiées s'exécute peut-être avec le même nom d'état de capture de données modifiées.  
  
## <a name="configuring-the-cdc-control-task"></a>Configuration de la tâche de contrôle de capture de données modifiées  
 Vous pouvez définir les propriétés par le biais du concepteur SSIS ou par programmation.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Propriétés personnalisées de la tâche de contrôle de capture de données modifiées](../../integration-services/control-flow/cdc-control-task-custom-properties.md)  
  
## <a name="related-tasks"></a>Tâches associées  
 [Définir une variable d’état](../../integration-services/data-flow/define-a-state-variable.md)  
  
## <a name="related-content"></a>Contenu associé  
  
-   Article technique, [Installation du service de capture de données modifiées Microsoft SQL Server 2012 pour Oracle par Attunity](https://go.microsoft.com/fwlink/?LinkId=252958), sur social.technet.microsoft.com.  
  
-   Article technique, [Résolution des problèmes de configuration dans le service de capture de données modifiées Microsoft SQL Server pour Oracle par Attunity](https://go.microsoft.com/fwlink/?LinkId=252960), sur social.technet.microsoft.com.  
  
-   Article technique, [Dépannage des erreurs d'instance du service de capture de données modifiées Microsoft SQL Server pour Oracle par Attunity](https://go.microsoft.com/fwlink/?LinkId=252961), sur social.technet.microsoft.com.  
  
## <a name="cdc-control-task-editor"></a>Éditeur de tâche de contrôle CDC
  Utilisez la boîte de dialogue **Éditeur de tâche de contrôle CDC** pour configurer la tâche de contrôle CDC. La configuration de la tâche de contrôle CDC inclut la définition d'une connexion à la base de données CDC, l'opération de la tâche CDC et des informations de gestion d'état.  
  
 Pour en savoir plus sur la tâche de contrôle CDC, consultez [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md).  
  
 **Pour ouvrir l'Éditeur de tâche de contrôle CDC**  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], ouvrez le package [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] doté de la tâche de contrôle CDC.  
  
2.  Sous l’onglet **Flux de contrôle** , double-cliquez sur la tâche de contrôle CDC.  
  
### <a name="options"></a>Options  
 **Gestionnaire de connexions ADO.NET de base de données CDC SQL Server**  
 Sélectionnez un gestionnaire de connexions existant dans la liste ou cliquez sur **Nouveau** pour créer une connexion. La connexion doit être établie avec une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] activée pour la capture de données modifiées et dans laquelle la table de modifications sélectionnée est localisée.  
  
 **Opération de contrôle de capture de données modifiées**  
 Sélectionnez l'opération à exécuter pour cette tâche. Toutes les opérations utilisent la variable d'état qui est stockée dans une variable de package SSIS qui stocke l'état et le passe entre les différents composants du package.  
  
-   **Marquer le début de la charge initiale**: cette opération est utilisée en exécutant une charge initiale d'une base de données active sans instantané. Elle est invoquée au début d'un package de chargement initial pour enregistrer le numéro LSN actuel dans la base de données source avant que le package de chargement initial commence à lire les tables sources. Cela requiert une connexion à la base de données source.  
  
     Si vous sélectionnez **Marquer le début de la charge initiale** quand vous travaillez sur la capture de données modifiées [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (autrement dit, hors d’Oracle) l’utilisateur spécifié dans le gestionnaire de connexions doit être  **db_owner** ou **sysadmin**.  
  
-   **Marquer la fin de la charge initiale**: cette opération est utilisée en exécutant une charge initiale d'une base de données active sans instantané. Elle est invoquée à la fin d'un package de chargement initial pour enregistrer le numéro LSN actuel dans la base de données source une fois que le package de chargement initial a fini de lire les tables sources. Ce numéro LSN est déterminé en enregistrant l'heure à laquelle cette opération s'est produite, puis en interrogeant la table de mappage `cdc.lsn_time_`dans la base de données CDC afin de rechercher une modification survenue après cette heure.  
  
     Si vous sélectionnez **Marquer la fin de la charge initiale** quand vous travaillez sur la capture de données modifiées [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (autrement dit, hors d’Oracle) l’utilisateur spécifié dans le gestionnaire de connexions doit être  **db_owner** ou **sysadmin**.  
  
-   **Marquer le début CDC** : cette opération est utilisée lorsque la charge initiale est effectuée à partir d'un fichier de base de données d'instantanés ou d'une base de données inactive. Elle est appelée à n'importe quel stade du package de charge initiale. L'opération accepte un paramètre qui peut être un numéro LSN d'instantané, un nom de base de données d'instantanés (de laquelle le numéro LSN d'instantané dérive automatiquement) ou qui peut être laissé vide, auquel cas le numéro LSN de la base de données actuelle est utilisé comme dernier numéro LSN pour le package de traitement des modifications.  
  
     Cette opération est utilisée à la place des opérations Marquer le début/la fin de la charge initiale.  
  
     Si vous sélectionnez **Marquer le début CDC** quand vous travaillez sur la capture de données modifiées [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (autrement dit, hors d’Oracle) l’utilisateur spécifié dans le gestionnaire de connexions doit être  **db_owner** ou **sysadmin**.  
  
-   **Get processing range**: cette opération est utilisée dans un package de traitement des modifications avant d'appeler le flux de données qui utilise le flux de données source CDC. Elle établit une plage de numéros LSN que le flux de données de la source CDC lit lorsqu'il est appelé. La plage est stockée dans une variable de package SSIS qui est utilisée par la source CDC pendant le traitement du flux de données.  
  
     Pour plus d’informations sur les états CDC stockés, consultez [Définir une variable d’état](../../integration-services/data-flow/define-a-state-variable.md).  
  
-   **Marquer la plage traitée**: cette opération est utilisée dans un package de traitement des modifications à la fin d’une exécution CDC (après que le flux de données CDC s’est terminé correctement) pour inscrire le dernier numéro LSN qui était complètement traité dans l’exécution CDC. Lors de la prochaine exécution de `GetProcessingRange` , cette position détermine le début de la prochaine plage de traitement.  
  
-   **Réinitialiser l'état CDC**: cette opération est utilisée pour réinitialiser l'état de capture de données modifiées (CDC) permanent associé au contexte CDC actuel. Une fois cette opération effectuée, le numéro LSN maximal actuel de la table d’horodatage des LSN `sys.fn_cdc_get_max_lsn` devient le début de la plage de traitement suivante. Cette opération requiert une connexion à la base de données source.  
  
     Cette opération est notamment utilisée lorsque vous souhaitez traiter uniquement les enregistrements de modifications récents et ignorer tous les anciens enregistrements de modifications.  
  
 **Variable contenant l'état CDC**  
 Sélectionnez la variable de package SSIS qui stocke les informations d'état de l'opération de tâche. Vous devez définir une variable avant de commencer. Si vous sélectionnez **Persistance d'état automatique**, la variable d'état est chargée et enregistrée automatiquement.  
  
 Pour plus d’informations sur la définition de la variable d’état, consultez [Définir une variable d’état](../../integration-services/data-flow/define-a-state-variable.md).  
  
 **Numéro séquentiel dans le journal (LSN) SQL Server pour démarrer le nom CDC/instantané :**  
 Entrez le numéro LSN de la base de données source ou de la base de données d'instantanés à partir de laquelle la charge initiale est effectuée pour déterminer le début de la capture de données modifiées. Le numéro est disponible uniquement si **Opération de contrôle CDC** est défini sur **Marquer le début CDC**.  
  
 Pour plus d'informations sur ces opérations, consultez [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md).  
  
 **Stockage automatique de l'état dans une table de base de données**  
 Activez cette case à cocher pour que la tâche de contrôle CDC gère automatiquement le chargement et le stockage de l'état CDC dans une table d'états contenue dans la base de données spécifiée. Si cette case n'est pas sélectionnée, le développeur doit charger l'état CDC au démarrage du package et l'enregistrer dès qu'il change.  
  
 **Gestionnaire de connexions de la base de données où l'état est stocké**  
 Sélectionnez un gestionnaire de connexions ADO.NET existant dans la liste ou cliquez sur Nouveau pour créer une nouvelle connexion. La connexion concerne une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contient la table State. La table State contient les informations d'état.  
  
 Ces informations sont disponibles uniquement si vous avez sélectionné **Persistance d'état automatique** et il s'agit d'un paramètre obligatoire.  
  
 **Table à utiliser pour stocker l'état**  
 Tapez le nom de la table d'état à utiliser pour stocker l'état CDC. La table spécifiée doit être composée de deux colonnes appelées **name** et **state** avec le type de données **varchar (256)**.  
  
 Vous pouvez éventuellement sélectionner **Nouveau** pour obtenir un script SQL qui génère une nouvelle table d'état avec les colonnes requises. Lorsque **Persistance d'état automatique** est sélectionné, le développeur doit créer une table d'état en fonction des spécifications ci-dessus.  
  
 Ces informations sont disponibles uniquement si vous avez sélectionné **Persistance d'état automatique** et il s'agit d'un paramètre obligatoire.  
  
 **Nom de l’état**  
 Entrez le nom à associer à l'état CDC persistant. La charge complète et les packages CDC qui fonctionnent avec le même contexte CDC auront un nom d'état commun. Ce nom est utilisé pour surveiller la ligne d'état dans la table d'état.  
  
