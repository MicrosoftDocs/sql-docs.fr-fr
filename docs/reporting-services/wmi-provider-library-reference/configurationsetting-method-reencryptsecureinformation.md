---
description: Méthode ReencryptSecureInformation (WMI MSReportServer_ConfigurationSetting)
title: ReencryptSecureInformation, méthode (WMI MSReportServer_ConfigurationSetting) | Microsoft Docs
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: wmi-provider-library-reference
ms.topic: conceptual
apiname:
- ReencryptSecureInformation (WMI MSReportServer_ConfigurationSetting Class)
apilocation:
- reportingservices.mof
apitype: MOFDef
helpviewer_keywords:
- ReencryptSecureInformation method
ms.assetid: 8a487497-c207-45b2-8c92-87c58cc68716
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 629df7fb638bbad4df760855ae7963dce763071b
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88468970"
---
# <a name="configurationsetting-method---reencryptsecureinformation"></a>Méthode ConfigurationSetting - ReencryptSecureInformation
  Génère une nouvelle clé de chiffrement et rechiffre toutes les informations sécurisées dans le catalogue à l'aide de cette nouvelle clé.  
  
## <a name="syntax"></a>Syntaxe  
  
```vb  
Public Sub ReencryptSecureInformation(ByRef HRESULT as Int32, ByRef ExtendedErrors() As String)  
```  
  
```csharp  
public void ReencryptSecureInformation (out Int32 HRESULT, out string[] ExtendedErrors);  
```  
  
## <a name="parameters"></a>Paramètres  
 *HRESULT*  
 [out] Valeur indiquant si l'appel a réussi ou échoué.  
  
 *ExtendedErrors[]*  
 [out] Tableau de chaînes contenant les erreurs supplémentaires retournées par l'appel.  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne un paramètre *HRESULT* qui indique si l'appel de la méthode a réussi ou a échoué. Une valeur 0 indique que l'appel de méthode a réussi. Une valeur différente de zéro indique qu'une erreur s'est produite.  
  
## <a name="remarks"></a>Notes  
 La méthode ReencryptSecureInformation permet à l’administrateur de remplacer la clé de chiffrement existante par une nouvelle clé.  
  
 Lorsque cette méthode est appelée, le serveur de rapports génère une nouvelle clé de chiffrement et parcourt tout le contenu chiffré afin de le rechiffrer avec la nouvelle clé de chiffrement.  
  
 Les extensions de remise peuvent stocker des informations sécurisées dans leurs structures de paramètres de remise. Quand ReencryptSecureInformation est appelée, le serveur de rapports charge chaque abonnement et l’extension de remise correspondante pour rechiffrer les informations stockées dans les paramètres associés.  
  
 Si cette méthode est exécutée sur un ordinateur dans un déploiement avec montée en puissance parallèle, chaque ordinateur dans le déploiement avec montée en puissance parallèle doit être réinitialisé.  
  
## <a name="requirements"></a>Spécifications  
 **Espace de noms :** [!INCLUDE[ssRSWMInmspcA](../../includes/ssrswminmspca-md.md)]  
  
## <a name="see-also"></a>Voir aussi  
 [Membres MSReportServer_ConfigurationSetting](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-members.md)  
  
  
