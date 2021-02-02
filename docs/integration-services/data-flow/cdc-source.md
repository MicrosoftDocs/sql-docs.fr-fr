---
description: Source CDC
title: Source CDC | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.designer.cdcsource.f1
- sql13.ssis.designer.cdcsource.connection.f1
- sql13.ssis.designer.cdcsource.columns.f1
- sql13.ssis.designer.cdcsource.errorhandling.f1
ms.assetid: 99775608-e177-44ed-bb44-aaccb0f4f327
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9c10e3f1aa1460f63f3fcd3e95bb95bef8e63065
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049207"
---
# <a name="cdc-source"></a>Source CDC

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La source CDC lit une plage de données modifiées dans les tables de modifications de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] et apporte les modifications en aval aux autres composants SSIS.  
  
 La plage de données modifiées lue par la source CDC est appelée « plage de traitement de capture de données modifiées » et est déterminée par la tâche de contrôle de capture de données modifiées exécutée avant le démarrage du flux de données actuel. La plage de traitement de capture de données modifiées est dérivée de la valeur d'une variable de package qui gère l'état du traitement de capture de données modifiées pour un groupe de tables.  
  
 La source CDC extrait les données d’une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide d’une table de base de données, d’une vue ou d’une instruction SQL.  
  
 La source CDC utilise les configurations suivantes :  
  
-   Un gestionnaire de connexions ADO.NET [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour accéder à la base de données CDC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations sur la configuration de la connexion à la source CDC, consultez [Éditeur de source CDC &#40;page Gestionnaire de connexions&#41;]().  
  
-   Une table pour laquelle la capture de données modifiées est activée.  
  
-   Le nom de l'instance de capture de la table sélectionnée (s'il en existe plusieurs).  
  
-   Le mode de traitement des modifications.  
  
-   Le nom de la variable de package d'état de capture de données modifiées à partir de laquelle la plage de traitement de capture de données modifiées est déterminée. La source CDC ne modifie pas cette variable.  
  
 Les données retournées par la source CDC sont les mêmes que celles retournées par les fonctions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CDC **cdc.fn_cdc_get_all_changes_\<capture-instance-name>** ou **cdc.fn_cdc_get_net_changes_\<capture-instance-name>** (si disponibles). Le seul ajout facultatif est la colonne **__$initial_processing** qui indique si la plage de traitement actuelle peut chevaucher une charge initiale de la table. Pour plus d’informations sur la traitement initial, consultez [Tâche de contrôle de capture de données modifiées](../../integration-services/control-flow/cdc-control-task.md).  
  
 La source CDC a une sortie normale et une sortie d'erreur.  
  
## <a name="error-handling"></a>Gestion des erreurs  
 la source CDC a une sortie d'erreur. La sortie d'erreur du composant contient les colonnes de sortie suivantes :  
  
-   **Code d’erreur**: la valeur est toujours -1.  
  
-   **Colonne d’erreur**: colonne source à l’origine de l’erreur (pour les erreurs de conversion).  
  
-   **Colonnes de ligne d’erreur**: données d’enregistrement à l’origine de l’erreur.  
  
 Selon le comportement paramétré pour les erreurs, la source CDC prend en charge les erreurs de retour (conversion de données, troncation) qui se produisent pendant le processus d'extraction dans la sortie d'erreur.  
  
## <a name="data-type-support"></a>Prise en charge du type de données  
 Le composant source CDC pour Microsoft prend en charge tous les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui sont mappés à des types de données SSIS appropriés.  
  
## <a name="troubleshooting-the-cdc-source"></a>Résolution des problèmes liés à la source CDC  
 Les éléments suivants contiennent des informations utiles à la résolution des problèmes liés à la source CDC.  
  
### <a name="use-this-script-to-isolate-problems-and-reproduce-them-in-sql-server-management-studio"></a>Utilisez ce script pour isoler les problèmes et les reproduire dans SQL Server Management Studio  
 L'opération de la source CDC est régie par l'opération de la tâche de contrôle de capture de données modifiées exécutée avant d'appeler la source CDC. La tâche de contrôle de capture de données modifiées prépare la valeur de la variable de package d'état de capture de données modifiées de sorte qu'elle contienne le NSE de début et le NSE de fin. Elle exécute une fonction équivalente au script suivant :  
  
```sql
use <cdc-enabled-database-name>  
               declare @start_lsn binary(10), @end_lsn binary(10)  
               set @start_lsn = sys.fn_cdc_increment_lsn(  
                       convert(binary(10),'0x' + '<value-from-state-cs>', 1))  
               set @end_lsn =   
                       convert(binary(10),'0x' + '<value-from-state-ce>', 1)  
               select * from cdc.fn_cdc_get_net_changes_dbo_Table1(@start_lsn,  
@end_lsn, '<mode>')  
```  
  
 où :  
  
-   \<cdc-enabled-database-name> est le nom de la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contenant les tables de modifications.  
  
-   \<value-from-state-cs> est la valeur affichée dans la variable d’état de capture des changements de données sous la forme CS/\<value-from-state-cs>/ (CS correspond à Current-processing-range-Start, début de la plage de traitement actuelle).  
  
-   \<value-from-state-ce> est la valeur affichée dans la variable d’état de capture des changements de données sous la forme CE/\<value-from-state-cs>/ (CE correspond à Current-processing-range-End, fin de la plage de traitement actuelle).  
  
-   \<mode> correspond aux modes de traitement de capture des changements de données. Les modes de traitement ont l’une des valeurs suivantes : **Tout**, **Tout avec les anciennes valeurs**, **NET**, **Net avec masque de mise à jour** et **Net avec fusion**.  
  
 Ce script aide à isoler les problèmes en les reproduisant dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], où il est facile de reproduire et d'identifier des erreurs.  
  
