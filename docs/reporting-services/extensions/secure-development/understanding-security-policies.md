---
title: Présentation des stratégies de sécurité | Microsoft Docs
description: En savoir plus sur les stratégies de sécurité Reporting Services, qui spécifient des autorisations pour les assemblys, les extensions et les expressions de rapport en fonction des groupes de codes.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- code groups [Reporting Services]
- code access security [Reporting Services], security policies
- Execution permission set
- custom assemblies [Reporting Services], security
- permission sets [Reporting Services]
- expressions [Reporting Services], security
- evidence [Reporting Services]
- FullTrust permission set
- security policies [Reporting Services]
- named permission sets [Reporting Services]
ms.assetid: a9bf043a-139a-4929-9a58-244815323df0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 072a2f88ce87c5330614961dc1aed842aae2e058
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065274"
---
# <a name="understanding-security-policies"></a>Présentation des stratégies de sécurité
  Tout code exécuté par un serveur de rapports doit faire partie d'une stratégie de sécurité d'accès du code spécifique. Ces stratégies de sécurité comprennent des groupes de codes qui mappent une preuve à un ensemble de jeux d'autorisations nommés. Souvent, les groupes de codes sont associés à un jeu d'autorisations nommé qui spécifie les autorisations pouvant être accordées au code dans ce groupe. Le runtime utilise la preuve fournie par un hôte approuvé ou par le chargeur pour déterminer les groupes de codes auxquels le code appartient et, par conséquent, les autorisations à accorder au code. [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] adhère à cette architecture de stratégie de sécurité telle qu’elle est définie par le Common Language Runtime (CLR) [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)]. Les sections suivantes décrivent les divers types de codes qui existent dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], ainsi que les règles de stratégie associées.  
  
## <a name="report-server-assemblies"></a>Assemblys du serveur de rapports  
 Les assemblys du serveur de rapports sont ceux qui contiennent du code faisant partie du produit [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] est écrit à l'aide d'assemblys de code managé ; tous ces assemblys portent un nom fort (autrement dit, ils sont signés numériquement). Les groupes de codes de ces assemblys sont définis à l’aide de **StrongNameMembershipCondition**, qui fournit une preuve basée sur les informations de clé publique pour le nom fort de l’assembly. Le jeu d’autorisations **FullTrust** est accordé au groupe de codes.  
  
## <a name="report-server-extensions-rendering-data-delivery-and-security"></a>Extensions du serveur de rapports (rendu, données, remise et sécurité)  
 Les extensions du serveur de rapports sont des extensions personnalisées en matière de données, de remise, de rendu et de sécurité que vous ou des personnes tierces créez afin d'étendre les fonctionnalités de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Vous devez accorder **FullTrust** à ces extensions ou au code assembleur dans les fichiers de configuration de stratégie associés au composant [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] que vous étendez. Les extensions fournies avec [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] sont signées à l’aide de la clé publique du serveur de rapports et reçoivent le jeu d’autorisations **FullTrust**.  
  
> [!IMPORTANT]  
>  Vous devez modifier les fichiers de configuration de stratégie [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] afin que **FullTrust** puisse être accordé aux extensions tierces. Si vous n’ajoutez pas de groupe de codes avec **FullTrust** à vos extensions personnalisées, ces dernières ne peuvent pas être utilisées par le serveur de rapports.  
  
 Pour plus d’informations sur les fichiers de configuration de stratégie dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], consultez [Utilisation des fichiers de stratégie de sécurité Reporting Services](../../../reporting-services/extensions/secure-development/using-reporting-services-security-policy-files.md).  
  
## <a name="expressions-used-in-reports"></a>Expressions utilisées dans les rapports  
 Les expressions de rapport sont des expressions de code incorporé ou des méthodes définies par l’utilisateur contenues dans l’élément **Code** d’un fichier RDL (Report Definition Language). Il existe un groupe de codes déjà configuré dans les fichiers de stratégie, qui accorde à ces expressions le jeu d’autorisations **Execution** par défaut. Le groupe de codes ressemble à ceci :  
  
