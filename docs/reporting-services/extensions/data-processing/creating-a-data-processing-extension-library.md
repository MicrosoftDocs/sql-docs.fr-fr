---
title: Création d’une bibliothèque d’extensions pour le traitement des données | Microsoft Docs
description: Découvrez comment créer une extension pour le traitement des données Reporting Services. Affichez l’exemple de code et déterminez les exigences en matière de fichiers de bibliothèque et d’espaces de noms à respecter.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- data processing extensions [Reporting Services], namespace assignments
- library [Reporting Services]
- assigning namespaces to extensions
ms.assetid: 82f4b71b-dd39-467d-8d8c-6771eb2b12de
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bd55657e325035821798c2b7d6a960a6f78cd59b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065444"
---
# <a name="creating-a-data-processing-extension-library"></a>Création d'une bibliothèque d'extensions pour le traitement des données
  Chaque extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] que vous créez doit être affectée à un espace de noms unique et intégrée dans un fichier bibliothèque ou d'assembly. Le nom exact de l'espace de noms n'est pas important, mais il doit être unique et ne pas être partagé avec une autre extension. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] utilise l'espace de noms <xref:Microsoft.ReportingServices.DataProcessing> pour les extensions pour le traitement des données fournies avec [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Vous devez créer vos propres espaces de noms uniques pour les extensions pour le traitement des données de votre entreprise.  
  
 L'exemple suivant indique le code pour commencer une extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] qui utilise les espaces de noms qui contiennent les interfaces de traitement des données et les classes d'utilitaires.  
  
```vb  
Imports System  
Imports Microsoft.ReportingServices.DataProcessing  
Imports Microsoft.ReportingServices.Interfaces  
  
Namespace CompanyName.ExtensionName  
   ...  
```  
  
```csharp  
using System;  
using Microsoft.ReportingServices.DataProcessing;  
using Microsoft.ReportingServices.Interfaces;  
  
namespace CompanyName.ExtensionName  
{  
   ...  
```  
  
 Lors de la compilation d'une extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], vous devez fournir au compilateur une référence à Microsoft.ReportingServices.Interfaces.dll, parce que les interfaces d'extension pour le traitement des données sont contenues dans ce fichier. L'espace de noms <xref:Microsoft.ReportingServices.DataProcessing> est nécessaire pour implémenter les interfaces d'extension pour le traitement des données, et l'espace de noms <xref:Microsoft.ReportingServices.Interfaces> est nécessaire pour implémenter l'interface <xref:Microsoft.ReportingServices.Interfaces.IExtension>. Par exemple, si tous les fichiers qui contiennent le code pour implémenter une extension pour le traitement des données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] écrit en C# sont dans un répertoire unique avec l'extension .cs, la commande suivante est issue de ce répertoire pour compiler les fichiers stockés dans CompanyName.ExtensionName.dll.  
  
```csharp  
csc /t:library /out:CompanyName.ExtensionName.dll *.cs /r:System.dll /r:Microsoft.ReportingServices.Interfaces.dll  
```  
  
 L’exemple de code suivant affiche la commande utilisée pour les fichiers [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] avec l’extension .vb.  
  
```vb  
vbc /t:library /out:CompanyName.ExtensionName.dll *.vb /r:System.dll /r:Microsoft.ReportingServices.Interfaces.dll  
```  
  
> [!NOTE]  
>  Vous pouvez aussi concevoir, développer et générer votre extension pour le traitement des données à l'aide de [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)]. Pour plus d'informations sur le développement des assemblys dans [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)], consultez votre documentation [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [Extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implémentation d’une extension pour le traitement des données](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
