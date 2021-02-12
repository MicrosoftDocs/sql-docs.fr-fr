---
title: Déclaration d’autorisations dans les assemblys personnalisés | Microsoft Docs
description: Découvrez comment accorder des autorisations pour implémenter un assembly personnalisé qui effectue des appels sécurisés vers des ressources protégées au sein de votre système de sécurité.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-assemblies
ms.topic: reference
helpviewer_keywords:
- secure calls [Reporting Services]
- custom assemblies [Reporting Services], permissions
- permission sets [Reporting Services]
- asserting permissions
- permissions [Reporting Services], custom assemblies
- limited permission sets
- security configuration files [Reporting Services]
ms.assetid: 3afb9631-f15e-405e-990b-ee102828f298
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e63239176b15eb1b1044c6b281cdc8014c26d973
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064604"
---
# <a name="asserting-permissions-in-custom-assemblies"></a>Déclaration d'autorisations dans les assemblys personnalisés
  Par défaut, le code d’assembly personnalisé s’exécute avec le jeu d’autorisations limité **Execution**. Dans certaines situations, vous voudrez peut-être implémenter un assembly personnalisé qui effectue des appels sécurisés à des ressources protégées au sein de votre système de sécurité (comme un fichier ou le Registre). Pour ce faire, vous devez procéder comme suit :  
  
1.  Identifiez les autorisations exactes dont votre code a besoin pour effectuer l'appel sécurisé. Si cette méthode fait partie d’une bibliothèque [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], ces informations doivent être incluses dans la documentation de la méthode.  
  
2.  Modifiez les fichiers de configuration de stratégie du serveur de rapports de manière à accorder les autorisations requises à l'assembly personnalisé. Pour plus d’informations sur les fichiers de configuration de stratégie de sécurité, consultez [Utilisation des fichiers de stratégie de sécurité Reporting Services](../../reporting-services/extensions/secure-development/using-reporting-services-security-policy-files.md).  
  
3.  Déclarez les autorisations requises dans le cadre de la méthode utilisée pour effectuer l'appel sécurisé. Cette étape est nécessaire car le code d’assembly personnalisé qui est appelé par le serveur de rapports fait partie de l’assembly hôte d’expressions du rapport, qui s’exécute par défaut avec l’autorisation **Execution**. Le jeu d’autorisations **Execution** permet au code de s’exécuter, mais pas d’utiliser des ressources protégées.  
  
4.  Marquez l’assembly personnalisé avec **AllowPartiallyTrustedCallersAttribute** s’il est signé avec un nom fort. Cette étape est nécessaire car les assemblys personnalisés sont appelés à partir d’une expression de rapport qui fait partie de l’assembly hôte d’expressions du rapport, à qui, par défaut, l’autorisation **FullTrust** n’est pas accordée. Il s’agit donc d’un appelant d’un niveau de confiance partiel. Pour plus d’informations, consultez [Utilisation d’assemblys personnalisés avec noms forts](../../reporting-services/custom-assemblies/using-strong-named-custom-assemblies.md).  
  
## <a name="implementing-a-secure-call"></a>Implémentation d'un appel sécurisé  
 Vous pouvez modifier les fichiers de configuration de stratégie de manière à accorder des autorisations spécifiques à votre assembly. Par exemple, si vous écrivez un assembly personnalisé pour gérer la conversion de devises, vous aurez peut-être besoin de lire le taux de change de la devise actuelle dans un fichier. Pour extraire les informations sur les taux de change, vous aurez besoin d’ajouter une autorisation de sécurité supplémentaire, **FileIOPermission**, au jeu d’autorisations de l’assembly. Vous pouvez ajouter l'entrée suivante au fichier de configuration de stratégie :  
  
```  
<PermissionSet class="NamedPermissionSet"  
   version="1"  
   Name="CurrencyRatesFilePermissionSet"  
   Description="A special permission set that grants read access to my currency rates file.">  
      <IPermission class="FileIOPermission"  
         version="1"  
         Read="C:\CurrencyRates.xml"/>  
      <IPermission class="SecurityPermission"  
         version="1"  
         Flags="Execution, Assertion"/>  
</PermissionSet>  
```  
  
 Vous pouvez ensuite ajouter une groupe de code qui référence ce jeu d'autorisations :  
  
```  
<CodeGroup class="UnionCodeGroup"  
   version="1"  
   PermissionSetName="CurrencyRatesFilePermissionSet"  
   Name="MyNewCodeGroup"  
   Description="A special code group for my custom assembly.">  
   <IMembershipCondition class="UrlMembershipCondition"  
      version="1"  
      Url="C:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\MSSQL\Reporting Services\ReportServer\bin\CurrencyConversion.dll"/>  
</CodeGroup>  
```  
  
 Pour que votre code acquière l'autorisation appropriée, vous devez déclarer cette autorisation dans le code de votre assembly personnalisé. Par exemple, si vous souhaitez ajouter l'accès en lecture seule à un fichier XML, C:\CurrencyRates.xml, vous devez ajouter le code suivant à votre méthode :  
  
```  
// C#  
FileIOPermission permission = new FileIOPermission(FileIOPermissionAccess.Read, @"C:\CurrencyRates.xml");  
try  
{  
   permission.Assert();  
   // Load the XML currency rates file  
   XmlDocument doc = new XmlDocument();  
   doc.Load(@"C:\CurrencyRates.xml");  
...  
```  
  
 Vous pouvez déclarer cette autorisation sous la forme d'un attribut de méthode :  
  
```  
[FileIOPermissionAttribute(SecurityAction.Assert, Read=@"C:\CurrencyRates.xml")]  
```  
  
 Pour plus d'informations, consultez la rubrique « Sécurité du .NET Framework » dans le Guide du développeur .NET Framework.  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d'assemblages personnalisés avec des rapports](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)  
  
  
