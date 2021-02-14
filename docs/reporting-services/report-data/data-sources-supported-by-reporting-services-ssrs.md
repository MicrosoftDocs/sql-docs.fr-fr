---
title: Sources de données prises en charge par Reporting Services | Microsoft Docs
description: Découvrez les différentes sources de données prises en charge par Reporting Services, notamment Microsoft SQL Server, Oracle et ODBC.
ms.date: 11/10/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
helpviewer_keywords:
- SQL Server data processing extension [Reporting Services]
- XML data processing extension [Reporting Services]
- reports [Reporting Services], data
- data processing extensions [Reporting Services], data sources
- Oracle [Reporting Services]
- OLE DB data processing extension
- data sources [Reporting Services], about data sources
- ODBC data processing extension
- Reporting Services, data sources
ms.assetid: 9d11d055-a3be-45aa-99a7-46447a94ed42
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a92e47d43e018c467ad53c06aa2943b9d51d1766
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978591"
---
# <a name="data-sources-supported-by-reporting-services-ssrs"></a>Sources de données prises en charge par Reporting Services (SSRS)
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] récupère des données de rapport dans des sources de données par l’intermédiaire d’une couche de données extensibles et modulaire qui utilise des extensions pour le traitement des données. Pour récupérer des données de rapport à partir d’une source de données, vous devez sélectionner une extension pour le traitement des données qui prend en charge le type de source de données, la version du logiciel s’exécutant sur la source de données ainsi que la plateforme de la source de données ( [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]32 bits ou 64 bits).  
  
 Quand vous déployez [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], un jeu d’extensions pour le traitement des données est installé et inscrit automatiquement sur le client de création de rapports et sur le serveur de rapports afin de permettre l’accès à divers types de sources de données. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] installe les types de sources de données suivants :  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour MDX, DMX, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et les modèles tabulaires  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]  
  
-   Oracle  
  
-   SAP BW 
  
-   [!INCLUDE[extEssbase](../../includes/extessbase-md.md)]  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] Liste SharePoint  
  
-   Teradata  
  
-   OLE DB  
  
-   ODBC  
  
-   XML  
  
 De plus, tout administrateur système peut installer et inscrire des extensions pour le traitement des données personnalisées et des fournisseurs de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] standard. Pour traiter et afficher un rapport, les fournisseurs de données et les extensions pour le traitement des données doivent être installés et inscrits sur le serveur de rapports ; pour afficher un aperçu d'un rapport, ces derniers doivent être installés et inscrits sur le client de création de rapports. Fournisseurs de données et extensions pour le traitement de données doivent être compilés en mode natif pour la plateforme sur laquelle ils sont installés. Si vous déployez une source de données par programmation à l'aide du service SOAP Web, vous devez définir l'extension de source de données. Utilisez les valeurs d’extension de données du fichier **RSReportDesigner.config** . Par défaut, le fichier se trouve dans le dossier suivant :  
  
```  
<drive letter>\Program Files (x86)\Microsoft Visual Studio 10.0\Common7\IDE\PrivateAssemblies  
```  
  
 Par exemple, l’extension de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] est OLEDB-MD.  
  
 De nombreux fournisseurs de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] tiers standard sont disponibles en téléchargement sur le [Centre de téléchargement Microsoft](https://www.microsoft.com/download/default.aspx) et sur des sites tiers. Vous pouvez aussi faire des recherches sur le forum public [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour obtenir des informations sur les fournisseurs de données tiers.  
  
> [!NOTE]  
>  Les fournisseurs de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] standard ne prennent pas nécessairement en charge toutes les fonctionnalités fournies par les extensions pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . De plus, certains fournisseurs de données OLE DB et pilotes ODBC peuvent être utilisés pour créer et afficher un aperçu de rapport mais ils ne sont pas conçus pour prendre en charge des rapports publiés sur un serveur de rapports. Par exemple, le fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB pour Jet n'est pas pris en charge sur le serveur de rapports. Pour plus d’informations, consultez [Extensions pour le traitement des données et fournisseurs de données .NET Framework &#40;SSRS&#41;](../../reporting-services/report-data/data-processing-extensions-and-net-framework-data-providers-ssrs.md).  
  
 Pour plus d’informations sur les extensions pour le traitement des données personnalisées, consultez [Implémentation d’une extension pour le traitement des données](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md). Pour plus d’informations sur les fournisseurs de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] standard, consultez l’espace de noms <xref:System.Data> .   
  
