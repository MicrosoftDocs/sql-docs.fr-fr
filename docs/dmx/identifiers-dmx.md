---
description: Identificateurs (DMX)
title: Identificateurs (DMX) | Microsoft Docs
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: d802cb37a972eb498b485d7253a82b8445f4566d
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813476"
---
# <a name="identifiers-dmx"></a>Identificateurs (DMX)
[!INCLUDE[ssas](../includes/applies-to-version/ssas.md)]

  Tous les objets dans [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] doivent avoir un identificateur. Le nom d'un objet constitue son identificateur. Les serveurs, les bases de données et les objets de base de données tels que sources de données, vues de source de données, cubes, dimensions, modèles d'exploration de données, etc., ont tous un identificateur.  
  
 Dans DMX (Data Mining Extensions), il existe deux catégories d'identificateurs :  
  
-   [Identificateurs réguliers](#RegularIdentifiers)  
  
-   [Identificateurs délimités](#DelimitedIdentifiers)  
  
 L'identificateur d'un objet se crée lorsque vous définissez l'objet. Vous utilisez ensuite l’identificateur pour référencer l’objet. Les identificateurs doivent avoir un maximum de 100 caractères.  
  
##  <a name="regular-identifiers"></a><a name="RegularIdentifiers"></a> Identificateurs réguliers  
 Les identificateurs réguliers dans DMX respectent les règles [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] relatives au format des identificateurs. Ils ne nécessitent pas de délimiteurs. Voici un exemple d’instruction DMX qui utilise un identificateur normal, non délimité :  
  
```  
SELECT * FROM Clustering.CONTENT;  
```  
  
### <a name="rules-for-regular-identifiers"></a>Règles pour identificateurs réguliers  
 Voici les règles relatives au format des identificateurs réguliers :  
  
1.  Le premier caractère d'un identificateur régulier doit être l'un des suivants :  
  
    -   Une lettre définie par la norme Unicode 2,0. Cela inclut les caractères latins de a à z et de A à Z, et les lettres d'autres langues.  
  
    -   Un caractère de soulignement (_).  
  
2.  Les caractères suivants peuvent être :  
  
    -   Les lettres définies dans la norme Unicode 2,0.  
  
    -   Des nombres décimaux de Basic Latin ou d'autres scripts nationaux.  
  
    -   Un caractère de soulignement (_).  
  
3.  L'identificateur ne doit pas être un mot réservé DMX. Dans DMX, les mots réservés ne respectent pas la casse des caractères. Pour plus d’informations, consultez [Mots clés réservés &#40;&#41;DMX ](../dmx/reserved-keywords-dmx.md).  
  
4.  L'identificateur ne peut contenir ni espaces insérés ni caractères spéciaux.  
  
 Vous devez placer entre crochets les identificateurs qui ne respectent pas ces règles lorsque vous les utilisez dans des instructions DMX.  
  
##  <a name="delimited-identifiers"></a><a name="DelimitedIdentifiers"></a> Identificateurs délimités  
 Les identificateurs délimités sont placés entre crochets ([ ]).  Voici l'exemple d'une instruction DMX avec un identificateur délimité qui respecte ces règles.  
  
```  
SELECT * FROM [Marketing_Clusters].CONTENT;  
```  
  
 S'il ne respecte pas les règles relatives au format des identificateurs réguliers, il doit toujours être délimité. Voici l'exemple d'une instruction DMX avec un identificateur délimité contenant un espace :  
  
```  
SELECT * FROM [Targeted Mailing].CONTENT;  
```  
  
 Utilisez les identificateurs délimités dans les cas suivants :  
  
-   Lorsque vous utilisez des mots réservés pour des noms d'objet ou des parties de noms d'objet.  
  
     Il est conseillé de ne pas utiliser de mots clés réservés en tant que noms d'objet. Les bases de données que vous mettez à niveau à partir de versions antérieures de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] peuvent contenir des identificateurs qui incluent des mots qui n’étaient pas réservés dans la version antérieure de, [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] mais qui sont des mots réservés pour [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] . Vous pouvez utiliser un identificateur délimité pour faire référence à ce type d'objet jusqu'à ce que vous puissiez renommer l'objet.  
  
-   Lorsque vous utilisez des caractères non répertoriés comme identificateurs qualifiés.  
  
     [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] permet d'utiliser tout caractère de la page de codes en cours dans un identificateur délimité ; cependant, l'utilisation intempestive de caractères spéciaux dans un nom d'objet peut rendre difficile la lecture et la maintenance des instructions DMX.  
  
### <a name="rules-for-delimited-identifiers"></a>Règles pour identificateurs délimités  
 Voici les règles relatives au format des identificateurs délimités :  
  
1.  Les identificateurs délimités peuvent contenir le même nombre de caractères que les identificateurs réguliers (de 1 à 100 caractères, sans compter les caractères de délimitation).  
  
2.  Le corps de l'identificateur peut contenir n'importe quelle combinaison de caractères dans la page de codes en cours, y compris les caractères de délimitation proprement dits. Si le corps de l'identificateur lui-même contient des caractères de délimitation, un traitement spécial est nécessaire :  
  
    -   Si le corps de l'identificateur contient un crochet gauche ([), aucun traitement supplémentaire n'est nécessaire.  
  
    -   Si le corps de l'identificateur contient un crochet droit (]), vous devez spécifier deux crochets droits (]]) pour le représenter dans la page de codes.  
  
### <a name="delimiting-identifiers-with-multiple-parts"></a>Identificateurs de délimitation en plusieurs parties  
 Lorsque vous utilisez un nom d'objet qualifié, vous pouvez être contraint de délimiter plusieurs des identificateurs qui le composent. Vous devez délimiter chaque identificateur individuellement.  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur la&#41; DMX &#40;Data Mining Extensions](../dmx/data-mining-extensions-dmx-reference.md)   
 [Data Mining Extensions &#40;les éléments de la syntaxe DMX&#41;](../dmx/data-mining-extensions-dmx-syntax-elements.md)   
 [Informations de référence sur les fonctions DMX&#41; Data Mining Extensions &#40;](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Informations de référence sur l’opérateur de&#41; DMX &#40;Data Mining Extensions](../dmx/data-mining-extensions-dmx-operator-reference.md)   
 [Informations de référence sur les instructions DMX&#41; &#40;Data Mining Extensions](../dmx/data-mining-extensions-dmx-statements.md)   
 [Conventions de syntaxe du&#41; DMX &#40;Data Mining Extensions](../dmx/data-mining-extensions-dmx-syntax-conventions.md)   
 [Fonctions de prédiction générales &#40;&#41;DMX ](../dmx/general-prediction-functions-dmx.md)   
 [Structure et utilisation des requêtes de prédiction DMX](../dmx/structure-and-usage-of-dmx-prediction-queries.md)   
 [Présentation de l'instruction DMX Select](../dmx/understanding-the-dmx-select-statement.md)  
  
  
