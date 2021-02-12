---
title: Utilisation des fichiers de stratégie de sécurité Reporting Services | Microsoft Docs
description: En savoir plus sur les fichiers config que Reporting Services utilise pour stocker les informations de stratégie de sécurité pour les assemblys de code. Découvrez comment les modifier.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- code groups [Reporting Services]
- CodeGroup elements
- configuration files [Reporting Services]
- code access security [Reporting Services], security policies
- security policies [Reporting Services]
- security configuration files [Reporting Services]
- named permission sets [Reporting Services]
ms.assetid: 2280fff6-3de7-44b1-87da-5db0ec975928
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e5f82b3db5f52656e88c54062f203d273d8ab24c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065264"
---
# <a name="using-reporting-services-security-policy-files"></a>Utilisation des fichiers de stratégie de sécurité Reporting Services
  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] stocke les informations de stratégie de sécurité dans trois fichiers de configuration qui sont copiés dans le système de fichiers au cours de l'installation. Ces fichiers de configuration peuvent contenir une combinaison de stratégies de sécurité à usage interne et définies par l'utilisateur pour les assemblys de code dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Les trois fichiers de configuration correspondent à trois composants sécurisables dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] : le service Windows Report Server, l'application Web du Gestionnaire de rapports et la fenêtre d'aperçu du Concepteur de rapports.  
  
> [!NOTE]  
>  Il y a deux modes d’aperçu pour le Concepteur de rapports : l’onglet d’aperçu et la fenêtre d’aperçu contextuelle, qui s’affiche quand votre projet de rapport démarre en mode **DebugLocal**. L’onglet **Aperçu** n’est pas un composant sécurisable et n’applique pas de paramètres de stratégie de sécurité. La fenêtre d'aperçu est destinée à simuler les fonctionnalités du serveur de rapports ; par conséquent, elle dispose d'un fichier de configuration de stratégie que vous ou un administrateur devez modifier pour utiliser les assemblys et extensions personnalisés dans le Concepteur de rapports.  
  
 Les fichiers de configuration de stratégie de sécurité contiennent des informations de classe de sécurité, quelques jeux d'autorisations nommés par défaut et les groupes de codes des assemblys dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Les fichiers de configuration de stratégie de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] sont semblables au fichier Security.config qui détermine la hiérarchie des groupes de codes et les jeux d'autorisations associés aux stratégies de niveaux ordinateur et entreprise du [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)]. L'emplacement de ce fichier est C:\WINDOWS\Microsoft.NET\Framework\v2.0.50727\CONFIG\security.config.  
  
## <a name="policy-files-in-reporting-services"></a>Fichiers de stratégie dans Reporting Services  
 Le tableau suivant répertorie les fichiers de configuration de stratégie dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], leurs emplacements (dans une installation par défaut) et leurs fonctions respectives.  
  
|Nom de fichier|Emplacement (installation par défaut)|Description|  
|---------------|---------------------------------------|-----------------|  
|rssrvpolicy.config|C:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportServer|Fichier de configuration de stratégie du serveur de rapports. Ces stratégies de sécurité affectent principalement les expressions de rapport et les assemblys personnalisés, une fois qu'un rapport est déployé sur un serveur de rapports. Ce fichier de stratégie affecte également les extensions personnalisées en matière de données, de remise, de rendu et de sécurité, qui sont déployées sur le serveur de rapports.|  
|rsmgrpolicy.config|C:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\Reporting Services\ReportManager|Fichier de configuration de stratégie du Gestionnaire de rapports. Ces stratégies de sécurité affectent tous les assemblys qui étendent le Gestionnaire de rapports, par exemple les extensions d'interface utilisateur d'abonnement pour une remise personnalisée.|  
|rspreviewpolicy.config|C:\Program Files\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies|Fichier de configuration de stratégie de l'aperçu autonome du Concepteur de rapports. Ces stratégies de sécurité affectent les assemblys personnalisés et les expressions de rapport utilisés dans les rapports pendant l'aperçu et le développement. Ces stratégies affectent également les extensions personnalisées, par exemple les extensions pour le traitement des données, qui sont déployées sur le Concepteur de rapports.|  
  
## <a name="modifying-configuration-files"></a>Modification des fichiers de configuration  
 Les paramètres de configuration sont spécifiés soit comme des éléments, soit comme des attributs XML. Si le langage XML et les fichiers de configuration vous sont familiers, vous pouvez modifier les paramètres définissables par l'utilisateur dans un éditeur de texte ou de code. Les fichiers de configuration de sécurité contiennent des informations sur la hiérarchie du groupe de codes et les jeux d'autorisations associés à un niveau de stratégie dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Il est recommandé d'employer l'utilitaire de configuration du .NET Framework (Mscorcfg.msc) ou l'utilitaire de stratégie de sécurité d'accès du code (Caspol.exe) pour modifier au préalable les stratégies de sécurité dans le fichier Security.config, afin que les modifications de stratégie correspondent aux éléments de configuration XML valides pour les fichiers de stratégie. Une fois que vous avez terminé, vous pouvez couper et coller les nouveaux groupes de codes et jeux d'autorisations de Security.config vers le fichier de stratégie du composant auquel vous ajoutez des autorisations de code.  
  
> [!IMPORTANT]  
>  Sauvegardez vos fichiers de configuration de stratégie avant d'apporter des modifications.  
  
 Cette approche présente un double intérêt. En premier lieu, elle vous permet d'utiliser un outil visuel pour générer vos groupes de codes et jeux d'autorisations pour [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Cela est beaucoup plus facile que d'écrire entièrement des éléments de configuration XML. En second lieu, vous êtes assuré de ne pas endommager les fichiers de configuration de stratégie de sécurité en utilisant des éléments et attributs XML incorrects. Pour plus d'informations sur l'utilitaire de stratégie de sécurité d'accès du code, consultez Utilisation des fichiers de stratégie de sécurité Reporting Services sur MSDN.  
  
 Avant de modifier des fichiers de configuration de stratégie, lisez toutes les informations disponibles dans cette section et les rubriques connexes. La modification de la configuration de stratégie de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] peut avoir un impact déterminant en matière de sécurité sur la façon dont les composants [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] exécutent des modules de code externes.  
  
## <a name="placement-of-codegroup-elements-for-extensions"></a>Placement des éléments CodeGroup pour les extensions  
 Le placement des éléments CodeGroup dans un fichier de stratégie de sécurité est important. Pour les extensions et les assemblys personnalisés que vous développez, il est recommandé de placer vos groupes de codes personnalisés directement en dessous de l'entrée existante pour l'appartenance (membership) URL "$CodeGen$/*", comme indiqué ci-après :  
  
```  
<CodeGroup  
    class="UnionCodeGroup"  
    version="1"  
    PermissionSetName="FullTrust">  
    <IMembershipCondition   
        class="UrlMembershipCondition"  
        version="1"  
        Url="$CodeGen$/*"  
    />  
</CodeGroup>  
<CodeGroup   
    class="UnionCodeGroup"  
    version="1"  
    PermissionSetName="FullTrust"  
    Name="MyCustomCodeGroup"  
    Description="Code group for my custom extension">  
        <IMembershipCondition class="UrlMembershipCondition"  
        version="1"  
        Url="C:\Program Files\Microsoft SQL Server\MSSQL\Reporting Services\ReportServer\bin\MyAssembly.dll"  
        />  
</CodeGroup>  
```  
  
 Les groupes de codes supplémentaires peuvent être ajoutés les uns à la suite des autres.  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation des stratégies de sécurité](../../../reporting-services/extensions/secure-development/understanding-security-policies.md)  
  
  
