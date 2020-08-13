---
title: Paramètres d’informations de périphérique CSV | Microsoft Docs
description: Découvrez les paramètres d’informations de périphérique CSV qui sont disponibles pour le rendu au format texte.
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- CSV [Reporting Services]
- device information settings [Reporting Services], CSV rendering
ms.assetid: f96f83a6-50bc-48ce-9fcd-fd9e1952d40a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 9df267e6ab605f3c328ed2a07c78770bc5dade92
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87248563"
---
# <a name="csv-device-information-settings"></a>Paramètres d'informations de périphérique CSV
  Les paramètres d'informations de périphérique de l'extension de rendu CSV permettent de modifier les séparateurs et les qualificateurs et de spécifier la manière de gérer les sauts de ligne. L'extension du fichier peut également être envoyée, ainsi que l'encodage et l'inclusion des lignes d'en-tête dans la sortie. Étant donné que les séparateurs sont probablement des caractères spéciaux, vous devez les encoder dans une section CDATA, si les paramètres sont écrits au format XML.  
  
 Le tableau suivant répertorie les paramètres d'informations de périphérique qui permettent un rendu du rapport au format Texte.  
  
|Paramètre|Valeur|  
|-------------|-----------|  
|**Encodage**|Nom IANA (Internet Assigned Numbers Authority) d'un encodage de caractères pris en charge par le .NET Framework. La valeur par défaut est **UTF-8**. Les exemples d'autres valeurs incluent ASCII, UTF-7 et UTF-16.|  
|**ExcelMode**|Indique que la sortie cible est destinée à Excel. La valeur par défaut est **true**.|  
|**FieldDelimiter**|Chaîne de séparateur à placer dans le résultat. La valeur par défaut est une virgule (,). Vous devez encoder la valeur de cette information de périphérique pour la transmettre à une URL. Par exemple, un caractère de tabulation utilisé en tant que séparateur doit être codé par « %09 ».<br /><br /> Vous pouvez changer de séparateur de champs par défaut et utiliser le caractère de votre choix, y compris TAB, en modifiant les paramètres d'informations de périphérique dans le fichier de configuration. Par exemple, pour utiliser la touche de tabulation, configurez le paramètre FieldDelimiter sur \<FieldDelimiter xml:space="preserve">[TAB]\</FieldDelimiter>.<br /><br /> Dans l'exemple, [TAB] est une vraie tabulation, ce qui signifie que l'espace blanc apparaît dans le fichier de configuration. L'attribut « xml:space » indique aux analyseurs de conserver l'espace blanc.|  
|**FileExtension**|Extension de fichier à placer dans le résultat. La valeur par défaut est **.CSV**. Si **FileExtension** et **Extension** sont tous deux spécifiés, **FileExtension** est prioritaire.|  
|**NoHeader**|Indique si la ligne d'en-tête est exclue de la sortie. La valeur par défaut est **false**.|  
|**Qualificateur**|Chaîne de qualificateur à placer dans les résultats qui contiennent le séparateur de champs ou le séparateur d'enregistrements. Si les résultats contiennent le qualificateur, celui-ci est répété. La valeur de **Qualificateur** doit être différente des valeurs de **FieldDelimiter** et de **RecordDelimiter** . La valeur par défaut est un guillemet (").|  
|**RecordDelimiter**|Séparateur d'enregistrements à placer à la fin de chaque d'enregistrement. La valeur par défaut est \<cr>\<lf>.|  
|**SuppressLineBreaks**|Indique si les sauts de ligne sont supprimés des données incluses dans la sortie. La valeur par défaut est **false**. Si la valeur est **true**, les valeurs de **FieldDelimiter**, **RecordDelimiter**et **Qualificateur** ne peuvent pas être un espace.|  
|**UseFormattedValues**|Indique si les chaînes mises en forme sont placées dans la sortie CSV. La valeur par défaut est **true** quand **ExcelMode** est **true**; sinon, la valeur est **false**.|  
  
## <a name="see-also"></a>Voir aussi  
 <xref:ReportExecution2005.ReportExecutionService.Render%2A>   
 [Transmission de paramètres d'informations de périphérique aux extensions de rendu](../reporting-services/report-server-web-service/net-framework/passing-device-information-settings-to-rendering-extensions.md)   
 [Personnaliser les paramètres d’extension de rendu dans RSReportServer.Config](../reporting-services/customize-rendering-extension-parameters-in-rsreportserver-config.md)   
 [Informations techniques de référence &#40;SSRS&#41;](../reporting-services/technical-reference-ssrs.md)  
  
  
