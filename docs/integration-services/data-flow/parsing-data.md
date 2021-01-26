---
description: Analyse de données
title: Analyse de données | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- parsing [Integration Services]
- data parsing [Integration Services]
ms.assetid: 8893ea9d-634c-4309-b52c-6337222dcb39
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 905c5988860aef1a2e495bb17d98a48eaf91c5ce
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594835"
---
# <a name="parsing-data"></a>Analyse de données

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Les flux de données des packages extraient et chargent des données à partir de banques de données hétérogènes qui peuvent utiliser différents types de données standard et personnalisés. Dans un flux de données, les sources [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sont chargées d’extraire les données, d’analyser les données de type string et de les convertir en données de type [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Les transformations effectuées par la suite peuvent analyser les données afin de les convertir en un type distinct ou créer des copies de colonnes avec d'autres types de données. Les expressions utilisées dans les composants peuvent également convertir les arguments et opérandes en d'autres types de données. Enfin, lorsque les données sont chargées dans une banque de données, la destination peut analyser les données afin de les convertir en un type de données utilisé par la destination. Pour plus d’informations, consultez [Types de données Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="two-types-of-parsing"></a>Deux types d’analyses  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] propose deux types d’analyses en vue de convertir les données : l’analyse rapide et l’analyse standard.  
  
-   L'analyse rapide est un ensemble de routines simple et rapide d'analyse des données. Elle ne prend pas en charge la conversion des données présentant des spécificités régionales et accepte uniquement les formats de date et d'heure les plus courants. 
  
-   L'analyse standard est un ensemble complet de routines d'analyse qui prend en charge la conversion de tous les types de données fournis par les API de conversion de type de données Automation disponibles dans Oleaut32.dll et Ole2dsip.dll.   
  
## <a name="fast-parse"></a>Analyse rapide
L'analyse rapide propose un ensemble de routines simples et rapides d'analyse des données. Ces routines ne tiennent pas compte des paramètres régionaux et prennent en charge uniquement un sous-ensemble de formats de date, d'heure et d'entier.  
  
### <a name="requirements-and-limitations"></a>Conditions requises et limitations :  
 En implémentant l'analyse rapide, un package perd sa capacité d'interpréter les données de type date, heure et nombre dans des formats régionaux et dans de nombreux formats de base et étendus ISO 9601 couramment utilisés, mais il améliore ses performances. Par exemple, l'analyse rapide prend uniquement en charge les formats de date les plus courants, tels que AAAAMMJJ et AAAA-MM-JJ, n'effectue aucune analyse des spécificités régionales, ne reconnaît pas les caractères spéciaux dans les devises et ne peut pas convertir les représentations hexadécimales ou scientifiques des entiers.  
  
 L'analyse rapide est disponible uniquement lorsque vous utilisez la source de fichier plat ou la transformation de conversion de données. L'amélioration des performances pouvant être significative, pensez à utiliser si possible l'analyse rapide dans ces composants de flux de données.  
  
 Si le flux de données du package requiert une analyse des spécificités régionales, il est préférable d'utiliser l'analyse standard. Par exemple, l'analyse rapide ne reconnaît pas les données qui incluent des symboles décimaux comme la virgule, les formats de date autres que année-mois-jour ou encore les symboles de devises.  
  
 Les représentations tronquées qui laissent supposer une ou plusieurs parties de la date, comme un siècle, une année ou un mois, ne sont pas reconnues par l'analyse rapide. Par exemple, l’analyse rapide ne reconnaît pas le format '**-AAMM**', qui indique une année et un mois dans un siècle implicite, ni le format '**--MM**', qui spécifie un mois dans une année implicite. Cependant, certaines représentations avec une précision réduite sont reconnues. Ainsi, l'analyse rapide reconnaît le format 'hhmm', qui indique les heures et les minutes uniquement, et '**AAAA**', qui indique l'année uniquement.  
  
 L'analyse rapide est spécifiée au niveau de la colonne. Dans la source de fichier plat et la transformation de conversion de données, vous pouvez spécifier l'analyse rapide sur les colonnes de sortie. Les entrées et les sorties peuvent inclure des colonnes respectant des spécificités régionales et des colonnes n'en respectant pas.  
 
## <a name="numeric-data-formats-fast-parse"></a>Formats de données numériques (Analyse rapide)
L'analyse rapide fournit un ensemble de routines simple, rapide et insensible aux paramètres régionaux pour l'analyse des données. Elle prend en charge uniquement un ensemble limité de formats pour les types de données integer.  
  
### <a name="integer-data-type"></a>Type de données Integer
 Les types de données integer fournis par [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sont DT_I1, DT_UI1, DT_I2, DT_UI2, DT_I4, DT_UI4, DT_I8 et DT_UI8. Pour plus d’informations, consultez [Types de données Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
 L'analyse rapide prend en charge les formats suivants pour les types de données integer :  
  
-   Zéro ou plusieurs espaces de début et de fin ou taquets de tabulation. Par exemple, la valeur «  123  » est valide. Une valeur composée uniquement d'espaces est évaluée comme zéro.  
  
-   Un signe plus ou signe moins de début, ou ni l'un ni l'autre. Par exemple, les valeurs +123, -123 et 123 sont valides.  
  
-   Un ou plusieurs chiffres hindou-arabe (0-9). Par exemple, la valeur 345 est valide. Aucun autre chiffre linguistique n'est pris en charge.  
  
 Les formats de données non pris en charge sont les suivants :  
  
-   Caractères spéciaux. Par exemple, le symbole monétaire $ n'est pas pris en charge et la valeur $20 ne peut pas être analysée.  
  
-   Les caractères d'espaces blancs tels que les sauts de ligne, les retours chariot et les espaces insécables. Par exemple, la valeur « 123» ne peut pas être analysée.  
  
-   Les représentations hexadécimales d'entiers. Par exemple, la valeur 2EE ne peut pas être analysée.  
  
-   La représentation en notation scientifique d'entiers. Par exemple, la valeur 1E+10 ne peut pas être analysée.  
  
 Les formats suivants sont des formats de données de sortie pour les entiers :  
  
-   Un signe moins pour les nombres négatifs et rien pour les nombres positifs.  
  
-   Aucun espace blanc.  
  
-   Un ou plusieurs chiffres hindou-arabe (0-9).  

## <a name="date-and-time-formats-fast-parse"></a>Formats de date et d’heure (Analyse rapide)
L'analyse rapide propose un ensemble de routines simples et rapides d'analyse des données. L'analyse rapide prend en charge les formats suivants de types de données de date et d'heure.  
  
### <a name="date-data-type"></a>Type de données Date 
 L'analyse rapide prend en charge les formats de chaîne suivants pour les données de date :  
  
-   Formats de date qui incluent des espaces de début. Par exemple, la valeur «  2004- 02-03 » est valide.  
  
-   Formats ISO 8601 répertoriés dans le tableau suivant :  
  
    |Format|Description|  
    |------------|-----------------|  
    |AAAAMMJJ<br /><br /> AAAA-MM-JJ|Formats de base et étendus pour une année à quatre chiffres, un mois à deux chiffres et un jour à deux chiffres. Dans le format étendu, les parties de la date sont séparées par un tiret (-).|  
    |AAAA-MM|Formats à précision réduite de base et étendus pour une année à quatre chiffres et un mois à deux chiffres. Dans le format étendu, les parties de la date sont séparées par un tiret (-).|  
    |YYYY|Le format à précision réduite est une année à quatre chiffres.|  
  
 L'analyse rapide ne prend pas en charge les formats suivants pour les données de date :  
  
-   Valeurs de mois alphabétiques. Par exemple, le format de date Oct-31-2003 n'est pas valide.  
  
-   Les formats ambigus tels que JJ-MM-AAAA et MM-JJ-AAAA. Par exemple, les dates 03-04-1995 et 04-03-1995 ne sont pas valides.  
  
-   Formats tronqués de base et étendus pour une année civile à quatre chiffres et un jour à trois chiffres dans une année, AAAAJJJ et AAAA-JJJ.  
  
-   Formats de base et étendus pour une année à quatre chiffres, un nombre à deux chiffres pour la semaine de l'année et un nombre à un chiffre pour le jour de la semaine, AAAASssJ et AAAA-Sss-J.  
  
-   Les formats tronqués de base et étendus pour une date comportant l'année et la semaine sont une année à quatre chiffres et un nombre à deux chiffres pour la semaine, AAASss et AAA-Sss.  
  
 L'analyse rapide génère en sortie des données sous la forme DT_DBDATE. Les valeurs de date aux formats tronqués sont complétées. Par exemple, AAA devient AAAA0101.  
  
 Pour plus d’informations, consultez [Types de données Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
### <a name="time-data-type"></a>Type de données Time
 L'analyse rapide prend en charge les formats de chaîne suivants pour les données d'heure :  
  
-   Formats d'heure qui incluent des espaces de début. Par exemple, la valeur « 10:24» est valide.  
  
-   Format 24 heures. L'analyse rapide ne prend pas en charge la notation AM et PM.  
  
-   Formats d'heure ISO 8601 répertoriés dans le tableau suivant :  
  
    |Format|Description|  
    |------------|-----------------|  
    |HHMISS<br /><br /> HH:MI:SS|Formats de base et étendus pour une heure à deux chiffres, une minute à deux chiffres et une seconde à deux chiffres. Dans le format étendu, les parties de l'heure sont séparées par deux points (:).|  
    |HHMI<br /><br /> HH:MI|Format tronqué de base et étendu pour une heure à deux chiffres et une minute à deux chiffres. Dans le format étendu, les parties de l'heure sont séparées par deux points (:).|  
    |HH|Format tronqué pour une heure à deux chiffres.|  
    |00:00:00<br /><br /> 000000<br /><br /> 0000<br /><br /> 00<br /><br /> 240000<br /><br /> 24:00:00<br /><br /> 2 400<br /><br /> 24|Format pour minuit.|  
  
-   Formats d'heure qui spécifient un fuseau horaire, répertoriés dans le tableau suivant :  
  
    |Format|Description|  
    |------------|-----------------|  
    |+HH:MI<br /><br /> +HHMI|Formats de base et étendus qui indiquent le nombre d'heures et de minutes ajoutées au temps universel coordonné (UTC) pour obtenir l'heure locale.|  
    |-HH:MI<br /><br /> -HHMI|Formats de base et étendus qui indiquent le nombre d'heures et de minutes soustraites du temps universel coordonné (UTC) pour obtenir l'heure locale.|  
    |+HH|Format tronqué qui indique le nombre d'heures ajoutées au temps universel coordonné (UTC) pour obtenir l'heure locale.|  
    |-HH|Format tronqué qui indique le nombre d'heures soustraites du temps universel coordonné (UTC) pour obtenir l'heure locale.|  
    |Z|Valeur 0 qui indique que l'heure est représentée au format UTC.|  
  
     Les formats de toutes les données d'heure et de date/heure peuvent inclure un élément de fuseau horaire. Toutefois, le système ignore la valeur de fuseau horaire, sauf lorsque les données sont de type DT_DBTIMESTAMPOFFSET. Pour plus d’informations, consultez [Types de données Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
     Dans les formats qui incluent un élément de fuseau horaire, aucun espace ne figure entre l'élément d'heure et l'élément de fuseau horaire, comme le montre l'exemple suivant :  
  
     HH:MI:SS[+HH:MI]  
  
     Dans l'exemple précédent, les crochets indiquent que la valeur de fuseau horaire est facultative.  
  
-   Formats d'heure qui incluent une fraction décimale, répertoriés dans le tableau suivant :  
  
    |Format|Description|  
    |------------|-----------------|  
    |HH[.nnnnnnn]|n est une valeur comprise entre 0 et 9 999 999 qui représente une fraction d'heures. Les crochets indiquent que cette valeur est facultative.<br /><br /> Par exemple, la valeur 12.750 signifie 12:45.|  
    |HHMI[.nnnnnnn]<br /><br /> HH:MI[.nnnnnnn]|n est une valeur comprise entre 0 et 9 999 999 qui représente une fraction de minutes. Les crochets indiquent que cette valeur est facultative.<br /><br /> Par exemple, la valeur 1220.500 signifie 12:20:30.|  
    |HHMISS[.nnnnnnn]<br /><br /> HH:MI:SS[.nnnnnnn]|n est une valeur comprise entre 0 et 9 999 999 qui représente une fraction de secondes. Les crochets indiquent que cette valeur est facultative.<br /><br /> Par exemple, la valeur 122040.250 signifie 12:20:40.15.|  
  
    > [!NOTE]  
    >  Dans le tableau précédent, le séparateur de fraction pour les formats d'heure peut être une décimale ou une virgule.  
  
-   Valeurs d'heure qui incluent un saut de seconde, comme le montrent les exemples suivants :  
  
     23:59:60[.0000000]  
  
     235960[.0000000]  
  
 L'analyse rapide génère en sortie des chaînes sous la forme DT_DBTIME et DT_DBTIME2. Les valeurs d'heure aux formats tronqués sont complétées. Par exemple, HH:MI devient HH:MM:00.000.  
  
 Pour plus d’informations, consultez [Types de données Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
### <a name="datetime-data-type"></a>Type de données de date/heure  
 L'analyse rapide prend en charge les formats de chaîne suivants pour les données de date/heure :  
  
-   Formats qui incluent des espaces de début. Par exemple, la valeur « 2003-01-10T203910» est valide.  
  
-   Combinaisons de formats de date valides et de formats d'heure valides séparés par un T majuscule, ainsi que de formats de fuseau horaire valides, par exemple YYYYMMDDT[HHMISS][+HH:MI]. Les valeurs d'heure et de fuseau horaire ne sont pas requises. Par exemple, « 2003-10-14 » est valide.  
  
 L'analyse rapide ne prend pas en charge les intervalles de temps. Par exemple, un intervalle de temps identifié par une date et une heure de début et de fin au format AAAAMMJJThhmmss/AAAAMMJJThhmmss ne peut pas être analysé.  
  
 L'analyse rapide génère en sortie des chaînes sous la forme DT_DATE, DT_DBTIMESTAMP, DT_DBTIMESTAMP2 et DT_DBTIMESTAMPOFFSET. Les valeurs de date/heure aux formats tronqués sont complétées. Le tableau suivant répertorie les valeurs ajoutées pour les parties des date et heure manquantes.  
  
|Partie de date/heure|Remplissage|  
|---------------------|-------------|  
|Secondes|Ajout de 00.|  
|Minutes|Ajouter 00:00.|  
|Heure|Ajout de 00:00:00.|  
|Jour|Ajout de 01 pour le jour du mois.|  
|Month (Mois)|Ajout de 01 pour le mois de l'année.|  
  
 Pour plus d’informations, consultez [Types de données Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="enable-fast-parse"></a>Activer l’analyse rapide
La propriété d'analyse rapide doit être définie pour chaque colonne de la source ou de la transformation utilisant l'analyse rapide. Pour définir cette propriété, faites appel à l'éditeur avancé de la source de fichier plat et de la transformation de conversion de données.  
  
1.  Cliquez avec le bouton droit sur la source de fichier plat ou sur la transformation de conversion de données, puis cliquez sur **Afficher l’éditeur avancé**.  
  
2.  Dans la boîte de dialogue **Éditeur avancé** , cliquez sur l'onglet **Propriétés d'entrée et de sortie** .  
  
3.  Dans le volet **Entrées et sorties** , cliquez sur la colonne pour laquelle vous souhaitez activer l'analyse rapide.  
  
4.  Dans la fenêtre Propriétés, développez le nœud **Propriétés personnalisées** , puis définissez la propriété **FastParse** à **True**.  
  
5.  Cliquez sur **OK**.  

## <a name="standard-parse"></a>Analyse standard
L'analyse standard est un ensemble de routines d'analyse spécifique à un pays qui prend en charge toutes les conversions de type de données fournies par les API de conversion de type de données Automation disponibles dans Oleaut32.dll et Ole2dsip.dll. Elle est équivalente aux API d'analyse OLE DB.  
  
 L'analyse standard assure la prise en charge de la conversion de type de données pour les données internationales et doit être utilisée si le format de données n'est pas pris en charge par l'analyse rapide. Pour plus d'informations sur l'API de conversion de type de données Automation, consultez « API de conversion de type de données » dans [MSDN Library](/documentation/). 
