---
description: Tâche d'exécution de package
title: Exécuter le package, tâche | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executepackagetask.f1
- sql13.dts.designer.executepackagetask.package.f1
- sql13.dts.designer.executepackagetask.parameter.F1
- sql13.dts.designer.executepackagetask.general.f1
helpviewer_keywords:
- Execution Package task [Integration Services]
- child packages
- parent packages [Integration Services]
ms.assetid: 042d4ec0-0668-401c-bb3a-a25fe2602eac
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8251f2ff30fafff584b4f56fe93a9b7423ea412b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352086"
---
# <a name="execute-package-task"></a>Tâche d'exécution de package

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche d'exécution de package étend les fonctionnalités d'entreprise de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en permettant à des packages d'exécuter d'autres packages au sein d'un flux de travail.  
  
 Vous pouvez utiliser la tâche d'exécution de package aux fins suivantes :  
  
-   Division d'un flux de travail de package complexe. Cette tâche vous permet de diviser le flux de travail en plusieurs packages, qui sont plus faciles à lire, à tester et à maintenir. Par exemple, si vous chargez des données dans un schéma en étoile, vous pouvez créer un package distinct pour remplir chaque dimension et la table de faits.  
  
-   Réutilisation de parties de packages. D'autres packages peuvent réutiliser des parties d'un flux de travail de package. Par exemple, vous pouvez créer un module d'extraction des données qui peut être appelé depuis différents packages. Chaque package qui appelle le module d'extraction peut exécuter différentes opérations de purge, de filtrage ou d'agrégation sur les données.  
  
-   Regroupement des unités de travail. Les unités de travail peuvent être encapsulées dans des packages distincts et incluses sous forme de composants transactionnels au flux de travail d'un package parent. Par exemple, le package parent exécute les packages secondaires et, en fonction de la réussite ou de l'échec des packages secondaires, il valide ou annule la transaction.  
  
-   Contrôle de la sécurité des packages. Les auteurs de package ont besoin d'accéder à une seule partie d'une solution multipackage. En séparant un package en plusieurs packages, vous pouvez fournir un niveau de sécurité plus élevé ; en effet, vous pouvez permettre à un auteur d'accéder aux seuls packages appropriés.  
  
 Un package qui exécute d'autres packages est généralement appelé « package parent », tandis que les packages exécutés par un flux de travail parent sont appelés « packages enfants ».  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] comprend des tâches qui effectuent des opérations de flux de travail, telles que l'exécution de fichiers de commandes, d'exécutables et de packages. Pour plus d'informations, consultez [Execute Process Task](../../integration-services/control-flow/execute-process-task.md).  
  
## <a name="running-packages"></a>Exécution des packages  
 La tâche d'exécution de package peut exécuter des packages enfants contenus dans le même projet qui contient le package parent. Vous sélectionnez un package enfant d'un projet en définissant la propriété **ReferenceType** sur **Project Reference**, puis en définissant la propriété **PackageNameFromProjectReference** .  
  
