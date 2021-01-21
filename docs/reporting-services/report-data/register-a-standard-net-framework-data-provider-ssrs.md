---
title: Inscrire un fournisseur de données .NET Framework standard | Microsoft Docs
description: Inscrivez un fournisseur de données .NET Framework standard pour utiliser un fournisseur de données .NET Framework tiers afin de récupérer des données pour un jeu de données de rapport Reporting Services.
ms.date: 05/24/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
helpviewer_keywords:
- reports [Reporting Services], data
- .NET Framework data providers for Reporting Services
- data processing extensions [Reporting Services]
- data providers [Reporting Services]
- data retrieval [Reporting Services]
- Reporting Services, data sources
ms.assetid: d92add64-e93c-4598-8508-55d1bc46acf6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6539c9552206893f5799fbeb30ff628355dd137d
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596941"
---
# <a name="register-a-standard-net-framework-data-provider-ssrs"></a>Inscrire un fournisseur de données .NET Framework standard (SSRS)
  Pour utiliser un fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] tiers afin d’extraire des données pour un dataset de rapport [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vous devez déployer et inscrire l’assembly de fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] à deux emplacements : sur le client de création de rapports et sur le serveur de rapports. Sur le client de création de rapports, vous devez inscrire le fournisseur de données comme type de source des données et l'associer à un concepteur de requêtes. Vous pouvez ensuite sélectionner ce fournisseur de données comme type de source des données lorsque vous créez un dataset de rapport. Le concepteur de requêtes associé s'ouvre pour vous permettre de créer des requêtes pour ce type de source de données. Sur le serveur de rapports, vous devez inscrire le fournisseur de données comme type de source de données. Vous pouvez ensuite traiter les rapports publiés qui extraient les données d'une source de données à l'aide de ce fournisseur de données.  
  
 Les fournisseurs de données tiers ne prennent pas nécessairement en charge toutes les fonctionnalités fournies par les extensions pour le traitement des données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Pour plus d’informations, consultez [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md). Pour en savoir plus sur l’extension des fonctionnalités d’un fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], consultez [Implémentation d’une extension pour le traitement des données](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md).  
  
 Vous devez disposer des informations d'identification de l'administrateur pour installer et inscrire des fournisseurs de données.  
  
## <a name="registering-a-net-framework-data-provider-on-the-report-server"></a>Inscription d'un fournisseur de données .NET Framework sur le serveur de rapports  
 Afin de traiter les rapports publiés qui font appel à ce fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] sur le serveur de rapports, vous devez installer l'assembly sur le serveur de rapports. Vous devez modifier deux fichiers de configuration. Modifiez le fichier rsreportserver.config pour inscrire le fournisseur de données. Modifiez le fichier rssrvpolicy.config pour octroyer les autorisations de la sécurité d'accès du code pour l'assembly.  
  
#### <a name="to-install-a-data-provider-assembly-on-the-report-server"></a>Pour installer un assembly de fournisseur de données sur le serveur de rapports  
  
1.  Accédez à l’emplacement par défaut du répertoire bin sur le serveur de rapports sur lequel vous souhaitez utiliser le fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] . L’emplacement par défaut du répertoire bin du serveur de rapports est *\<drive>* :\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer\bin.  
  
2.  Copiez votre assembly à partir de votre emplacement sur le répertoire bin du serveur de rapports. Une autre solution consiste à charger votre assembly dans le Global Assembly Cache (GAC). Pour plus d’informations, consultez [Utilisation d’assemblys et du Global Assembly Cache](/dotnet/framework/app-domains/working-with-assemblies-and-the-gac) dans la documentation du SDK [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] sur MSDN.  
  
#### <a name="to-register-a-net-data-provider-on-the-report-server"></a>Pour inscrire un fournisseur de données .NET sur le serveur de rapports  
  
1.  Faites une sauvegarde du fichier RSReportServer.config dans le répertoire parent ReportServer du répertoire bin.  
  
2.  Ouvrez RSReportServer.config. Vous pouvez ouvrir le fichier de configuration à l’aide de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] ou à l’aide d’un simple éditeur de texte tel que le Bloc-notes.  
  
