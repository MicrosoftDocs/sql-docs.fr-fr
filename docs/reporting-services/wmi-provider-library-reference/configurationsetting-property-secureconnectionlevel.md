---
description: Propriété SecureConnectionLevel (WMI MSReportServer_ConfigurationSetting)
title: SecureConnectionLevel, propriété (WMI MSReportServer_ConfigurationSetting) | Microsoft Docs
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: wmi-provider-library-reference
ms.topic: conceptual
apiname:
- SecureConnectionLevel
apilocation:
- reportingservices.mof
apitype: MOFDef
helpviewer_keywords:
- SecureConnectionLevel property
ms.assetid: fd5549e7-b874-41e2-866e-2f58caf6f733
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: dcf7fdc038f5284d78835e64e568bb348756a3d7
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88373155"
---
# <a name="configurationsetting-property---secureconnectionlevel"></a>Propriété ConfigurationSetting - SecureConnectionLevel
  Retourne le niveau de connexion sécurisée qui est spécifié dans le fichier RSReportServer.config. Lecture seule.  
  
## <a name="syntax"></a>Syntaxe  
  
```vb  
Public Dim SecureConnectionLevel As Integer  
```  
  
```csharp  
public Integer SecureConnectionLevel;  
```  
  
## <a name="property-values"></a>Valeurs de la propriété  
 Valeur de type **Entier** qui représente le niveau de la connexion sécurisée. Les valeurs de retour indiquent que le protocole TLS est configuré ou non. Une valeur supérieure ou égale à 1 indique que le protocole TLS est activé. La valeur 0 indique que le protocole TLS est désactivé.  
  
## <a name="example-code"></a>Exemple de code  
 [Classe MSReportServer_ConfigurationSetting](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-class.md)  
  
## <a name="remarks"></a>Notes

Dans SQL Server 2008 R2, SecureConnectionLevel est un commutateur d’activation ou de désactivation. Pour plus d’informations, consultez [Méthode ConfigurationSetting - SetSecureConnectionLevel](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-setsecureconnectionlevel.md).

## <a name="requirements"></a>Spécifications  
 **Espace de noms :** [!INCLUDE[ssRSWMInmspcA](../../includes/ssrswminmspca-md.md)]  
  
## <a name="see-also"></a>Voir aussi  
 [Membres MSReportServer_ConfigurationSetting](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-members.md)  
  
  
