---
title: Déploiement d’un assembly personnalisé | Microsoft Docs
description: Découvrez comment déployer un assembly personnalisé dans SQL Server Reporting Services. Découvrez également comment accorder des privilèges d’assemblys personnalisés au-delà des autorisations d’exécution.
ms.date: 11/23/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-assemblies
ms.topic: reference
helpviewer_keywords:
- deploying custom assemblies [Reporting Services]
- custom assemblies [Reporting Services], deploying
- custom assemblies [Reporting Services], updating
- updating custom assemblies
ms.assetid: c6674cd8-0de7-4a5a-9e7c-12ffa49f6fd2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 9aa3a01e88b699f184ea05dfcb3650b4653837cb
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595503"
---
# <a name="deploying-a-custom-assembly"></a>Déploiement d'un assembly personnalisé
  Pour déployer un assembly personnalisé dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], placez-le dans les dossiers d’application du Concepteur de rapports et du serveur de rapports. L’autorisation **Execution** dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] est accordée par défaut aux assemblys personnalisés. Pour accorder des privilèges d’assemblys personnalisés supérieurs à l’autorisation Execute, vous devez modifier le fichier de configuration rssrvpolicy.config pour le serveur de rapports et le fichier de configuration rspreviewpolicy.config pour la fenêtre d’aperçu du Concepteur de rapports. Une autre solution consiste à installer votre assembly personnalisé dans le Global Assembly Cache (GAC).  
  
> [!NOTE]  
>  Il y a deux modes d’aperçu pour le Concepteur de rapports : l’onglet Aperçu et la fenêtre d’aperçu contextuelle, qui s’affiche quand votre projet de rapport démarre en mode **DebugLocal**. L’onglet Aperçu exécute toutes les expressions de rapport à l’aide de l’autorisation **FullTrust** définie et n’applique pas de paramètres de stratégie de sécurité. La fenêtre d'aperçu contextuelle a pour but de simuler les fonctionnalités du serveur de rapports ; par conséquent, elle dispose d'un fichier de configuration de stratégie qu'un administrateur ou vous-même devez modifier pour utiliser des assemblys dans le Générateur de rapports. Cet aperçu contextuel verrouille également l'assembly personnalisé. Par conséquent, vous devez fermer la fenêtre d'aperçu afin de modifier ou mettre à jour le code de votre assembly personnalisé.  
  
## <a name="to-deploy-a-custom-assembly-in-reporting-services"></a>Pour déployer un assembly personnalisé dans Reporting Services  
  
