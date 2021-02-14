---
title: Création d’une bibliothèque d’extensions de remise | Microsoft Docs
description: Découvrez comment attribuer une extension de remise que vous créez dans Reporting Services à un espace de noms unique et la générer dans une bibliothèque ou un fichier d'assembly.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- delivery extensions [Reporting Services], namespace assignments
- library [Reporting Services]
- assigning namespaces to extensions
ms.assetid: 63b32f93-4bab-4b07-bd72-39a47aca1cda
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 390b35970b58c4348d9ebd35e1b67540db37222c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065341"
---
# <a name="creating-a-delivery-extension-library"></a>Création d'une bibliothèque d'extensions de remise
  Chaque extension de remise [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] que vous créez doit être assignée à un espace de noms unique et intégrée à une bibliothèque ou à un fichier d'assembly. Le nom exact de l'espace de noms n'est pas important, mais il doit être unique et ne pas être partagé avec une autre extension. Vous devez créer vos propres espaces de noms uniques pour les extensions de remise de votre société.  
  
 L'exemple suivant montre le début du code d'une extension de remise [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] qui utilise les espaces de noms contenant les interfaces de remise et toute classe utilitaire.  
  
```vb  
Imports System  
Imports Microsoft.ReportingServices.Interfaces  
  
Namespace CompanyName.ExtensionName  
   ...  
```  
  
```csharp  
using System;  
using Microsoft.ReportingServices.Interfaces;  
  
namespace CompanyName.ExtensionName  
{  
   ...  
```  
  
 Lors de la compilation d'une extension de remise [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], vous devez fournir au compilateur une référence au fichier Microsoft.ReportingServices.Interfaces.dll, car celui-ci contient les interfaces et les classes de l'extension de remise. L'espace de noms <xref:Microsoft.ReportingServices.Interfaces> est nécessaire pour implémenter l'interface <xref:Microsoft.ReportingServices.Interfaces.IExtension>, l'interface <xref:Microsoft.ReportingServices.Interfaces.IDeliveryExtension>, ainsi que d'autres interfaces. Par exemple, si tous les fichiers contenant le code utilisé pour implémenter une extension de remise [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] écrite en C# sont réunis dans un répertoire unique portant l’extension .cs, la commande suivante est émise depuis ce répertoire pour compiler les fichiers stockés dans CompanyName.ExtensionName.dll.  
  
```csharp  
csc /t:library /out:CompanyName.ExtensionName.dll *.cs /r:System.dll   
/r:Microsoft.ReportingServices.Interfaces.dll  
```  
  
 L’exemple de code suivant affiche la commande utilisée pour les fichiers [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] avec l’extension .vb.  
  
```vb  
vbc /t:library /out:CompanyName.ExtensionName.dll *.vb /r:System.dll   
/r:Microsoft.ReportingServices.Interfaces.dll  
```  
  
> [!NOTE]  
>  Vous pouvez également concevoir, développer et générer votre extension de remise à l'aide de [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)]. Pour plus d'informations sur le développement des assemblys dans [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)], consultez votre documentation [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [Extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implémentation d’une extension de remise](../../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
