---
description: Destination SQL Server
title: Destination SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sqlserverdest.f1
- sql13.dts.designer.sqlserverdestadapter.connection.f1
- sql13.dts.designer.sqlserverdestadapter.mappings.f1
- sql13.dts.designer.sqlserverdestadapter.advanced.f1
helpviewer_keywords:
- SQL Server destination
- loading data
- destinations [Integration Services], SQL Server
- inserting data
- bulk load [Integration Services]
ms.assetid: a0227cd8-6944-4547-87e8-7b2507e26442
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6e980e075f23d3a943870fc6dd02aa420797f8a4
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596477"
---
# <a name="sql-server-destination"></a>Destination SQL Server

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La destination SQL Server se connecte à une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale et charge en masse des données dans des tables et des vues [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous ne pouvez pas utiliser la destination SQL Server dans des packages ayant accès à une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un serveur distant. Les packages doivent plutôt utiliser la destination OLE DB. Pour plus d’informations, consultez [OLE DB Destination](../../integration-services/data-flow/ole-db-destination.md).  
  
## <a name="permissions"></a>Autorisations  
 Les utilisateurs qui exécutent des packages incluant la destination SQL Server nécessitent l'autorisation « Create global objects » (Créer des objets globaux). Vous pouvez attribuer cette autorisation aux utilisateurs à l’aide de l’outil Stratégie de sécurité locale accessible dans le menu **Outils d’administration** . Si vous recevez un message d'erreur lors de l'exécution d'un package qui utilise la destination SQL Server, assurez-vous que le compte exécutant le package a l'autorisation « Create global objects » (Créer des objets globaux).  
  
## <a name="bulk-inserts"></a>Insertions en bloc  
 Si vous tentez d'utiliser la destination SQL Server pour charger en masse des données dans une base de données SQL Server distante, il est possible qu'un message d'erreur semblable au message suivant s'affiche : « Un enregistrement OLE DB est disponible. Source : « Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client » Hresult : 0x80040E14 Description : « Chargement en masse impossible, car l’objet de mappage de fichier SSIS 'Global\DTSQLIMPORT ' n’a pas pu être ouvert. Code d'erreur du système d'exploitation 2 (Le système ne trouve pas le fichier spécifié.). Vérifiez que vous accédez à un serveur local par le biais de la sécurité Windows." »  
  
 La destination SQL Server offre la même insertion rapide de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que la tâche d’insertion en bloc ; toutefois, l’utilisation d’une destination SQL Server permet à un package d’appliquer des transformations à des données de colonne avant que les données ne soient chargées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Pour le chargement de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], envisagez l'utilisation de la destination SQL Server plutôt que la destination OLE DB.  
  
### <a name="bulk-insert-options"></a>Options d'insertion en bloc  
 Si la destination SQL Server utilise un mode d'accès aux données par chargement rapide, vous pouvez spécifier les options de chargement rapide suivantes :  
  
-   Conservation des valeurs d'identité du fichier de données importé ou utilisation de valeurs uniques assignées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Conservation des valeurs nulles durant l'opération de chargement en masse.  
  
-   Vérification des contraintes sur la table ou la vue cible durant l'opération d'importation en bloc.  
  
-   Acquisition d'un verrou au niveau de la table pour la durée de l'opération de chargement en masse.  
  
-   Exécution de déclencheurs d'insertion définis sur la table de destination durant l'opération de chargement en masse.  
  
-   Spécification du numéro de la première ligne de l'entrée à charger durant l'opération d'insertion en bloc.  
  
-   Spécification du numéro de la dernière ligne de l'entrée à charger durant l'opération d'insertion en bloc.  
  
-   Spécification du nombre maximal d'erreurs tolérées avant l'annulation de l'opération de chargement en masse. Chaque ligne ne pouvant pas être importée est comptée comme une erreur.  
  
