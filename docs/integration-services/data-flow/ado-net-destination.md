---
description: Destination ADO NET
title: Destination ADO NET | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.adonetdest.f1
- sql13.dts.designer.adonetdest.connection.f1
- sql13.dts.designer.adonetdest.mappings.f1
- sql13.dts.designer.adonetdest.erroroutput.f1
helpviewer_keywords:
- destinations [Integration Services], ADO.NET
- ADO.NET destination
ms.assetid: cb883990-d875-4d8b-b868-45f9f15ebeae
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4ae905d868554c20948af3eb0191937382eff56f
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536373"
---
# <a name="ado-net-destination"></a>Destination ADO NET

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La destination ADO NET charge des données dans différentes bases de données compatibles [!INCLUDE[vstecado](../../includes/vstecado-md.md)]qui utilisent une table ou une vue de base de données. Vous pouvez charger ces données dans une table ou une vue existante ou créer une table et y charger les données.  
  
 Vous pouvez utiliser la destination ADO .NET. pour vous connecter à [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]. La connexion à [!INCLUDE[ssSDS](../../includes/sssds-md.md)] via OLE DB n'est pas prise en charge. Pour plus d’informations sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)], consultez [Recommandations générales et limitations (Azure SQL Database)](/previous-versions/azure/ee336245(v=azure.100)).  
  
## <a name="troubleshooting-the-ado-net-destination"></a>Résolution des problèmes liés à la destination ADO NET  
 Vous pouvez consigner les appels que la destination ADO NET effectue auprès de fournisseurs de données externes. Cette fonctionnalité de journalisation permet de résoudre des problèmes liés à l'enregistrement de données vers des sources de données externes que réalise la destination ADO NET. Pour consigner les appels aux fournisseurs de données externes effectués par la destination ADO.NET, activez la journalisation du package et sélectionnez l’événement **Diagnostic** au niveau du package. Pour plus d’informations, consultez [Outils de dépannage pour l’exécution des packages](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).  
  
## <a name="configuring-the-ado-net-destination"></a>Configuration de la destination ADO NET  
 Cette destination utilise un gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] pour se connecter à une source de données. Il indique quel fournisseur [!INCLUDE[vstecado](../../includes/vstecado-md.md)] utiliser. Pour plus d’informations, consultez [Gestionnaire de connexions ADO.NET](../../integration-services/connection-manager/ado-net-connection-manager.md).  
  
 Une destination ADO NET inclut des mappages entre les colonnes d'entrée et les colonnes de la source de données de destination. Vous n'avez pas besoin de mapper les colonnes d'entrée à toutes les colonnes de destination. Toutefois, les propriétés de certaines colonnes de destination peuvent requérir le mappage de colonnes d'entrée. Sinon, des erreurs peuvent se produire. Par exemple, si une colonne de destination n'autorise pas les valeurs Null, vous devez mapper une colonne d'entrée à cette colonne. Par ailleurs, les types de données des colonnes mappées doivent être compatibles. Par exemple, vous ne pouvez pas mapper une colonne d’entrée avec un type de données chaîne à une colonne de destination avec un type de données numérique si le fournisseur [!INCLUDE[vstecado](../../includes/vstecado-md.md)] ne prend pas en charge ce mappage.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge l’insertion de texte dans des colonnes dont le type de données est défini sur image. Pour plus d’informations sur les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).  
  
