---
title: Définir les paramètres régionaux d’un rapport ou d’une zone de texte (Reporting Services) | Microsoft Docs
description: Utilisez la propriété Language d’une zone de texte pour fournir les paramètres régionaux des formats qui affichent des données qui diffèrent par la langue et la région dans le Générateur de rapports.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
helpviewer_keywords:
- locales [Reporting Services]
ms.assetid: df115b01-184b-47f0-b5ec-0ad965ff9bee
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 345be03e2156e9097d9a7e60681ce901bd1e87b0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045011"
---
# <a name="set-the-locale-for-a-report-or-text-box-reporting-services"></a>Définir les paramètres régionaux d'un rapport ou d'une zone de texte (Reporting Services)
  La propriété **Language** d'un rapport ou d'une zone de texte contient les paramètres régionaux, qui déterminent les formats par défaut d'affichage des données de rapport qui varient selon la langue et la région géographique, comme la date, la monnaie ou les valeurs numériques. La propriété **Language** d'une zone de texte substitue la propriété **Language** du rapport. Si aucune valeur n'est spécifiée pour **Language**, Reporting Services utilise les paramètres régionaux du système d'exploitation du serveur de rapports pour les rapports publiés ou de l'ordinateur qui a servi à créer les rapports pour en afficher l'aperçu.  
  
 Pour les rapports HTML, vous pouvez remplacer la valeur **Language** par défaut par la langue spécifiée dans l’en-tête HTTP du client du navigateur en utilisant le champ prédéfini User!Language dans une expression pour la propriété **Language** d’un rapport ou d’une zone de texte.  
  
 Vous pouvez également spécifier la propriété **Language** d'un rapport dans une URL. Pour plus d’informations, consultez [Définir la langue des paramètres de rapport dans une URL](../../reporting-services/set-the-language-for-report-parameters-in-a-url.md).  
  
### <a name="to-set-the-locale-for-a-report"></a>Pour définir les paramètres régionaux d'un rapport  
  
1.  En mode Conception, cliquez à l'extérieur de l'aire de conception du rapport pour sélectionner le rapport.  
  
2.  Dans le volet Propriétés, pour la propriété **Language** , tapez ou sélectionnez la langue à utiliser pour le rapport.  
  
### <a name="to-set-the-locale-for-a-text-box"></a>Pour définir les paramètres régionaux d'une zone de texte  
  
1.  En mode Conception, sélectionnez la zone de texte à laquelle vous souhaitez appliquer les paramètres régionaux.  
  
2.  Dans le volet Propriétés, effectuez les opérations suivantes :  
  
    -   Pour la propriété **Calendar** , tapez ou sélectionnez le calendrier que vous souhaitez utiliser pour les dates.  
  
    -   Pour la propriété **Direction** , tapez ou sélectionnez la direction dans laquelle le texte doit être écrit.  
  
    -   Pour la propriété **Language** , tapez ou sélectionnez la langue que vous souhaitez affecter à la zone de texte. Cette valeur substitue la propriété **Language** du rapport.  
  
    -   Pour la propriété **NumeralLanguage** , tapez ou sélectionnez le format à utiliser pour les nombres dans la zone de texte.  
  
    -   Pour la propriété **NumeralVariant** , tapez ou sélectionnez la variante du format à utiliser pour les nombres dans la zone de texte.  
  
    -   Pour la propriété **UnicodeBiDi** , sélectionnez le niveau d'incorporation bidirectionnelle à appliquer à la zone de texte.  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation d’expressions dans les rapports &#40;Générateur de rapport et SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Considérations sur la conception de la solution pour les déploiements multilingues ou globaux (Reporting Services)](/previous-versions/sql/)  
  