3.  Localisez l’élément **Data** dans le fichier RSReportServer.config. Effectuez une entrée pour le fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] à l'emplacement suivant :  
  
    ```  
    <Extensions>  
       <Data>  
          <Extension Your data provider configuration information goes here />  
       </Data>  
    </Extensions>  
    ```  
  
4.  Ajoutez une entrée pour le fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] .  
  
    |Attribut|Description|  
    |---------------|-----------------|  
    |**Nom**|Donnez un nom unique au fournisseur de données, par exemple, **MyNETDataProvider**. La longueur maximale de l'attribut **Name** est de 255 caractères. Le nom doit être unique au sein de toutes les entrées de l’élément **Extension** d’un fichier de configuration. La valeur que vous insérez à cet emplacement s’affiche dans la liste déroulante des types de source de données quand vous créez une source de données.|  
    |**Type**|Entrez une liste séparée par des virgules qui comprend l’espace de noms complet de la classe qui implémente l’interface <xref:System.Data.IDbConnection> suivie du nom de l’assembly du fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] (sans compter l’extension de nom de fichier .dll).|  
  
     Par exemple, l'entrée peut ressembler à ce qui suit pour un fichier .dll déployé vers le répertoire bin du serveur de rapports :  
  
    ```  
    <Extension Name="MyNETDataProvider" Type="CompanyName.ExtensionName.DataProviderConnectionClass, DataProviderAssembly" />   
    ```  
  
     si vous chargez votre assembly dans le GAC (Global Assembly Cache), vous devez fournir les propriétés de nom fort. Par exemple :  
  
    ```  
    <Extension Name="MyNETDataProvider" Type="CompanyName.ExtensionName.DataProviderConnectionClass, DataProviderAssembly,Version=1.0.0.0, Culture=neutral, PublicKeyToken=MyPublicToken"/>  
    ```  
  
#### <a name="to-set-the-code-group-policy-for-a-net-data-provider"></a>Pour définir la stratégie du groupe de codes pour un fournisseur de données .NET  
  
1.  Faites une copie de sauvegarde du fichier rssrvpolicy.config dans le répertoire parent ReportServer du répertoire bin.  
  
2.  Ouvrez rssrvpolicy.config. Vous pouvez ouvrir le fichier de configuration à l'aide de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] ou à l'aide d'un simple éditeur de texte tel que le Bloc-notes.  
  
3.  Localisez l’élément **CodeGroup** dans le fichier rssrvpolicy.config.  
  
4.  Ajoutez un groupe de codes pour l’assembly de fournisseur de données qui octroie l’autorisation **FullTrust** . Votre groupe de codes doit ressembler au code suivant :  
  
    ```  
    <CodeGroup class="UnionCodeGroup"  
       version="1"  
       PermissionSetName="FullTrust"  
       Name="ThisDataProviderCodeGroup"  
       Description="Code group for the .NET data provider">  
          <IMembershipCondition class="UrlMembershipCondition"  
             version="1"  
             Url=  
    "C:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer\bin\DataProviderAssembly.dll"  
           />  
    </CodeGroup>  
    ```  
  
 L'appartenance d'URL n'est qu'une des nombreuses conditions d'appartenance que vous pouvez sélectionner pour le fournisseur de données.  
  
### <a name="verifying-the-deployment-and-registration"></a>Vérification du déploiement et de l'inscription  
 Vous pouvez vérifier si le fournisseur de données a été correctement déployé vers le serveur de rapports en ouvrant le portail web et en vérifiant que le fournisseur de données est inclut dans la liste des sources de données disponibles. Pour plus d’informations sur le portail web et les sources de données, consultez [Créer, modifier et supprimer des sources de données partagées &#40;SSRS&#41;](../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md).  
  
## <a name="registering-a-net-framework-data-provider-on-the-report-designer-client"></a>Inscription d'un fournisseur de données .NET Framework sur le client du Concepteur de rapports  
 Afin de créer des rapports qui utilisent ce fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] pour une source de données, vous devez installer l'assembly sur votre ordinateur client qui exécute le Concepteur de rapports. Vous devez modifier deux fichiers de configuration. Modifiez le fichier RSReportDesigner.config pour inscrire le fournisseur de données comme source de données et pour utiliser le concepteur de requêtes générique. Modifiez le fichier RSPreviewPolicy.config pour octroyer les autorisations de la sécurité d'accès du code pour l'assembly de fournisseur de données.  
  