1.  Copiez votre assembly personnalisé à partir de votre emplacement de génération vers le dossier bin du serveur de rapports ou le dossier du Générateur de rapports. 

     Si vous placez votre assembly personnalisé dans le dossier bin du serveur de rapports, vous pouvez publier des rapports qui font référence à votre assembly personnalisé. L’emplacement par défaut du dossier bin du serveur de rapports est le suivant :

     Reporting Services 2016
     ```
     %ProgramFiles%\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer\bin
     ```
     Reporting Services 2017 et versions ultérieures
     ```
     %ProgramFiles%\Microsoft SQL Server Reporting Services\SSRS\ReportServer\bin
     ```
     Si vous le placez dans le dossier du Concepteur de rapports, vous pouvez exécuter et déboguer les rapports qui font référence à votre assembly personnalisé dans le Concepteur de rapports. L'emplacement par défaut du Concepteur de rapports est le suivant :

     Visual Studio 2012
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\PrivateAssemblies
     ```
     Visual Studio 2013
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\PrivateAssemblies
     ```
     Visual Studio 2015
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\PrivateAssemblies
     ```
     Visual Studio 2017
     ```
     C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS
     ```
     Visual Studio 2019
     ```
     C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS
     ```  
2.  Ouvrez le fichier de configuration approprié. L’emplacement par défaut de rssrvpolicy.config du serveur de rapports est le suivant :

     Reporting Services 2016
     ```
     %ProgramFiles%\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer
     ```
     Reporting Services 2017 et versions ultérieures
     ```
     %ProgramFiles%\Microsoft SQL Server Reporting Services\SSRS\ReportServer
     ```
     Les fichiers à mettre à jour pour le Concepteur de rapports sont :

     Visual Studio 2012
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\PrivateAssemblies\PreviewProcessingService.exe.config
     C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\PrivateAssemblies\RSPreviewPolicy.config
     C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\PrivateAssemblies\RSReportDesigner.config
     C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\PrivateAssemblies\RSReportHost11.exe.config
     ```
     Visual Studio 2013
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\PrivateAssemblies\PreviewProcessingService.exe.config
     C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\PrivateAssemblies\RSPreviewPolicy.config
     C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\PrivateAssemblies\RSReportDesigner.config
     C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\PrivateAssemblies\RSReportHost.exe.config
     ```
     Visual Studio 2015
     ```
     C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\PrivateAssemblies\PreviewProcessingService.exe.config
     C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\PrivateAssemblies\RSPreviewPolicy.config
     C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\PrivateAssemblies\RSReportDesigner.config
     C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\PrivateAssemblies\RSReportHost.exe.config
     ```
     Visual Studio 2017
     ```
     C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\PreviewProcessingService.exe.config
     C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSPreviewPolicy.config
     C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSReportDesigner.config
     C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSReportHost.exe.config
     ```
     Visual Studio 2019
     ```
     C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\PreviewProcessingService.exe.config
     C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSPreviewPolicy.config
     C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSReportDesigner.config
     C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\SSRS\RSReportHost.exe.config
     ```    
3.  Ajoutez un groupe de codes pour votre assembly personnalisé. Pour plus d’informations, consultez [Développement sécurisé &#40;Reporting Services&#41;](../../reporting-services/extensions/secure-development/secure-development-reporting-services.md).  
  
## <a name="updating-custom-assemblies"></a>Mise à jour d'assemblys personnalisés  
 Au bout d'un certain temps, vous risquez d'avoir besoin de mettre à jour une version d'un assembly personnalisé actuellement référencé par plusieurs rapports publiés. Si cet assembly existe déjà dans le répertoire bin du serveur de rapports ou du Générateur de rapports et si le numéro de version de l'assembly est incrémenté ou a changé d'une certaine façon, les rapports actuellement publiés ne fonctionnent plus correctement. Vous devez mettre à jour la version de l’assembly qui est référencé dans l’élément **CodeModules** de la définition de rapport, puis republier les rapports. Si vous savez que vous allez souvent mettre à jour un assembly personnalisé et que vos rapports actuellement publiés vont avoir besoin de référencer le nouvel assembly, vous pouvez envisager d'utiliser le même numéro de version dans toutes les mises à jour d'un assembly particulier.  
  
 Si vous n'avez pas besoin de vos rapports actuellement publiés pour référencer la nouvelle version de l'assembly, vous pouvez déployer votre assembly personnalisé sur le Global Assembly Cache. Le Global Assembly Cache peut gérer plusieurs versions du même assembly, afin que vos rapports actuels puissent référencer la version antérieure de votre assembly et que vos nouveaux rapports publiés puissent référencer l'assembly mis à jour. Pourtant, une autre approche consisterait à définir la redirection de liaison du serveur de rapports afin de forcer une redirection de toutes les demandes de l'ancien assembly vers le nouvel assembly. Il vous faudrait modifier le fichier Web.config du serveur de rapports et le fichier ReportingServicesService.exe.config du serveur de rapports. L'entrée pourrait se présenter de la manière suivante :  
  
```  
<configuration>  
   <runtime>  
      <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">  
         <dependentAssembly>  
            <assemblyIdentity name="myAssembly"  
                              publicKeyToken="32ab4ba45e0a69a1"  
                              culture="neutral" />  
            <bindingRedirect oldVersion="1.0.0.0"  
                             newVersion="2.0.0.0"/>  
         </dependentAssembly>  
      </assemblyBinding>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d'assemblys personnalisés avec des rapports](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)   
 [Utilisation d'assemblys et du Global Assembly Cache](/dotnet/framework/app-domains/working-with-assemblies-and-the-gac)  
  
