---
description: Propriété SqlServiceType (classe SqlServiceAdvancedProperty)
title: Propriété SqlServiceType (SqlServiceAdvancedProperty)
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: wmi
ms.topic: reference
apiname:
- SqlServiceType Property (SqlServiceAdvancedProperty Class)
apilocation:
- sqlmgmproviderxpsp2up.mof
apitype: MOFDef
helpviewer_keywords:
- SqlServiceType property
ms.assetid: 20f1663a-9a14-4f14-8c1b-8aa133e272c3
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 70260b9b24e3f71e84e986c48a5a4a6b58d5446c
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88485026"
---
# <a name="sqlservicetype-property-sqlserviceadvancedproperty-class"></a>Propriété SqlServiceType (classe SqlServiceAdvancedProperty)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Obtient le type du service managé associé à la propriété avancée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.SetBoolValue(NumValue)  
```  
  
## <a name="parts"></a>Éléments  
 *object*  
 Objet de [classe SqlServiceAdvancedProperty](../../../relational-databases/wmi-provider-configuration-classes/sqlserviceadvancedproperty-class/sqlserviceadvancedproperty-class.md) qui représente une propriété avancée.  
  
## <a name="property-valuereturn-value"></a>Valeur de propriété/valeur de retour  
 Valeur uint32 qui spécifie le type du service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="remarks"></a>Notes  
 Les valeurs retournées peuvent être les suivantes :  
  
|Type|Définition|  
|----------|----------------|  
|*1*|MSSQLSERVER est le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|*2*|SQLSERVERAGENT est le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent.|  
|*3*|MSFTESQL est le service du moteur de recherche en texte intégral [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|*4*|MsDtsServer est le service [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] .|  
|*5*|MSSQLServerOLAPService est le service [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] .|  
|*6*|ReportServer est le service [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .|  
|*7*|SQLBrowser est le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Browser.|  
|*8*|NsService est le [!INCLUDE[ssNoVersion](../../../includes/ssns-md.md)] service de notification.|  
|*9*|MSSQLFDLauncher est le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] service du lanceur de démon de filtre de texte intégral.|  
|*10*|SQLPBENGINE est le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] service de moteur Polybase.|  
|*11*|SQLPBDMS est le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] service de déplacement de données Polybase.|  
|*12*|MSSQLLaunchpad est le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] service launchpad.|  
  
## <a name="see-also"></a>Voir aussi  
 [Démarrage et arrêt des services](https://technet.microsoft.com/library/ms174886\(v=sql.105\).aspx)  
  
  