#### <a name="to-install-a-data-provider-assembly-on-the-report-designer-client"></a>Pour installer un assembly de fournisseur de données sur le client du Concepteur de rapports  
  
1.  Accédez à l’emplacement par défaut du répertoire PrivateAssemblies sur le client du Concepteur de rapports sur lequel vous souhaitez utiliser le fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] . L’emplacement par défaut du répertoire PrivateAssemblies est *\<drive>* :\Program Files\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies.  
  
2.  Copiez votre assembly à partir de votre emplacement sur le répertoire PrivateAssemblies du client du Concepteur de rapports. Une autre solution consiste à charger votre assembly dans le Global Assembly Cache (GAC). Pour plus d’informations, consultez [Utilisation d’assemblys et du Global Assembly Cache](/dotnet/framework/app-domains/working-with-assemblies-and-the-gac) dans la documentation du SDK [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] sur MSDN.  
  
#### <a name="to-register-a-net-data-provider-on-the-report-designer-client"></a>Pour inscrire un fournisseur de données .NET sur le client du Concepteur de rapports  
  
1.  Effectuez une copie de sauvegarde du fichier RSReportDesigner.config dans le répertoire PrivateAssemblies.  
  
2.  Ouvrez le fichier RSReportDesigner.config à l'aide de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] ou à l'aide d'un simple éditeur de texte tel que le Bloc-notes.  
  
3.  Localisez l’élément **Data** dans le fichier RSReportDesigner.config. Effectuez une entrée pour le fournisseur de données à l'emplacement suivant :  
  
    ```  
    <Extensions>  
       <Data>  
          <Extension Your data provider configuration information goes here />  
       </Data>  
    </Extensions>  
    ```  
  
4.  Ajoutez une entrée pour le fournisseur de données.  
  
    |Attribut|Description|  
    |---------------|-----------------|  
    |**Nom**|Donnez un nom unique au fournisseur de données, par exemple, **MyNETDataProvider**. La longueur maximale de l'attribut **Name** est de 255 caractères. Le nom doit être unique au sein de toutes les entrées de l’élément **Extension** d’un fichier de configuration. La valeur que vous insérez à cet emplacement s'affiche dans la liste déroulante des types de source de données lorsque vous créez une nouvelle source de données.|  
    |**Type**|Entrez une liste séparée par des virgules qui comprend l’espace de noms complet de la classe qui implémente l’interface <xref:System.Data.IDbConnection> suivie du nom de l’assembly du fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] (sans compter l’extension de nom de fichier .dll).|  
  
     Par exemple, l’entrée peut ressembler à ce qui suit pour un fichier .dll déployé vers le répertoire PrivateAssemblies [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] :  
  
    ```  
    <Extension Name="MyNETDataProvider" Type="CompanyName.ExtensionName.DataProviderConnectionClass, DataProviderAssembly" />   
    ```  
  
     si vous chargez votre assembly dans le GAC, vous devez fournir les propriétés de nom fort. Par exemple :  
  
    ```  
    <Extension Name="MyNETDataProvider" Type="CompanyName.ExtensionName.DataProviderConnectionClass, DataProviderAssembly, Version=1.0.0.0, Culture=neutral, PublicKeyToken=MyPublicToken"/>  
    ```  
  
5.  Localisez l’élément **Designer** dans le fichier RSReportDesigner.config. Effectuez une entrée pour le fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] à l'emplacement suivant :  
  
    ```  
    <Extensions>  
       <Designer>  
          <Your data provider configuration information goes here>  
       </Designer>  
    </Extensions>  
    ```  
  
6.  Ajoutez l’entrée suivante au fichier RSReportDesigner.config sous l’élément **Designer** . Il vous suffit de remplacer l’attribut **Name** par le nom que vous avez fourni dans les entrées précédentes.  
  
    ```  
    <Extension Name="MyNETDataProvider" Type="Microsoft.ReportingServices.QueryDesigners.GenericQueryDesigner,Microsoft.ReportingServices.QueryDesigners"/>  
    ```  
  
