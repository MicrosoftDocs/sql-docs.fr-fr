---
title: Types de données dans les expressions (Générateur de rapports) | Microsoft Docs
description: Explorez la façon dont les types de données représentent différents genres de données afin que les données puissent être stockées et traitées dans le Générateur de rapports.
ms.date: 08/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 94fdf921-270c-4c12-87b3-46b1cc98fae5
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0202703dec933871d6b6a611ff1139a68ee2c938
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595363"
---
# <a name="data-types-in-expressions-report-builder-and-ssrs"></a>Types de données dans les expressions (Générateur de rapports et SSRS)
  Les données sont représentées par différents types de données permettant de les stocker et de les traiter de manière efficace. Les types de données typiques incluent du texte (également appelé chaînes), des numéros avec et sans décimales, des dates, des heures et des images. Les valeurs dans un rapport doivent être un type de données RDL (Report Definition Language). Vous pouvez mettre en forme une valeur selon votre préférence lorsque vous l'affichez dans un rapport. Par exemple, un champ qui représente une devise est stocké dans la définition de rapport sous la forme d'un nombre à virgule flottante, mais peut être affiché sous divers formats en fonction de la propriété de format choisie.  
  
 Pour plus d’informations sur les formats d’affichage, consultez [Mise en forme des éléments de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-report-items-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="report-definition-language-rdl-data-types-and-common-language-runtime-clr-data-types"></a>Types de données RDL (Report Definition Language) et CLR (Common Language Runtime)  
 Les valeurs spécifiées dans un fichier RDL doivent être un type de données RDL. Lorsque le rapport est compilé et traité, les types de données RDL sont convertis en types de données CLR. Le tableau suivant affiche la conversion, marquée Valeur par défaut :  
  
|Type RDL|Types CLR|  
|--------------|---------------|  
|String|Valeur par défaut : String<br /><br /> Chart, GUID, Timespan|  
|Boolean|Valeur par défaut : Boolean|  
|Integer|Valeur par défaut : Int64<br /><br /> Int16, Int32, Uint16, Uint64, Byte, Sbyte|  
|DateTime|Valeur par défaut : DateTime<br /><br /> DateTimeOffset|  
|Float|Valeur par défaut : Double<br /><br /> Single, Decimal|  
|Binary|Valeur par défaut : Byte[]|  
|Variant|Une des valeurs ci-dessus à l'exception de Byte[]|  
|VariantArray|Tableau de type Variant|  
|Sérialisable|Variant ou types marqués avec Serializable ou qui implémentent ISerializable.|  
  
## <a name="understanding-data-types-and-writing-expressions"></a>Fonctionnement des types de données et écriture des expressions  
 Il est important de maîtriser les types de données lorsque vous écrivez des expressions de comparaison ou de combinaison de valeurs, notamment lorsque vous définissez des expressions de groupe ou de filtre ou calculez des agrégats. Les comparaisons et calculs sont uniquement valides entre éléments du même type de données. Si les types de données ne concordent pas, vous devez convertir le type de données de manière explicite dans l'élément de rapport à l'aide d'une expression.  
  
 La liste suivante décrit les circonstances dans lesquelles vous pouvez être amené à convertir des données dans un type différent :  
  
-   Comparaison de la valeur d'un paramètre de rapport d'un type de données spécifique avec un champ de dataset d'un type de données différent.  
  
-   Écriture d'expressions de filtre qui comparent des valeurs de types de données différents.  
  
-   Écriture d'expressions de tri qui associent des champs de types de données différents.  
  
-   Écriture d'expressions de groupe qui associent des champs de types de données différents.  
  
-   Conversion d'une valeur extraite d'une source de données d'un type de données à un autre.  
  
## <a name="determining-the-data-type-of-report-data"></a>Détermination du type de données d'un rapport  
 Pour déterminer le type de données d'un élément de rapport, vous pouvez écrire une expression qui retourne son type de données. Par exemple, pour afficher le type de données du champ `MyField`, ajoutez l'expression suivante à une cellule de tableau : `=Fields!MyField.Value.GetType().ToString()`. Le résultat affiche le type de données CLR utilisé pour représenter `MyField`, par exemple, **System.String** ou **System.DateTime**.  
  
## <a name="converting-dataset-fields-to-a-different-data-type"></a>Conversion de champs de dataset en un type de données différent  
 Vous pouvez également convertir des champs de dataset avant de les utiliser dans un rapport. La liste suivante décrit les différentes méthodes de conversion d'un champ de dataset existant :  
  
-   Modifiez la requête de dataset pour ajouter un nouveau champ de requête avec les données converties. Pour les sources de données relationnelles ou multidimensionnelles, cette procédure utilise les ressources de la source de données pour effectuer la conversion.  
  
-   Créez un champ calculé à partir d'un champ de dataset du rapport existant en écrivant une expression qui convertit toutes les données d'une colonne d'ensemble de résultats en une nouvelle colonne utilisant un type de données différent. Par exemple, l'expression suivante convertit la valeur du champ Année de nombre entier en chaîne : `=CStr(Fields!Year.Value)`. Pour plus d’informations, consultez [Ajouter, modifier ou actualiser des champs dans le volet des données de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md).  
  
-   Vérifiez si l'extension de traitement des données que vous utilisez inclut les métadonnées permettant d'extraire des données préformatées. Par exemple, une requête MDX [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] inclut une propriété étendue FORMATTED_VALUE pour les valeurs de cube qui ont déjà été mises en forme pendant le traitement du cube. Pour plus d’informations, consultez [Propriétés de champ étendues pour une base de données Analysis Services &#40;SSRS&#41;](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md).  
  
## <a name="understanding-parameter-data-types"></a>Présentation des types de données des paramètres  
 Les paramètres de rapport doivent avoir l’un des cinq types de données : Boolean, DateTime, Integer, Float ou Text (également appelé String). Lorsqu'une requête de dataset comprend des paramètres de requête, des paramètres de rapport sont automatiquement créés et liés aux paramètres de requête. Le type de données par défaut d'un paramètre de rapport est String. Pour modifier le type de données par défaut d’un paramètre de rapport, sélectionnez la valeur correcte dans la liste déroulante **Type de données** de la page **Général** de la boîte de dialogue **Propriétés du paramètre de rapport** .  
  
> [!NOTE]  
>  Les paramètres de rapport utilisant le type de données DateTime ne prennent pas en charge les millisecondes. Même s'il est possible de créer un paramètre basé sur des valeurs incluant des millisecondes, vous ne pouvez pas sélectionner de valeur dans une liste déroulante de valeurs disponibles qui comprend des valeurs Date ou Time utilisant des millisecondes.  
  
## <a name="writing-expressions-that-convert-data-types-or-extract-parts-of-data"></a>Écriture d'expressions convertissant des types de données ou extrayant des données partielles  
 Lorsque vous associez des champs de dataset et de texte à l'aide de l'opérateur de l'opérateur de concaténation (&), le common language runtime (CLR) fournit généralement des formats par défaut. Lorsque vous devez convertir explicitement un champ de dataset ou un paramètre dans un type de données spécifique, vous devez utiliser une méthode CLR ou une fonction de bibliothèque Runtime Visual Basic pour convertir les données.  
  
 Le tableau suivant présente des exemples de conversion de types de données.  
  
|Type de conversion|Exemple|  
|------------------------|-------------|  
|DateTime en String|`=CStr(Fields!Date.Value)`|  
|String en DateTime|`=DateTime.Parse(Fields!DateTimeinStringFormat.Value)`|  
|String en DateTimeOffset|`=DateTimeOffset.Parse(Fields!DateTimeOffsetinStringFormat.Value)`|  
|Extraction de l'année|`=Year(Fields!TimeinStringFormat.Value)`<br /><br /> `-- or --`<br /><br /> `=Year(Fields!TimeinDateTimeFormat.Value)`|  
|Boolean en Integer|`=CInt(Parameters!BooleanField.Value)`<br /><br /> -1 est True et 0 est False.|  
|Boolean en Integer|`=System.Convert.ToInt32(Fields!BooleanFormat.Value)`<br /><br /> 1 est True et 0 est False.|  
|Uniquement la partie DateTime d'une valeur DateTimeOffset|`=Fields!MyDatetimeOffset.Value.DateTime`|  
|Uniquement la partie Offset d'une valeur DateTimeOffset|`=Fields!MyDatetimeOffset.Value.Offset`|  
  
 Vous pouvez également utiliser la fonction Format pour contrôler le format d'affichage de la valeur. Pour plus d’informations, consultez [Fonctions (Visual Basic)](/dotnet/visual-basic/language-reference/functions/).  
  
## <a name="advanced-examples"></a>Exemples avancés  
 Lorsque vous vous connectez à une source de données par le biais d'un fournisseur de données qui ne prend pas en charge la conversion de tous les types de données de la source, le type de données par défaut utilisé pour les types de source de données non pris en charge est Chaîne. Les exemples suivants proposent des solutions pour les types de données spécifiques retournés en tant que chaîne.  
  
### <a name="concatenating-a-string-and-a-clr-datetimeoffset-data-type"></a>Concaténation d'une chaîne en type de données CLR DateTimeOffset  
 Pour la plupart des types de données, le CLR assure des conversions par défaut vous permettant de concaténer des valeurs utilisant des types de données différents en une seule chaîne à l'aide de l'opérateur &. Par exemple, l’expression suivante concatène le texte « Les date et heure sont : » avec un champ de dataset StartDate, qui est une valeur <xref:System.DateTime> : `="The date and time are: " & Fields!StartDate.Value`.  
  
 Pour certains types de données, il peut être nécessaire d'inclure la fonction ToString. Par exemple, l’expression suivante affiche le même exemple en utilisant le type de données CLR <xref:System.DateTimeOffset>qui inclut la date, l’heure et un décalage de fuseau horaire par rapport au fuseau horaire UTC : `="The time is: " & Fields!StartDate.Value.ToString()`.  
  
### <a name="converting-a-string-data-type-to-a-clr-datetime-data-type"></a>Conversion d'un type de données String en type de données CLR DateTime  
 Si une extension de traitement de données ne prend pas en charge tous les types de données définis dans une source de données, les données peuvent être extraites au format texte. Par exemple, une valeur utilisant le type de données **datetimeoffset(7)** peut être extraite en tant que type de données String. À Perth, en Australie, la valeur de chaîne pour le 1er juillet 2008, à 6:05:07.9999999 A.M. s'affiche alors sous la forme :  
  
 `2008-07-01 06:05:07.9999999 +08:00`  
  
 Cet exemple montre la date (le 1er juillet 2008), suivie de l'heure avec une précision de 7 chiffres (6:05:07.9999999 A.M.), suivie d'un décalage par rapport au fuseau horaire UTC exprimé en heures et en minutes (plus 8 heures, 0 minutes). Dans les exemples suivants, cette valeur a été placée dans un champ **Chaîne** appelé `MyDateTime.Value`.  
  
 Vous pouvez utiliser l'une des méthodes suivantes pour convertir ces données en une ou plusieurs valeurs CLR :  
  
-   Dans une zone de texte, utilisez une expression pour extraire certaines parties de la chaîne. Par exemple :  
  
    -   L'expression suivante extrait uniquement la partie Heure du décalage par rapport au fuseau horaire UTC et la convertit en minutes : `=CInt(Fields!MyDateTime.Value.Substring(Fields!MyDateTime.Value.Length-5,2)) * 60`  
  
         Le résultat est `480`.  
  
    -   L'expression suivante convertit la chaîne en une valeur de date et heure : `=DateTime.Parse(Fields!MyDateTime.Value)`  
  
         Si la chaîne `MyDateTime.Value` utilise un décalage UTC, la fonction `DateTime.Parse` ajuste tout d'abord le décalage UTC (7 A.M. - [`+08:00`] de manière à afficher l'heure UTC 11 P.M. la nuit précédente). La fonction `DateTime.Parse` applique ensuite le décalage UTC du serveur de rapports local et, si nécessaire, ajuste à nouveau l'heure pour tenir compte de l'heure d'été. Par exemple, à Redmond, dans l'état de Washington, le décalage horaire local ajusté pour tenir compte de l'heure d'été est `[-07:00]`, ou 7 heures avant 11 PM. Le résultat correspond à la valeur **DateTime** suivante : `2007-07-06 04:07:07 PM` (6 juillet 2007 à 4:07 P.M).  
  
 Pour plus d’informations sur la conversion de chaînes en types de données **DateTime** , consultez les rubriques [Analyse des chaînes de date/heure](https://go.microsoft.com/fwlink/?LinkId=89703), [Mise en forme de la date et de l’heure pour une culture spécifique](https://go.microsoft.com/fwlink/?LinkId=89704)et [Choosing Between DateTime, DateTimeOffsetet TimeZoneInfo](/dotnet/standard/datetime/choosing-between-datetime) dans la bibliothèque MSDN.  
  
-   Ajoutez un nouveau champ calculé au dataset du rapport qui utilise une expression pour extraire certaines parties de la chaîne. Pour plus d’informations, consultez [Ajouter, modifier ou actualiser des champs dans le volet des données de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md).  
  
-   Modifiez la requête de dataset du rapport pour utiliser des fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] afin d'extraire indépendamment les valeurs de date et heure et de créer des colonnes séparées. L'exemple suivant montre comment utiliser la fonction **DatePart** afin d'ajouter une colonne pour l'année et une autre pour le fuseau horaire UTC converti en minutes.  
  
     `SELECT`  
  
     `MyDateTime,`  
  
     `DATEPART(year, MyDateTime) AS Year,`  
  
     `DATEPART(tz, MyDateTime) AS OffsetinMinutes`  
  
     `FROM MyDates`  
  
     L'ensemble de résultats comporte trois colonnes. La première colonne correspond à la date et à l'heure, la deuxième à l'année, et la troisième au décalage UTC en minutes La ligne suivante affiche des exemples de données :  
  
     `2008-07-01 06:05:07             2008                   480`  
  
 Pour plus d’informations sur les types de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md) et [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 Pour plus d’informations sur les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], consultez [Types de données dans Analysis Services](/analysis-services/multidimensional-models/olap-physical/data-types-in-analysis-services).  
  
## <a name="see-also"></a>Voir aussi  
 [Mise en forme des éléments de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-report-items-report-builder-and-ssrs.md)  
  