```  
<CodeGroup  
   class="UnionCodeGroup"  
   version="1"  
   PermissionSetName="Execution"  
   Name="Report_Expressions_Default_Permissions"  
   Description="This code group grants default permissions for code in report expressions and Code element. ">  
    <IMembershipCondition  
       class="StrongNameMembershipCondition"  
       version="1"  
       PublicKeyBlob="002400..."  
    />  
</CodeGroup>  
```  
  
 L’autorisation **Execution** permet au code de s’exécuter mais pas d’utiliser des ressources protégées. Toutes les expressions d'un rapport sont compilées en un assembly (appelé assembly « hôte d'expressions ») qui est stocké en tant que partie du rapport compilé. Lorsque le rapport est exécuté, le serveur de rapports charge l'assembly hôte d'expressions et effectue des appels dans cet assembly pour exécuter les expressions. Les assemblys hôtes d'expressions sont signés avec une clé spécifique utilisée pour définir le groupe de codes de tous les hôtes d'expressions.  
  
 Les expressions de rapport référencent des collections du modèle objet de rapport (champs, paramètres, etc.) et effectuent des tâches simples comme l'arithmétique et des opérations de chaîne. Le code qui effectue ces opérations simples requiert uniquement l’autorisation **Execution**. Par défaut, les méthodes définies par l’utilisateur dans l’élément **Code** et les assemblys personnalisés reçoivent l’autorisation **Execution** dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Par conséquent, pour la plupart des expressions, la configuration actuelle ne requiert pas que vous modifiiez les fichiers de stratégie de sécurité. Pour accorder des autorisations supplémentaires aux assemblys hôtes d'expressions, un administrateur doit modifier les fichiers de configuration de stratégie du serveur de rapports et du Concepteur de rapports ; en outre, il doit également modifier le groupe de codes des expressions de rapport. Dans la mesure où il s'agit d'un paramètre global, la modification des autorisations par défaut des hôtes d'expressions affecte tous les rapports. Pour cette raison, il est vivement recommandé que vous placiez l'ensemble du code qui requiert une sécurité supplémentaire dans un assembly personnalisé. Seul cet assembly recevra les autorisations dont vous avez besoin.  
  
> [!IMPORTANT]  
>  Le code qui appelle des assemblys externes ou des ressources protégées doit être incorporé dans un assembly personnalisé à utiliser dans les rapports. Vous disposez ainsi d'un plus grand contrôle des autorisations requises et déclarées par votre code. N’effectuez pas d’appels pour sécuriser des méthodes dans l’élément **Code**. En effet, cela vous oblige à accorder **FullTrust** à l’hôte d’expressions de rapport ; cela vous oblige également à accorder l’accès complet à l’ensemble du code personnalisé pour le Common Language Runtime.  
  
> [!CAUTION]  
>  N’accordez pas **FullTrust** au groupe de codes pour un hôte d’expressions de rapport. Sinon, vous permettez à toutes les expressions de rapport d'effectuer des appels système protégés.  
  
## <a name="custom-assemblies-referenced-in-reports"></a>Assemblys personnalisés référencés dans les rapports  
 Certaines expressions de rapport peuvent appeler des assemblys de code tiers, également nommés assemblys personnalisés dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Pour le serveur de rapports, ces assemblys sont censés disposer au moins de l’autorisation **Execution** dans les fichiers de configuration de stratégie. Par défaut, les fichiers de stratégie fournis avec [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] accordent l’autorisation **Execution** à tous les assemblys à partir de la zone « Mon Ordinateur ». Vous pouvez accorder des autorisations supplémentaires aux assemblys personnalisés en fonction de vos besoins.  
  
 Dans certains cas, vous devrez peut-être effectuer une opération qui requiert des autorisations de code spécifique dans une expression de rapport. En règle générale, cela signifie qu'une expression de rapport doit appeler une méthode de bibliothèque CLR sécurisée (par exemple une méthode de bibliothèque qui accède aux fichiers ou au Registre système). La documentation du [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] décrit les autorisations de code requises pour effectuer cet appel sécurisé ; pour exécuter l'appel, le code appelant doit recevoir ces autorisations sécurisées spécifiques. Si vous effectuez l’appel à partir d’une expression de rapport ou de l’élément **Code**, les autorisations appropriées doivent être accordées à l’assembly hôte d’expressions. Cependant, une fois que vous avez accordé les autorisations appropriées à l'hôte d'expressions, tout code s'exécutant dans l'expression d'un rapport reçoit désormais cette autorisation spécifique. Il est beaucoup plus sûr d'effectuer l'appel à partir d'un assembly personnalisé et d'accorder à ce dernier les autorisations spécifiques.  
  
## <a name="see-also"></a>Voir aussi  
 [Sécurité d’accès du code dans Reporting Services](../../../reporting-services/extensions/secure-development/code-access-security-in-reporting-services.md)   
 [Développement sécurisé &#40;Reporting Services&#41;](../../../reporting-services/extensions/secure-development/secure-development-reporting-services.md)  
  
  