#### <a name="to-set-the-code-group-policy-for-a-net-data-provider-on-the-report-designer-client"></a>Pour définir la stratégie du groupe de codes pour un fournisseur de données .NET sur le client du Concepteur de rapports  
  
1.  Effectuez une copie de sauvegarde du fichier RSPreviewPolicy.config dans le répertoire PrivateAssemblies.  
  
2.  Ouvrez le fichier RSPreviewPolicy.config à l’aide de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] ou d’un simple éditeur de texte tel que le Bloc-notes.  
  
3.  Localisez l’élément **CodeGroup** dans le fichier RSPreviewPolicy.config.  
  
4.  Ajoutez un groupe de codes pour l’assembly de fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] qui octroie l’autorisation **FullTrust** . Votre groupe de codes doit ressembler au code suivant :  
  
    ```  
    <CodeGroup class="UnionCodeGroup"  
       version="1"  
       PermissionSetName="FullTrust"  
       Name="ThisDataProviderCodeGroup"  
       Description="Code group for the .NET data provider">  
          <IMembershipCondition class="UrlMembershipCondition"  
             version="1"  
             Url=  
    " C:\Program Files\Microsoft Visual Studio 9\Common7\IDE\PrivateAssemblies\DataProviderAssembly.dll"  
           />  
    </CodeGroup>  
    ```  
  
 L'appartenance d'URL n'est qu'une des nombreuses conditions d'appartenance que vous pouvez sélectionner pour le fournisseur de données.  
  
### <a name="verifying-the-deployment-and-registration-on-the-report-designer-client"></a>Vérification du déploiement et de l'inscription du client du Concepteur de rapports  
 Avant de vérifier le déploiement, vous devez fermer toutes les instances de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] sur votre ordinateur local. Une fois que vous avez clôturé toutes les sessions en cours, vous pouvez vérifier si le fournisseur de données s’est déployé correctement dans le Concepteur de rapports en créant un projet de rapport dans [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. Le fournisseur de données doit être inclus dans la liste des types de source de données disponible lorsque vous créez un nouveau jeu de données pour votre rapport.  
  
## <a name="platform-considerations"></a>Considérations relatives à la plateforme  
 Sur une plateforme 64 bits (x64), [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] s'exécute en mode WOW 32 bits. Lorsque vous publiez des rapports sur une plateforme x64, vous devez disposer de fournisseurs de données 32 bits sur le client de création de rapports pour prévisualiser vos rapports. Si vous publiez le rapport sur le même système, vous devez disposer de fournisseurs de données x64 pour afficher le rapport dans le portail web.  
  
 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] n’est pas pris en charge pour les plateformes [!INCLUDE[vcpritanium](../../includes/vcpritanium-md.md)].  
  
 Les extensions pour le traitement des données installées à l’aide de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] doivent être compilées en mode natif pour chaque plateforme et installées dans les emplacements corrects. Si vous inscrivez un fournisseur de données personnalisé ou un fournisseur de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] standard, celui-ci doit être compilé en mode natif pour la plateforme appropriée et installé dans les emplacements corrects. Si votre système s'exécute sur une plateforme 32 bits, le fournisseur de données doit être compilé pour une plateforme 32 bits. Si votre système s'exécute sur une plateforme 64 bits, le fournisseur de données doit être compilé pour la plateforme 64 bits. Si vous ne pouvez pas utiliser de fournisseur de données 32 bits intégré à des interfaces 64 bits sur une plateforme 64 bits. Vérifiez les informations sur votre logiciel tiers pour déterminer si le fournisseur de données fonctionne sur la plateforme installée. Pour plus d’informations sur les fournisseurs de données et la prise en charge de plateforme, consultez [Sources de données prises en charge par Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer et administrer un serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md)   
 [Implémentation d’une extension pour le traitement des données](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Fichiers de configuration de Reporting Services](../../reporting-services/report-server/reporting-services-configuration-files.md)   
 [Sécurité d'accès du code dans Reporting Services](../../reporting-services/extensions/secure-development/code-access-security-in-reporting-services.md)  
  
