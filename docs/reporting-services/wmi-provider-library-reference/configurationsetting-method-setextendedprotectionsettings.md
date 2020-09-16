---
description: Méthode SetExtendedProtectionSettings (WMI MSReportServer_ConfigurationSetting)
title: SetExtendedProtectionSettings, méthode (WMI MSReportServer_ConfigurationSetting) | Microsoft Docs
ms.date: 03/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: wmi-provider-library-reference
ms.topic: conceptual
ms.assetid: 2d8e7232-42f4-41b6-98eb-c856f6c85d8c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 80693f5720974bedc8c0d4dae13ed4bb24727980
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88480641"
---
# <a name="configurationsetting-method---setextendedprotectionsettings"></a>Méthode ConfigurationSetting - SetExtendedProtectionSettings
  La méthode SetExtendedProtectionSettings sert à définir les propriétés RSWindowsExtendedProtectionLevel et RSWindowsExtendedProtectionScenario dans le fichier de configuration [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] (RSReportServer.config).  
  
## <a name="syntax"></a>Syntaxe  
  
```vb  
Public Sub SetExtendedProtectionSettings( _  
        ByVal ExtendedProtectionLevel As String, _  
        ByVal ExtendedProtectionScenario As String, _  
        ByRef Warnings() As String, _  
        ByRef Length As Int32, _  
        ByRef HRESULT As Int32)  
```  
  
```csharp  
public void SetExtendedProtectionSettings(  
            string ExtendedProtectionLevel,  
            string ExtendedProtectionScenario,  
            out string[] Warnings,  
            out Int32 Length,  
            out Int32 HRESULT);  
```  
  
## <a name="parameters"></a>Paramètres  
 *ExtendedProtectionLevel*  
 Définit RSWindowsExtendedProtectionLevel dans le fichier RSRreportserver.config. La valeur est obligatoire et ne respecte pas la casse.  
  
 La liste suivante affiche les valeurs valides :  
  
 `"Off | Allow | Require"`  
  
 *ExtendedProtectionScenario*  
 Définit RSWindowsExtendedProtectionScenario dans le fichier RSReportserver.config. La valeur est obligatoire et ne respecte pas la casse.  
  
 La liste suivante affiche les valeurs valides :  
  
 `"Any" | "Proxy" | "Direct"`  
  
## <a name="remarks"></a>Notes  
 Les propriétés RSWindowsExtendedProtectionLevel et RSWindowsExtendedProtectionScenario s'appliquent lorsque AuthenticationTypes dans le fichier RSReportServer.config inclut RSWindowNTLM, RSWindowsNegotiate ou RSWindowsKerberos. La définition de ces propriétés change la manière dont les utilisateurs et le logiciel client sont authentifiés au niveau du serveur de rapports. Nous vous recommandons de lire la documentation relative à la protection étendue avant d’affecter la valeur **Allow** ou **Require**à la propriété ExtendedProtectionLevel.  
  
 Pour définir la propriété ExtendedProtectionLevel, l'utilisateur doit être un membre du groupe BUILTIN\Administrators sur le serveur de rapports.  
  
## <a name="requirements"></a>Spécifications  
 **Espace de noms :** [!INCLUDE[ssRSWMInmspcA](../../includes/ssrswminmspca-md.md)]  
  
## <a name="see-also"></a>Voir aussi  
 [Propriété RSWindowsExtendedProtectionScenario &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/rswindowsextendedprotectionscenario-property.md)   
 [Propriété RSWindowsExtendedProtectionLevel &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/rswindowsextendedprotectionlevel-property.md)   
 [Protection étendue de l’authentification avec Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)   
 [Fichier de configuration RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)  
  
  