> [!NOTE]  
>  La destination ADO NET ne prend pas en charge le mappage d'une colonne d'entrée dont le type a la valeur DT_DBTIME avec une colonne de base de données dont le type a la valeur datetime. Pour plus d’informations sur les types de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , consultez [Types de données d’Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
 La destination ADO NET comporte une entrée standard et une sortie d'erreur.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 La boîte de dialogue **Éditeur avancé** reflète les propriétés qui peuvent être définies par programmation. Pour plus d'informations sur les propriétés définissables dans la boîte de dialogue **Éditeur avancé** ou par programmation, cliquez sur l'une des rubriques suivantes :  
  
-   [Propriétés communes](./set-the-properties-of-a-data-flow-component.md)  
  
-   [Propriétés personnalisées ADO NET](../../integration-services/data-flow/ado-net-custom-properties.md)  
  
 Pour plus d’informations sur la façon de définir les propriétés, consultez [Définir les propriétés d’un composant de flux de données](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="ado-net-destination-editor-connection-manager-page"></a>Éditeur de destination ADO NET (page Gestionnaire de connexions)
  Utilisez la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de destination ADO NET** pour sélectionner la connexion [!INCLUDE[vstecado](../../includes/vstecado-md.md)] de la destination. Cette page vous permet également de sélectionner une table ou une vue à partir de la base de données.  
  
 **Pour ouvrir la page Gestionnaire de connexions**  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui possède la destination ADO NET.  
  
2.  Sous l’onglet **Flux de données** , double-cliquez sur la destination ADO NET.  
  
3.  Dans **l’Éditeur de destination ADO NET**, cliquez sur **Gestionnaire de connexions**.  
  
### <a name="static-options"></a>Options statiques  
 **Connection manager**  
 Sélectionnez un gestionnaire de connexions existant dans la liste ou créez une nouvelle connexion en cliquant sur **Nouveau**.  
  
 **Nouveau**  
 Créez un gestionnaire de connexions à partir de la boîte de dialogue **Configurer le gestionnaire de connexions ADO.NET** .  
  
 **Utiliser une table ou une vue**  
 Sélectionnez une table ou une vue existante dans la liste ou créez une table en cliquant sur **Nouvelle**.  
  
 **Nouveau**  
 Créez une table ou vue à l’aide de la boîte de dialogue **Créer une table** .  
  
> [!NOTE]  
>  Quand vous cliquez sur **Nouvelle**, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] génère une instruction CREATE TABLE par défaut, basée sur la source de données connectée. Cette instruction CREATE TABLE par défaut n'inclut pas l'attribut FILESTREAM, même si la table source inclut une colonne dans laquelle l'attribut FILESTREAM est déclaré. Pour exécuter un composant [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] avec l'attribut FILESTREAM, implémentez d'abord le stockage FILESTREAM sur la base de données de destination. Ajoutez ensuite l’attribut FILESTREAM à l’instruction CREATE TABLE dans la boîte de dialogue **Créer une table** . Pour plus d’informations, consultez [Données blob &#40;Binary Large Object&#41; &#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md).  
  
 **Préversion**  
 Affichez un aperçu des résultats à l’aide de la boîte de dialogue **Visualiser les résultats de la requête** . L'aperçu peut afficher jusqu'à 200 lignes.  
  
 **Utiliser l'insertion en bloc le cas échéant**  
 Spécifiez s’il convient d’utiliser l’interface <xref:System.Data.SqlClient.SqlBulkCopy> pour améliorer les performances des opérations d’insertion en bloc.  
  
 Seuls les fournisseurs ADO.NET qui retournent un objet <xref:System.Data.SqlClient.SqlConnection> prennent en charge l’utilisation de l’interface <xref:System.Data.SqlClient.SqlBulkCopy> . Le fournisseur de données .NET pour SQL Server (SqlClient) retourne un objet <xref:System.Data.SqlClient.SqlConnection> , et un fournisseur personnalisé peut retourner un objet <xref:System.Data.SqlClient.SqlConnection> .  
  
 Vous pouvez utiliser le fournisseur de données .NET pour SQL Server (SqlClient) pour vous connecter à [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)].  
  
 Si vous sélectionnez **Utiliser l’insertion en bloc le cas échéant** et affectez à l’option **Erreur** la valeur **Rediriger la ligne**, le lot de données que la destination redirige vers la sortie d’erreur peut inclure des lignes correctes. Pour plus d’informations sur la gestion des erreurs dans les opérations en bloc, consultez [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md). Pour plus d’informations sur l’option **Erreur** , consultez [Éditeur de destination ADO NET &#40;page Sortie d’erreur&#41;]().  
  
> [!NOTE]
>  Si une table source SQL Server ou Sybase inclut une colonne d’identité, vous devez utiliser les tâches d’exécution de requêtes SQL pour activer IDENTITY_INSERT avant la destination ADO NET et le désactiver après. (La propriété de la colonne d’identité spécifie une valeur incrémentielle pour la colonne. L’instruction SET IDENTITY_INSERT permet l’insertion de valeurs explicites de la table source dans la colonne d’identité de la table de destination.)  
> 
>   Pour exécuter avec succès les instructions SET IDENTITY_INSERT et le chargement des données, vous devez effectuer les opérations suivantes.  
>       1. Utilisez le même gestionnaire de connexions ADO.NET pour les tâches d’exécution de requêtes SQL et la destination ADO NET.  
>       2. Dans le gestionnaire de connexions, définissez la propriété **RetainSameConnection** et la propriété **MultipleActiveResultSets** sur True.  
>       3. Sur la destination ADO.NET, définissez la propriété **UseBulkInsertWhenPossible** sur False.   
> 
>  Pour plus d’informations, consultez [SET IDENTITY_INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/set-identity-insert-transact-sql.md) et [IDENTITY &#40;Propriété&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md).  
  
## <a name="external-resources"></a>Ressources externes  
 Article technique sur sqlcat.com, traitant du [chargement rapide de données sur Azure SQL Database](https://techcommunity.microsoft.com/t5/datacat/loading-data-to-sql-azure-the-fast-way/ba-p/305066)  
  
## <a name="ado-net-destination-editor-mappings-page"></a>Éditeur de destination ADO NET (page Mappages)
  Utilisez la page **Mappages** de la boîte de dialogue **Éditeur de destination ADO NET** pour mapper des colonnes d’entrée à des colonnes de destination.  
  
 **Pour ouvrir la page Mappages**  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui possède la destination ADO NET.  
  
2.  Sous l’onglet **Flux de données** , double-cliquez sur la destination ADO NET.  
  
3.  Dans **l’Éditeur de destination ADO NET**, cliquez sur **Mappages**.  
  
### <a name="options"></a>Options  
 **Colonnes d'entrée disponibles**  
 Affichez la liste des colonnes d'entrée disponibles. Au moyen du glisser-déplacer, mappez les colonnes d'entrée disponibles dans la table sur des colonnes de destination.  
  
 **Colonnes de destination disponibles**  
 Affichez la liste des colonnes de destination disponibles. Utilisez une opération de glisser-déplacer pour mapper les colonnes de destination disponibles dans la table aux colonnes d'entrée.  
  
 **Colonne d'entrée**  
 Affichez les colonnes d’entrée que vous avez sélectionnées. Vous pouvez supprimer des mappages en sélectionnant **\<ignore>** pour exclure des colonnes de la sortie.  
  
 **Colonne de destination**  
 Indique chaque colonne de destination disponible, qu'elle soit mappée ou non.  
  
## <a name="ado-net-destination-editor-error-output-page"></a>Éditeur de destination ADO NET (page Sortie d'erreur)
  Utilisez la page **Sortie d'erreur** de la boîte de dialogue **Éditeur de destination ADO NET** pour spécifier les options de gestion des erreurs.  
  
 **Pour ouvrir la page Sortie d'erreur**  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui possède la destination ADO NET.  
  
2.  Sous l’onglet **Flux de données** , double-cliquez sur la destination ADO NET.  
  
3.  Dans **l’Éditeur de destination ADO NET**, cliquez sur **Sortie d’erreur**.  
  
### <a name="options"></a>Options  
 **Entrée ou Sortie**  
 Affichez le nom de l'entrée.  
  
 **Colonne**  
 Non utilisé.  
  
 **Error**  
 Indiquez ce qui doit se produire lorsqu'une erreur se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Rubriques connexes :** [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md)  
  
 **Troncation**  
 Non utilisé.  
  
 **Description**  
 Affichez la description de l'opération.  
  
 **Définir cette valeur sur les cellules sélectionnées**  
 Indiquez ce qui doit se produire pour l'ensemble des cellules sélectionnées lorsqu'une erreur ou une troncation se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
 **Appliquer**  
 Appliquez l'option de gestion des erreurs aux cellules sélectionnées.  
  