-   Spécification des colonnes de l'entrée qui contiennent des données triées.  
  
 Pour plus d’informations sur les options de chargement en masse, consultez [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
#### <a name="performance-improvements"></a>Amélioration des performances  
 Pour améliorer les performances d'une insertion en bloc et l'accès aux données de table durant l'opération d'insertion en bloc, vous devez modifier les options par défaut comme suit :  
  
-   Ne pas vérifier les contraintes sur la table ou la vue cible durant l'opération d'importation en bloc.  
  
-   Ne pas exécuter de déclencheurs d'insertion définis sur la table de destination durant l'opération de chargement en masse.  
  
-   Ne pas appliquer de verrou sur la table. De cette manière, la table reste disponible pour les autres utilisateurs et applications durant l'opération d'insertion en bloc.  
  
## <a name="configuration-of-the-sql-server-destination"></a>Configuration de la destination SQL Server  
 Vous pouvez configurer la destination SQL Server de plusieurs manières :  
  
-   Spécifiez la table ou la vue dans laquelle charger les données en masse.  
  
-   Personnalisez l'opération de chargement en masse en spécifiant des options telles que la vérification des contraintes.  
  
-   Indiquez si toutes les lignes doivent être validées en un seul traitement ou définissez le nombre maximum de lignes à valider en tant que traitement.  
  
-   Spécifiez un délai d'expiration pour l'opération de chargement en masse.  
  
 Cette destination utilise un gestionnaire de connexions OLE DB pour se connecter à une source de données et le gestionnaire de connexions spécifie le fournisseur OLE DB à utiliser. Pour plus d’informations, consultez [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
 Un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] fournit également l'objet de source de données à partir duquel vous pouvez créer un gestionnaire de connexions OLE DB. Les sources de données et les vues de sources de données sont ainsi disponibles pour la destination SQL Server.  
  
 La destination SQL Server possède une entrée. Elle ne prend pas en charge de sortie d'erreur.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](./set-the-properties-of-a-data-flow-component.md)  
  
-   [Propriétés personnalisées de la destination SQL Server](../../integration-services/data-flow/sql-server-destination-custom-properties.md)  
  
 Pour plus d'informations sur la définition des propriétés, cliquez sur l'une des rubriques suivantes :  
  
-   [Charger des données en masse à l'aide de la destination SQL Server](../../integration-services/data-flow/bulk-load-data-by-using-the-sql-server-destination.md)  
  
-   [Définir les propriétés d’un composant de flux de données](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="related-tasks"></a>Tâches associées  
  
-   [Charger des données en masse à l'aide de la destination SQL Server](../../integration-services/data-flow/bulk-load-data-by-using-the-sql-server-destination.md)  
  
-   [Définir les propriétés d’un composant de flux de données](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="related-content"></a>Contenu associé  
  
-   Article technique, [You may get "Unable to prepare the SSIS bulk insert for data insertion" error on UAC enabled systems](/troubleshoot/sql/integration-services/error-you-run-ssis-package), sur support.microsoft.com.  
  
-   Article technique, [Guide des performances de chargement des données](/previous-versions/sql/sql-server-2008/dd425070(v=sql.100)), sur le site msdn.microsoft.com.  
  
-   Article technique, [Using SQL Server Integration Services to Bulk Load Data](https://go.microsoft.com/fwlink/?LinkId=233701), sur le site simple-talk.com.  
  
## <a name="sql-destination-editor-connection-manager-page"></a>Éditeur de destination SQL (page Gestionnaire de connexions)
  Utilisez la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de destination SQL** pour spécifier des informations sur la source de données et afficher un aperçu des résultats. La destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] charge les données dans des tables ou des vues, dans une base de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="options"></a>Options  
 **Gestionnaire de connexions OLE DB**  
 Sélectionnez un gestionnaire de connexions existant dans la liste ou créez une connexion en cliquant sur **Nouvelle**.  
  
 **Nouveau**  
 Crée une connexion en utilisant la boîte de dialogue **Configurer le gestionnaire de connexions OLE DB** .  
  
 **Utiliser une table ou une vue**  
 Sélectionnez une table ou une vue existante dans la liste, ou créez une connexion en cliquant sur **Nouvelle**.  
  
 **Nouveau**  
 Utilisez la boîte de dialogue **Créer une table** pour créer une table.  
  
> [!NOTE]  
>  Quand vous cliquez sur **Nouvelle**, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] génère une instruction CREATE TABLE par défaut, basée sur la source de données connectée. Cette instruction CREATE TABLE par défaut n'inclut pas l'attribut FILESTREAM, même si la table source inclut une colonne dans laquelle l'attribut FILESTREAM est déclaré. Pour exécuter un composant [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] avec l'attribut FILESTREAM, implémentez d'abord le stockage FILESTREAM sur la base de données de destination. Ajoutez ensuite l’attribut FILESTREAM à l’instruction CREATE TABLE dans la boîte de dialogue **Créer une table** . Pour plus d’informations, consultez [Données blob &#40;Binary Large Object&#41; &#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md).  
  
 **Préversion**  
 Affichez un aperçu des résultats à l’aide de la boîte de dialogue **Visualiser les résultats de la requête** . L'aperçu peut afficher jusqu'à 200 lignes.  
  
## <a name="sql-destination-editor-mappings-page"></a>Éditeur de destination SQL (page Mappages)
  La page **Mappages** de la boîte de dialogue **Éditeur de destination SQL** vous permet de mapper les colonnes d'entrée aux colonnes de destination.  
  
### <a name="options"></a>Options  
 **Colonnes d'entrée disponibles**  
 Affichez la liste des colonnes d'entrée disponibles. Au moyen du glisser-déplacer, mappez les colonnes d'entrée disponibles dans la table sur des colonnes de destination.  
  
 **Colonnes de destination disponibles**  
 Affichez la liste des colonnes de destination disponibles. Utilisez une opération de glisser-déplacer pour mapper les colonnes de destination disponibles dans la table aux colonnes d'entrée.  
  
 **Colonne d'entrée**  
 Affiche les colonnes d'entrée sélectionnées dans le tableau ci-dessus. Vous pouvez modifier les mappages au moyen de la liste **Colonnes d'entrée disponibles**.  
  
 **Colonne de destination**  
 Affiche chaque colonne de destination disponible, qu'elle soit mappée ou non.  
  
## <a name="sql-destination-editor-advanced-page"></a>Éditeur de destination SQL (page Avancé)
  Utilisez la page **Avancé** de la boîte de dialogue **Éditeur de destination SQL** pour spécifier les options avancées pour l’insertion en bloc.  
  
### <a name="options"></a>Options  
 **Conserver l'identité**  
 Spécifie si la tâche doit insérer des valeurs dans les colonnes d'identité. La valeur par défaut de cette propriété est **False**.  
  
 **Conserver les valeurs NULL**  
 Spécifie si la tâche doit conserver les valeurs NULL. La valeur par défaut de cette propriété est **False**.  
  
 **Verrou de table**  
 Spécifie si la table est verrouillée lors du chargement des données. La valeur par défaut de cette propriété est **True**.  
  
 **Contraintes de validation**  
 Spécifie si la tâche doit vérifier les contraintes. La valeur par défaut de cette propriété est **True**.  
  
 **Exécuter les déclencheurs**  
 Spécifie si l'insertion en bloc doit exécuter les déclencheurs sur les tables. La valeur par défaut de cette propriété est **False**.  
  
 **Première ligne**  
 Spécifie la première ligne à insérer. La valeur par défaut de cette propriété est **-1**, ce qui signifie qu’aucune valeur ne lui a été assignée.  
  
> [!NOTE]  
>  Effacez la zone de texte dans **l’Éditeur de destination SQL** pour indiquer que vous ne voulez pas assigner de valeur à cette propriété. Utilisez -1 dans la fenêtre **Propriétés** , **l’Éditeur avancé** et le modèle objet.  
  
 **Dernière ligne**  
 Spécifie la dernière ligne à insérer. La valeur par défaut de cette propriété est **-1**, ce qui signifie qu’aucune valeur ne lui a été assignée.  
  
> [!NOTE]  
>  Effacez la zone de texte dans **l’Éditeur de destination SQL** pour indiquer que vous ne voulez pas assigner de valeur à cette propriété. Utilisez -1 dans la fenêtre **Propriétés** , **l’Éditeur avancé** et le modèle objet.  
  
 **Nombre maximum d'erreurs**  
 Spécifie le nombre d'erreurs au-delà duquel l'insertion en bloc s'arrête. La valeur par défaut de cette propriété est **-1**, ce qui signifie qu’aucune valeur ne lui a été assignée.  
  
> [!NOTE]  
>  Effacez la zone de texte dans **l’Éditeur de destination SQL** pour indiquer que vous ne voulez pas assigner de valeur à cette propriété. Utilisez -1 dans la fenêtre **Propriétés** , **l’Éditeur avancé** et le modèle objet.  
  
 **Délai d'expiration**  
 Spécifie le nombre de secondes pouvant s'écouler avant l'expiration de l'insertion en bloc.  
  
 **Ordre des colonnes**  
 Tapez le nom des colonnes de tri. Chaque colonne peut être triée par ordre croissant ou décroissant. Si vous utilisez plusieurs colonnes de tri, délimitez la liste par des virgules.  
  
## <a name="see-also"></a>Voir aussi  
 [Flux de données](../../integration-services/data-flow/data-flow.md)  
