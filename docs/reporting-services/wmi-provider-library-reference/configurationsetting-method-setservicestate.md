---
description: Méthode SetServiceState (WMI MSReportServer_ConfigurationSetting)
title: SetServiceState, méthode (WMI MSReportServer_ConfigurationSetting) | Microsoft Docs
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: wmi-provider-library-reference
ms.topic: conceptual
apiname:
- SetServiceState (WMI MSReportServer_ConfigurationSetting Class)
apilocation:
- reportingservices.mof
apitype: MOFDef
helpviewer_keywords:
- SetServiceState method
ms.assetid: 9e1ee42d-b388-4929-89c7-8741b956c3be
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 3607488ec4a9511c9cf95410a4c249a740132d82
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88480601"
---
# <a name="configurationsetting-method---setservicestate"></a>Méthode ConfigurationSetting - SetServiceState
  Active ou désactive les services Web et Windows Report Server.  
  
## <a name="syntax"></a>Syntaxe  
  
```vb  
Public Sub SetServiceState(ByVal EnableWindowsService As Boolean, _  
    ByVal EnableWebService As Boolean, ByVal EnableReportManager As Boolean, _  
    ByRef HRESULT As Int32)  
```  
  
```csharp  
public void SetServiceState(Boolean EnableWindowsService,  
    Boolean EnableWebService, Boolean EnableReportManager, out Int32 HRESULT);  
```  
  
## <a name="parameters"></a>Paramètres  
 *EnableWindowsService*  
 Valeur **booléenne** indiquant l’état du service Windows. La valeur **true** démarre le service Windows Report Server ; la valeur **false** arrête le service Windows.  
  
 *EnableWebService*  
 Valeur **booléenne** indiquant l’état du service web [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . La valeur **true** démarre le service Web Report Server ; la valeur **false** arrête le service Web.  
  
 *EnableReportManager*  
 Valeur **booléenne** indiquant l’état souhaité du Gestionnaire de rapports.
 
 > [!NOTE] 
 > Ce paramètre est déprécié à compter de SQL Server 2016 Reporting Services Cumulative Update 2. Le portail web sera toujours activé. La valeur sera ignorée.
  
 *HRESULT*  
 [out] Valeur indiquant si l'appel a réussi ou échoué.  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne un paramètre *HRESULT* qui indique si l'appel de la méthode a réussi ou a échoué. Une valeur 0 indique que l'appel de méthode a réussi. Une valeur différente de zéro indique qu'une erreur s'est produite.  
  
## <a name="remarks"></a>Notes  
  
## <a name="requirements"></a>Spécifications  
 **Espace de noms :** [!INCLUDE[ssRSWMInmspcA](../../includes/ssrswminmspca-md.md)]  
  
## <a name="see-also"></a>Voir aussi  
 [Membres MSReportServer_ConfigurationSetting](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-members.md)  
  
  