#### <a name="sql-server-error-message"></a>Message d'erreur SQL Server  
 Le message suivant peut être retourné par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
 **Nombre d’arguments fournis insuffisant pour la procédure ou la fonction cdc.fn_cdc_get_net_changes_\<..>.**  
  
 Cette erreur n'indique pas qu'un argument est manquant. Elle signifie que les valeurs du NSE de début ou du NSE de fin dans la variable d'état de capture de données modifiées ne sont pas valides.  
  
## <a name="configuring-the-cdc-source"></a>Configuration de la source CDC  
 Vous pouvez configurer la source CDC par programme ou par le biais du concepteur SSIS.  
  
 Pour plus d’informations, consultez l’une des rubriques suivantes :  
  
-   [Éditeur de source CDC &#40;page Gestionnaire de connexions&#41;]()  
  
-   [Éditeur de source CDC &#40;page Colonnes&#41;]()  
  
-   [Éditeur de source CDC &#40;page Sortie d’erreur&#41;]()  
  
 La boîte de dialogue **Éditeur avancé** contient les propriétés qui peuvent être définies par programme.  
  
 Pour ouvrir la boîte de dialogue **Éditeur avancé** :  
  
-   Dans l’écran **Flux de données** de votre projet [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] , cliquez avec le bouton droit sur la source CDC, puis sélectionnez **Afficher l’éditeur avancé**.  
  
 Pour plus d’informations sur les propriétés que vous pouvez définir dans la boîte de dialogue **Éditeur avancé** , consultez [Propriétés personnalisées des sources CDC](../../integration-services/data-flow/cdc-source-custom-properties.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Propriétés personnalisées des sources CDC](../../integration-services/data-flow/cdc-source-custom-properties.md)  
  
-   [Extraire des données modifiées à l’aide de la source de capture de données modifiées](../../integration-services/data-flow/extract-change-data-using-the-cdc-source.md)  
  
## <a name="cdc-source-editor-connection-manager-page"></a>Éditeur de source CDC (page Gestionnaire de connexions)
  Utilisez la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de source CDC** pour sélectionner le gestionnaire de connexions ADO.NET de la base de données [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à partir de laquelle la source CDC lit les lignes modifiées (la base de données CDC). Une fois la base de données CDC sélectionnée, vous devez sélectionner une table capturée dans la base de données.  
  
 Pour plus d'informations sur la source CDC, consultez [CDC Source](../../integration-services/data-flow/cdc-source.md).  
  
### <a name="task-list"></a>Liste des tâches  
 **Pour ouvrir l'Éditeur de source CDC (page Gestionnaire de connexions)**  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], ouvrez le package [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] qui possède la source CDC.  
  
2.  Sous l’onglet **Flux de données** , double-cliquez sur la source CDC.  
  
3.  Dans **l’Éditeur de source CDC**, cliquez sur **Gestionnaire de connexions**.  
  
### <a name="options"></a>Options  
 **Gestionnaire de connexions ADO.NET**  
 Sélectionnez un gestionnaire de connexions existant dans la liste ou cliquez sur **Nouveau** pour créer une connexion. La connexion doit être établie avec une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] activée pour la capture de données modifiées et dans laquelle la table de modifications sélectionnée est localisée.  
  
 **Nouveau**  
 Cliquez sur **Nouveau**. La boîte de dialogue **Configurer l’Éditeur du gestionnaire de connexions ADO.NET** s’ouvre et vous permet de créer un gestionnaire de connexions.  
  
 **Table CDC**  
 Sélectionnez la table source CDC qui contient les modifications capturées que vous souhaitez lire et envoyer aux composants SSIS en aval pour le traitement.  
  
 **Instance de capture**  
 Sélectionnez ou tapez le nom de l'instance de capture CDC avec la table CDC à lire.  
  
 Une table source capturée peut contenir une ou deux instances capturées pour gérer la transition transparente de la définition de table lors des modifications de schéma. Si plusieurs instances de capture sont définies pour la table source qui est capturée, sélectionnez l'instance de capture à utiliser ici. Le nom par défaut de l’instance de capture pour une table [schema].[table] est \<schema>_\<table>, mais le nom réel utilisé pour cette instance de capture peut être différent. La table réelle dans laquelle les données sont lues est la table **cdc .\<capture-instance>_CT**.  
  
 **Mode de traitement CDC**  
 Sélectionnez le mode de traitement le plus adapté pour la gestion de vos besoins de traitement. Les options possibles sont les suivantes :  
  
-   **Tout**: retourne les modifications apportées à la plage de capture de données modifiées actuelle sans les valeurs **Avant la mise à jour** .  
  
-   **Tout avec les anciennes valeurs**: retourne les modifications apportées à la plage de traitement de capture de données modifiées actuelle, dont les anciennes valeurs (**Avant la mise à jour**). Chaque opération de mise à jour utilise deux lignes, une avec les valeurs avant la mise à jour et une avec la valeur après la mise à jour.  
  
-   **Net**: retourne une seule ligne de modification par ligne source modifiée dans la plage de capture de données modifiées actuelle. Si une ligne source a été mise à jour plusieurs fois, la modification associée est appliquée (par exemple, l'insertion et la mise à jour sont considérées comme une mise à jour unique, et la mise à jour et la suppression sont considérées comme une suppression unique). Lorsque vous travaillez dans le mode de traitement de modifications Net, il est possible de fractionner les modifications apportées aux sorties de suppression, d'insertion et de mise à jour et de les traiter en parallèle car la ligne source apparaît dans plusieurs sorties.  
  
-   **Net avec masque de mise à jour** : ce mode est semblable au mode Net standard, à ceci près qu’il ajoute des colonnes booléennes au modèle de nom **__$\<column-name>\__Changed** qui indique les colonnes modifiées dans la ligne de modification active.  
  
-   **Net avec fusion**: ce mode est semblable au mode Net standard, à ceci près que les opérations d’insertion et de mise à jour sont fusionnées en une seule opération de fusion (UPSERT).  
  
> [!NOTE]  
>  Pour toutes les options de modifications Net, la table source doit avoir une clé primaire ou un index unique. Pour les tables sans clé primaire ou sans index unique, vous devez utiliser l’option **Tout** .  
  
 **Variable contenant l'état CDC**  
 Sélectionnez la variable de package de chaîne SSIS qui gère l'état de capture de données modifiées pour le contexte de capture de données modifiées actuel. Pour plus d’informations sur la variable d’état CDC, consultez [Définir une variable d’état](../../integration-services/data-flow/define-a-state-variable.md).  
  
 **Inclure la colonne de l'indicateur de retraitement**  
 Cochez cette case pour créer une colonne spéciale de sortie nommée **__$reprocessing**.  
  
 Cette colonne a la valeur **true** quand la plage de traitement CDC chevauche la plage de traitement initiale (la plage de NSE correspondant à la période de charge initiale) ou lorsqu’une plage de traitement CDC est retraitée suite à une erreur lors d’une exécution précédente. Cette colonne d'indicateur permet au développeur SSIS de gérer les erreurs différemment lors du retraitement des modifications (par exemple, les actions telles que la suppression d'une ligne inexistante et une insertion ayant échoué sur une clé dupliquée peuvent être ignorées).  
  
 Pour plus d’informations, consultez [Propriétés personnalisées des sources CDC](../../integration-services/data-flow/cdc-source-custom-properties.md).  
  
## <a name="cdc-source-editor-columns-page"></a>Éditeur de source CDC (page Colonnes)
  Utilisez la page **Colonnes** de la boîte de dialogue **Éditeur de source CDC** pour mapper une colonne de sortie à chaque colonne externe (source).  
  
### <a name="task-list"></a>Liste des tâches  
 **Pour ouvrir l'Éditeur de source CDC (page Colonnes)**  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], ouvrez le package [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] qui possède la source CDC.  
  
2.  Sous l’onglet **Flux de données** , double-cliquez sur la source CDC.  
  
3.  Dans l' **Éditeur de source CDC**, cliquez sur **Colonnes**.  
  
### <a name="options"></a>Options  
 **Colonnes externes disponibles**  
 Liste des colonnes externes disponibles dans la source de données. Vous ne pouvez pas ajouter ou supprimer des colonnes à l'aide de cette table. Sélectionnez les colonnes à utiliser dans la source. Les colonnes sélectionnées sont ajoutées à la liste **Colonne externe** dans l'ordre de leur sélection.  
  
 **Colonne externe**  
 Vue des colonnes externes (sources) selon l'ordre dans lequel vous les visualisez lorsque vous configurez des composants qui consomment des données à partir de la source CDC. Vous pouvez modifier cet ordre en supprimant d'abord les colonnes sélectionnées dans la liste **Colonnes externes disponibles** , puis en choisissant des colonnes externes dans la liste selon un ordre différent. Les colonnes sélectionnées sont ajoutées à la liste **Colonne externe** dans l'ordre de leur sélection.  
  
 **Colonne de sortie**  
 Spécifiez un nom unique pour chaque colonne de sortie. Le nom par défaut est celui de la colonne externe (source) sélectionnée ; toutefois, vous pouvez choisir n'importe quel nom unique et descriptif. Le nom entré est affiché dans le concepteur SSIS.  
  
## <a name="cdc-source-editor-error-output-page"></a>Éditeur de source CDC (page Sortie d'erreur)
  Utilisez la page **Sortie d'erreur** de la boîte de dialogue **Éditeur de source CDC** pour sélectionner les options de gestion des erreurs.  
  
### <a name="task-list"></a>Liste des tâches  
 **Pour ouvrir l'Éditeur de source CDC (page Sortie d'erreur)**  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], ouvrez le package [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] qui possède la source CDC.  
  
2.  Sous l’onglet **Flux de données** , double-cliquez sur la source CDC.  
  
3.  Dans l' **Éditeur de source CDC**, cliquez sur **Sortie d'erreur**.  
  
### <a name="options"></a>Options  
 **Entrée/sortie**  
 Affichez le nom de la source de données.  
  
 **Colonne**  
 Affichez les colonnes externes (sources) que vous avez sélectionnées dans la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de source CDC** .  
  
 **Error**  
 Sélectionnez la façon dont la source CDC doit gérer les erreurs dans un flux : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Troncation**  
 Sélectionnez la façon dont la source CDC doit gérer la troncation dans un flux : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Description**  
 Non utilisé.  
  
 **Définir cette valeur sur les cellules sélectionnées**  
 Sélectionnez la façon dont la source CDC gère l'ensemble des cellules sélectionnées lorsqu'une erreur ou une troncation se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Appliquer**  
 Appliquez les options de gestion des erreurs aux cellules sélectionnées.  
  
### <a name="error-handling-options"></a>Options de gestion des erreurs  
 Vous pouvez utiliser les options suivantes pour configurer la façon dont la source CDC gère les erreurs et les troncations.  
  
 **Composant défaillant**  
 La tâche de flux de données échoue lorsqu'une erreur ou une troncation a lieu. Il s'agit du comportement par défaut.  
  
 **Ignorer l'échec**  
 L'erreur ou la troncation est ignorée et la ligne de données est dirigée vers la sortie de la source CDC.  
  
 **Rediriger le flux**  
 La ligne de données qui présente l'erreur ou la troncation est dirigée vers la sortie d'erreur de la source CDC. Dans ce cas, la gestion des erreurs de la source CDC est utilisée. Pour plus d'informations, consultez [CDC Source](../../integration-services/data-flow/cdc-source.md).  
  
## <a name="related-content"></a>Contenu associé  
  
-   Entrée de blog, [Processing Modes for the CDC Source](https://www.mattmasson.com/2012/01/processing-modes-for-the-cdc-source/)(Modes de traitement pour la source CDC), sur mattmasson.com.  
  