## <a name="platform-support-for-report-data-sources"></a>Prise en charge de plateforme pour les sources de données de rapport  
 Les sources de données utilisables dans un déploiement [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] varient selon l’édition [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la version [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et la plateforme. Pour plus d’informations sur les fonctionnalités, consultez [Fonctionnalités Reporting Services prises en charge par les éditions de SQL Server](../../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md). Le tableau plus loin dans cette rubrique fournit des informations sur les sources de données prises en charge par version et par plateforme.  
  
 Les considérations relatives aux plateformes pour les sources de données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] diffèrent pour le client de création de rapports et pour le serveur de rapports.  
  
### <a name="on-the-report-authoring-client"></a>Pour le client de création de rapports  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)] est une application 32 bits. [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)] n’est pas pris en charge sur les plateformes Itanium. Sur une plateforme x64, pour modifier ou afficher un aperçu de rapports dans le Concepteur de rapports, vous devez disposer de fournisseurs de données 32 bits installés dans le répertoire de plateforme (x86).  
  
### <a name="on-the-report-server"></a>Sur le serveur de rapports  
 Si vous déployez un rapport sur un serveur de rapports 64 bits, des fournisseurs de données 64 bits compilés en mode natif doivent être installés sur ce serveur. Englober un fournisseur de données 32 bits dans des interfaces 64 bits n'est pas pris en charge. Pour plus d'informations, consultez la documentation du fournisseur de données.  
  
## <a name="supported-data-sources"></a>Sources de données prises en charge  
 Le tableau suivant répertorie les extensions pour le traitement des données et les fournisseurs de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] vous permettant d’extraire des données pour les datasets du rapport et les modèles de rapport. Pour plus d'informations sur une extension ou un fournisseur de données, cliquez sur le lien dans la deuxième colonne. Les colonnes du tableau sont décrites comme suit :  
  
-   Source de données de rapport : Type de données en cours d'accès. Par exemple, base de données relationnelle, base de données multidimensionnelle, fichier plat ou XML. Cette colonne répond à la question : « Quels types de données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] peut-il utiliser pour un rapport ? »  
  
-   Type de source de données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] : Un des types de source de données de la liste déroulante s'affiche lorsque vous définissez une source de données dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Cette liste est renseignée à partir des fournisseurs de données et extensions installés et inscrits. Cette colonne répond à la question : « Quel type de source de données faut-il sélectionner dans la liste déroulante lors de la création d'une source de données de rapport ? »  
  
-   Nom du fournisseur de données/extension pour le traitement des données : L'extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou tout autre fournisseur de données qui correspond au type de source de données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sélectionné. Cette colonne répond à la question : « Quand je sélectionne un type de source de données, quelle extension pour le traitement des données ou fournisseur de données correspondant est utilisé ? »  
  
-   Version du fournisseur de données sous-jacent (facultatif) : Certains types de sources de données prennent en charge plusieurs fournisseurs de données. Il peut s'agir de versions différentes du même fournisseur ou de différentes implémentations tierces pour un type de fournisseur de données. Le nom du fournisseur apparaît souvent dans la chaîne de connexion une fois que vous avez configuré une source de données. Cette colonne répond à la question : « Après avoir sélectionné le type de source de données, quel fournisseur de données faut-il sélectionner dans la boîte de dialogue **Propriétés de connexion** ? »  
  
-   Source de données *\<platform>*  : Plateforme de la source de données prise en charge par l'extension pour le traitement des données ou le fournisseur de données pour la source de données cible. Cette colonne répond à la question : « Est-ce que cette extension pour le traitement des données ou ce fournisseur de données peut extraire des données d'une source de données sur ce type de plateforme ? »  
  
-   Version de la source de données : Version de la source de données cible prise en charge par le fournisseur de données ou par l'extension pour le traitement des données. Cette colonne répond à la question : « Cette extension pour le traitement des données ou ce fournisseur de données peut-il extraire des données à partir de cette version de la source de données ? »  
  
-   RS *\<platform>*  : Plateformes pour le serveur de rapports et le client de création de rapports où vous pouvez installer une extension pour le traitement des données ou un fournisseur de données personnalisé(e). Les extensions pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrées sont fournies avec toutes les installations de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Une extension pour le traitement des données personnalisée ou un fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] doit être compilé en mode natif pour une plateforme spécifique. Cette colonne répond à la question : « Cette extension pour le traitement des données ou ce fournisseur de données peut-il être installé sur ce type de plateforme ? »  
  
###  <a name="types-of-data-sources"></a><a name="DataSourcesTable"></a> Types de sources de données  
  