> [!NOTE]  
>  L’option **ReferenceType** est en lecture seule et est définie sur **Référence externe** si le projet qui contient le package n’a pas été converti en modèle de déploiement de projet. [Déployer des projets et des packages Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
 La tâche d’exécution de package peut exécuter des packages stockés dans la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb et des packages stockés dans le système de fichiers. La tâche utilise un gestionnaire de connexions OLE DB pour se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou un gestionnaire de connexions de fichiers pour accéder au système de fichiers. Pour plus d'informations, consultez [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md) et [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
 La tâche d'exécution de package peut également exécuter un plan de maintenance de base de données, ce qui vous permet de gérer les packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] et les plans de maintenance de base de données dans la même solution [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Un plan de maintenance de base de données est similaire à un package [!INCLUDE[ssIS](../../includes/ssis-md.md)] , mais il ne peut inclure que des tâches de maintenance de base de données et est toujours stocké dans la base de données msdb.  
  
 Si vous choisissez un package stocké dans le système de fichiers, vous devez fournir le nom et l'emplacement du package. Le package peut se trouver à n'importe quel endroit dans le système de fichiers ; il n'est pas nécessaire qu'il figure dans le même dossier que le package parent.  
  
 Le package enfant peut être exécuté dans le processus du package parent ou dans son propre processus. L'exécution du package enfant dans son propre processus nécessite davantage de mémoire, mais elle offre plus de souplesse. Par exemple, si le processus enfant échoue, l'exécution du processus parent peut se poursuivre.  
  
 Dans d'autres situations, vous pouvez préférer que les packages parent et enfants échouent ensemble comme une même unité, ou bien éviter la charge de traitement supplémentaire d'un autre processus. Par exemple, si un processus enfant échoue et que les traitements ultérieurs du processus parent du package dépendent de la réussite du processus enfant, alors le package enfant doit s'exécuter dans le processus du package parent.  
  
 Par défaut, la propriété ExecuteOutOfProcess de la tâche d’exécution du package a la valeur **False**, et le package enfant s’exécute dans le même processus que le package parent. Si vous affectez la valeur **True** à cette propriété, le package enfant s'exécute dans un processus indépendant. Cela peut ralentir le lancement du package enfant. En outre, si vous affectez la valeur **True** à la propriété, vous ne pouvez pas déboguer le package dans une installation d’outils uniquement. Vous devez installer [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Pour plus d’informations, consultez [Installer Integration Services](../../integration-services/install-windows/install-integration-services.md).  
  
## <a name="extending-transactions"></a>Extension des transactions  
 La transaction que le package parent utilise peut être étendue au package enfant ; par conséquent, le travail réalisé par les deux packages peut être validé ou annulé. Par exemple, les insertions dans une base de données effectuées par le package parent peuvent être validées ou annulées, en fonction de celles réalisées par le package enfant, et vice versa. Pour plus d'informations, consultez [Inherited Transactions](../integration-services-transactions.md).  
  
## <a name="propagating-logging-details"></a>Propagation des détails de la journalisation  
 Le package enfant exécuté par la tâche d'exécution de package peut ou non être configuré de manière à utiliser la journalisation, mais il transfère toujours les détails du journal au package parent. Si la tâche d'exécution de package est configurée de manière à utiliser la journalisation, elle consigne les informations détaillées du journal à partir du package enfant. Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
## <a name="passing-values-to-child-packages"></a>Passage de valeurs à des packages enfants  
 Un package enfant utilise fréquemment des valeurs transmises par un autre package qui l'appelle, généralement son package parent. L'utilisation de valeurs issues d'un package parent est utile dans les scénarios suivants :  
  
-   Les parties d'un flux de travail plus volumineux sont affectées à différents packages. Par exemple, un package télécharge des données chaque nuit, les résume, affecte des valeurs de données de synthèse à des variables, puis les passe à un autre package qui les traite à son tour.  
  
-   Le package parent coordonne dynamiquement les tâches d'un package enfant. Par exemple, le package parent détermine le nombre de jours d'un mois en cours et affecte ce nombre à une variable, puis le package enfant effectue une tâche autant de fois que la valeur du nombre.  
  
-   Un package enfant nécessite l'accès à des données dérivées dynamiquement par le package parent. Par exemple, le package parent extrait des données d'une table et charge l'ensemble de lignes dans une variable, puis le package enfant effectue des opérations supplémentaires sur les données.  
  
 Vous pouvez utiliser les méthodes suivantes pour passer des valeurs à un package enfant :  
  
-   **Configurations de package**  
  
     [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contient un type de configuration, en l'occurrence la configuration Variable de package parent, qui permet de passer les valeurs du package parent au package enfant. La configuration est basée sur le package enfant et utilise une variable dans le package parent. La configuration est mappée à une variable ou à la propriété d'un objet du package enfant. La variable peut également être utilisée dans les scripts utilisés par la tâche de script ou le composant de script.  
  
-   **Paramètres**  
  
     Vous pouvez configurer la tâche d'exécution des packages pour mapper les variables ou paramètres de package parent (ou les paramètres du projet) aux paramètres de package enfant. Le projet doit utiliser le modèle de déploiement de projet et le package enfant doit être contenu dans le même projet qui contient le package parent. Pour plus d'informations, consultez [Execute Package Task Editor]().  
  
    > [!NOTE]  
    >  Si le paramètre de package enfant ne respecte pas la casse et s'il est mappé à un paramètre parent qui respecte la casse, le package enfant ne peut pas s'exécuter.  
    >   
    >  Les conditions de mappage suivantes sont prises en charge :  
    >   
    >  Le paramètre de package enfant qui respecte la casse est mappé à un paramètre parent qui respecte la casse.  
    >   
    >  Le paramètre de package enfant qui respecte la casse est mappé à un paramètre parent qui ne respecte pas la casse.  
    >   
    >  Le paramètre de package enfant qui ne respecte pas la casse est mappé à un paramètre parent qui ne respecte pas la casse.  
  
 La variable du package parent peut être définie dans l'étendue de la tâche d'exécution de package ou dans un conteneur parent tel que le package. Si plusieurs variables de même nom sont disponibles, la variable utilisée est celle définie dans l'étendue de la tâche d'exécution de package ou celle qui, du point de vue de l'étendue, est la plus proche de la tâche.  
  
 Pour plus d’informations, consultez [Utiliser les valeurs des variables et des paramètres dans un package enfant](../../integration-services/packages/legacy-package-deployment-ssis.md#child).  
  
### <a name="accessing-parent-package-variables"></a>Accès aux variables de package parent  
 Les packages enfants peuvent accéder à des variables de package parent à l'aide de la tâche de script. Quand vous entrez le nom de la variable de package parent sur la page **Script** dans l’**Éditeur de tâche de script**, n’incluez pas **Utilisateur :** dans le nom de la variable. Sinon, le package enfant ne localise pas la variable quand vous exécutez le package parent.  
  
## <a name="configuring-the-execute-package-task"></a>Configuration de la tâche d'exécution de package  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="configuring-the-execute-package-task-programmatically"></a>Configuration de la tâche d'exécution de package par programmation  
 Pour plus d'informations sur la définition par programme de ces propriétés, cliquez sur la rubrique suivante :  
  
-   [N:Microsoft.SqlServer.Dts.Tasks.ExecutePackageTask](https://technet.microsoft.com/library/microsoft.sqlserver.dts.tasks.executepackagetask\(v=sql.110\).aspx)  
  
## <a name="execute-package-task-editor"></a>Éditeur de tâche d'exécution de package
  Utilisez l'Éditeur de tâche d'exécution de package pour configurer la tâche d'exécution de package. La tâche d'exécution de package étend les fonctionnalités d'entreprise de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en permettant à des packages d'exécuter d'autres packages au sein d'un flux de travail.  
  
 **Que voulez-vous faire ?**  
  
-   [Ouvrir l'Éditeur de tâche d'exécution de package](#open)  
  
-   [Définir les options sur la page Général](#general)  
  
-   [Définir les options sur la page Package](#package)  
  
-   [Définir les options sur la page Liaisons de paramètre](#parameter)  
  
###  <a name="open-the-execute-package-task-editor"></a><a name="open"></a> Ouvrir l'Éditeur de tâche d'exécution de package  
  
1.  Ouvrez un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] dans [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] qui contient une tâche d'exécution de package.  
  
2.  Cliquez avec le bouton droit sur la tâche dans le Concepteur SSIS, puis cliquez sur **Modifier**.  
  
###  <a name="set-the-options-on-the-general-page"></a><a name="general"></a> Définir les options sur la page Général  
 **Nom**  
 Fournissez un nom unique pour la tâche d'exécution de package. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Entrez une description de la tâche d'exécution de package.  
  
###  <a name="set-the-options-on-the-package-page"></a><a name="package"></a> Définir les options sur la page Package  
 **ReferenceType**  
 Sélectionnez **Référence du projet** pour les packages enfants qui sont dans le projet. Sélectionnez **Référence externe** pour les packages enfants situés en dehors du package.  
  
> [!NOTE]  
>  L’option **ReferenceType** est en lecture seule et est définie sur **Référence externe** si le projet qui contient le package n’a pas été converti en modèle de déploiement de projet. [Déployer des projets et des packages Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
 **Mot de passe**  
 Si le package enfant s’avère protégé par un mot de passe, indiquez ce dernier ou cliquez sur le bouton représentant des points de suspension (...) afin de définir un nouveau mot de passe.  
  
 **ExecuteOutOfProcess**  
 Spécifiez si le package enfant s'exécute dans le processus du package parent ou dans un processus distinct. Par défaut, la propriété ExecuteOutOfProcess de la tâche d’exécution du package a la valeur **False**, et le package enfant s’exécute dans le même processus que le package parent. Si vous affectez la valeur **true** à cette propriété, le package enfant s'exécute dans un processus indépendant. Cela peut ralentir le lancement du package enfant. En outre, si vous avez défini la propriété sur **true**, vous ne pouvez pas déboguer le package dans une installation d’outils uniquement : vous devez installer le produit [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d’informations, consultez [Installer Integration Services](../../integration-services/install-windows/install-integration-services.md).  
  
#### <a name="referencetype-dynamic-options"></a>Options dynamiques de ReferenceType  
  
##### <a name="referencetype--external-reference"></a>ReferenceType = Référence externe  
 **Lieu**  
 Sélectionnez l'emplacement du package enfant. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**SQL Server**|Définissez l'emplacement d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**Système de fichiers**|Permet de définir l'emplacement du système de fichiers.|  
  
 **Connection**  
 Sélectionnez le type d’emplacement de stockage du package enfant.  
  
 **PackageNameReadOnly**  
 Permet d'afficher le nom du package.  
  
##### <a name="referencetype--project-reference"></a>ReferenceType = Référence du projet  
 **PackageNameFromProjectReference**  
 Sélectionnez un package contenu dans le projet, qui sera le package enfant.  
  
#### <a name="location-dynamic-options"></a>Options dynamiques pour la définition de l'emplacement  
  
##### <a name="location--sql-server"></a>Emplacement = SQL Server  
 **Connection**  
 Sélectionnez un gestionnaire de connexions OLE DB existant dans la liste ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md)  
  
 **PackageName**  
 Permet d’entrer le nom du package enfant ou de cliquer sur le bouton représenté par des points de suspension (...) pour atteindre le package.  
  
##### <a name="location--file-system"></a>Emplacement = Système de fichiers  
 **Connection**  
 Sélectionnez un gestionnaire de connexions de fichiers dans la liste ou cliquez sur \<**New connection...**> pour créer un gestionnaire de connexions.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md)  
  
 **PackageNameReadOnly**  
 Permet d'afficher le nom du package.  
  
###  <a name="set-the-options-on-the-parameter-bindings-page"></a><a name="parameter"></a> Définir les options sur la page Liaisons de paramètre  
 Vous pouvez passer des valeurs du package parent ou du projet au package enfant. Le projet doit utiliser le modèle de déploiement de projet et le package enfant doit être contenu dans le même projet qui contient le package parent.  
  
 Pour plus d’informations sur la conversion de projets en modèle de déploiement de projet, consultez [Déployer des projets et des packages Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  
  
 **Paramètre de package enfant**  
 Entrez ou sélectionnez un nom pour le paramètre de package enfant.  
  
 **Variable ou paramètre de liaison**  
 Sélectionnez le paramètre ou la variable contenant la valeur que vous souhaitez transmettre au package enfant.  
  
 **Ajouter**  
 Cliquez pour mapper un paramètre ou une variable à un paramètre de package enfant.  
  
 **Remove**  
 Cliquez pour supprimer un mappage entre un paramètre ou une variable et un paramètre de package enfant.  
  