|Source des<br /><br /> données de rapport|Type de la source de données Reporting Services|Nom du fournisseur de données/extension pour le traitement des données|Version du fournisseur de données sous-jacent<br /><br /> (facultatif)|Données<br /><br /> Source<br /><br /> Plateforme x86|Données<br /><br /> Source<br /><br /> Plateforme x64|Version de la source de données|RS<br /><br /> Plateforme x86|RS<br /><br /> Plateforme x64|  
|-------------------------------|-----------------------------------------|------------------------------------------------------|-------------------------------------------------------|--------------------------------------|--------------------------------------|----------------------------|-------------------------|-------------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données relationnelle|[Microsoft SQL Server](#MicrosoftSQLServer)|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Étend System.Data.SqlClient|O|O|SQL Server 2012 et versions ultérieures.|O|O|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données relationnelle|OLEDB|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Étend System.Data.OledbClient|O|O|SQL Server 2012 et versions ultérieures.|O|O|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données relationnelle|[ODBC](#ODBC)|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Étend System.Data.OdbcClient|O|O|SQL Server 2012 et versions ultérieures.|O|O|  
|[!INCLUDE[ssSDS](../../includes/sssds-md.md)]|[Microsoft Azure SQL Database](#Azure)|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Étend System.Data.SqlClient|N/A|NON APPLICABLE|[!INCLUDE[ssSDS](../../includes/sssds-md.md)]|O|O|
|Azure Synapse Analytics|[Microsoft Azure SQL Database](#Azure)|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Étend System.Data.SqlClient|N/A|NON APPLICABLE|Azure Synapse Analytics|O|O| 
|[!INCLUDE[ssDW](../../includes/ssdw-md.md)] Appliance|[Microsoft Parallel Data Warehouse](#PWD)|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dépréciée|N/A|N/A|NON APPLICABLE|[!INCLUDE[ssDWfull](../../includes/ssdwfull-md.md)]|N|N|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] base de données multidimensionnelle ou tabulaire|[Microsoft SQL Server Analysis Services](#AnalysisServices)|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Utilise ADOMD.NET|O|O|SQL Server 2012 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et versions ultérieures|O|O|  
|Jeu de données Power BI Premium (à compter de Reporting Services 2019 et Power BI Report Server janvier 2020) |[Microsoft SQL Server Analysis Services](#AnalysisServices)|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Utilise ADOMD.NET|O|O|SQL Server 2019 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et versions ultérieures|O|O|
|Azure Analysis Services |[Microsoft SQL Server Analysis Services](#AnalysisServices)|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Utilise ADOMD.NET|O|O|SQL Server 2017 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et versions ultérieures|O|O| 
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Base de données multidimensionnelle|OLEDB|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Étend System.Data.OledbClient<br /><br /> Version 10.0|O|O|SQL Server 2012 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|O|O|
|Listes SharePoint|[Liste Microsoft SharePoint](#SharePointList)|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Obtient des données de Lists.asmx ou des interfaces API du modèle d'objet SharePoint.<br /><br /> Consultez [Remarque](#SharePointList).|N|O|Produits SharePoint 2013 et ultérieur|O|O|   
|XML|[XML](#XML)|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Les sources de données XML n'ont pas de dépendance de plateforme.|N/A|NON APPLICABLE|[!INCLUDE[vstecwebservices](../../includes/vstecwebservices-md.md)] ou documents|O|O|  
|Modèle Report Server|Modèle de rapport|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dépréciée pour un fichier SMDL publié|Les sources de données pour un modèle font appel aux extensions pour le traitement des données intégrées.<br /><br /> Les modèles basés sur Oracle requièrent les composants clients Oracle.<br /><br /> Les modèles basés sur Teradata requièrent le fournisseur de données .NET pour Teradata de Teradata.<br /><br /> Consultez la documentation Teradata pour la prise en charge de plateforme.|N/A|NON APPLICABLE|Les modèles peuvent être créés à partir de[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures.<br /><br /> [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]<br /><br /> Oracle 9.2.0.3 ou version ultérieure<br /><br /> Teradata V14, v13, v12 et v6.2|N|N|  
|Base de données multidimensionnelle SAP|SAP BW|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Consultez la documentation SAP pour la prise en charge de plateforme.|N/A|NON APPLICABLE|SAP BW 7.0-7.5|O|N/A|  
|[!INCLUDE[extEssbase](../../includes/extessbase-md.md)]|[Hyperion Essbase](#Hyperion)|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Consultez la documentation Hyperion pour la prise en charge de plateforme.|O|N/A|[!INCLUDE[extEssbase](../../includes/extessbase-md.md)] 9.3.1|O|N/A|  
|Base de données relationnelle Oracle|[Oracle](#OracleClient)|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Nécessite les composants clients Oracle 12c ou supérieurs.|O|N/A|Oracle 11g, 11g R2, 12c, 18c, 19c|O|O|  
|Teradata |[Teradata](#Teradata)|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Étend le fournisseur de données .NET pour Teradata de Teradata.<br /><br /> Requiert le fournisseur de données .NET pour Teradata de Teradata.<br /><br /> Consultez la documentation Teradata pour la prise en charge de plateforme.|O|N/A|Teradata v15<br /><br />Teradata v14<br /><br /> Teradata v13|O|N|  
|Base de données relationnelle DB2|Nom d'extension de données inscrite personnalisée||Host Integration (HI) Server 2004<br /><br /> |O|N/A|NON APPLICABLE|O|N|  
|Source de données OLE DB générique|OLEDB|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Toutes les sources de données qui prennent en charge OLE DB.<br /><br /> Consultez la documentation de la source de données pour la prise en charge de plateforme.|O|N/A|Toutes les sources de données qui prennent en charge OLE DB. Consultez [Remarque](#OLEDBStandard).|O|N/A|  
|Source de données ODBC générique|[ODBC](#ODBCGeneric)|Extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] intégrée|Toutes les sources de données qui prennent en charge ODBC.<br /><br /> Consultez la documentation de la source de données pour la prise en charge de plateforme.|O|N/A|Toutes les sources de données qui prennent en charge ODBC. Consultez [Remarque](#ODBCGeneric).|O|O|  
  
 Pour plus d’informations sur l’utilisation de sources de données externes, consultez [Ajouter des données à partir de sources de données externes &#40;SSRS&#41;](../../reporting-services/report-data/add-data-from-external-data-sources-ssrs.md).  
  
 De nombreux fournisseurs de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] standard sont disponibles auprès de fournisseurs tiers. Pour plus d'informations, recherchez les sites Web ou forums de fournisseurs tiers.  
  
 Pour installer et inscrire une extension pour le traitement des données personnalisée ou un fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] standard, consultez la documentation de référence du fournisseur de données. Pour plus d’informations, consultez [Inscrire un fournisseur de données .NET Framework standard &#40;SSRS&#41;](../../reporting-services/report-data/register-a-standard-net-framework-data-provider-ssrs.md).  
  
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
## <a name="reporting-services-data-processing-extensions"></a>Extensions pour le traitement des données Reporting Services  
 Les extensions pour le traitement des données suivantes sont automatiquement installées avec [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)]. Pour plus d’informations, et pour vérifier l’installation, consultez [Fichier de configuration RSReportDesigner](../../reporting-services/report-server/rsreportdesigner-configuration-file.md) et [Fichier de configuration RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).  
  
> [!NOTE]
>  L’extension pour le traitement des données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] n’est pas prise en charge actuellement.  
  
 Pour plus d'informations sur les extensions pour le traitement des données prises en charge par le Générateur de rapports, consultez [Créer des chaînes de connexion de données - Générateur de rapports et SSRS](data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).
  
###  <a name="microsoft-sql-server-data-processing-extension"></a><a name="MicrosoftSQLServer"></a> Extension pour le traitement des données Microsoft SQL Server  
 Le type de source de données **Microsoft SQL Server** inclut et étend le fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette extension pour le traitement des données est compilée en mode natif pour les plateformes x86 et [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]et s’exécute sur celles-ci.  
  
 Dans [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)], le concepteur de requêtes associé à cette extension de données est le Concepteur Visual Database Tools. Si vous utilisez le concepteur de requêtes en mode graphique, la requête est analysée, voire réécrite. Utilisez le concepteur de requêtes textuel pour contrôler la syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] exacte utilisée pour une requête. Pour plus d’informations, consultez [Interface utilisateur du concepteur de requêtes graphique](../../reporting-services/report-data/graphical-query-designer-user-interface.md).  
  
 Pour plus d’informations, consultez [Type de connexion SQL Server &#40;SSRS&#41;](../../reporting-services/report-data/sql-server-connection-type-ssrs.md).  
  
 Dans le Générateur de rapports, le Concepteur de requêtes associé à cette extension de données est le Concepteur de requêtes relationnelles. 
  
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
###  <a name="microsoft-azure-sql-database-processing-extension"></a><a name="Azure"></a> Extension de traitement Microsoft Azure SQL Database  
 Le type de source de données **Microsoft Azure[!INCLUDE[ssSDS](../../includes/sssds-md.md)]** inclut et étend le fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Dans [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)], le concepteur de requêtes graphique associé à cette extension de données est le Concepteur de requêtes relationnelles et non le Concepteur Visual Database Tools que vous utilisez avec le type de source de données **Microsoft SQL Server**.  
  
 [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)] différencie automatiquement les types de sources de données **Microsoft Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)]** et **Microsoft SQL Server** et ouvre le concepteur de requêtes graphique associé au type de source de données.  
  
 Si vous utilisez le concepteur de requêtes en mode graphique, la requête est analysée, voire réécrite. Un concepteur de requêtes textuel est également disponible pour l'écriture de requêtes. Utilisez le concepteur de requêtes textuel pour contrôler la syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] exacte utilisée pour une requête.   
  
 La récupération des données à partir de [!INCLUDE[ssSDS](../../includes/sssds-md.md)], Azure Synapse Analytics et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est semblable, mais quelques spécifications s’appliquent uniquement à [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Pour plus d’informations, consultez [Type de connexion SQL Azure &#40;SSRS&#41;](../../reporting-services/report-data/sql-azure-connection-type-ssrs.md).  
  
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
###  <a name="microsoft-sql-server-parallel-data-warehouse-processing-extension"></a><a name="PWD"></a> Extension pour le traitement des données Microsoft SQL Server Parallel Data Warehouse  
Cette source de données a été dépréciée. Utilisez le type de source de données SQL Server pour vous connecter à Microsoft Analytics Platform System (APS).
  
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
###  <a name="microsoft-sql-server-analysis-services-data-processing-extension"></a><a name="AnalysisServices"></a> Extension pour le traitement des données Microsoft SQL Server Analysis Services  
 Quand vous sélectionnez un type de source de données **Microsoft SQL Server Analysis Services**, vous sélectionnez une extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] qui étend le fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] pour [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Cette extension pour le traitement des données est compilée en mode natif pour les plateformes x86 et s'exécute sur celles-ci.  
  
 Ce fournisseur de données fait appel au modèle d'objet ADOMD.NET pour créer des requêtes qui utilisent XML for Analysis (XMLA) version 1.1. Les résultats sont retournés sous la forme d'un ensemble de lignes aplati. Pour plus d’informations, consultez [Type de connexion Analysis Services pour MDX &#40;SSRS&#41;](../../reporting-services/report-data/analysis-services-connection-type-for-mdx-ssrs.md), [Type de connexion Analysis Services pour DMX &#40;SSRS&#41;](../../reporting-services/report-data/analysis-services-connection-type-for-dmx-ssrs.md), [Interface utilisateur du Concepteur de requêtes MDX Analysis Services](../../reporting-services/report-data/analysis-services-mdx-query-designer-user-interface.md) et [Interface utilisateur du Concepteur de requêtes DMX Analysis Services](../../reporting-services/report-data/analysis-services-dmx-query-designer-user-interface.md). 
 
 Pour les sources de données de jeu de données Azure Analysis Services et Power BI Premium, notez que l’authentification multifacteur est désactivée pour les informations d’identification utilisées pour la connexion à la source de données. Si l’authentification multifacteur doit être activée pour votre environnement, consultez <a href="/azure/active-directory/conditional-access/overview">Accès conditionnel Azure Active Directory</a> en tant qu’option de désactivation de l’authentification multifacteur pour les informations d’identification utilisées dans la source de données.
  
 Quand vous utilisez un jeu de données Power BI Premium comme source de données, seuls le mode Importation et DirectQuery sont pris en charge.
  
 Au moment de la connexion à une source de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], l’extension pour le traitement des données [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prend en charge des paramètres à valeurs multiples et mappe les propriétés de cellule et de membre à des propriétés étendues prises en charge par [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Pour plus d’informations, consultez [Propriétés de champ étendues pour une base de données Analysis Services &#40;SSRS&#41;](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md).  
  
 Vous pouvez également créer des modèles à partir de sources de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
###  <a name="ole-db-data-processing-extension"></a><a name="OLEDBAll"></a> OLE DB Data Processing Extension  
 L'extension pour le traitement des données OLE DB nécessite de choisir une couche supplémentaire de fournisseur de données basée sur la version de la source de données que vous utilisez dans votre rapport. Si vous ne choisissez pas un fournisseur de données spécifique, un fournisseur par défaut est fourni. Sélectionnez un fournisseur de données spécifique dans la boîte de dialogue **Propriétés de connexion** accessible via le bouton **Modifier** dans la boîte de dialogue Source de données ou Source de données partagée.  
  
 Pour plus d’informations sur le concepteur de requêtes associé OLE DB, consultez [Interface utilisateur du concepteur de requêtes graphique](../../reporting-services/report-data/graphical-query-designer-user-interface.md). Pour plus d’informations sur la prise en charge spécifique pour les fournisseurs de données OLE DB, consultez [Visual Studio .NET Designer Tool Supports Specific OLE DB Providers](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/811241) (L’outil de conception Visual Studio .NET prend en charge des fournisseurs OLE DB spécifiques) dans la Base de connaissances [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
####  <a name="ole-db-for-sql-server"></a><a name="OLEDBSQL"></a> OLE DB pour SQL Server  
 Quand vous sélectionnez un type de source de données **OLE DB**, vous sélectionnez une extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] qui étend le fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] pour OLE DB. Cette extension pour le traitement des données est compilée en mode natif pour les plateformes x86 et x64 et s'exécute sur celles-ci.  
  
 Pour plus d’informations, consultez [Type de connexion OLE DB &#40;SSRS&#41;](../../reporting-services/report-data/ole-db-connection-type-ssrs.md).  
  
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
#### <a name="ole-db-for-olap-70"></a>OLE DB pour OLAP 7.0  
 Le fournisseur OLE DB pour OLAP Services 7.0 n'est pas pris en charge.  
  
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
####  <a name="ole-db-for-oracle"></a><a name="OracleOLEDB"></a> OLE DB pour Oracle  
 L'extension pour le traitement des données OLE DB pour Oracle ne prend pas en charge les types de données Oracle suivants : BLOB, CLOB, NCLOB, BFILE, UROWID.  
  
 Les paramètres sans nom qui dépendent de la position sont pris en charge. Les paramètres nommés ne sont pas pris en charge par cette extension. Pour utiliser des paramètres nommés, utilisez l’extension pour le traitement des données [Oracle](#OracleClient) .  
  
 Pour plus d’informations sur la configuration d’Oracle comme source de données, consultez [Comment utiliser Reporting Services pour configurer une source de données Oracle et y accéder](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/834305). Pour plus d’informations sur la configuration d’autorisations supplémentaires, consultez [How to add permissions for the NETWORK SERVICE security principal](https://mskb.pkisolutions.com/kb/870668) (Comment ajouter des autorisations pour le principal de sécurité SERVICE RÉSEAU) dans la Base de connaissances [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
####  <a name="ole-db-standard-net-framework-data-provider"></a><a name="OLEDBStandard"></a> Fournisseur de données OLE DB Standard .NET Framework  
 Pour extraire des données d’une source de données qui prend en charge les fournisseurs de données OLE DB [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] , utilisez le type de source de données **OLE DB** et sélectionnez le fournisseur de données par défaut, ou choisissez parmi les fournisseurs de données installés dans la boîte de dialogue **Chaîne de connexion** .  
  
> [!NOTE]  
>  Même si un fournisseur de données prend en charge l'affichage d'un aperçu d'un rapport sur votre client de création de rapports, les fournisseurs de données OLE DB ne sont pas tous conçus pour prendre en charge les rapports publiés sur un serveur de rapports.  
  
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
###  <a name="odbc-data-processing-extension"></a><a name="ODBC"></a> ODBC Data Processing Extension  
 Quand vous sélectionnez un type de source de données **ODBC**, vous sélectionnez une extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] qui étend le fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] pour ODBC. Cette extension pour le traitement des données est compilée en mode natif pour les plateformes x86 et [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)] et s’exécute sur celles-ci. Cette extension vous permet de vous connecter à des sources de données ayant un fournisseur ODBC et d'en récupérer des données.  
  
> [!NOTE]  
>  Même si un fournisseur de données prend en charge l'affichage d'un aperçu d'un rapport sur votre client de création de rapports, les fournisseurs de données ODBC ne sont pas tous conçus pour prendre en charge les rapports publiés sur un serveur de rapports.  
  
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
####  <a name="odbc-standard-net-framework-data-provider"></a><a name="ODBCGeneric"></a> Fournisseur de données ODBC Standard .NET Framework  
 Pour extraire des données d’une source de données qui prend en charge un fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] ODBC standard, utilisez le type de source de données **ODBC** et sélectionnez le fournisseur de données par défaut, ou choisissez parmi les fournisseurs de données installés dans la boîte de dialogue **Chaîne de connexion** .  
  
> [!NOTE]  
>  Même si un fournisseur de données prend en charge l'affichage d'un aperçu d'un rapport sur votre client de création de rapports, les fournisseurs de données ODBC ne sont pas tous conçus pour prendre en charge les rapports publiés sur un serveur de rapports.  
  
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
###  <a name="oracle-data-processing-extension"></a><a name="OracleClient"></a> Extension pour le traitement des données Oracle  
 Lorsque vous sélectionnez le type de source de données **Oracle**, vous sélectionnez un extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] qui utilise le fournisseur de données Oracle directement et non plus par le biais de System.Data.OracleClient. Pour extraire des données de rapport dans une base de données Oracle, votre administrateur doit installer les outils clients Oracle. La version de l’application cliente doit être 11g ou supérieure. Ces outils doivent être installés sur le client de création de rapport pour obtenir un aperçu des rapports et sur le serveur de rapports pour afficher les rapports publiés.  
 
Pour installer les outils clients Oracle, vous pouvez procéder comme suit :
 
1.  Accédez au [site de téléchargement Oracle](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html).
2.  Téléchargez ODAC 12C Release 4 (12.1.0.2.4) pour Windows (64 bits pour le serveur, 32 bits pour les outils).
3.  Installez le fournisseur de données pour .NET 4.
  
 Les paramètres nommés sont pris en charge par cette extension. Pour Oracle version 11g ou supérieure, les paramètres à valeurs multiples sont pris en charge. Pour les paramètres sans nom et qui dépendent de la position, utilisez l’extension pour le traitement des données OLE DB avec le fournisseur de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB pour Oracle. Pour plus d’informations sur la configuration d’Oracle comme source de données, consultez [Comment utiliser Reporting Services pour configurer une source de données Oracle et y accéder](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/834305). Pour plus d’informations sur la configuration d’autorisations supplémentaires, consultez [How to add permissions for the NETWORK SERVICE security principal](https://mskb.pkisolutions.com/kb/870668) (Comment ajouter des autorisations pour le principal de sécurité SERVICE RÉSEAU) dans la Base de connaissances [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 Vous pouvez extraire des données dans les procédures stockées avec plusieurs paramètres d'entrée, mais la procédure stockée ne doit retourner qu'un seul curseur de sortie. Pour plus d’informations, consultez [Retour de résultats avec Oracle REF CURSOR](/dotnet/framework/data/adonet/retrieving-data-using-a-datareader#returning-results-with-oracle-ref-cursors) dans « Récupérer des données à l’aide d’un DataReader ».
  
 Pour plus d’informations, consultez [Type de connexion Oracle &#40;SSRS&#41;](../../reporting-services/report-data/oracle-connection-type-ssrs.md). Pour plus d’informations sur le concepteur de requêtes associé, consultez [Interface utilisateur du concepteur de requêtes graphique](../../reporting-services/report-data/graphical-query-designer-user-interface.md).  
  
 Vous pouvez également créer des modèles basés sur une base de données Oracle.  
  
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
###  <a name="teradata-data-processing-extension"></a><a name="Teradata"></a> Extension pour le traitement des données Teradata  
 Quand vous sélectionnez un type de source de données **Teradata**, vous sélectionnez une extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] qui étend le fournisseur de données .NET Framework pour Teradata. Pour récupérer des données de rapport de Teradata, l’administrateur système doit installer le fournisseur de données .NET Framework pour Teradata sur le client de création de rapports afin de modifier et d’afficher un aperçu des rapports sur le client, ainsi que sur le serveur de rapports pour afficher les rapports publiés.  
  
 Pour les projets Report Server, aucun concepteur de requêtes graphique n'est disponible pour cette extension. Vous devez utiliser le concepteur de requêtes textuel pour créer les requêtes.  
  
 Le tableau suivant indique les versions du fournisseur de données .NET pour Teradata prises en charge pour la définition d’une source de données dans une définition de rapport dans [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)]:  
  
|Version [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)]|Version Teradata|Version du fournisseur de données .NET Framework pour Teradata|  
|-----------------------------------|-------------------------------|-------------------------------------------------------|    
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|12,00|12.00.01|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|6.20|12.00.01|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|13.00|13.0.0.1|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|12,00|12.00.01|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|6.20|12.00.01|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|13.00|13.0.0.1|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|6.20|12.00.01|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|12,00|12.00.01|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|13.00|13.0.0.1|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|14.00|14.00.01| 
|SQL Server 2016|13.00|13.0.0.1|  
|SQL Server 2016|14.00|14.00.01|
|SQL Server 2016|15,00|15.00.01| 
  
 Les paramètres à valeurs multiples sont pris en charge par cette extension. Les macros peuvent être spécifiées dans une requête en utilisant la commande EXECUTE en mode de requête TEXT.  
  
 Pour plus d’informations, consultez [Type de connexion Teradata &#40;SSRS&#41;](../../reporting-services/report-data/teradata-connection-type-ssrs.md).  
 
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
###  <a name="sharepoint-list-data-extension"></a><a name="SharePointList"></a> Extension de données Liste SharePoint  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] inclut l’extension de données Liste SharePoint [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] afin que vous puissiez utiliser les listes SharePoint comme une source de données dans un rapport. Vous pouvez récupérer des données de la liste à partir de :  
  
-   Serveur SharePoint 2016  

-   [!INCLUDE[SPS2013](../../includes/sps2013-md.md)]  
  
 Il existe trois implémentations du fournisseur de données Liste SharePoint.  
  
1.  À partir d’un environnement de création de rapports tel que Générateur de rapports ou le Concepteur de rapports dans [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)], ou pour un serveur de rapports configuré en mode natif, les données de liste proviennent du service web Lists.asmx pour le site SharePoint.  
  
2.  Sur un serveur de rapports configuré en mode intégré SharePoint, les données de liste proviennent du service Web Lists.asmx correspondant ou d'appels de programmation à l'API SharePoint. Ce mode vous permet de récupérer des données de liste d'une batterie de serveurs SharePoint.  
  
3.  Pour [!INCLUDE[SPS2013](../../includes/sps2013-md.md)] et SharePoint Server 2016, le complément [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour [!INCLUDE[msCoName](../../includes/msconame-md.md)] SharePoint Technologies vous permet de récupérer des données de liste d’un service web Lists.asmx pour un site SharePoint, ou d’un site SharePoint qui fait partie d’une batterie de serveurs SharePoint. Ce scénario est également appelé *mode local* , car un serveur de rapports n’est pas obligatoire.  
  
 Les informations d'identification que vous pouvez spécifier dépendent de l'implémentation que l'application cliente utilise. Pour plus d’informations, consultez [Type de connexion de liste SharePoint &#40;SSRS&#41;](../../reporting-services/report-data/sharepoint-list-connection-type-ssrs.md).  
  
###  <a name="xml-data-processing-extension"></a><a name="XML"></a> Extension pour le traitement des données XML  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] comprend une extension pour le traitement des données XML qui vous permet d’utiliser des données XML dans un rapport. Les données peuvent être récupérées à partir d'un document XML, d'un service Web ou d'une application Web accessible via une URL. Pour plus d’informations, consultez [Type de connexion XML &#40;SSRS&#41;](../../reporting-services/report-data/xml-connection-type-ssrs.md). Pour plus d’informations sur le concepteur de requêtes associé, consultez la section relative au concepteur de requêtes textuel dans [Interface utilisateur du concepteur de requêtes graphique](../../reporting-services/report-data/graphical-query-designer-user-interface.md).
  
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
###  <a name="sap-bw-data-processing-extension"></a><a name="SAPBW"></a> Extension pour le traitement des données SAP BW  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] inclut une extension pour le traitement des données qui vous permet d’utiliser des données provenant d’une source de données SAP BW dans un rapport.
  
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
###  <a name="hyperion-essbase-business-intelligence-data-processing-extension"></a><a name="Hyperion"></a> Extension pour le traitement des données Hyperion Essbase Business Intelligence  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] inclut une extension pour le traitement des données qui vous permet d’utiliser des données provenant d’une source de données [!INCLUDE[extEssbase](../../includes/extessbase-md.md)] dans un rapport.  
  
 Pour plus d’informations, consultez [Type de connexion Hyperion Essbase &#40;SSRS&#41;](../../reporting-services/report-data/hyperion-essbase-connection-type-ssrs.md). Pour plus d’informations sur le concepteur de requêtes associé, consultez [Interface utilisateur du Concepteur de requêtes Hyperion Essbase](../../reporting-services/report-data/hyperion-essbase-query-designer-user-interface.md).  
  
 Pour plus d’informations sur [!INCLUDE[extEssbase](../../includes/extessbase-md.md)], consultez [Utilisation de SQL Server Reporting Services avec Hypérion Essbase](../../reporting-services/report-data/hyperion-essbase-query-designer-user-interface.md). 
  
 [Retourner à la table des sources de données](#DataSourcesTable)  
  
## <a name="see-also"></a>Voir aussi  
 [Créer des chaînes de connexion de données - Générateur de rapports et SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)   
 [Datasets de rapport &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)  
D’autres questions ? [Essayez le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
  
